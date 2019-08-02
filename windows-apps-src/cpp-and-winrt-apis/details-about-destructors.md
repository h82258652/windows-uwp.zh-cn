---
description: 使用 C++/WinRT 2.0 可以延迟析构实现类型，并可在析构期间安全地进行查询。 本主题介绍了这些功能，并说明了何时应使用这些功能。
title: 有关析构函数的详细信息
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 延迟析构, 安全查询
ms.localizationpriority: medium
ms.openlocfilehash: 9806ea54665b24c246f2023714a14d94ec3bcc8e
ms.sourcegitcommit: 02cc7aaa408efe280b089ff27484e8bc879adf23
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2019
ms.locfileid: "68387794"
---
# <a name="details-about-destructors"></a>有关析构函数的详细信息

使用 C++/WinRT 2.0 可以延迟析构实现类型，并可在析构期间安全地进行查询。 本主题介绍了这些功能，并说明了何时应使用这些功能。

## <a name="deferred-destruction"></a>延迟析构

在 [Diagnosing direct allocations](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc)（诊断直接分配）主题中，我们提到实现类型不能有专用析构函数。

使用公共析构函数的好处是，它支持延迟析构。此功能可以检测在对象上进行的最终 [**IUnknown::Release**](/windows/win32/api/unknwn/nf-unknwn-iunknown-release) 调用，然后通过获取该对象的所有权来无限期延迟其析构。

回忆一下，经典 COM 对象进行内部引用计数；引用计数通过 [**IUnknown::AddRef**](/windows/win32/api/unknwn/nf-unknwn-iunknown-addref) 和 **IUnknown::Release** 函数来管理。 在 **Release** 的传统实现中，一旦引用计数为 0，就会调用经典 COM 对象的 C++ 析构函数。

```cppwinrt
uint32_t WINRT_CALL Release() noexcept
{
    uint32_t const remaining{ subtract_reference() };
 
    if (remaining == 0)
    {
        delete this;
    }
 
    return remaining;
}
```

`delete this;` 在释放对象占用的内存之前调用对象的析构函数。 如果你不需要在析构函数中执行某些微妙的操作，那么这没有任何问题。

```cppwinrt
using namespace winrt::Windows::Foundation;
... 
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    ~Sample() noexcept
    {
        // Too late to do anything interesting.
    }
};
```

这里的“微妙”是指什么呢？  首先，析构函数本质上是同步的。 不能通过切换线程来执行某些操作&mdash;例如，不能析构另一上下文中某些特定于线程的资源。 不能通过可靠方式查询对象来获取某个其他的接口，而该接口可能是释放某些资源所必需的。 此外还有其他例子，这里不一一例举。 如果析构影响重大，则需要更灵活的解决方案。 这种情况下，需要使用 C++/WinRT 的 **final_release** 函数。

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        // This is the first stop...
    }
 
    ~Sample() noexcept
    {
        // ...And this happens only when *unique_ptr* finally deletes the object.
    }
};
```

我们已更新了 **Release** 的 C++/WinRT 实现，一旦对象的引用计数为 0 就会调用 **final_release**。 在该状态下，对象可以确信不会有更多待处理引用，因此其现在对自己有独占性的所有权。 由于这个原因，它可以将自身的所有权移交给静态 **final_release** 函数。

换而言之，此对象已将自身从支持共享所有权的对象转换成所有权被独占的对象。 **std::unique_ptr** 独占此对象的所有权，自然会通过其语义来析构此对象，因此需要在 **std::unique_ptr** 超出作用域时使用公共析构函数（前提是没有在此之前将其移到其他位置）。 这很关键。 可以无限期地使用此对象，前提是 **std::unique_ptr** 让此对象保持活动状态。 下面显示了如何将对象移到别处。

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        gc.push_back(std::move(ptr));
    }
};
```

可以将它视为更具确定性的垃圾回收器。 可以将 **final_release** 函数转换成协同程序，集中处理其最终析构，同时可以根据需要暂停和切换线程，这样也许更实际且功能更强大。

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static winrt::fire_and_forget final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        co_await winrt::resume_background(); // Unwind the calling thread.
 
        // Safely perform complex teardown here.
    }
};
```

暂停点会导致调用线程（最初启动对 **IUnknown::Release** 函数的调用）返回，向调用方表明它曾经拥有的对象不再通过该接口指针提供。 UI 框架通常需确保在最初创建对象的特定 UI 线程上将对象析构。 此功能使得那样的要求履行起来很简单，因为析构独立于对象的释放。

## <a name="safe-queries-during-destruction"></a>在析构期间进行安全的查询

基于延迟析构概念之上的是在析构期间安全查询接口的功能。

经典 COM 基于两个核心概念。 第一个是引用计数，第二个是查询接口。 除了 **AddRef** 和 **Release**，**IUnknown** 接口还提供 [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(refiid_void)。 该方法由特定的 UI 框架（例如 XAML）在模拟其可组合类型系统时频繁用来遍历 XAML 层次结构。 让我们考虑一个简单示例。

```cppwinrt
struct MainPage : PageT<MainPage>
{
    ~MainPage()
    {
        DataContext(nullptr);
    }
};
```

该示例看起来似乎无害。  此 XAML 页需清除其析构函数中的数据上下文。 但是，[**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 是 **FrameworkElement** 基类的属性，它生存在独特的 **IFrameworkElement** 接口上。 因此，C++/WinRT 必须注入一个针对 **QueryInterface** 的调用来查找正确的 vtable，然后才能调用 **DataContext** 属性。 但是，之所以我们位于析构器中，是因为引用计数已经为 0。 在这里调用 **QueryInterface** 会暂时影响该引用计数；当它再次为 0 时，对象会再次析构。

C++/WinRT 2.0 在经过强化后支持此功能。 下面是 C++/WinRT 2.0 对 Release 的实现，采用简化的形式。

```cppwinrt
uint32_t Release() noexcept
{
    uint32_t const remaining{ subtract_reference() };
 
    if (remaining == 0)
    {
        m_references = 1; // Debouncing!
        T::final_release(...);
    }
 
    return remaining;
}
```

你可能已经预测到，它首先将引用计数递减，然后，只有在没有待处理引用的情况下才进行操作。 但是，在调用我们此前在本主题中介绍的静态 **final_release** 函数之前，它会将引用计数设置为 1，使之保持稳定。 我们称之为“去抖”（借用电气工程的术语）。  这对于防止最终引用的释放很重要。 一旦发生释放，引用计数将变得不稳定，无法可靠地支持对 **QueryInterface** 的调用。

在释放最终引用以后，调用 **QueryInterface** 是很危险的，因为引用计数随后可能会无限增加。 你有责任确保只调用已知的不会延长对象生存期的代码路径。 C++/WinRT 可确保这些 **QueryInterface** 调用的可靠性，因此符合你的要求。

其实现这一点的方式是稳定引用计数。 释放最终引用以后，实际的引用计数为 0，或者为某个根本无法预测的值。 如果涉及弱引用，则可能会发生后面这种情况。 不管什么情况，如果随后调用 **QueryInterface**，则这会变得不可持续，因为这必然会导致引用计数临时性递增&mdash;因此需要让引用去抖。 将它设置为 1 可确保永远不会在该对象上再次对 **Release** 进行最终调用。 这正是我们所需要的，因为 **std::unique_ptr** 现在拥有该对象，但是对 **QueryInterface**/**Release** 对进行绑定调用将会是安全的。

让我们考虑一个更有趣的示例。

```cppwinrt
struct MainPage : PageT<MainPage>
{
    ~MainPage()
    {
        DataContext(nullptr);
    }

    static winrt::fire_and_forget final_release(std::unique_ptr<MainPage> ptr)
    {
        co_await 5s;
        co_await winrt::resume_foreground(ptr->Dispatcher());
        ptr = nullptr;
    }
};
```

首先调用 **final_release** 函数，通知实现是时候进行清理了。 这里的 **final_release** 刚好是一个协同程序。 它一开始会在线程池上等待几秒钟，模拟第一个暂停点。 然后，它会在页面的调度程序线程上继续。 这最后一步涉及一个查询，因为 [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher) 是 **DependencyObject** 基类的一个属性。 最后，通过将 `nullptr` 分配给 **std::unique_ptr**，页面被实际删除。 接着就会调用页面的析构函数。

我们在析构函数中清除数据上下文；我们知道，这需要查询 **FrameworkElement** 基类。

这一切之所以能够实现，是因为 C++/WinRT 2.0 提供的引用计数去抖功能（也称为引用计数稳定功能）。
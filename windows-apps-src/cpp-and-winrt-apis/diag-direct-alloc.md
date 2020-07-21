---
description: '本主题深入探讨了一种 C++/WinRT 2.0 功能，该功能可帮助你诊断在堆栈上创建实现类型对象的错误，而无需使用 [**WinRT:: make**](/uwp/cpp-ref-for-winrt/make) 系列的帮助程序。'
title: 诊断直接分配
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 直接, 堆栈, 分配, 实现
ms.localizationpriority: medium
ms.openlocfilehash: 7fe8ff6653b8655ee25cd9adc0c11acb22d42a11
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "68372788"
---
# <a name="diagnosing-direct-allocations"></a>诊断直接分配

如[使用 C++/WinRT 创作 API](/windows/uwp/cpp-and-winrt-apis/author-apis) 中所述，在创建实现类型对象时，应该使用 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 系列的帮助程序。 本主题深入探讨了一种 C++/WinRT 2.0 功能，该功能可帮助你诊断在堆栈上直接分配实现类型对象的错误。

此类错误可能会导致莫名其妙的崩溃或损坏，调试起来难度大且费时间。 因此，这是一项重要的功能，有必要了解相关背景。

## <a name="setting-the-scene-with-mystringable"></a>从 **MyStringable** 着手

首先，让我们考虑一个简单的 [**IStringable**](/uwp/api/windows.foundation.istringable) 实现。

```cppwinrt
struct MyStringable : implements<MyStringable, IStringable>
{
    winrt::hstring ToString() const { return L"MyStringable"; }
};
```

现在，假设你需要（从实现中）调用某个函数，该函数需要使用 **IStringable** 作为参数。

```cppwinrt
void Print(IStringable const& stringable)
{
    printf("%ls\n", stringable.ToString().c_str());
}
```

问题在于，**MyStringable** 类型不  是 **IStringable**。

- **MyStringable** 类型是 **IStringable** 接口的实现。
- **IStringable** 类型是投影类型。

> [!IMPORTANT]
> 必须了解实现类型和投影类型之间的区别。   若要了解基本概念和术语，请务必阅读[通过 C++/WinRT 使用 API](consume-apis.md) 和[使用 C++/WinRT 创作 API](author-apis.md)。

实现和投影之间的区别可能难以把握。 事实上，若要尝试让实现感受起来更像投影一点，可以利用实现提供的隐式转换功能，将类型转换为它所实现的每个投影类型。 这并不意味着我们可以轻松实现这一点。

```cppwinrt
struct MyStringable : implements<MyStringable, IStringable>
{
    winrt::hstring ToString() const;
 
    void Call()
    {
        Print(this);
    }
};
```

相反，我们需要获取一个引用，这样就可以选择转换运算符来解析调用。

```cppwinrt
void Call()
{
    Print(*this);
}
```

这样做是可行的。 隐式转换提供的转换很高效，可以将实现类型转换为投影类型，在许多情况下十分方便。 如果没有该功能，大量的实现类型对创作者无疑是很大的负担。 如果只使用 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 函数模板（或 [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self)）来分配实现，则没有任何问题。

```cppwinrt
IStringable stringable{ winrt::make<MyStringable>() };
```

## <a name="potential-pitfalls-with-cwinrt-10"></a>使用 C++/WinRT 1.0 时可能存在的陷阱

隐式转换仍可能出现问题。 考虑一下这个并没有什么帮助的帮助程序函数。

```cppwinrt
IStringable MakeStringable()
{
    return MyStringable(); // Incorrect.
}
```

或者，甚至可以考虑一下这个明显无害的语句。

```cppwinrt
IStringable stringable{ MyStringable() }; // Also incorrect.
```

遗憾的是，由于存在该隐式转换，这样的代码  可以通过 C++/WinRT 1.0 进行编译。 这里的问题（很严重）是，可能会返回一个投影类型，该类型指向的引用计数对象的后备存储器位于临时堆栈上。

下面是可以通过 C++/WinRT 1.0 编译的其他代码。

```cppwinrt
MyStringable* stringable{ new MyStringable() }; // Very inadvisable.
```

原始指针很危险，其产生的 Bug 修复起来费时费力。 在不需要的情况下，请勿使用它们。 C++/WinRT 会通过各种措施让你在不使用原始指针的情况下也能高效运行所有代码。 下面是可以通过 C++/WinRT 1.0 编译的其他代码。

```cppwinrt
auto stringable{ std::make_shared<MyStringable>(); } // Also very inadvisable.
```

从多方面来看，这是一个错误。 我们为同一对象设置了两个不同的引用计数。 Windows 运行时（以及其之前的经典 COM）所基于的内部引用计数与 **std::shared_ptr** 不兼容。 当然，**std::shared_ptr** 有许多有效的应用，但在你共享 Windows 运行时（和经典 COM）对象时，这样应用是完全没有必要的。 这最后一个代码示例也可以通过 C++/WinRT 1.0 编译。

```cppwinrt
auto stringable{ std::make_unique<MyStringable>() }; // Highly dubious.
```

同样，此代码也问题很大。 这独一无二的所有权与 **MyStringable** 的内部引用计数的共享生存期矛盾。

## <a name="the-solution-with-cwinrt-20"></a>C++/WinRT 2.0 提供的解决方案

使用 C++/WinRT 2.0 时，所有这些直接分配实现类型的尝试都会导致编译器错误。 这种错误类型是最合适的，比莫名其妙的运行时 Bug 要好得多。

在需要创建实现时，直接使用 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 或 [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self) 即可，如上所示。 现在，如果你忘记这样做，则会出现编译器错误，其中会引用名为 **use_make_function_to_create_this_object** 的抽象函数，提示你必须这样做。 它不一定是 `static_assert`，但也差不多。 若要检测所有描述的错误，这仍然是最可靠的方式。

这意味着，我们需要为实现施加一些小的约束。 考虑到我们是通过是否缺少某个重写来检测直接分配，因此 **winrt::make** 函数模板必须以某种方式通过重写来满足抽象虚拟函数的要求。 其这样做的方式是：通过一个提供该重写的 `final` 类从实现派生。 关于此过程，有一些需要注意的事项。

首先，虚拟函数仅存在于调试版本中。 这意味着，检测不会影响已优化版本中 vtable 的大小。

其次，由于 **winrt::make** 使用的派生类是 `final`，这意味着优化器可能推导出的任何去虚拟化都会发生，即使你以前选择不将实现类标记为 `final`。 因此，这是一项改进。 反过来的情况是，你的实现不能  是 `final`。 同样，这也不要紧，因为实例化的类型将始终是 `final`。

其三，没有任何情况可以阻止你将实现中的任何虚拟函数标记为 `final`。 当然，C++/WinRT 十分不同于经典的 COM 和实现（例如 WRL），后者中的有关实现的一切都趋向于虚拟。 在 C++/WinRT 中，虚拟调度仅限于应用程序二进制接口（ABI，始终为 `final`），而实现方法依赖于编译时或静态多态性。 这避免了不必要的运行时多态性，而且也意味着，没有必要在 C++/WinRT 实现中使用虚拟函数。 这是很好的做法，大大提高了内联的可预测性。

其四，由于 **winrt::make** 注入派生类，因此实现不能有专用析构函数。 专用析构函数在经典 COM 实现中很常用，同样是因为一切都是虚拟的。通常会直接处理原始指针，因此很容易意外调用 `delete` 而不是 [**Release**](/windows/win32/api/unknwn/nf-unknwn-iunknown-release)。 C++/WinRT 会通过各种措施让你难以直接处理原始指针。 你必须费很大的力气  来获取 C++/WinRT 中的原始指针，然后才有可能对其调用 `delete`。 值语义是指你处理的是值和引用，很少处理指针。

因此，C++/WinRT 对我们固有的经典 COM 代码编写方式认知是一个挑战。 这也难怪，因为 WinRT 不是经典 COM。 经典 COM 是 Windows 运行时的汇编语言。 它不应该是你日常编写的代码。 相反，C++/WinRT 可以让你编写更类似于现代 C++ 而根本不同于经典 COM 的代码。

## <a name="important-apis"></a>重要的 API
* [winrt::make 函数模板](/uwp/cpp-ref-for-winrt/make)
* [winrt::make_self 函数模板](/uwp/cpp-ref-for-winrt/make-self)

## <a name="related-topics"></a>相关主题
* [通过 C++/WinRT 使用 API](consume-apis.md)
* [使用 C++/WinRT 创作 API](/windows/uwp/cpp-and-winrt-apis/author-apis)
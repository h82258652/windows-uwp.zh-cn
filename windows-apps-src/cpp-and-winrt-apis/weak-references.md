---
description: Windows 运行时是引用在其中占有重要地位的一个系统；在这样的系统中，了解强引用与弱引用的意义和区别非常重要。
title: C++/WinRT 中的强引用和弱引用
ms.date: 05/16/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 强, 弱, 引用
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 781b63f9f32a0fdf7edee6479b60fd82822cc745
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853334"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>C++/WinRT 中的强引用和弱引用

Windows 运行时是引用在其中占有重要地位的一个系统；在这样的系统中，了解强引用与弱引用（以及非强、非弱引用，例如隐式 *this* 指针）的意义和区别非常重要。 如本主题中所述，了解如何正确管理这些引用可以了解平稳运行的可靠系统与不可预见地崩溃的系统之间的差别。 通过提供深度支持语言投影的帮助器函数，[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 基本上能够满足方便正确地构建更复杂系统的需求。

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>在类成员协同例程中安全访问 *this* 指针

有关协同例程的详细信息和代码示例，请参阅[使用 C++/WinRT 执行并发和异步操作](/windows/uwp/cpp-and-winrt-apis/concurrency)。

以下代码列表显示了某个协同例程（某个类的成员函数）的典型示例。 可将此示例复制并粘贴到新的 **Windows 控制台应用程序 (C++/WinRT)** 项目中指定的文件内。

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

struct MyClass : winrt::implements<MyClass, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    IAsyncOperation<winrt::hstring> RetrieveValueAsync()
    {
        co_await 5s;
        co_return m_value;
    }
};

int main()
{
    winrt::init_apartment();

    auto myclass_instance{ winrt::make_self<MyClass>() };
    auto async{ myclass_instance->RetrieveValueAsync() };

    winrt::hstring result{ async.get() };
    std::wcout << result.c_str() << std::endl;
}
```

**MyClass::RetrieveValueAsync** 将花费一段时间来运行，最终返回 `MyClass::m_value` 数据成员的副本。 调用 **RetrieveValueAsync** 会导致创建异步对象，该对象具有隐式 *this* 指针（最终将通过此指针访问 `m_value`）。

请记住，在协同例程中，在第一个暂停点之前，执行是同步的；到达第一个暂停点时，控制返回到调用方。 在 **RetrieveValueAsync** 中，第一个 `co_await` 是第一个暂停点。 当协同例程恢复时（在此示例中为大约五秒以后），我们用来访问 `m_value` 的隐式 *this* 指针可能会发生任何情况。

下面是完整的事件序列。

1. 在 **main** 中创建 **MyClass** 的实例 (`myclass_instance`)。
2. 创建指向（通过 *this*）`myclass_instance` 的 `async` 对象。
3. **Winrt::Windows::Foundation::IAsyncAction::get** 函数在遇到第一个暂停点时会阻塞几秒钟，然后返回 **RetrieveValueAsync** 的结果。
4. **RetrieveValueAsync** 返回 `this->m_value` 值。

只有在 *this* 始终有效的情况下，步骤 4 才是安全的。

但是，如果在异步操作完成之前销毁了类实例，会出现什么情况？ 在异步方法完成之前，类实例可能会在各种情况下超出范围。 但是，我们可以通过将类实例设置为 `nullptr` 来模拟这些情况。

```cppwinrt
int main()
{
    winrt::init_apartment();

    auto myclass_instance{ winrt::make_self<MyClass>() };
    auto async{ myclass_instance->RetrieveValueAsync() };
    myclass_instance = nullptr; // Simulate the class instance going out of scope.

    winrt::hstring result{ async.get() }; // Behavior is now undefined; crashing is likely.
    std::wcout << result.c_str() << std::endl;
}
```

在销毁类实例的那一刻之后，似乎我们不再直接引用它。 但是，异步对象肯定有指向它的 *this* 指针，它会尝试使用该指针复制类实例中存储的值。 协同例程是一个成员函数，预期它能够使用其 *this* 指针，但存在某种代价。

对代码进行此项更改后，我们会在步骤 4 中遇到问题，因为类实例已销毁，并且 *this* 不再有效。 只要异步对象尝试访问类实例内部的变量，它就会崩溃（或执行某种完全未定义的操作）。

解决方法是为异步操作（协同例程）提供其自身的对类实例的强引用。 根据当前编写的代码，协同例程有效保存了指向类实例的原始 *this* 指针；但这并不足以使类实例保持活动状态。

为使类实例保持活动状态，请按如下所示更改 **RetrieveValueAsync** 的实现。

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

C++/WinRT 类直接或间接派生自 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 模板。 因此， C++/WinRT 对象可以调用其 [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) 受保护成员函数来检索对其 *this* 指针的强引用。 请注意，在上述代码示例中无需实际使用 `strong_this` 变量；只需调用 **get_strong** 即可递增 C++/WinRT 对象的引用计数，并使其隐式 *this* 指针保持有效。

> [!IMPORTANT]
> 由于 **get_strong** 是 **winrt::implements** 结构模板的成员函数，因此你只能从直接或间接派生自 **winrt::implements** 的类（例如某个 C++/WinRT 类）调用该函数。 有关派生自 **winrt::implements** 的详细信息和示例，请参阅[使用 C++/WinRT 创作 API](/windows/uwp/cpp-and-winrt-apis/author-apis)。

这可以解决前面在步骤 4 中遇到的问题。 即使对类实例的所有其他引用消失，协同例程也会采取预防措施来保证其依赖项的稳定。

如果强引用不合适，则你可以改为调用 [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) 来检索对 *this* 的弱引用。 只需确认在访问 *this* 之前能够检索强引用。 同样，**get_weak** 是 **winrt::implements** 结构模板的成员函数。

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto weak_this{ get_weak() }; // Maybe keep *this* alive.

    co_await 5s;

    if (auto strong_this{ weak_this.get() })
    {
        co_return m_value;
    }
    else
    {
        co_return L"";
    }
}
```

在上述示例中，当未保留任何强引用时，弱引用无法防止销毁类实例。 但是，它可以让你检查在访问成员变量之前，是否可以获取强引用。

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>使用事件处理委托安全访问 *this* 指针

### <a name="the-scenario"></a>方案

有关事件处理的一般信息，请参阅[在 C++/WinRT 中使用委托处理事件](handle-events.md)。

上一部分重点描述了在协同例程和并发方面可能存在的生存期问题。 但是，如果你使用对象的成员函数处理事件，或者从对象成员函数中的某个 lambda 函数内部处理事件，则需要考虑事件接收方（处理事件的对象）和事件源（引发事件的对象）的相对生存期。 让我们看一看几个代码示例。

以下代码列表首先定义一个简单的 **EventSource** 类，该类引发一个泛型事件，添加到该类中的任何委托将会处理该事件。 此示例事件正好使用 [**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler) 委托类型，但此处所述的问题和补救措施适用于任何委托类型。

然后，**EventRecipient** 类以 lambda 函数的形式为 **EventSource::Event** 事件提供一个处理程序。

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;

struct EventSource
{
    winrt::event<EventHandler<int>> m_event;

    void Event(EventHandler<int> const& handler)
    {
        m_event.add(handler);
    }

    void RaiseEvent()
    {
        m_event(nullptr, 0);
    }
};

struct EventRecipient : winrt::implements<EventRecipient, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    void Register(EventSource& event_source)
    {
        event_source.Event([&](auto&& ...)
        {
            std::wcout << m_value.c_str() << std::endl;
        });
    }
};

int main()
{
    winrt::init_apartment();

    EventSource event_source;
    auto event_recipient{ winrt::make_self<EventRecipient>() };
    event_recipient->Register(event_source);
    event_source.RaiseEvent();
}
```

模式是，事件接收方具有一个依赖于其 *this* 指针的 lambda 事件处理程序。 每当事件接收方的生存期超过事件源时，它的生存期也会超过这些依赖项。 在这种常见的情况下，该模式可正常运作。 这些情况中有一部分很明显，例如当 UI 页面处理由页面上的控件引发的事件时。 页面的生存期超过按钮 &mdash; 因此，处理程序的生存期也超过按钮。 每当接收方拥有源（例如作为数据成员）时，或者每当接收方和源是同级且直接由其他某个对象拥有时，就会出现这种情况。

在不确定是否会遇到处理程序的生存期不会超过它依赖的 *this* 的情况时，通常可以捕获 *this* 而不考虑强或弱生存时间。

但仍有一些情况，*this* 的生存期不及它在处理程序（包括用于由异步操作和运算引发的完成和进度事件的处理程序）中的使用时间。必须了解如何处理这种情况。

- 当事件源以同步方式引发其事件时，你就可以放心地撤销处理程序：不会收到更多事件了。  但对于异步事件，即使在撤销（尤其是在析构函数中撤销）后，你的对象在开始析构后仍可能收到正在进行的事件。 在析构之前找到取消订阅的地方也许可以缓解此问题，但若要查找稳妥的解决方案，请继续阅读。
- 如果你要创作用于实现异步方法的协同例程，那么这是可能的。
- 在存在特定 XAML UI 框架对象（例如，[**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel)）的极少数情况下，如果接收方在完成时没有从事件源取消注册，那么这是可能的。

### <a name="the-issue"></a>问题

以下 **main** 函数版本模拟在事件源仍引发事件的情况下，当事件接收方销毁时会发生什么情况（也许会超出范围）。

```cppwinrt
int main()
{
    winrt::init_apartment();

    EventSource event_source;
    auto event_recipient{ winrt::make_self<EventRecipient>() };
    event_recipient->Register(event_source);
    event_recipient = nullptr; // Simulate the event recipient going out of scope.
    event_source.RaiseEvent(); // Behavior is now undefined within the lambda event handler; crashing is likely.
}
```

将会销毁事件接收方，但其中的 lambda 事件处理程序仍会订阅 **Event** 事件。 引发该事件时，lambda 会尝试取消引用 *this* 指针，而此时该指针是无效的。 因此，尝试使用该指针的处理程序中的代码（或协同例程的继续操作）会产生访问冲突结果。

> [!IMPORTANT]
> 如果遇到类似的情况，则需要考虑 *this* 对象的生存期；以及已捕获的 *this* 对象的生存期是否超过捕获时间。 如果未超过，请使用强引用或弱引用捕获它，下面将演示此操作。
>
> 或者，如果它对你的场景有意义，而线程处理注意事项进一步增加实现的可能，则你还可以选择在接收方完成该事件后撤销处理程序，或者在接收方的销毁器中撤销处理程序。 请参阅[撤销已注册的委托](handle-events.md#revoke-a-registered-delegate)。

这就是我们注册处理程序的方式。

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

lambda 自动按引用捕获局部变量。 因此，在此示例中，我们可以相应地编写此代码。

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

在这两种情况下，我们都只需捕获原始的 *this* 指针。 这不会对引用计数造成影响，因此，没有任何因素会阻止销毁当前对象。

### <a name="the-solution"></a>解决方案

解决方法是捕获强引用（或者根据情况捕获弱引用，这一点我们会在后面介绍）。 强引用确实会递增引用计数，且确实会使当前对象保持活动状态。   只需声明一个捕获变量（在此示例中名为 `strong_this`），并通过调用 [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) 将其初始化，以检索对 *this* 指针的强引用。

> [!IMPORTANT]
> 由于 **get_strong** 是 **winrt::implements** 结构模板的成员函数，因此你只能从直接或间接派生自 **winrt::implements** 的类（例如某个 C++/WinRT 类）调用该函数。 有关派生自 **winrt::implements** 的详细信息和示例，请参阅[使用 C++/WinRT 创作 API](/windows/uwp/cpp-and-winrt-apis/author-apis)。

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

甚至可以省略当前对象的自动捕获，并通过捕获变量而不是隐式的 *this* 访问数据成员。

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

如果强引用不合适，则你可以改为调用 [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) 来检索对 *this* 的弱引用。 弱引用不会使当前对象保持活动状态。  因此，只需确认在访问成员之前仍可从弱引用检索强引用即可。

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

如果捕获了原始指针，则需确保让指向的对象保持活动状态。

### <a name="if-you-use-a-member-function-as-a-delegate"></a>如果使用成员函数作为委托

与使用 lambda 函数时一样，这些原则同样适用于使用成员函数作为委托的情况。 语法有所不同，让我们看一些代码。 首先，下面是可能不安全的成员函数事件处理程序，其中使用了原始 *this* 指针。

```cppwinrt
struct EventRecipient : winrt::implements<EventRecipient, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    void Register(EventSource& event_source)
    {
        event_source.Event({ this, &EventRecipient::OnEvent });
    }

    void OnEvent(IInspectable const& /* sender */, int /* args */)
    {
        std::wcout << m_value.c_str() << std::endl;
    }
};
```

这是引用对象及其成员函数的一种标准常规方式。 为确保此处理程序安全，可以在注册该处理程序的位置建立一个强引用或弱引用 &mdash; 从 Windows SDK 版本 10.0.17763.0（Windows 10 版本 1809）开始。 此时，已知事件接收方对象仍保持活动状态。

对于强引用，只需调用 [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) 来取代原始 *this* 指针。 C++/WinRT 确保生成的委托保留对当前对象的强引用。

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

捕获强引用意味着，只有在处理程序已取消注册且所有当前的回调均已返回之后，才能销毁你的对象。 不过，该保证仅适用于引发事件的时候。 如果事件处理程序是异步的，则需在第一个挂起点之前为协同程序提供一个对类实例的强引用（如需详细信息和代码，请参阅本主题前面的[在类成员协同程序中安全访问 *this* 指针](#safely-accessing-the-this-pointer-in-a-class-member-coroutine)部分）。 但这样会在事件源和你的对象之间创建一个循环引用，因此需通过撤销事件来显式中断该循环。

对于弱引用，请调用 [**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)。 C++/WinRT 确保生成的委托保留弱引用。 最后，该委托会在幕后尝试将弱引用解析为强引用，如果成功，它只会调用成员函数。

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

如果委托  调用了成员函数，则 C++/WinRT 会使对象保持活动状态，直至处理程序返回。 但是，如果处理程序是异步的，并且在挂起点返回，则需在第一个挂起点之前为协同程序提供一个对类实例的强引用。 同样，如需详细信息，请参阅本主题前面的[在类成员协同程序中安全访问 *this* 指针](#safely-accessing-the-this-pointer-in-a-class-member-coroutine)部分。

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>使用 **SwapChainPanel::CompositionScaleChanged** 的弱引用示例

在此代码示例中，我们将根据另一段弱引用演示使用 [**SwapChainPanel::CompositionScaleChanged**](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) 事件。 代码将注册一个事件处理程序，该处理程序使用捕获对接收方的弱引用的 lambda。

```cppwinrt
winrt::Windows::UI::Xaml::Controls::SwapChainPanel m_swapChainPanel;
winrt::event_token m_compositionScaleChangedEventToken;

void RegisterEventHandler()
{
    m_compositionScaleChangedEventToken = m_swapChainPanel.CompositionScaleChanged([weak_this{ get_weak() }]
        (Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
        Windows::Foundation::IInspectable const& object)
    {
        if (auto strong_this{ weak_this.get() })
        {
            strong_this->OnCompositionScaleChanged(sender, object);
        }
    });
}

void OnCompositionScaleChanged(Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
    Windows::Foundation::IInspectable const& object)
{
    // Here, we know that the "this" object is valid.
}
```

在 lamba 捕获子句中，将创建一个临时变量，用来表示对 *this* 的弱引用。 在 lambda 的正文中，如果可以获取对 *this* 的强引用，则会调用 **OnCompositionScaleChanged** 函数。 这样，在 **OnCompositionScaleChanged** 中便可以安全地使用 *this*。

## <a name="weak-references-in-cwinrt"></a>C++/WinRT 中的弱引用

在上面，我们看到使用了弱引用。 一般情况下，弱引用非常适合用于中断循环引用。 例如，对于基于 XAML 的 UI 框架的本机实现 &mdash; 由于框架的历史设计原因 &mdash; 需要使用 C++/WinRT 中的弱引用来处理循环引用。 不过，在 XAML 的外部，可能不需要使用弱引用（因为弱引用没有固有的 XAML 相关信息）。 你通常应该能够设计自己的 C++/WinRT API，以避免需要进行循环引用和弱引用。 

对于声明的任何给定类型，在 C++/WinRT 中，是否或者何时需要弱引用并不是显而易见的。 因此，C++/WinRT 在结构模板 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)（你自己的 C++/WinRT 类型将从中直接或间接派生）上自动提供弱引用支持。 它是付费使用的，除非针对 [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) 实际查询对象，否则不会收取任何费用。 你可以明确选择[退出该支持](#opting-out-of-weak-reference-support)。

### <a name="code-examples"></a>代码示例
[**Winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref) 结构模板是一个用于获取对类实例的弱引用的一个选项。

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```

或者，可以使用 [**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak) 帮助器函数。

```cppwinrt
Class c;
auto weak = winrt::make_weak(c);
```

创建弱引用不会影响对对象自身的引用计数；这只会导致分配一个控制块。 该控制块负责实现弱引用语义。 然后你可以尝试将弱引用提升为强引用，并在成功之后使用它。

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

如果某些其他强引用仍然存在，[**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weak_refget-function) 调用将会增加引用计数并向调用方返回强引用。

### <a name="opting-out-of-weak-reference-support"></a>退出弱引用支持
弱引用支持是自动启用的。 但是，你可以通过将 [**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref) 标记结构作为模板参数传递给基类，明确退出该项支持。

如果直接从 **winrt::implements** 派生。

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, no_weak_ref>
{
    ...
}
```

如果正在创作运行时类。

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, no_weak_ref>
{
    ...
}
```

标记结构在 variadic 参数包中的位置无关紧要。 如果你请求对某个已退出类型的弱引用，则编译器将帮助你退出并显示“此项仅用于弱引用支持”  。

## <a name="important-apis"></a>重要的 API
* [implements::get_weak 函数](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [winrt::make_weak 函数模板](/uwp/cpp-ref-for-winrt/make-weak)
* [winrt::no_weak_ref 标记结构](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [winrt::weak_ref 结构模板](/uwp/cpp-ref-for-winrt/weak-ref)

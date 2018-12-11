---
description: Windows 运行时是一种引用计数系统;请务必要了解有关的、 重要性和区别，系统中，并强和弱引用。
title: C++/WinRT 中的弱引用
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10，uwp，标准，c + +，cpp，winrt，投影，强大、 弱引用
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 507b3cee71819df1d0163380a494e6a15936109f
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8889497"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>强和弱引用在 C + + WinRT

Windows 运行时是一种引用计数系统;和在此类系统中十分重要可以了解有关的、 重要性和之间，区别强和弱引用 （以及既不，如隐式*此*指针的引用）。 你将看到在本主题中，了解如何管理这些引用正确可能意味着可靠的系统运行顺畅和居中崩溃的一个之间的区别。 通过提供语言投影中, 具有深入支持的帮助程序函数[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)满足中间中生成更复杂的系统，只需并正常工作。

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>安全地访问类成员协同程序中的*此*指针

下面的代码列表显示协同程序是一个类的成员函数的典型示例。

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

**MyClass::RetrieveValueAsync**工作的一段时间，并且然后最终它将返回一份`MyClass::m_value`数据成员。 调用**RetrieveValueAsync**导致异步对象将创建，且该对象具有隐式*此*指针 (通过其最终，`m_value`访问)。

下面是完整的事件序列。

1. 在**主**，创建的**MyClass**实例 (`myclass_instance`)。
2. `async`创建对象时，指向 （通过其*此*） `myclass_instance`。
3. **Winrt::Windows::Foundation::IAsyncAction::get**函数阻止几秒钟，然后返回**RetrieveValueAsync**的结果。
4. **RetrieveValueAsync**返回的值`this->m_value`。

步骤 4 是安全的只要*这*是有效。

但是，在异步操作完成之前类实例将被破坏怎么办？ 有所有类型的类实例可以离开作用域之前完成异步方法的方法。 但是，我们可以模拟它通过类实例设置为`nullptr`。

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

我们销毁类实例，其中点之后, 它看起来像我们不直接将其引用再次。 但当然异步对象具有*此*指针，并尝试使用它们来复制存储在类实例内的值。 协同程序是成员函数，并且它预期要能够使用 impunity 其*此*指针。

通过这项更改到代码，我们会遇到问题在步骤 4，因为此类实例已被删除，并且*此*不再有效。 只要异步对象会尝试访问内部类实例的变量，它将崩溃 （或执行完全未定义）。

解决方案是为了提供异步操作&mdash;协同程序&mdash;对类实例自己强引用。 作为当前编写的代码，该协同程序有效地保留原始*此*指针的类实例中;但是，不足以使类实例保持活动状态。

若要使类实例保持活动状态，如下所示的更改**RetrieveValueAsync**的实现。

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

因为 C + + /winrt 对象直接或间接派生[**winrt:: implements**](/uwp/cpp-ref-for-winrt/implements)模板的 C + + /winrt 对象可以调用其[**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function)受保护成员函数以检索其*此*指针的强引用。 请注意，无需实际上使用`strong_this`变量;只需调用**get_strong**增加你引用计数，并使你隐式*此*指针保持有效。

这可以解决该问题，我们之前已时我们步骤 4。 即使对类实例的所有其他引用消失，该协同程序已采取保证其依赖项稳定的预防措施。

如果不适当的强引用，然后可以改为调用[**implements:: get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)检索对*此*的弱引用。 只需确认你可以访问*此*前检索的强引用。

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

在上面的示例中，将弱引用不会保留从被销毁时将不保留任何强引用的类实例。 但是，它会为你提供一种方法检查是否可以访问成员变量之前获取的强引用。

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>安全地访问*此*指针事件处理委托

### <a name="the-scenario"></a>方案

有关事件处理的常规信息，请参阅[C + 使用委托处理事件 + WinRT](handle-events.md)。

在上一节突出显示的协同程序和并发区域中的潜在生命周期问题。 但是，如果你处理事件的对象的成员函数，或从需要考虑事件接收者 （处理事件的对象） 和事件源 （的对象的相对生存期对象的成员函数，那么你的 lambda 函数引发事件）。 让我们看一下一些代码示例。

下面的代码列表首先定义一个简单的**EventSource**类，这会引发任何已添加到它的委托由进行处理的一般事件。 此示例将会发生事件使用[**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler)委托类型，但问题和补救措施此处适用于所有的委托类型。

然后， **EventRecipient**类提供一个处理程序形式的 lambda 函数的**EventSource::Event**事件。

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

模式是事件接收者在其*此*指针上具有依赖关系的 lambda 事件处理程序。 每当事件接收者的生存时间超过事件源，其生存时间超过这些依赖项。 和在这些情况下，通用的该模式运行良好。 这些情况中有一部分很明显，例如当 UI 页面处理由页面上的控件引发的事件时。 在页面的生存时间超过该按钮&mdash;因此，在处理程序还生存时间超过该按钮。 每当接收者拥有源（例如作为数据成员）时，或者每当接收者和源是同级且直接由其他某个对象拥有时，就会出现这种情况。 如果你不确定是否遇到处理程序的生存时间不会超过它依赖的 *this* 的情况，则通常可以捕获 *this* 而不考虑强或弱生存时间。

但仍有*此*不生存时间超过在 （包括用于完成和进度由异步操作和运算引发的事件处理程序），一个处理程序中使用它并且务必知道如何处理它们的情况。

- 如果你要创作用于实现异步方法的协同程序，那么这是可能的。
- 在存在特定 XAML UI 框架对象（例如，[**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel)）的极少数情况下，如果接收者在完成时没有从事件源取消注册，那么这是可能的。

### <a name="the-issue"></a>该问题

此下一版本的**main**函数模拟销毁事件接收者时，会发生什么情况 （可能是超出范围） 时事件源仍引发事件。

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

事件接收者将被销毁，但 lambda 事件处理程序内它仍订阅**事件**。 引发该事件时，会尝试取消引用*此*指针，此时无效的 lambda。 因此，从代码中处理程序 （或在协同程序的延续中） 产生访问冲突尝试使用它。

> [!IMPORTANT]
> 如果你遇到这种情况，然后你将需要考虑*此*对象; 的生命周期并且，指示捕获*此*对象的生存时间超过捕获。 如果没有，则使用捕获它的强引用或弱引用，如我们将如下所示。
>
> 或者&mdash;如果这对你的方案，意义和线程处理注意事项使它甚至可能&mdash;然后另一个选项是后在接收者完成该事件，或者在接收者的析构函数中撤销处理程序。 请参阅[撤销已注册的委托](handle-events.md#revoke-a-registered-delegate)。

这是我们如何注册处理程序。

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Lambda 自动捕获引用的任何本地变量。 因此，对于此示例中，我们可以编写等效代码这。

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

在这两种情况下，我们只需正在捕获的原始*此*指针。 且不会影响引用计数，因此没有阻止当前对象被销毁。

### <a name="the-solution"></a>解决方案

解决方案是捕获的强引用。 *执行*强引用增加引用计数，并且它*执行*保持当前处于活动状态。 你只需声明捕获变量 (称为`strong_this`在此示例中)，并将其初始化通过调用[**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function)，检索我们*此*指针的强引用。

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

你甚至可以省略自动捕获的当前的对象，并通过捕获变量而不是隐式*此*通过访问的数据成员。

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

如果不适当的强引用，然后可以改为调用[**implements:: get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)检索对*此*的弱引用。 只需确认，你仍然可以检索的强引用从其访问成员前。

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

### <a name="if-you-use-a-member-function-as-a-delegate"></a>如果你使用的成员函数用作代理

以及 lambda 函数，这些原则也适用于使用委托作为一个成员函数。 语法是不同，因此我们来看一些代码。 首先，下面是可能不安全的成员函数的事件处理程序，使用原始*此*指针。

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

这是指对象和其成员函数的标准的传统方法。 若要使其成为安全，你可以&mdash;从 Windows SDK 版本 10.0.17763.0 (Windows 10 版本 1809年) 起&mdash;建立强引用或弱引用在其中注册处理程序的点。 此时，事件收件人对象是已知的仍处于活动状态。

对于强引用，只需调用[**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function)原始*此*指针代替。 C + + WinRT 可确保结果委托保留对当前对象的强引用。

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

对于弱引用，调用[**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)。 C + + WinRT 可确保结果委托保留的弱引用。 最后，在幕后委托会尝试解析到一个强大的弱引用并且仅调用成员函数，它是否成功。

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>使用**SwapChainPanel::CompositionScaleChanged**的弱引用示例

此代码示例中，我们将使用通过的弱引用的另一个图示[**SwapChainPanel::CompositionScaleChanged**](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged)事件。 代码将注册事件处理程序使用捕获对接收者的弱引用的 lambda。

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

在 lamba 捕获子句中，将创建一个临时变量，用来表示对 *this* 的弱引用。 在 lambda 的正文中，如果可以获取对 *this* 的强引用，则会调用 **OnCompositionScaleChanged** 函数。 这样，在 **OnCompositionScaleChanged** 内便可以安全地使用 *this*。

## <a name="weak-references-in-cwinrt"></a>C++/WinRT 中的弱引用

上方，我们可以看到正在使用的弱引用。 一般情况下，它们适合破坏循环引用。 例如，对于基于 XAML 的 UI 框架的本机实现&mdash;由于框架的历史设计&mdash;的弱引用在 C + + WinRT 是来处理循环引用。 在 XAML 之外，不过，你可能不需要使用弱引用 (没有任何内容不本质上是特定于 XAML 的有关它们)。 相反，通常会，应该能够设计你自己的 C + + 的方式避免需要进行循环引用和弱引用的 WinRT Api。 

对于你声明的任何给定的类型，在 C++/WinRT 中，是否或者何时需要弱引用并不是显而易见的。 因此，C++/WinRT 在结构模板 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)（你自己的 C++/WinRT 类型将从中直接或间接派生）上自动提供弱引用支持。 它是付费使用的，除非针对 [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) 实际查询对象，否则不会向你收取任何费用。 并且，你可以选择明确地[选择退出该支持](#opting-out-of-weak-reference-support)。

### <a name="code-examples"></a>代码示例
[**Winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref) 结构模板是一个用于获取对类实例的弱引用的一个选项。

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```

或者，你可以使用 [**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak) 帮助程序函数。

```cppwinrt
Class c;
auto weak = winrt::make_weak(c);
```

创建弱引用不会影响对对象自身的引用计数；这只会导致分配一个控制块。 该控制块将负责实现弱引用语义。 你然后可以尝试将弱引用提升为强引用，并在成功之后使用它。

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

如果某些其他强引用仍然存在，[**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weakrefget-function) 调用将会增加引用计数并向调用方返回强引用。

### <a name="opting-out-of-weak-reference-support"></a>选择退出弱引用支持
将自动提供弱引用支持。 但你可以通过以下方法选择明确地选择退出该支持：将 [**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref) 标记结构作为模板参数传递给基类。

如果你直接从 **winrt::implements** 派生。

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, no_weak_ref>
{
    ...
}
```

如果你正在创作运行时类。

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, no_weak_ref>
{
    ...
}
```

标记结构在 variadic 参数包中的位置无关紧要。 如果你请求对某个已选择退出类型的弱引用，那么编译器将帮助你退出并显示“*此项仅用于弱引用支持*”。

## <a name="important-apis"></a>重要的 API
* [implements::get_weak 函数](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [winrt::make_weak 函数模板](/uwp/cpp-ref-for-winrt/make-weak)
* [winrt::no_weak_ref 标记结构](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [winrt::weak_ref 结构模板](/uwp/cpp-ref-for-winrt/weak-ref)

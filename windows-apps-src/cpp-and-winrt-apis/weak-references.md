---
description: Windows 运行时是引用在其中占有重要地位的一个系统；在这样的系统中，了解强引用与弱引用的意义和区别非常重要。
title: C++/WinRT 中的弱引用
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10、 uwp、 标准、 c + +、 cpp、 winrt、 投影、 强、 弱引用
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0e2e40daaf777e36094b698d058f21840b1804c8
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291825"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>在强和弱引用C++/WinRT

Windows 运行时是引用计数系统;在此类系统是您必须了解的有关重要性，并区分，强和弱引用 (和既不等的隐式引用*这*指针)。 正如您将看到在本主题中，了解如何正确地管理这些引用可能意味着崩溃不可预见的一个非常顺利，运行的可靠系统之间的区别。 通过提供具有深入支持语言投影中的帮助器函数[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)能满足您的一半中构建更复杂的系统，只需和正确的工作。

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>安全地访问*这*类成员协同程序中的指针

以下代码清单显示是类的成员函数的协同程序的典型示例。

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

**MyClass::RetrieveValueAsync**花费一些时间解决，并最终返回一份`MyClass::m_value`数据成员。 调用**RetrieveValueAsync**会导致要创建异步对象，该对象具有隐式*这*指针 (通过其最终，`m_value`访问)。

下面是完整的一系列事件。

1. 在中**主要**，实例**MyClass**创建 (`myclass_instance`)。
2. `async`创建对象，指向 (通过其*这*) 到`myclass_instance`。
3. **Winrt::Windows::Foundation::IAsyncAction::get**函数阻止几秒钟，然后返回的结果**RetrieveValueAsync**。
4. **RetrieveValueAsync**返回的值`this->m_value`。

步骤 4，则可以安全，只要*这*有效。

但是，如果在异步操作完成之前销毁类实例呢？ 有各种类实例可能会超出范围之前已完成的异步方法的方式。 但我们可以通过类实例设置为模拟它`nullptr`。

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

在其中我们销毁类实例的点之后, 它看起来我们不直接引用它再次。 但当然异步对象*这*指向它，并尝试使用的复制的类实例中存储的值。 协同例程是成员函数，和它期望能够使用其*这*impunity 的指针。

与对代码进行此更改，我们遇到一个问题在步骤 4 中，因为已销毁类实例，并*这*不再有效。 只要异步对象尝试访问类实例内的变量，它将崩溃 （或未完全定义）。

解决方案是为提供异步操作&mdash;协同例程&mdash;自己对类实例的强引用。 按照当前的编写，在协同程序有效保留原始*这*指向类实例; 但这并不足够，以使类实例保持活动状态。

若要使类实例保持活动状态，更改的实现**RetrieveValueAsync**到，如下所示。

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

因为C++/WinRT 对象直接或间接派生自[ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements)模板， C++/WinRT 对象可以调用其[ **implements.get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)受保护成员函数来检索到的强引用其*这*指针。 请注意，无需实际使用`strong_this`变量; 只需调用**get_strong**递增你引用计数，并保留在隐式*这*指针有效。

可以解决此问题，我们以前必须时我们一到步骤 4。 即使所有其他引用类的实例会消失，协同例程花费了保证其依赖项稳定的预防措施。

如果强引用并不合适，则可以改为调用[ **implements::get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)来检索到的弱引用*这*。 只需确认可以访问之前检索的强引用*这*。

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

在上述示例中，弱引用不会保留，当没有强引用保留时销毁类实例。 但它提供了一种方法检查是否可以访问的成员变量之前获取的强引用。

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>安全地访问*这*与事件处理委托的指针

### <a name="the-scenario"></a>该方案

有关事件处理的常规信息，请参阅[通过使用中的委托处理事件C++/WinRT](handle-events.md)。

在上一部分突出显示在协同例程和并发方面的潜在生存期问题。 但是，如果处理的事件与对象的成员函数，或在需要相对事件接收方 （处理事件的对象） 和事件源 （的对象的生存期纳入考虑内对象的成员函数，则你的 lambda 函数引发事件）。 让我们看一些代码示例。

以下第一个代码清单定义了一个简单**EventSource**类，该类会引发一般事件处理的任何已添加到它的委托。 使用此示例事件恰好[ **Windows::Foundation::EventHandler** ](/uwp/api/windows.foundation.eventhandler)委托类型，但问题和此处的补救措施应用于所有委托类型。

然后，将**EventRecipient**类提供的处理程序**EventSource::Event** lambda 函数的形式中的事件。

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

事件接收方具有依赖项的 lambda 事件处理程序的模式是其*这*指针。 每当事件收件人长于事件源，它将长于这些依赖项。 并且在这些情况下，很常见，该模式也适用。 这些情况中有一部分很明显，例如当 UI 页面处理由页面上的控件引发的事件时。 页面长于按钮&mdash;因此，该处理程序还长于按钮。 每当接收者拥有源（例如作为数据成员）时，或者每当接收者和源是同级且直接由其他某个对象拥有时，就会出现这种情况。 如果你不确定是否遇到处理程序的生存时间不会超过它依赖的 *this* 的情况，则通常可以捕获 *this* 而不考虑强或弱生存时间。

但仍有些情况下，*这*不的生存期长于在处理程序 （包括引发的异步操作和操作的完成和进度事件的处理程序），其使用，必须知道如何处理它们。

- 如果你要创作用于实现异步方法的协同程序，那么这是可能的。
- 在存在特定 XAML UI 框架对象（例如，[**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel)）的极少数情况下，如果接收者在完成时没有从事件源取消注册，那么这是可能的。

### <a name="the-issue"></a>此问题

此下一版本的**主要**函数模拟销毁事件接收方时，会发生什么情况 （也许是离开作用域） 时的事件源仍引发事件。

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

事件接收方已被破坏，但 lambda 事件处理程序中它仍订阅**事件**事件。 当引发该事件时，尝试取消引用 lambda*这*指针，它是在该点无效。 因此，访问冲突而得出的处理程序中 （或在协同例程的延续） 代码尝试使用它。

> [!IMPORTANT]
> 如果遇到这种情况，则你将需要考虑的生存期*这*对象; 以及是否捕获*这*对象长于捕获。 如果没有，然后捕获它与一个强或弱引用，正如我们将要演示如下。
>
> 或者&mdash;适合你的方案，如果和线程处理注意事项使得即使&mdash;然后另一个选项是撤消该处理程序后接收方与事件，或接收方的析构函数中完成。 请参阅[撤消注册的委托](handle-events.md#revoke-a-registered-delegate)。

这是我们如何注册处理程序。

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Lambda 自动按引用捕获本地变量。 因此，对于此示例中，我们可以具有相对编写如下。

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

在这两种情况下，我们只捕获原始*这*指针。 并因此执行任何操作正在阻止正在销毁当前对象具有对引用计数，没有影响。

### <a name="the-solution"></a>该解决方案

解决方案是捕获的强引用。 强引用*does*递增引用计数，和它*does*使当前对象保持活动状态。 您只需声明捕获变量 (称为`strong_this`在此示例中)，并将其初始化通过调用[ **implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)，检索到的强引用我们*这*指针。

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

您甚至可以省略自动捕获当前的对象，并通过而不是通过隐式捕获变量访问的数据成员*这*。

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

如果强引用并不合适，则可以改为调用[ **implements::get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)来检索到的弱引用*这*。 只需确认，您仍然可以检索的强引用从它访问成员之前。

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

### <a name="if-you-use-a-member-function-as-a-delegate"></a>如果使用成员函数作为委托

以及 lambda 函数，这些原则也适用于使用您的代理作为一个成员函数。 语法不同，因此，让我们看一些代码。 首先，下面是可能不安全的成员函数的事件处理程序，使用原始*这*指针。

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

这是标准的常规方法，来指代对象并将其成员函数。 若要使此安全，您可以&mdash;截至 10.0.17763.0 (Windows 10，版本 1809年) 版本的 Windows SDK&mdash;建立具有强名称或处理程序注册所在的点处的弱引用。 此时，事件收件人对象称为仍保持活动状态。

强引用，只需调用[ **get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)代替原始*这*指针。 C++/ WinRT 可确保结果委托保存对当前对象的强引用。

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

有关弱引用，调用[ **get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)。 C++/ WinRT 可确保结果委托保留的弱引用。 在最后一分钟在后台，委托会尝试解析为强的弱引用和仅调用成员函数，如果成功。

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>弱引用的示例使用**SwapChainPanel::CompositionScaleChanged**

在此代码示例中，我们将使用[ **SwapChainPanel::CompositionScaleChanged** ](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged)通过弱引用的另一个图的事件。 代码注册事件处理程序使用 lambda 捕获对接收方的弱引用。

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

更高版本，我们可以看到正在使用的弱引用。 一般情况下，它们适用于中断循环引用。 例如，对于基于 XAML 的 UI 框架的本机实现&mdash;由于历史的框架设计&mdash;弱引用机制中的C++/WinRT 是处理循环引用的需要。 外部 XAML，不过，您可能不需要使用弱引用 (不，没有任何内容本质上是 XAML 特定信息)。 而是，在大多数情况，应该能够设计自己C++中的方式避免循环引用和弱引用的 WinRT Api。 

对于你声明的任何给定的类型，在 C++/WinRT 中，是否或者何时需要弱引用并不是显而易见的。 因此，C++/WinRT 在结构模板 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)（你自己的 C++/WinRT 类型将从中直接或间接派生）上自动提供弱引用支持。 它是付费使用的，除非针对 [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) 实际查询对象，否则不会向你收取任何费用。 并且，你可以选择明确地[选择退出该支持](#opting-out-of-weak-reference-support)。

### <a name="code-examples"></a>代码示例
[  **Winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref) 结构模板是一个用于获取对类实例的弱引用的一个选项。

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

如果某些其他强引用仍然存在，[**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weak_refget-function) 调用将会增加引用计数并向调用方返回强引用。

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
* [implements::get_weak 函数](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [winrt::make_weak 函数模板](/uwp/cpp-ref-for-winrt/make-weak)
* [winrt::no_weak_ref marker struct](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [winrt::weak_ref 结构模板](/uwp/cpp-ref-for-winrt/weak-ref)

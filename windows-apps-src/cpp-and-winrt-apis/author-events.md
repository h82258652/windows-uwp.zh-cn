---
description: 本主题演示了如何创作包含引发事件的运行时类的 Windows 运行时组件。 它还演示了使用该组件并处理事件的应用。
title: 在 C++/WinRT 中创作事件
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 创作, 事件
ms.localizationpriority: medium
ms.openlocfilehash: 980f39f20de369bce226c4d8c1070bda851480c2
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493652"
---
# <a name="author-events-in-cwinrt"></a>在 C++/WinRT 中创作事件

本主题基于 Windows 运行时组件和使用方应用程序（[使用 C++/WinRT 创建 Windows 运行时组件](/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt)主题演示如何生成）。

以下是此主题添加的新功能。
- 更新银行帐户运行时类以在其余额进入借方时引发事件。
- 更新使用银行帐户运行时类的核心应用，使其处理该事件。

> [!NOTE]
> 有关安装和使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Visual Studio 扩展 (VSIX) 和 NuGet 包（两者共同提供项目模板，并生成支持）的信息，请参阅[适用于 C++/WinRT 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

> [!IMPORTANT]
> 有关支持你了解如何利用 C++/WinRT 来使用和创作运行时类的基本概述和术语，请参阅[通过 C++/WinRT 使用 API](consume-apis.md) 和[通过 C++/WinRT 创作 API](author-apis.md)。

## <a name="create-bankaccountwrc-and-bankaccountcoreapp"></a>创建 BankAccountWRC 和 BankAccountCoreApp

如果要按照本主题所示的更新执行操作，以便可以生成和运行代码，则第一步是遵循[使用 C++/WinRT 创建 Windows 运行时组件](/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt)主题中演练的指示。 这样一来，你将拥有 BankAccountWRC Windows 运行时组件和使用它的 BankAccountCoreApp 核心应用。

## <a name="update-bankaccountwrc-to-raise-an-event"></a>更新 BankAccountWRC 以引发事件

更新 `BankAccount.idl`，使其看起来如下所示。 此示例说明了如何使用单精度浮点数的参数声明委托类型为 [EventHandler](/uwp/api/windows.foundation.eventhandler-1) 的事件。

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    runtimeclass BankAccount
    {
        BankAccount();
        void AdjustBalance(Single value);
        event Windows.Foundation.EventHandler<Single> AccountIsInDebit;
    };
}
```

保存该文件。 项目不会在当前状态下完成生成，但在所有情况下立即执行生成可生成 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` 和 `BankAccount.cpp` 存根文件的更新版本。 现在，可在这些文件中看到 AccountIsInDebit 事件的存根实现。 在 C++/WinRT 中，IDL 声明事件作为一组重载函数实现（类似于属性作为重载 get 和 set 函数实现的方式）。 一个重载需要注册一个委托，并返回令牌 ([winrt::event_token](/uwp/cpp-ref-for-winrt/event-token))。 另一个重载则需要令牌，并撤销关联代理的注册。

现在，打开 `BankAccount.h` 和 `BankAccount.cpp`，并更新 BankAccount 运行时类的实现。 在 `BankAccount.h` 中，添加两个重载的 AccountIsInDebit 函数，以及用于实现这些函数的专用事件数据成员。

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...
        winrt::event_token AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler);
        void AccountIsInDebit(winrt::event_token const& token) noexcept;

    private:
        winrt::event<Windows::Foundation::EventHandler<float>> m_accountIsInDebitEvent;
        ...
    };
}
...
```

如上所示，事件由 [winrt::event](/uwp/cpp-ref-for-winrt/event) 结构模板表示，该结构模板由特定委托类型（其本身可以由 args 类型进行参数化）进行参数化。

在 `BankAccount.cpp` 中，实现两个重载的 AccountIsInDebit 函数。

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(winrt::event_token const& token) noexcept
    {
        m_accountIsInDebitEvent.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
        if (m_balance < 0.f) m_accountIsInDebitEvent(*this, m_balance);
    }
}
```

> [!NOTE]
> 若要详细了解事件自动撤销程序是什么，请参阅[撤销已注册的委托](handle-events.md#revoke-a-registered-delegate)。 可以免费获得适用于你的事件的事件自动撤销程序实现。 换而言之，你无需为事件撤销程序实现重载&mdash;此功能已由 C++/WinRT 投影提供。

其他重载（注册和手动撤销重载）不会合并到投影中。 这是为了让你能够灵活地以最佳方式针对自己的方案来实现它们。 像这些实现中所示那样调用 [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) 和 [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) 是高效且并发/线程安全的默认设置。 但是，如果你有大量事件，那么你可能不希望每个事件都有一个事件字段，而是改为选择某些类型的稀疏实现。

你还可以从上述实现中发现，AdjustBalance 函数的实现已更新为在余额变为负数时引发 AccountIsInDebit 事件 。

## <a name="update-bankaccountcoreapp-to-handle-the-event"></a>更新 BankAccountCoreApp 以处理事件

在 BankAccountCoreApp 项目的 `App.cpp` 中，对代码进行以下更改以注册事件处理程序，然后使该帐户进入借方。

`WINRT_ASSERT` 是宏定义，并且扩展到 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)。

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_bankAccount.AccountIsInDebit([](const auto &, float balance)
        {
            WINRT_ASSERT(balance < 0.f); // Put a breakpoint here.
        });
    }
    ...

    void Uninitialize()
    {
        m_bankAccount.AccountIsInDebit(m_eventToken);
    }
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

请注意对 OnPointerPressed 方法的更改。 现在，每次单击该窗口，就会从银行帐户的余额中减去 1。 而且应用现在处理余额变为负数时引发的事件。 要演示按预期引发事件，请在用于处理 AccountIsInDebit 事件的 lambda 表达式内部放置一个断点，运行该应用，然后单击此窗口内部。

## <a name="parameterized-delegates-across-an-abi"></a>跨 ABI 的参数化委托

如果事件必须跨应用程序二进制接口 (ABI) 可访问（例如在组件及其所用应用程序之间），那么该事件必须使用 Windows 运行时委托类型。 上述示例使用 [Windows::Foundation::EventHandler\<T\>](/uwp/api/windows.foundation.eventhandler) Windows 运行时委托类型。 [TypedEventHandler\<TSender, TResult\>](/uwp/api/windows.foundation.eventhandler) 是另一种 Windows 运行时委托类型。

这两种委托类型的类型参数必须跨 ABI，因此类型参数也必须是 Windows 运行时类型。 这包括 Windows 运行时类、第三方运行时类，以及数字和字符串等基本类型。 如果你忘记了此约束，编译器将帮助你处理“必须为 WinRT 类型”错误。

下面是代码列表形式的示例。 从在本主题前面部分创建的 BankAccountWRC 和 BankAccountCoreApp 项目开始，然后编辑这些项目中的代码，使其看起来类似于这些列表中的代码 。

第一个列表针对 BankAccountWRC 项目。 按如下所示编辑 `BankAccountWRC.idl` 后，生成项目，然后将 `MyEventArgs.h` 和 `.cpp` 复制到项目中（从 `Generated Files` 文件夹），就像之前对 `BankAccount.h` 和 `.cpp` 一样。

```cppwinrt
// BankAccountWRC.idl
namespace BankAccountWRC
{
    [default_interface]
    runtimeclass MyEventArgs
    {
        Single Balance{ get; };
    }

    [default_interface]
    runtimeclass BankAccount
    {
        ...
        event Windows.Foundation.EventHandler<BankAccountWRC.MyEventArgs> AccountIsInDebit;
        ...
    };
}

// MyEventArgs.h
#pragma once
#include "MyEventArgs.g.h"

namespace winrt::BankAccountWRC::implementation
{
    struct MyEventArgs : MyEventArgsT<MyEventArgs>
    {
        MyEventArgs() = default;
        MyEventArgs(float balance);
        float Balance();

    private:
        float m_balance{ 0.f };
    };
}

// MyEventArgs.cpp
#include "pch.h"
#include "MyEventArgs.h"
#include "MyEventArgs.g.cpp"

namespace winrt::BankAccountWRC::implementation
{
    MyEventArgs::MyEventArgs(float balance) : m_balance(balance)
    {
    }

    float MyEventArgs::Balance()
    {
        return m_balance;
    }
}

// BankAccount.h
...
struct BankAccount : BankAccountT<BankAccount>
{
...
    winrt::event_token AccountIsInDebit(Windows::Foundation::EventHandler<BankAccountWRC::MyEventArgs> const& handler);
...
private:
    winrt::event<Windows::Foundation::EventHandler<BankAccountWRC::MyEventArgs>> m_accountIsInDebitEvent;
...
}
...

// BankAccount.cpp
#include "MyEventArgs.h"
...
winrt::event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<BankAccountWRC::MyEventArgs> const& handler) { ... }
...
void BankAccount::AdjustBalance(float value)
{
    m_balance += value;

    if (m_balance < 0.f)
    {
        auto args = winrt::make_self<winrt::BankAccountWRC::implementation::MyEventArgs>(m_balance);
        m_accountIsInDebitEvent(*this, *args);
    }
}
...
```

此列表针对 BankAccountCoreApp 项目。

```cppwinrt
// App.cpp
...
void Initialize(CoreApplicationView const&)
{
    m_eventToken = m_bankAccount.AccountIsInDebit([](const auto&, BankAccountWRC::MyEventArgs args)
    {
        float balance = args.Balance();
        WINRT_ASSERT(balance < 0.f); // Put a breakpoint here.
    });
}
...
```

## <a name="simple-signals-across-an-abi"></a>跨 ABI 的简单信号

如果无需连同事件传递任何形参或实参，则可以定义自己的简单 Windows 运行时委托类型。 以下示例展示 BankAccount 运行时类的更简易版本。 它声明名为 SignalDelegate 的委托类型，然后使用该类型来引发信号类型事件，而不是具有参数的事件。

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    delegate void SignalDelegate();

    runtimeclass BankAccount
    {
        BankAccount();
        event BankAccountWRC.SignalDelegate SignalAccountIsInDebit;
        void AdjustBalance(Single value);
    };
}
```

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

        winrt::event_token SignalAccountIsInDebit(BankAccountWRC::SignalDelegate const& handler);
        void SignalAccountIsInDebit(winrt::event_token const& token);
        void AdjustBalance(float value);

    private:
        winrt::event<BankAccountWRC::SignalDelegate> m_signal;
        float m_balance{ 0.f };
    };
}
```

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::SignalAccountIsInDebit(BankAccountWRC::SignalDelegate const& handler)
    {
        return m_signal.add(handler);
    }

    void BankAccount::SignalAccountIsInDebit(winrt::event_token const& token)
    {
        m_signal.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
        if (m_balance < 0.f)
        {
            m_signal();
        }
    }
}
```

```cppwinrt
// App.cpp
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_bankAccount.SignalAccountIsInDebit([] { /* ... */ });
    }
    ...

    void Uninitialize()
    {
        m_bankAccount.SignalAccountIsInDebit(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>项目中的参数化委托、简单信号和回调

如果所需事件是 Visual Studio 项目内部的（未跨二进制文件），而在内部这些事件不限于 Windows 运行时类型，则仍可使用 [winrt::event](/uwp/cpp-ref-for-winrt/event)\<Delegate\> 类模板。 请直接使用 [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) 而不是实际的 Windows 运行时委托类型，因为 **winrt::delegate** 也支持非 Windows 运行时参数。

以下示例先显示不采用任何参数的委托签名（本质上即简单信号），然后显示采用字符串的委托签名。

```cppwinrt
winrt::event<winrt::delegate<>> signal;
signal.add([] { std::wcout << L"Hello, "; });
signal.add([] { std::wcout << L"World!" << std::endl; });
signal();

winrt::event<winrt::delegate<std::wstring>> log;
log.add([](std::wstring const& message) { std::wcout << message.c_str() << std::endl; });
log.add([](std::wstring const& message) { Persist(message); });
log(L"Hello, World!");
```

注意如何向事件添加尽可能多的订阅委托。 但会产生一些与事件相关的开销。 如果只需仅具有一个订阅委托的简单回调，则你可以独立使用 [winrt::delegate&lt;…T&gt;](/uwp/cpp-ref-for-winrt/delegate)。

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

如果你正在从项目内部使用事件和代理的 C++/CX 基本代码移植，winrt::delegate 将帮助你复制 C++/WinRT 中的这个模式。

## <a name="design-guidelines"></a>设计指南

建议将事件而不是委托作为函数参数传递。 [winrt::event](/uwp/cpp-ref-for-winrt/event) 的 add 函数是例外，因为在这种情况下必须传递委托 。 这条指导原则的原因是，委托在各种 Windows 运行时语中可以具有各种形式（根据支持一个还是多个客户端注册）。 事件及其多个订阅者模型构成更可预测且更一致的选项。

事件处理程序委托的签名应该由以下两个形参组成：发送方 (IInspectable) 和实参（[RoutedEventArgs](/uwp/api/windows.ui.xaml.routedeventargs) 等事件实参类型）。

注意，如果设计的是内部 API，这些指导原则不一定适用。 但随时间推移，内部 API 通常会公开。

## <a name="related-topics"></a>相关主题
* [使用 C++/WinRT 创作 API](author-apis.md)
* [通过 C++/WinRT 使用 API](consume-apis.md)
* [在 C++/WinRT 中使用委托处理事件](handle-events.md)
* [使用 C++/WinRT 创建 Windows 运行时组件](/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt)

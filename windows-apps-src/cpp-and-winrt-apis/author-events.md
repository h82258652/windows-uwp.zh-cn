---
author: stevewhims
description: 本主题演示如何创作包含引发事件的运行时类的 Windows 运行时组件。 它还演示使用该组件并处理事件的应用。
title: 在 C++/WinRT 中创作事件
ms.author: stwhi
ms.date: 07/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 创作, 事件
ms.localizationpriority: medium
ms.openlocfilehash: 1b0aa1a53ce69369c442cef7f3b5912631593242
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3957909"
---
# <a name="author-events-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>在 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中创作事件

本主题演示如何创作 Windows 运行时组件，该组件包含表示银行帐户的运行时类，当该帐户的余额进入借方时其会引发事件。 它还演示使用该银行帐户运行时类、调用函数以调整余额并处理产生的任何事件的核心应用。

> [!NOTE]
> 有关 C++/WinRT Visual Studio Extension (VSIX)（提供项目模板支持以及 C++/WinRT MSBuild 属性和目标）的安装和使用的信息，请参阅[针对 C++/WinRT 以及 VSIX 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

> [!IMPORTANT]
> 有关支持你了解如何利用 C++/WinRT 来使用和创作运行时类的基本概述和术语，请参阅[通过 C++/WinRT 使用 API](consume-apis.md) 和[通过 C++/WinRT 创作 API](author-apis.md)。

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>创建 Windows 运行时组件 (BankAccountWRC)

首先在 Microsoft Visual Studio 中创建新项目。 创建**Visual c + +** > **Windows 通用** > **Windows 运行时组件 (C + + WinRT)** 项目，然后将其命名为*BankAccountWRC* （针对"银行帐户 Windows 运行时组件"）。

该新建项目包含一个名为 `Class.idl` 的文件。 重命名该文件`BankAccount.idl`(重命名`.idl`文件自动重命名依赖于`.h`和`.cpp`文件，过)。 内容替换为`BankAccount.idl`使用下面的列表。

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    runtimeclass BankAccount
    {
        BankAccount();
        event Windows.Foundation.EventHandler<Single> AccountIsInDebit;
        void AdjustBalance(Single value);
    };
}
```

保存文件。 该项目将不会生成完成时，但现在生成有用的操作来执行操作，因为它会生成源代码文件将在其中实现**BankAccount**运行时类。 因此继续操作并立即生成 (你会看到此阶段生成错误只需使用`Class.h`和`Class.g.h`找不到)。 在生成过程中，`midl.exe`工具会运行以创建组件的 Windows 运行时元数据文件 (即`\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`)。 然后，`cppwinrt.exe` 工具运行（具有 `-component` 选项）以生成源代码文件，从而为你在创作组件时提供支持。 这些文件包含让你开始实现已在 IDL 中声明的**BankAccount**运行时类的存根。 这些存根是 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` 和 `BankAccount.cpp`。

在文件资源管理器，将复制的存根文件`BankAccount.h`和`BankAccount.cpp`文件夹中`\BankAccountWRC\BankAccountWRC\Generated Files\sources\`到包含项目文件的文件夹，即`\BankAccountWRC\BankAccountWRC\`，并替换目标中的文件。 现在，让我们打开 `BankAccount.h` 和 `BankAccount.cpp` 并实现运行时类。 在 `BankAccount.h` 中，将两个私有成员添加到 BankAccount 的实现（*不是*出厂实现）。

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        winrt::event<Windows::Foundation::EventHandler<float>> m_accountIsInDebitEvent;
        float m_balance{ 0.f };
    };
}
...
```

如你所见上述，根据[**winrt::event**](/uwp/cpp-ref-for-winrt/event)结构模板中，通过特定委托类型参数化实现事件。

在 `BankAccount.cpp` 中，实现如下面的代码示例所示的函数。 在 C++/WinRT 中，IDL 声明事件作为一组重载函数实现（类似于属性作为重载 get 和 set 函数实现的方式）。 一个重载需要注册一个代理，并返回令牌。 另一个重载则需要令牌，并撤销关联代理的注册。

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(winrt::event_token const& token)
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

你无需为事件撤销程序实现重载（有关详细信息，请参阅[撤销已注册的代理](handle-events.md#revoke-a-registered-delegate)）&mdash;该程序由 C++/WinRT 投影为你处理。 其他重载不会合并到投影中，以便为你提供灵活性，让你能够以最佳方式为自己的方案实现它们。 像这样调用 [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) 和 [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) 是高效且并发/线程安全的默认设置。 但是，如果你有大量事件，那么你可能不希望每个事件都有一个事件字段，而是改为选择某些类型的稀疏实现。

你还可以从上述情况中发现，如果余额变为负，**AdjustBalance** 函数的实现将引发 **AccountIsInDebit** 事件。

如果任何警告阻止你进行生成，然后解决这些错误或将项目属性**C/c + +** > **常规** > **将警告视为错误**到**否 (/ WX-)**，然后重新生成项目。

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>创建核心应用 (BankAccountCoreApp) 以测试 Windows 运行时组件

现在创建新项目（在 `BankAccountWRC` 解决方案中，或在一个新解决方案中）。 创建**Visual c + +** > **Windows 通用** > **核心应用 (C + + WinRT)** 项目，然后将其命名为*BankAccountCoreApp*。

添加一个引用，然后浏览到`\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`（或如果这两个项目在同一个解决方案中添加项目到项目的引用，）。 单击**添加**，然后单击**确定**。 立即生成 BankAccountCoreApp。 你看到的错误的罕见的负载文件`readme.txt`不存在，从 Windows 运行时组件项目中排除该文件，重新生成它，然后重新生成 BankAccountCoreApp。

生成过程期间,`cppwinrt.exe` 工具会运行以将引用的 `.winmd` 文件处理到包含投影类型的源代码文件中,从而为你在使用组件时提供支持。 组件的运行时类的投影类型的标头&mdash;名为 `BankAccountWRC.h`&mdash;将生成在文件夹 `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\` 中。

`App.cpp` 中包含该标头。

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

此外，在 `App.cpp` 中，添加以下代码以实例化 BankAccount（使用投影类型的默认构造函数），注册事件处理程序，然后导致该帐户进入借方。

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_bankAccount.AccountIsInDebit([](const auto &, float balance)
        {
            WINRT_ASSERT(balance < 0.f);
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

每次单击此窗口时，将从该银行帐户的余额中减去 1。 演示按预期，引发该事件，放置一个断点正在处理**AccountIsInDebit**事件的 lambda 表达式内部、 运行该应用，以及在窗口内单击。

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>参数化的委托和简单信号，跨 ABI

如果你的事件必须可访问性跨应用程序二进制接口 (ABI)&mdash;例如组件和其使用的应用程序&mdash;事件必须使用 Windows 运行时委托类型。 上面的示例使用[**Windows::Foundation::EventHandler\ < 所 >**](/uwp/api/windows.foundation.eventhandler) Windows 运行时委托类型。 [**TypedEventHandler\ < TSender，TResult\ >**](/uwp/api/windows.foundation.eventhandler)是 Windows 运行时委托类型的另一个示例。

这些两个委托类型的类型参数必须跨 ABI，以便类型参数必须也是 Windows 运行时类型。 这包括第一方和第三方运行时类，以及基元类型，如数字和字符串。 编译器帮助你与"*必须是 WinRT 类型*"错误，如果你忘记了此约束。

如果你不需要任何参数或与你的事件参数传递，你可以定义自己简单的 Windows 运行时委托类型。 下面的示例显示**BankAccount**运行时类的简化版本。 它声明了名为**SignalDelegate**的委托类型，然后它使用的引发信号类型的事件而不是具有参数的事件。

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

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>参数化的委托、 简单信号和在项目内的回调

如果你事件仅在内部用于在 C + + WinRT 项目 （而不是跨二进制文件），则你仍然使用[**winrt::event**](/uwp/cpp-ref-for-winrt/event)结构模板，但参数化通过 C + + WinRT 的非运行 Windows 时[**winrt:: delegate&lt;...T&gt;**](/uwp/cpp-ref-for-winrt/delegate)结构模板，这是一个高效、 引用计数的委托。 它支持任意数量的参数，并且它们并不局限于 Windows 运行时类型。

下面的示例首先显示委托，不带任何参数 （本质上是简单信号），签名，然后另一个接受字符串。

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

注意如何可以添加到事件与你希望的多个订阅委托。 但是，还有一些与事件关联的开销。 如果你只是简单仅单个订阅的委托回调，则可以使用[**winrt:: delegate&lt;...T&gt;**](/uwp/cpp-ref-for-winrt/delegate)自身。

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

如果你要移植从 C + + /CX 基本代码其中事件和委托内部使用在一个项目，然后**winrt:: delegate**将帮助你复制该模式在 C + + WinRT。

## <a name="design-guidelines"></a>设计指南

我们建议作为函数参数传递事件，并不委托。 [**Winrt::event**](/uwp/cpp-ref-for-winrt/event) **添加**函数是一个例外，因为你必须在此情况下传递委托。 本指南的原因是因为委托可以跨不同的 Windows 运行时语言 （根据它们是否支持一个客户端注册、 或多个） 采用不同形式。 事件，其多个订阅服务器模型后，构成更加可预测且一致的选项。

事件处理程序委托的签名应包含两个参数：*发送者*(**IInspectable**)，并且*参数*（某些事件参数类型，例如[**RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)）。

请注意，是否设计一个内部 API，这些指南不一定适用。 尽管内部 Api 通常成为公共随着时间的推移。

## <a name="related-topics"></a>相关主题
* [使用 C++/WinRT 创作 API](author-apis.md)
* [通过 C++/WinRT 使用 API](consume-apis.md)
* [在 C++/WinRT 中使用委托处理事件](handle-events.md)

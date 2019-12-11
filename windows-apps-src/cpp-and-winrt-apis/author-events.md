---
description: 本主题演示了如何创作包含引发事件的运行时类的 Windows 运行时组件。 它还演示了使用该组件并处理事件的应用。
title: 在 C++/WinRT 中创作事件
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 创作, 事件
ms.localizationpriority: medium
ms.openlocfilehash: 6fb9b98ec362b59ad2593bbce24654f1dcfc7638
ms.sourcegitcommit: 27cb7c4539bb6417d32883824ccea160bb948c15
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2019
ms.locfileid: "74830788"
---
# <a name="author-events-in-cwinrt"></a>在 C++/WinRT 中创作事件

本主题演示如何创作 Windows 运行时组件，该组件包含表示银行帐户（当该银行帐户将透支时，该银行帐户会引发事件）的运行时类。 本主题还演示一个使用该银行帐户运行时类、调用函数以调整余额并处理引发的任何事件的核心应用。

> [!NOTE]
> 有关安装和使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Visual Studio 扩展 (VSIX) 和 NuGet 包（两者共同提供项目模板，并生成支持）的信息，请参阅[适用于 C++/WinRT 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

> [!IMPORTANT]
> 有关支持你了解如何利用 C++/WinRT 来使用和创作运行时类的基本概述和术语，请参阅[通过 C++/WinRT 使用 API](consume-apis.md) 和[通过 C++/WinRT 创作 API](author-apis.md)。

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>创建 Windows 运行时组件 (BankAccountWRC)

首先在 Microsoft Visual Studio 中创建新项目。 创建一个 Windows 运行时组件 (C++/WinRT) 项目，然后将其命名为 BankAccountWRC（针对“银行帐户 Windows 运行时组件”）   。 将项目命名为 *BankAccountWRC* 会让你在执行本主题的其余步骤时拥有最轻松的体验。 暂时不要生成该项目。

该新建项目包含一个名为 `Class.idl` 的文件。 将该文件重命名为 `BankAccount.idl`（重命名 `.idl` 文件还会自动重命名从属的 `.h` 和 `.cpp` 文件）。 将 `BankAccount.idl` 中的内容替换为下表。

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

保存文件。 该项目目前不会完全生成，但是现在生成很有助益，因为它会生成源代码文件，而你将在该文件中实现 BankAccount 运行时类  。 因此，继续生成（此阶段可能发生的生成错误与找不到 `Class.h` 和 `Class.g.h` 有关）。

在生成过程中，`midl.exe` 工具会运行以创建组件的 Windows 运行时元数据文件（即 `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`）。 然后，`cppwinrt.exe` 工具运行（具有 `-component` 选项）以生成源代码文件，从而为你在创作组件时提供支持。 这些文件包含让你开始实现已在 IDL 中声明的 BankAccount 运行时类的存根  。 这些存根是 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` 和 `BankAccount.cpp`。

右键单击项目节点，然后单击“打开文件资源管理器中的文件夹”  。 执行此操作，将在文件资源管理器中打开项目文件夹。 将存根文件 `BankAccount.h` 和 `BankAccount.cpp` 从文件夹 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` 复制到包含项目文件的文件夹（即 `\BankAccountWRC\BankAccountWRC\`），并替换目标中的文件。 现在，让我们打开 `BankAccount.h` 和 `BankAccount.cpp` 并实现运行时类。 在 `BankAccount.h` 中，将两个私有成员添加到 **BankAccount** 的实现（不是工厂实现）  。

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

如上所示，该事件是根据 [winrt::event](/uwp/cpp-ref-for-winrt/event) 结构模板实现的，由特定的委托类型参数化  。

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

其他重载（注册和手动撤销重载）不会合并到投影中。  这是为了让你能够灵活地以最佳方式针对自己的方案来实现它们。 像这些实现中所示那样调用 [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) 和 [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) 是高效且并发/线程安全的默认设置。 但是，如果你有大量事件，那么你可能不希望每个事件都有一个事件字段，而是改为选择某些类型的稀疏实现。

你还可以从上述情况中发现，如果余额变为负，AdjustBalance 函数的实现将引发 AccountIsInDebit 事件   。

如果任何警告阻止你进行生成，请处理这些警告或将项目属性“C/C++” > “常规” > “将警告视为错误”设置为“否(/WX-)”，然后重新生成该项目     。

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>创建核心应用 (BankAccountCoreApp) 以测试 Windows 运行时组件

现在创建新项目（在 *BankAccountWRC* 解决方案中，或在一个新解决方案中）。 创建核心应用 (C++/WinRT) 项目，然后将其命名为 BankAccountCoreApp   。

> [!NOTE]
> 如前文所述，文件夹 `\BankAccountWRC\Debug\BankAccountWRC\` 中创建了 Windows 运行时组件的 Windows 运行时元数据文件（其项目命名为 *BankAccountWRC*）。 该路径的第一段是包含解决方案文件的文件夹的名称；下一段是名为 `Debug` 的子目录；最后一段是为 Windows 运行时组件命名的子目录。 如果未将项目命名为 *BankAccountWRC*，则元数据文件将位于 `\<YourProjectName>\Debug\<YourProjectName>\` 文件夹中。

现在，在“核心应用”项目 (*BankAccountCoreApp*) 中添加一个引用，然后浏览到 `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`（或者，如果同一解决方案中有两个项目，则添加一个项目到项目的引用）。 单击“添加”，然后单击“确定”   。 现在生成 *BankAccountCoreApp*。 在极少数情况下，如果看到一个显示负载文件 `readme.txt` 不存在的错误，请从 Windows 运行时组件项目中排除该文件，重新生成该文件，然后重新生成 *BankAccountCoreApp*。

生成过程期间,`cppwinrt.exe` 工具会运行以将引用的 `.winmd` 文件处理到包含投影类型的源代码文件中,从而为你在使用组件时提供支持。 组件的运行时类的投影类型的标头&mdash;名为 `BankAccountWRC.h`&mdash;将生成在文件夹 `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\` 中。

`App.cpp` 中包含该标头。

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

此外，在 `App.cpp` 中，添加以下代码以实例化 **BankAccount**（使用投影类型的默认构造函数），注册事件处理程序，然后导致该帐户透支。

`WINRT_ASSERT` 是宏定义，并且扩展到 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)。

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

每次单击此窗口时，将从该银行帐户的余额中减去 1。 要演示按预期引发事件，请在用于处理 AccountIsInDebit 事件的 lambda 表达式内部放置一个断点，运行该应用，然后单击此窗口内部  。

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>跨 ABI 的参数化委托和简单信号

如果事件必须跨应用程序二进制接口 (ABI) 可访问（例如在组件及其所用应用程序之间），那么该事件必须使用 Windows 运行时委托类型。 上述示例使用 [Windows::Foundation::EventHandler\<T\>](/uwp/api/windows.foundation.eventhandler) Windows 运行时委托类型  。 [TypedEventHandler\<TSender, TResult\>](/uwp/api/windows.foundation.eventhandler) 是另一种 Windows 运行时委托类型  。

这两种委托类型的类型参数必须跨 ABI，因此类型参数也必须是 Windows 运行时类型。 这包括第一和第三方运行时类，以及数字和字符串等基本类型。 如果你忘记了此约束，编译器将帮助你处理“必须为 WinRT 类型”错误  。

如果无需连同事件传递任何形参或实参，则可以定义自己的简单 Windows 运行时委托类型。 以下示例展示 BankAccount 运行时类的更简易版本  。 它声明名为 SignalDelegate 的委托类型，然后使用该类型来引发信号类型事件，而不是具有参数的事件  。

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
如果所需事件是 Visual Studio 项目内部的（未跨二进制文件），而在内部这些事件不限于 Windows 运行时类型，则仍可使用 [**winrt::event**](/uwp/cpp-ref-for-winrt/event)\<Delegate\> 类模板。 请直接使用 [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) 而不是实际的 Windows 运行时委托类型，因为 **winrt::delegate** 也支持非 Windows 运行时参数。

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

注意如何向事件添加尽可能多的订阅委托。 但会产生一些与事件相关的开销。 如果只需仅具有一个订阅委托的简单回调，则你可以独立使用 [winrt::delegate&lt;…  T&gt;](/uwp/cpp-ref-for-winrt/delegate)。

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

如果你正在从项目内部使用事件和代理的 C++/CX 基本代码移植，winrt::delegate 将帮助你复制 C++/WinRT 中的这个模式  。

## <a name="design-guidelines"></a>设计指南

建议将事件而不是委托作为函数参数传递。 [winrt::event](/uwp/cpp-ref-for-winrt/event) 的 add 函数是例外，因为在这种情况下必须传递委托   。 这条指导原则的原因是，委托在各种 Windows 运行时语中可以具有各种形式（根据支持一个还是多个客户端注册）。 事件及其多个订阅者模型构成更可预测且更一致的选项。

事件处理程序委托的签名应该由以下两个形参组成：发送方 (IInspectable) 和实参（[RoutedEventArgs](/uwp/api/windows.ui.xaml.routedeventargs) 等事件实参类型）     。

注意，如果设计的是内部 API，这些指导原则不一定适用。 但随时间推移，内部 API 通常会公开。

## <a name="related-topics"></a>相关主题
* [使用 C++/WinRT 创作 API](author-apis.md)
* [通过 C++/WinRT 使用 API](consume-apis.md)
* [在 C++/WinRT 中使用委托处理事件](handle-events.md)
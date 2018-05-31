---
author: stevewhims
description: 本主题演示如何创作包含引发事件的运行时类的 Windows 运行时组件。 它还演示使用该组件并处理事件的应用。
title: 在 C++/WinRT 中创作事件
ms.author: stwhi
ms.date: 04/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 创作, 事件
ms.localizationpriority: medium
ms.openlocfilehash: b7574f1a3406dae665ced80294f7bc1cf91aeb8c
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832021"
---
# <a name="author-events-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>在 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中创作事件
> [!NOTE]
> **与在商业发行之前可能会进行实质性修改的预发布产品相关的一些信息。 Microsoft 对于此处提供的信息不作任何明示或默示的担保。**

本主题演示如何创作 Windows 运行时组件，该组件包含表示银行帐户的运行时类，当该帐户的余额进入借方时其会引发事件。 它还演示使用该银行帐户运行时类、调用函数以调整余额并处理产生的任何事件的核心应用。

> [!NOTE]
> 有关 C++/WinRT Visual Studio Extension (VSIX)（提供项目模板支持以及 C++/WinRT MSBuild 属性和目标）的当前可用性的信息，请参阅 [Visual Studio 支持 C++/WinRT 和 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

> [!IMPORTANT]
> 有关支持你了解如何利用 C++/WinRT 来使用和创作运行时类的基本概述和术语，请参阅[通过 C++/WinRT 使用 API](consume-apis.md) 和[通过 C++/WinRT 创作 API](author-apis.md)。

## <a name="windowsfoundationeventhandlerlttgt-and-typedeventhandlerlttgt"></a>Windows::Foundation::EventHandler&lt;T&gt; 和 TypedEventHandler&lt;T&gt;
如果你要从在 Windows 运行时组件中实现的运行时类中引发事件，则应针对事件的委托类型使用 [**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler) 或 [**TypedEventHandler**](/uwp/api/windows.foundation.eventhandler)。 类型参数必须为 Windows 运行时类型，因此允许基元类型以及第三方运行时类。

如果你忘记了此约束，编译器将帮助你处理“*必须为 WinRT 类型*”错误。

## <a name="winrtdelegatelttgt"></a>winrt::delegate&lt;...T&gt;
如果你要从 C++ 类型中引发事件（在同一项目中创作和使用），则可以针对事件的委托类型使用 C++/WinRT's **winrt::delegate&lt;...T&gt;**。 在这种情况下，类型参数无需为 Windows 运行时类型。

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>创建 Windows 运行时组件 (BankAccountWRC)
首先在 Microsoft Visual Studio 中创建新项目。 创建一个 **Visual C++ Windows 运行时组件 (C++/WinRT)** 项目，然后将其命名为 *BankAccountWRC*（针对“银行帐户 Windows 运行时组件”）。

该新建项目包含一个名为 `Class.idl` 的文件。 将该文件重命名为 `BankAccountWRC.idl`，以便在你生成时，组件的 Windows 运行时元数据文件是针对该组件本身命名的。 在 `BankAccountWRC.idl` 中，将界面定义为以下列表所示。

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

保存文件并生成项目。 该生成尚未成功。 但是，在生成过程中，`midl.exe` 工具会运行以创建组件的 Windows 运行时元数据库（即 `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`）。 然后，`cppwinrt.exe` 工具运行（具有 `-component` 选项）以生成源代码文件，从而为你在创作组件时提供支持。 这些文件包含让你开始实现已在 IDL 中声明的 `BankAccount` 运行时类的存根。 这些存根是 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` 和 `BankAccount.cpp`。

将这些存根文件 `BankAccount.h` 和 `BankAccount.cpp` 从 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` 复制到项目文件夹中，即 `\BankAccountWRC\BankAccountWRC\`。 在**解决方案资源管理器**中，确保将**显示所有文件**切换为打开。 右键单击已复制的存根文件，然后单击**包括在项目中**。 此外，右键单击 `Class.h` 和 `Class.cpp`，然后单击**从项目中排除**。

现在，让我们打开 `BankAccount.h` 和 `BankAccount.cpp` 并实现运行时类。 在 `BankAccount.h` 中，将两个私有成员添加到 BankAccount 的实现（*不是*出厂实现）。

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        event<Windows::Foundation::EventHandler<float>> accountIsInDebitEvent;
        float balance{ 0.f };
    };
}
...
```

在 `BankAccount.cpp` 中，实现如下所示的函数。

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(event_token const& token)
    {
        accountIsInDebitEvent.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        balance += value;
        if (balance < 0.f) accountIsInDebitEvent(*this, balance);
    }
}
```

如果余额变为负，**AdjustBalance** 函数的实现将引发 **AccountIsInDebit** 事件。

如果任何警告阻止你进行生成，请将项目属性 **C/C++** > **常规** > **将警告视为错误**设置为**否(/WX-)**，然后重新生成该项目。

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>创建核心应用 (BankAccountCoreApp) 以测试 Windows 运行时组件
现在创建新项目（在 `BankAccountWRC` 解决方案中，或在一个新解决方案中）。 创建 **Visual C++ 核心应用 (C++/WinRT)** 项目，然后将其命名为 *BankAccountCoreApp*。

添加一个引用，然后浏览到 `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`。 单击**添加**，然后单击**确定**。 立即生成 BankAccountCoreApp。 如果你看到一个显示负载文件 `readme.txt` 不存在的错误，从 Windows 运行时组件项目中排除该文件，重新生成该文件，然后重新生成 BankAccountCoreApp。

生成过程期间,`cppwinrt.exe` 工具会运行以将引用的 `.winmd` 文件处理到包含投影类型的源代码文件中,从而为你在使用组件时提供支持。 组件的运行时类的投影类型的标头&mdash;名为 `BankAccountWRC.h`&mdash;将生成在文件夹 `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\` 中。

`App.cpp` 中包含该标头。

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

此外，在 `App.cpp` 中，添加以下代码以实例化 BankAccount（使用投影类型的默认构造函数），注册事件处理程序，然后导致该帐户进入借方。

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount bankAccount;
    event_token eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        eventToken = bankAccount.AccountIsInDebit([](const auto &, float balance)
        {
            assert(balance < 0.f);
        });
    }
    ...

    void Uninitialize()
    {
        bankAccount.AccountIsInDebit(eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

每次单击此窗口时，将从该银行帐户的余额中减去 1。 要演示按预期引发事件，请在 lambda 表达式内部放置一个断点，运行该应用，然后单击此窗口内部。

## <a name="related-topics"></a>相关主题
* [使用 C++/WinRT 创作 API](author-apis.md)
* [通过 C++/WinRT 使用 API](consume-apis.md)
* [在 C++/WinRT 中使用委托处理事件](handle-events.md)

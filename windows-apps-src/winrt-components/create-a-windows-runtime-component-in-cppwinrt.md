---
title: 用 c + +/WinRT Windows 运行时组件
description: 本主题演示如何使用[c + +/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)创建和使用 Windows 运行时组件 &mdash; ，该组件可从使用任意 Windows 运行时语言构建的通用 Windows 应用程序调用。
ms.date: 07/06/2020
ms.topic: article
keywords: windows 10、uwp、windows、运行时、组件、组件 Windows 运行时组件、WRC、c + +/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: e47175579fcfc5544587ff36baaaa653003c4c63
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86494144"
---
# <a name="windows-runtime-components-with-cwinrt"></a>用 c + +/WinRT Windows 运行时组件

本主题演示如何使用[c + +/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)创建和使用 Windows 运行时组件 &mdash; ，该组件可从使用任意 Windows 运行时语言构建的通用 Windows 应用程序调用。

用 c + +/WinRT. 生成 Windows 运行时组件有多种原因。
- 在复杂或计算密集型操作中享有 c + + 的性能优势。
- 用于重用已编写和测试的标准 c + + 代码。
- 向编写的通用 Windows 平台（UWP）应用公开 Win32 功能，例如 c #。

通常，当你创作 c + +/WinRT 组件时，你可以使用标准 c + + 库中的类型和内置类型，但在应用程序二进制接口（ABI）边界内，你要将数据传递到另一个包中的代码或从其他包中的代码传递数据 `.winmd` 。 在 ABI 上，使用 Windows 运行时类型。 此外，在 c + +/WinRT 代码中，使用委托和事件之类的类型来实现可从你的组件引发的事件，并使用另一种语言进行处理。 有关 c + +/WinRT. 的详细信息，请参阅[c + +/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

本主题的其余部分将介绍如何在 c + +/WinRT 中创作 Windows 运行时组件，然后从应用程序使用该组件。

本主题中将生成的 Windows 运行时组件包含表示银行帐户的运行时类。 本主题还演示使用银行帐户运行时类的核心应用程序，并调用函数来调整余额。

> [!NOTE]
> 有关安装和使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Visual Studio 扩展 (VSIX) 和 NuGet 包（两者共同提供项目模板，并生成支持）的信息，请参阅[适用于 C++/WinRT 的 Visual Studio 支持](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

> [!IMPORTANT]
> 有关支持你了解如何利用 C++/WinRT 来使用和创作运行时类的基本概述和术语，请参阅[通过 C++/WinRT 使用 API](/windows/uwp/cpp-and-winrt-apis/consume-apis) 和[通过 C++/WinRT 创作 API](/windows/uwp/cpp-and-winrt-apis/author-apis)。

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>创建 Windows 运行时组件 (BankAccountWRC)

首先在 Microsoft Visual Studio 中创建新项目。 创建一个 Windows 运行时组件 (C++/WinRT) 项目，然后将其命名为 BankAccountWRC（针对“银行帐户 Windows 运行时组件”）。 请确保未选中 **"将解决方案和项目放在同一目录中"** 。 面向 Windows SDK 的最新正式发布（非预览）版本。 将项目命名为 *BankAccountWRC* 会让你在执行本主题的其余步骤时拥有最轻松的体验。 

暂时不要生成该项目。

该新建项目包含一个名为 `Class.idl` 的文件。 在解决方案资源管理器中，将该文件重命名为 `BankAccount.idl`（重命名 `.idl` 文件还会自动重命名从属的 `.h` 和 `.cpp` 文件）。 将 `BankAccount.idl` 中的内容替换为下表。

> [!NOTE]
> 不用说，您也不会以这种方式实现生产金融软件;`Single`在此示例中，只是为了方便起见。

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    runtimeclass BankAccount
    {
        BankAccount();
        void AdjustBalance(Single value);
    };
}
```

保存该文件。 该项目目前不会完全生成，但是现在生成很有助益，因为它会生成源代码文件，而你将在该文件中实现 BankAccount 运行时类。 因此，继续生成（此阶段可能发生的生成错误与找不到 `Class.h` 和 `Class.g.h` 有关）。

在生成过程中，`midl.exe` 工具会运行以创建组件的 Windows 运行时元数据文件（即 `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`）。 然后，`cppwinrt.exe` 工具运行（具有 `-component` 选项）以生成源代码文件，从而为你在创作组件时提供支持。 这些文件包含让你开始实现已在 IDL 中声明的 BankAccount 运行时类的存根。 这些存根是 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` 和 `BankAccount.cpp`。

右键单击项目节点，然后单击“打开文件资源管理器中的文件夹”。 执行此操作，将在文件资源管理器中打开项目文件夹。 将存根文件 `BankAccount.h` 和 `BankAccount.cpp` 从文件夹 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` 复制到包含项目文件的文件夹（即 `\BankAccountWRC\BankAccountWRC\`），并替换目标中的文件。 现在，让我们打开 `BankAccount.h` 和 `BankAccount.cpp` 并实现运行时类。 在中 `BankAccount.h` ，将新的私有成员添加到**BankAccount**的实现（*而非*工厂实现中）。

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        float m_balance{ 0.f };
    };
}
...
```

在中 `BankAccount.cpp` ，实现**AdjustBalance**方法，如下列表所示。

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
    }
}
```

还需要删除这两个文件中的 `static_assert`。

如果任何警告阻止你进行生成，请处理这些警告或将项目属性“C/C++” > “常规” > “将警告视为错误”设置为“否(/WX-)”，然后重新生成该项目   。

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>创建核心应用 (BankAccountCoreApp) 以测试 Windows 运行时组件

现在创建新项目（在 *BankAccountWRC* 解决方案中，或在一个新解决方案中）。 创建核心应用 (C++/WinRT) 项目，然后将其命名为 BankAccountCoreApp。 如果两个项目位于同一个解决方案中，则将 BankAccountCoreApp 设置为启动项目。

> [!NOTE]
> 如前文所述，文件夹 `\BankAccountWRC\Debug\BankAccountWRC\` 中创建了 Windows 运行时组件的 Windows 运行时元数据文件（其项目命名为 *BankAccountWRC*）。 该路径的第一段是包含解决方案文件的文件夹的名称；下一段是名为 `Debug` 的子目录；最后一段是为 Windows 运行时组件命名的子目录。 如果未将项目命名为 *BankAccountWRC*，则元数据文件将位于 `\<YourProjectName>\Debug\<YourProjectName>\` 文件夹中。

现在，在“核心应用”项目 (*BankAccountCoreApp*) 中添加一个引用，然后浏览到 `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`（或者，如果同一解决方案中有两个项目，则添加一个项目到项目的引用）。 单击“添加”，然后单击“确定” 。 现在生成 *BankAccountCoreApp*。 在极少数情况下，如果看到一个显示负载文件 `readme.txt` 不存在的错误，请从 Windows 运行时组件项目中排除该文件，重新生成该文件，然后重新生成 *BankAccountCoreApp*。

生成过程期间,`cppwinrt.exe` 工具会运行以将引用的 `.winmd` 文件处理到包含投影类型的源代码文件中,从而为你在使用组件时提供支持。 组件的运行时类的投影类型的标头&mdash;名为 `BankAccountWRC.h`&mdash;将生成在文件夹 `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\` 中。

`App.cpp` 中包含该标头。

```cppwinrt
// App.cpp
...
#include <winrt/BankAccountWRC.h>
...
```

此外，在中 `App.cpp` ，添加以下代码以实例化**BankAccount**对象（使用投影类型的默认构造函数），并对银行帐户对象调用方法。

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(1.f);
        ...
    }
    ...
};
```

每次单击该窗口时，将递增银行帐户对象的余额。 如果要单步执行代码以确认应用程序确实正在调用 Windows 运行时组件，则可以设置断点。

## <a name="next-steps"></a>后续步骤

若要将更多功能或新 Windows 运行时类型添加到 c + +/WinRT Windows 运行时组件，可以遵循上面所示的相同模式。 首先，使用 IDL 定义要公开的功能。 然后在 Visual Studio 中生成项目以生成存根实现。 然后，根据需要完成实现。 IDL 中定义的任何方法、属性和事件都对使用 Windows 运行时组件的应用程序可见。 有关 IDL 的详细信息，请参阅[Microsoft 接口定义语言3.0 的简介](/uwp/midl-3/intro)。

有关如何向 Windows 运行时组件添加事件的示例，请参阅[在 c + + 中创作事件/WinRT](/windows/uwp/cpp-and-winrt-apis/author-events)。

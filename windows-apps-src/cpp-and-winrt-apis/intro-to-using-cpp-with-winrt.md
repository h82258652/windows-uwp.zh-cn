---
description: 对 C++/WinRT（一种适用于 Windows 运行时 API 的标准 C++ 语言投影）的介绍。
title: C++/WinRT 简介
ms.date: 01/31/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 简介
ms.localizationpriority: medium
ms.openlocfilehash: 5281049aa9ddec58a97283a2ca6ba5d229a49c4e
ms.sourcegitcommit: 038fe813c73804285d5e74d97864ac1a2fb531f3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2019
ms.locfileid: "9042601"
---
# <a name="introduction-to-cwinrt"></a>C++/WinRT 简介
&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT 是 Windows 运行时 (WinRT) API 的完全标准新式 C++17 语言投影，以基于标头文件的库的形式实现，旨在为你提供对新式 Windows API 的一流访问。 利用 C++/WinRT，你可以采用任何符合标准的 C++17 编译器创作和使用 Windows 运行时 API。 Windows SDK 包含 C++/WinRT；它已在版本 10.0.17134.0（Windows 10，版本 1803）中引用。

C + + /winrt 是 Microsoft 的建议的替换[C + + CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live)语言投影，以及[Windows 运行时 c + + 模板库 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live)。 完整列表[主题有关 C + + WinRT](index.md#topics-about-cwinrt)包括信息与，进行互操作和从移植，C + + /CX 和 WRL。

> [!IMPORTANT]
> 两个最重要的部分 C + + WinRT 需要注意的部分中进行了描述[SDK 支持 C + + WinRT](#sdk-support-for-cwinrt)和[Visual Studio 支持 C + + WinRT、 XAML，VSIX 扩展和 NuGet 程序包](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="language-projections"></a>语言投影
Windows 运行时基于组件对象模型 (COM) API，可通过*语言投影* 访问它。 投影将隐藏 COM 详细信息，并为给定语言提供更自然的编程体验。

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Windows UWP API 引用内容中的 C++/WinRT 语言投影
当你浏览 [Windows UWP API](https://docs.microsoft.com/uwp/api/) 时，请单击右上角的**语言**组合框，然后选择 **C++/WinRT** 以查看 API 语法块（当它们在 C++/WinRT 语言投影中出现时）。

## <a name="sdk-support-for-cwinrt"></a>对 C++/WinRT 的 SDK 支持
从版本 10.0.17134.0（Windows 10 版本 1803）起，Windows SDK 包含基于标头文件的标准 C++ 库以用于使用第一方 Windows API（Windows 命名空间中的 Windows 运行时 API）。 C++/WinRT 还附带了 `cppwinrt.exe` 工具，你可以在 Windows 运行时元数据 (`.winmd`) 文件指向该工具来生成基于标头文件的*投影* API 的标准 C++ 库，如从 C++/WinRT 代码使用元数据中所述。 Windows 运行时元数据 (`.winmd`) 文件提供了描述 Windows 运行时 API 图面的 Canonical 方法。 通过在元数据指向 `cppwinrt.exe`，在中，你可以生成使用在第二方或第三方 Windows 运行时组件中实现的或在你自己的应用程序中实现的任何运行时类的库。 有关详细信息，请参阅[通过 C++/WinRT 使用 API](consume-apis.md)。

通过 C++/WinRT，你还可以使用标准 C++ 实现自己的运行时类，而不必求助于 COM 样式的编程。 对于运行时类，你只需在 IDL 文件中描述类型，`midl.exe` 和 `cppwinrt.exe` 将为你生成实现样板源代码文件。 你也可以通过从 C++/WinRT 基类派生来只实现接口。 有关详细信息，请参阅[通过 C++/WinRT 创作 API](author-apis.md)。

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>Visual Studio 支持 C + + WinRT、 XAML，VSIX 扩展和 NuGet 程序包
对于 Visual Studio 支持，除了最低 Windows SDK 目标版本 10.0.17134.0 (Windows 10，版本 1803年)，你将需要 Visual Studio 2017 (至少版本 15.6; 我们建议至少是 15.7)，或 Visual Studio 2019。 如果你尚未安装它，你将需要安装从 Visual Studio 安装程序内的**c + + 通用 Windows 平台工具**选项。 并在 Windows**设置** > **更新 \& 安全** > **适用于开发人员**，选择**开发人员模式**选项，而不是**旁加载应用**选项。

你将需要下载并安装最新版本的[C + + /winrt Visual Studio 扩展 (VSIX)](https://aka.ms/cppwinrt/vsix)从[Visual Studio Marketplace](https://marketplace.visualstudio.com/)。

- VSIX 扩展提供 C + + WinRT 项目模板和项目模板在 Visual Studio 中，以便你可以开始使用 C + + WinRT 开发。
- 此外，它将提供 Visual Studio 本机调试可视化效果 (natvis) 的 C + + /winrt 投影类型;提供与 C# 调试类似的体验。 Natvis 对于调试版本是自动的。 你可以通过定义符号 WINRT_NATVIS 选择加入到该发布版本。

Visual Studio 项目模板的 C + + WinRT 如下所述。 当你创建新的 C + + WinRT 项目最新版本的安装，VSIX 扩展的新 C + + WinRT 项目会自动安装[Microsoft.Windows.CppWinRT NuGet 程序包](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)。 **Microsoft.Windows.CppWinRT** NuGet 程序包提供 C + + WinRT 构建支持 （MSBuild 属性和目标），使你的项目之间的开发计算机和生成代理移植 (在其上只有 NuGet 程序包，并不 VSIX 扩展已安装）。

> [!IMPORTANT]
> 如果你有使用创建 （或在升级） 的项目更早版本的 VSIX 扩展的版本比 1.0.190128.4，然后查看[较早版本的 VSIX 扩展](#earlier-versions-of-the-vsix-extension)。 该部分包含有关你的项目，你将需要知道其使用 VSIX 扩展的最新版本进行升级的配置的重要信息。

因为 C + + /winrt 使用 C + + 17 标准的功能，它必须项目属性**C/c + +** > **语言** > **c + + 语言标准** > **ISO C + + 17 标准 (/ std:c + + 17)**。 你可能还希望设置**合规模式: 是(/permissive-)**，它将进一步约束代码以符合标准。

要注意的另一个项目属性是 **C/C++** > **常规** > **将警告视为错误**。 请根据喜好将此项设置为**是(/WX)** 或**否(/WX-)**。 有时候，由 `cppwinrt.exe` 工具生成的源文件会生成警告，除非向其添加实现。

使用系统设置最多上文所述，你将能够创建和生成，或者打开，C + + WinRT 项目在 Visual Studio 中，并将其部署。

或者，也可以通过手动安装**Microsoft.Windows.CppWinRT** NuGet 程序包来转换现有项目。 之后安装 （或更新到） VSIX 扩展，最新版本在 Visual Studio 中打开现有的项目，单击**项目** \> **管理 NuGet 程序包...** \> **浏览**，键入或将**Microsoft.Windows.CppWinRT**粘贴搜索框中，选择搜索结果中的项，然后单击**安装**安装该项目的程序包。 添加程序包后，你将获得 C + + /winrt MSBuild 支持该项目，包括调用`cppwinrt.exe`工具。

你可以标识项目使用 C + + /winrt MSBuild 支持**Microsoft.Windows.CppWinRT** NuGet 程序包安装在项目中是否存在。

下面是由 VSIX 扩展提供的 Visual Studio 项目模板。

### <a name="windows-console-application-cwinrt"></a>Windows 控制台应用程序 (C++/WinRT)
适用于 Windows 桌面版的 C++/WinRT 客户端应用程序的项目模板，具有控制台用户界面。

### <a name="blank-app-cwinrt"></a>空白应用 (C++/WinRT)
通用 Windows 平台 (UWP) 应用的项目模板，具有 XAML 用户界面。

Visual Studio 提供了 XAML 编译器支持，用于从位于每个 XAML 标记文件后面的接口定义语言 (IDL) (`.idl`) 文件生成实现和标头存根。 在 IDL 文件中，定义你要在应用的 XAML 页面中引用的任何本地运行时类，然后构建项目一次以在 `Generated Files` 中生成实现模板并在 `Generated Files\sources` 中生成存根类型定义。 然后使用这些存根类型定义作为参考以实现本地运行时类。 我们建议在每个运行时类自己的 IDL 文件中声明这些类。

Visual Studio 的 XAML 设计面的支持 C + + WinRT 是接近与 C# 的奇偶校验。 有一个例外是**事件**选项卡的**属性**窗口。 通过 C# 项目，你可以使用该选项卡来添加事件处理程序;通过 C + + WinRT 项目中，该功能不存在。 但请参见[通过 C + 使用委托处理事件 + WinRT](handle-events.md)有关如何将事件处理程序添加到你的代码的信息。

### <a name="core-app-cwinrt"></a>核心应用 (C++/WinRT)
通用 Windows 平台 (UWP) 应用的项目模板，不使用 XAML。

相反，它使用 Windows.ApplicationModel.Core 命名空间的 C++/WinRT Windows 命名空间标头。 构建并运行后，单击空白空间以添加彩色方块；然后单击彩色方块以拖动它。

### <a name="windows-runtime-component-cwinrt"></a>Windows 运行时组件 (C++/WinRT)
组件的项目模板；通常针对从通用 Windows 平台 (UWP) 的使用。

此模板演示了 `midl.exe` > `cppwinrt.exe` 工具链，其中 Windows 运行时元数据 (`.winmd`) 从 IDL 生成，然后实现和标头存根从 Windows 运行时元数据生成。

在 IDL 文件中，在组件、组件的默认接口和组件实现的任何其他接口中定义运行时类。 构建项目一次以生成 `module.g.cpp`、`module.h.cpp`、`Generated Files` 中的实现模板和 `Generated Files\sources` 中的存根类型定义。 然后使用这些存根类型定义作为参考以在组件中实现运行时类。 我们建议在每个运行时类自己的 IDL 文件中声明这些类。

将生成的 Windows 运行时组件二进制文件及其 `.winmd` 与使用它们的 UWP 应用绑定。

## <a name="earlier-versions-of-the-vsix-extension"></a>较早版本的 VSIX 扩展
我们建议你安装 （或更新） [VSIX 扩展](https://aka.ms/cppwinrt/vsix)的最新版本。 它配置为默认情况下自行更新。 如果你执行此操作，并且你有使用 VSIX 扩展早于 1.0.190128.4，则本部分的版本创建的项目中包含有关升级这些项目，以使用最新版本的重要信息。 如果你不更新，然后你将仍然有用信息本部分中。

支持的 Windows SDK 和 Visual Studio 版本和 Visual Studio 配置中的信息[Visual Studio 支持 C + + WinRT、 XAML，VSIX 扩展和 NuGet 程序包](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)以上部分适用于较早版本的 VSIX扩展。 下面的信息介绍关于行为的重要差异，并配置更早版本项目与创建 （或升级后使用） 版本。

### <a name="created-earlier-than-101810022"></a>早于 1.0.181002.2 创建
如果你的项目创建版本的 VSIX 扩展早于 1.0.181002.2，则 C + + WinRT 生成支持已内置于该版本的 VSIX 扩展。 你的项目具有`<CppWinRTEnabled>true</CppWinRTEnabled>`中设置属性`.vcxproj`文件。

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

你可以通过手动安装**Microsoft.Windows.CppWinRT** NuGet 程序包升级你的项目。 之后安装 （或升级到） VSIX 扩展，最新版本在 Visual Studio 中打开项目，单击**项目** \> **管理 NuGet 程序包...** \> **浏览**，键入或粘贴**Microsoft.Windows.CppWinRT**在搜索框中，选择搜索结果中的项目，然后单击**安装**，以安装你的项目的包。

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>使用创建 （或升级到） 1.0.181002.2 和 1.0.190128.3 之间
如果你的项目创建 VSIX 扩展 1.0.181002.2 和 1.0.190128.3 之间的版本，非独占，然后**Microsoft.Windows.CppWinRT** NuGet 程序包已在项目中自动安装的项目模板。 你可能还升级较旧的项目，以在此范围内使用 VSIX 扩展的版本。 如果你这样做，则&mdash;也仍存在于版本在此范围内的 VSIX 扩展中生成支持起计&mdash;升级后的项目可能或可能没有安装**Microsoft.Windows.CppWinRT** NuGet 程序包。

若要升级你的项目，按照上一节中的说明，并确保你的项目确实具有**Microsoft.Windows.CppWinRT** NuGet 程序包安装。

### <a name="invalid-upgrade-configurations"></a>无效的升级配置
使用 VSIX 扩展的最新版本，它不是项目具有有效`<CppWinRTEnabled>true</CppWinRTEnabled>`如果还没有安装**Microsoft.Windows.CppWinRT** NuGet 程序包的属性。 一个项目，此配置将产生生成错误消息，"C + + /winrt VSIX 不再提供项目的生成支持。  请添加对 Microsoft.Windows.CppWinRT Nuget 程序包的项目引用。

正如提到的上面，C + + WinRT 项目现在需要有在其中安装 NuGet 程序包。

由于`<CppWinRTEnabled>`元素已过时，则可以选择性地编辑你`.vcxproj`，并删除该元素。 它不是绝对需要，但它是一个选项。

## <a name="custom-types-in-the-cwinrt-projection"></a>C++/WinRT 投影中的自定义类型
在 C + /winrt 编程中，你可以使用标准 c + + 语言功能和[标准 c + + 数据类型和 C + + WinRT](std-cpp-data-types.md)&mdash;包括一些 c + + 标准库数据类型。 但你还会在投影中发现一些自定义数据类型，并且可以选择使用它们。 例如，我们使用 [C++/WinRT 入门](get-started.md)中快速入门代码示例中的 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)。

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array) 是你可能在某个时间使用的另一个类型。 但你不太可能直接使用 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) 之类的类型。 或者，你可能选择不使用它，以便在等效类型出现在 C++ 标准库中时不用更改任何代码。

> [!WARNING]
> 如果你仔细研究 C++/WinRT Windows 命名空间标头，你可能也会发现一些类型。 一个示例是 **winrt::param::hstring**，另外还有一些示例。 这些单纯用于优化输入参数的绑定，他们实现了显著的性能改进，并使相大多数调用模式“只”为相关的标准 C++ 类型和容器工作。 在增加最大价值的情况下，这些类型只被投影使用过。 它们被高度优化且不用于一般用途；请不要尝试自行使用。 也不应该使用来自 `winrt::impl` 命名空间的任何内容，因为这些属于实现类型，因此可能更改。 你应该继续使用标准类型，或来自 [winrt 命名空间](/uwp/cpp-ref-for-winrt/winrt)的类型。

## <a name="important-apis"></a>重要的 API
* [winrt::hstring 结构](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 命名空间](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相关主题
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT Visual Studio 扩展 (VSIX)](https://aka.ms/cppwinrt/vsix)
* [C++/WinRT 入门](get-started.md)
* [标准 C++ 数据类型和 C++/WinRT](std-cpp-data-types.md)
* [C++/WinRT 中的字符串处理](strings.md)
* [Windows UWP API](https://docs.microsoft.com/uwp/api/)

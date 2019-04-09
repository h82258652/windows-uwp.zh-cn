---
description: 对 C++/WinRT（一种适用于 Windows 运行时 API 的标准 C++ 语言投影）的介绍。
title: C++/WinRT 简介
ms.date: 04/02/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 简介
ms.localizationpriority: medium
ms.openlocfilehash: e9e84370f0503a8f361df9b43b60a2870be745a3
ms.sourcegitcommit: 940645c705865ba9635ccae2da9d917420faf608
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2019
ms.locfileid: "58812596"
---
# <a name="introduction-to-cwinrt"></a>C++/WinRT 简介
&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT 是 Windows 运行时 (WinRT) API 的完全标准新式 C++17 语言投影，以基于标头文件的库的形式实现，旨在为你提供对新式 Windows API 的一流访问。 利用 C++/WinRT，你可以采用任何符合标准的 C++17 编译器创作和使用 Windows 运行时 API。 Windows SDK 包含 C++/WinRT；它已在版本 10.0.17134.0（Windows 10，版本 1803）中引用。

C++/ WinRT 是 Microsoft 的建议的替代[ C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live)语言投影，并且[Windows 运行时C++模板库 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live)。 完整列表[有关的主题C++/WinRT](index.md#topics-about-cwinrt)包括，与进行互操作和从，移植的相关信息C++/CX 和 WRL。

> [!IMPORTANT]
> 一些最重要的部分C++/WinRT 需要注意的部分中介绍了[SDK 支持用于C++/WinRT](#sdk-support-for-cwinrt)并[适用于 Visual Studio 支持C++/WinRT、 XAML、 VSIX 扩展和 NuGet包](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="language-projections"></a>语言投影
Windows 运行时基于组件对象模型 (COM) API，可通过*语言投影* 访问它。 投影将隐藏 COM 详细信息，并为给定语言提供更自然的编程体验。

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Windows UWP API 引用内容中的 C++/WinRT 语言投影
当你浏览 [Windows UWP API](https://docs.microsoft.com/uwp/api/) 时，请单击右上角的**语言**组合框，然后选择 **C++/WinRT** 以查看 API 语法块（当它们在 C++/WinRT 语言投影中出现时）。

## <a name="sdk-support-for-cwinrt"></a>对 C++/WinRT 的 SDK 支持
从版本 10.0.17134.0（Windows 10 版本 1803）起，Windows SDK 包含基于标头文件的标准 C++ 库以用于使用第一方 Windows API（Windows 命名空间中的 Windows 运行时 API）。

对于兼容性，Windows SDK 还附带了`cppwinrt.exe`工具。 但是，我们建议你改为安装和使用的最新版本`cppwinrt.exe`，这是附带**Microsoft.Windows.CppWinRT** NuGet 包。 该程序包，和`cppwinrt.exe`下, 一节所述。

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>Visual Studio 支持 C + WinRT、 XAML，VSIX 扩展和 NuGet 包
对于 Visual Studio 支持，除了最小的 Windows SDK 目标版本的 10.0.17134.0 (Windows 10，版本 1803年)，你将需要 Visual Studio 2019 或 Visual Studio 2017 (至少版本 15.6; 我们建议至少 15.7)。 如果你尚未安装它，你将需要安装**C++通用 Windows 平台工具**选项在 Visual Studio 安装程序中。 并且，在 Windows 中**设置** > **更新\&安全** > **面向开发人员**，选择**开发人员模式**选项而非**旁加载应用程序**选项。

将需要下载并安装最新版[ C++WinRT Visual Studio 扩展 (VSIX)](https://aka.ms/cppwinrt/vsix)从[Visual Studio Marketplace](https://marketplace.visualstudio.com/)。

- VSIX 扩展为你提供C++在 Visual Studio 中，/WinRT 项目和项模板，以便你可以开始使用C++/WinRT 开发。
- 此外，它为您提供 Visual Studio 本机调试可视化 (natvis) 的C++/WinRT 投影类型;提供的体验类似于C#调试。 Natvis 对于调试版本是自动的。 你可以通过定义符号 WINRT_NATVIS 选择加入到该发布版本。

Visual Studio 项目模板 C + WinRT 如下所述。 当您创建一个新C++安装的 VSIX 扩展的最新版本的 /WinRT 项目，则新C++/WinRT 项目会自动安装[Microsoft.Windows.CppWinRT NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)。 **Microsoft.Windows.CppWinRT** NuGet 包提供了C++/WinRT 生成支持 （MSBuild 属性和目标），使你的项目的开发计算机和生成代理之间可移植 (在其上仅 NuGet程序包，不 VSIX 扩展，已安装）。

> [!IMPORTANT]
> 如果必须使用创建 （或升级以使用） 的项目的 VSIX 扩展以前的版本比 1.0.190128.4，然后请参阅[早期版本的 VSIX 扩展](#earlier-versions-of-the-vsix-extension)。 该部分包含有关你将需要知道要将它们升级为使用 VSIX 扩展的最新版本的项目配置的重要信息。

> [!NOTE]
> 因为C++/WinRT 使用标准 C + + 17 中的功能，NuGet 包设置项目属性**C /C++** > **语言** >   **C++语言标准** > **ISO C + + 17 标准 (/ /std: c + + 17)** Visual Studio 中。 它还添加了[/bigobj](/cpp/build/reference/bigobj-increase-number-of-sections-in-dot-obj-file)编译器选项。
> 
> 您可能还要设置**符合模式：是 (触发的)**，这进一步约束代码是符合标准。 要注意的另一个项目属性是 **C/C++** > **常规** > **将警告视为错误**。 请根据喜好将此项设置为**是(/WX)** 或**否(/WX-)**。 有时候，由 `cppwinrt.exe` 工具生成的源文件会生成警告，除非向其添加实现。

将与按上面所述设置你的系统，是能够创建和生成，或打开状态， C++/WinRT 项目在 Visual Studio 中，并将其部署。

或者，可以将现有项目转换通过手动安装**Microsoft.Windows.CppWinRT** NuGet 包。 后安装 （或更新到） 最新版本的 VSIX 扩展，Visual Studio 中打开现有项目中，单击**项目** \> **管理 NuGet 包...**\> **浏览**，键入或粘贴**Microsoft.Windows.CppWinRT**在搜索框中，在搜索结果中选择的项，然后单击**安装**若要安装该项目的包。 添加包后，你将获得C++的项目，包括调用 WinRT MSBuild 支持`cppwinrt.exe`工具。 从版本 2.0 中，开始**Microsoft.Windows.CppWinRT** NuGet 包包含`cppwinrt.exe`工具。

您可以将指向`cppwinrt.exe`工具在 Windows 运行时元数据 (`.winmd`) 文件生成基于标头文件的标准C++库的*项目*从消耗的元数据中所述的 Api C++/ WinRT 代码。 Windows 运行时元数据 (`.winmd`) 文件提供了描述 Windows 运行时 API 图面的 Canonical 方法。 通过在元数据指向 `cppwinrt.exe`，在中，你可以生成使用在第二方或第三方 Windows 运行时组件中实现的或在你自己的应用程序中实现的任何运行时类的库。 有关详细信息，请参阅[通过 C++/WinRT 使用 API](consume-apis.md)。

通过 C++/WinRT，你还可以使用标准 C++ 实现自己的运行时类，而不必求助于 COM 样式的编程。 对于运行时类，你只需在 IDL 文件中描述类型，`midl.exe` 和 `cppwinrt.exe` 将为你生成实现样板源代码文件。 你也可以通过从 C++/WinRT 基类派生来只实现接口。 有关详细信息，请参阅[通过 C++/WinRT 创作 API](author-apis.md)。

可以识别的项目使用C++是否存在由 WinRT MSBuild 支持**Microsoft.Windows.CppWinRT**项目中安装的 NuGet 包。

以下是 Visual Studio 项目模板提供的 VSIX 扩展。

### <a name="windows-console-application-cwinrt"></a>Windows 控制台应用程序 (C++/WinRT)
适用于 Windows 桌面版的 C++/WinRT 客户端应用程序的项目模板，具有控制台用户界面。

### <a name="blank-app-cwinrt"></a>空白应用 (C++/WinRT)
通用 Windows 平台 (UWP) 应用的项目模板，具有 XAML 用户界面。

Visual Studio 提供了 XAML 编译器支持，用于从位于每个 XAML 标记文件后面的接口定义语言 (IDL) (`.idl`) 文件生成实现和标头存根。 在 IDL 文件中，定义你要在应用的 XAML 页面中引用的任何本地运行时类，然后构建项目一次以在 `Generated Files` 中生成实现模板并在 `Generated Files\sources` 中生成存根类型定义。 然后使用这些存根类型定义作为参考以实现本地运行时类。 我们建议在每个运行时类自己的 IDL 文件中声明这些类。

Visual Studio 的 XAML 设计面支持 C + WinRT 即将与相同的C#。 是一个例外**事件**选项卡**属性**窗口。 使用C#项目中，您可以使用该选项卡添加事件处理程序;使用C++/WinRT 项目中，设备不存在。 但请参阅[通过使用中的委托处理事件C++/WinRT](handle-events.md)有关如何将事件处理程序添加到你的代码的信息。

### <a name="core-app-cwinrt"></a>核心应用 (C++/WinRT)
通用 Windows 平台 (UWP) 应用的项目模板，不使用 XAML。

相反，它使用 Windows.ApplicationModel.Core 命名空间的 C++/WinRT Windows 命名空间标头。 构建并运行后，单击空白空间以添加彩色方块；然后单击彩色方块以拖动它。

### <a name="windows-runtime-component-cwinrt"></a>Windows 运行时组件 (C++/WinRT)
组件的项目模板；通常针对从通用 Windows 平台 (UWP) 的使用。

此模板演示了 `midl.exe` > `cppwinrt.exe` 工具链，其中 Windows 运行时元数据 (`.winmd`) 从 IDL 生成，然后实现和标头存根从 Windows 运行时元数据生成。

在 IDL 文件中，在组件、组件的默认接口和组件实现的任何其他接口中定义运行时类。 构建项目一次以生成 `module.g.cpp`、`module.h.cpp`、`Generated Files` 中的实现模板和 `Generated Files\sources` 中的存根类型定义。 然后使用这些存根类型定义作为参考以在组件中实现运行时类。 我们建议在每个运行时类自己的 IDL 文件中声明这些类。

将生成的 Windows 运行时组件二进制文件及其 `.winmd` 与使用它们的 UWP 应用绑定。

## <a name="earlier-versions-of-the-vsix-extension"></a>早期版本的 VSIX 扩展
我们建议你安装 （或更新到） 的最新版本[VSIX 扩展](https://aka.ms/cppwinrt/vsix)。 它被配置为默认情况下更新本身。 如果您这样做，并且必须使用早于 1.0.190128.4，则本部分中的 VSIX 扩展的版本创建的项目包含有关升级这些项目以使用新版本的重要信息。 如果你未更新，然后您将仍找到信息在本部分中非常有用。

支持的 Windows SDK 和 Visual Studio 版本以及 Visual Studio 配置中中的信息[适用于 Visual Studio 支持C++/WinRT、 XAML、 VSIX 扩展中，和 NuGet 包](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)上面部分适用于早期版本的 VSIX 扩展。 以下信息介绍了有关行为的重要差异以及配置之前项目中使用 （或创建已升级为使用） 版本。

### <a name="created-earlier-than-101810022"></a>早于 1.0.181002.2 创建
如果你的项目使用早于 1.0.181002.2，VSIX 扩展的版本然后创建C++/WinRT 生成支持已内置到该版本的 VSIX 扩展。 你的项目具有`<CppWinRTEnabled>true</CppWinRTEnabled>`属性中设置`.vcxproj`文件。

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

可以通过手动安装升级你的项目**Microsoft.Windows.CppWinRT** NuGet 包。 后安装 （或升级到） 最新版本的 VSIX 扩展，Visual Studio 中打开你的项目中，单击**项目** \> **管理 NuGet 包...**\> **浏览**，键入或粘贴**Microsoft.Windows.CppWinRT**在搜索框中，在搜索结果中选择的项，然后单击**安装**安装程序包为您的项目。

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>使用创建 （或升级到） 1.0.181002.2 和 1.0.190128.3 之间
如果使用你的项目创建了之间 1.0.181002.2 和 1.0.190128.3，非独占，VSIX 扩展的版本然后**Microsoft.Windows.CppWinRT**自动由项目的项目中安装了 NuGet 包模板。 您可能会同时升级的较旧的项目，在此范围中使用的 VSIX 扩展的版本。 如果您这样做，然后&mdash;还在此范围内的 VSIX 扩展的版本中仍存在生成支持以来&mdash;升级后的项目可能或可能不具有**Microsoft.Windows.CppWinRT** NuGet 包安装。

若要升级您的项目，请按照上一节中的说明，并确保项目确实含有**Microsoft.Windows.CppWinRT**安装 NuGet 包。

### <a name="invalid-upgrade-configurations"></a>无效的升级配置
VSIX 扩展的最新版本，它不能用于项目能够`<CppWinRTEnabled>true</CppWinRTEnabled>`属性，如果还没有**Microsoft.Windows.CppWinRT**安装 NuGet 包。 使用此配置项目生成生成错误消息" C++WinRT VSIX 不再提供项目的生成支持。  请添加对 Microsoft.Windows.CppWinRT Nuget 包的项目引用。"

如上所述， C++/WinRT 项目现在需要在其中安装了 NuGet 程序包。

由于`<CppWinRTEnabled>`元素现已过时，则可以根据需要编辑你`.vcxproj`，并删除该元素。 不是必需的但它是一个选项。

此外，如果你`.vcxproj`包含`<RequiredBundles>$(RequiredBundles);Microsoft.Windows.CppWinRT</RequiredBundles>`，则可以以便可以生成而无需移除C++安装 WinRT VSIX 扩展。

## <a name="custom-types-in-the-cwinrt-projection"></a>C++/WinRT 投影中的自定义类型
在你C++/WinRT 编程，您可以使用标准C++语言功能和[标准C++数据类型和C++/WinRT](std-cpp-data-types.md)&mdash;包括一些C++标准库的数据类型。 但你还会在投影中发现一些自定义数据类型，并且可以选择使用它们。 例如，我们使用 [C++/WinRT 入门](get-started.md)中快速入门代码示例中的 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)。

[**winrt::com_array** ](/uwp/cpp-ref-for-winrt/com-array)是您可能要在某个时间点使用的另一种类型。 但你不太可能直接使用 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) 之类的类型。 或者，你可能选择不使用它，以便在等效类型出现在 C++ 标准库中时不用更改任何代码。

> [!WARNING]
> 如果你仔细研究 C++/WinRT Windows 命名空间标头，你可能也会发现一些类型。 一个示例是 **winrt::param::hstring**，另外还有一些示例。 这些单纯用于优化输入参数的绑定，他们实现了显著的性能改进，并使相大多数调用模式“只”为相关的标准 C++ 类型和容器工作。 在增加最大价值的情况下，这些类型只被投影使用过。 它们被高度优化且不用于一般用途；请不要尝试自行使用。 也不应该使用来自 `winrt::impl` 命名空间的任何内容，因为这些属于实现类型，因此可能更改。 你应该继续使用标准类型，或来自 [winrt 命名空间](/uwp/cpp-ref-for-winrt/winrt)的类型。

## <a name="important-apis"></a>重要的 API
* [winrt::hstring 结构](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 命名空间](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相关主题
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/ WinRT Visual Studio 扩展 (VSIX)](https://aka.ms/cppwinrt/vsix)
* [C++/WinRT 入门](get-started.md)
* [标准 C++ 数据类型和 C++/WinRT](std-cpp-data-types.md)
* [字符串中的处理C++/WinRT](strings.md)
* [Windows 的 UWP Api](https://docs.microsoft.com/uwp/api/)

---
description: 对 C++/WinRT（一种适用于 Windows 运行时 API 的标准 C++ 语言投影）的介绍。
title: C++/WinRT 简介
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 简介
ms.localizationpriority: medium
ms.openlocfilehash: ddf2cd876ac629f4cb3c49e349e43dee8fdb0c7a
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730299"
---
# <a name="introduction-to-cwinrt"></a>C++/WinRT 简介
&nbsp;
> [!VIDEO https://www.youtube.com/embed/X41j_gzSwOY]

&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT 是 Windows 运行时 (WinRT) API 的完全标准新式 C++17 语言投影，以基于标头文件的库的形式实现，旨在为你提供对新式 Windows API 的一流访问。 利用 C++/WinRT，你可以采用任何符合标准的 C++17 编译器创作和使用 Windows 运行时 API。 Windows SDK 包含 C++/WinRT；它已在版本 10.0.17134.0（Windows 10，版本 1803）中引用。

C++/WinRT 是 Microsoft 推荐的用于替代 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) 语言投影和 [Windows 运行时 C++ 模板库 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live) 的替代品。 [有关 C++/WinRT 的主题](index.md#topics-about-cwinrt)的完整列表介绍了如何与 C++/CX 和 WRL 进行互操作，以及如何通过它们进行移植。

> [!IMPORTANT]
> C++/WinRT 最需要注意的某些重要内容在[针对 C++/WinRT 的 SDK 支持](#sdk-support-for-cwinrt)和[针对 C++/WinRT、XAML、VSIX 扩展和 NuGet 包的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)部分进行了说明。

## <a name="language-projections"></a>语言投影
Windows 运行时基于组件对象模型 (COM) API，根据设计，可通过语言投影  访问它。 投影将隐藏 COM 详细信息，并为给定语言提供更自然的编程体验。

### <a name="the-cwinrt-language-projection-in-the-windows-runtime-api-reference-content"></a>Windows 运行时 API 引用内容中的 C++/WinRT 语言投影
浏览 [Windows 运行时 API](https://docs.microsoft.com/uwp/api/) 时，请单击右上角的“语言”  组合框，然后选择“C++/WinRT”  以查看 API 语法块（当它们在 C++/WinRT 语言投影中出现时）。

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>针对 C++/WinRT、XAML、VSIX 扩展和 NuGet 包的 Visual Studio 支持
若要获取 Visual Studio 支持，需要 Visual Studio 2019 或 Visual Studio 2017（至少需要版本 15.6；建议至少使用 15.7）。 从 Visual Studio 安装程序中，安装“通用 Windows 平台开发”  工作负荷。 在“安装详细信息”   >   “通用 Windows 平台开发”中，选中“C++ (v14x) 通用 Windows 平台工具”  选项（如果尚未这样做）。 另外，请在 Windows 的“设置”   > “更新和安全”\&   >   “面向开发人员”中选择“开发人员模式”选项而非“旁加载应用”选项。  

虽然我们建议使用最新版 Visual Studio 和 Windows SDK 进行开发，但如果你使用的 C++/WinRT 版本是 10.0.17763.0（Windows 10 版本 1809）之前的 Windows SDK 随附的，则至少需要在项目中使用 Windows SDK 目标版本 10.0.17134.0（Windows 10 版本 1803）才能使用上述 Windows 命名空间标头。

需要从 [Visual Studio Marketplace](https://marketplace.visualstudio.com/) 下载并安装最新版 [C++/WinRT Visual Studio 扩展 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)。

- VSIX 扩展为你提供 Visual Studio 中的 C++/WinRT 项目和项模板，让你可以开始进行 C++/WinRT 开发。
- 另外，它还为你提供 C++/WinRT 投影类型的 Visual Studio 本机调试可视化效果 (natvis)；提供与 C# 调试类似的体验。 Natvis 对于调试版本是自动的。 可以通过定义 WINRT_NATVIS 符号选择加入其发布版本。

用于 C++/WinRT 的 Visual Studio 项目模板在下面的部分介绍。 使用已安装的最新版 VSIX 扩展创建新的 C++/WinRT 项目时，新的 C++/WinRT 项目会自动安装 [Microsoft.Windows.CppWinRT NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)。 **Microsoft.Windows.CppWinRT** NuGet 包提供 C++/WinRT 生成支持（MSBuild 属性和目标），使项目可以在开发计算机和生成代理（在其上仅安装了 NuGet 包，未安装 VSIX 扩展）之间移植。

也可通过手动安装 **Microsoft.Windows.CppWinRT** NuGet 包来转换现有项目。 在安装（或更新到）最新版 VSIX 扩展以后，请在 Visual Studio 中打开现有项目，然后单击“项目”\>“管理 NuGet 包...”   \>“浏览”，在搜索框中键入或粘贴 Microsoft.Windows.CppWinRT  ，在搜索结果中选择该项，然后单击“安装”以安装该项目的包。   添加该包后，你将获得对该项目的 C++/WinRT MSBuild 支持，包括调用 `cppwinrt.exe` 工具。

> [!IMPORTANT]
> 如果项目是使用 1.0.190128.4 之前的 VSIX 扩展版本创建的（或者这些项目在升级后兼容该扩展版本），请参阅[早期版本的 VSIX 扩展](#earlier-versions-of-the-vsix-extension)。 该部分包含有关项目配置的重要信息。在升级项目以使用最新版 VSIX 扩展之前，需了解该信息。

- 由于 C++/WinRT 使用 C++17 标准版中的功能，因此 NuGet 包会在 Visual Studio 中设置项目属性“C/C++” > “语言” > “C++ 语言标准版” > “ISO C++17 标准版(/std:c++17)”。    
- 它还会添加 [/bigobj](/cpp/build/reference/bigobj-increase-number-of-sections-in-dot-obj-file) 编译器选项。
- 它会添加 [/await](/cpp/build/reference/await-enable-coroutine-support) 编译器选项来启用 `co_await`。
- 它会指导 XAML 编译器发出 C++/WinRT codegen。
- 你可能还需要设置“合规模式:  是(/permissive-)”，以便进一步约束代码，使之符合标准。
- 要注意的另一个项目属性是“C/C++”   > “常规”   >   “将警告视为错误”。 请根据喜好将此项设置为“是(/WX)”  或“否(/WX-)”  。 有时候，由 `cppwinrt.exe` 工具生成的源文件会生成警告，除非向其添加实现。

根据上述说明设置系统以后，即可在 Visual Studio 中创建和生成或者打开 C++/WinRT 项目，然后部署它。

从 2.0 版开始，**Microsoft.Windows.CppWinRT** NuGet 包包含 `cppwinrt.exe` 工具。 可以将 `cppwinrt.exe` 工具指向 Windows 运行时元数据 (`.winmd`) 文件，以便生成基于标头文件且可投影  API 的标准 C++ 库，详见“在 C++/WinRT 代码中使用的元数据”。 Windows 运行时元数据 (`.winmd`) 文件提供了描述 Windows 运行时 API 图面的规范方法。 在元数据中指向 `cppwinrt.exe` 后，即可生成一个库，该库可以与在第二方或第三方 Windows 运行时组件中实现的或在你自己的应用程序中实现的任何运行时类配合使用。 有关详细信息，请参阅[通过 C++/WinRT 使用 API](consume-apis.md)。

通过 C++/WinRT，你还可以使用标准 C++ 实现自己的运行时类，而不必求助于 COM 样式的编程。 对于运行时类，你只需在 IDL 文件中描述类型，`midl.exe` 和 `cppwinrt.exe` 将为你生成实现样板源代码文件。 也可以从 C++/WinRT 基类派生，这样就可以只实现接口。 有关详细信息，请参阅[使用 C++/WinRT 创作 API](author-apis.md)。

如需 `cppwinrt.exe` 工具的自定义选项的列表，请通过项目属性进行设置，详见 Microsoft.Windows.CppWinRT NuGet 包[自述文件](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing)。

可以通过项目中安装的 **Microsoft.Windows.CppWinRT** NuGet 包来标识使用 C++/WinRT MSBuild 支持的项目。

下面是由 VSIX 扩展提供的 Visual Studio 项目模板。

### <a name="blank-app-cwinrt"></a>空白应用 (C++/WinRT)
通用 Windows 平台 (UWP) 应用的项目模板，具有 XAML 用户界面。

Visual Studio 提供了 XAML 编译器支持，用于从每个 XAML 标记文件后面的接口定义语言 (IDL) (`.idl`) 文件生成实现和标头存根。 在 IDL 文件中，定义需要在应用的 XAML 页面中引用的任何本地运行时类，然后构建项目一次，以便在 `Generated Files` 中生成实现模板并在 `Generated Files\sources` 中生成存根类型定义。 然后使用这些存根类型定义作为参考来实现本地运行时类。 请参阅[将运行时类重构到 Midl 文件 (.idl) 中](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)。

用于 C++/WinRT 的 Visual Studio 2019 中的 XAML 设计图面支持与 C# 类似。 在 Visual Studio 2019 中，可以使用“属性”窗口的“事件”选项卡   在 C++/WinRT 项目中添加事件处理程序。 也可将事件处理程序手动添加到代码中&mdash;有关详细信息，请参阅[在 C++/WinRT 中使用委托处理事件](handle-events.md)。

### <a name="core-app-cwinrt"></a>核心应用 (C++/WinRT)
通用 Windows 平台 (UWP) 应用的项目模板，不使用 XAML。

相反，它使用 Windows.ApplicationModel.Core 命名空间的 C++/WinRT Windows 命名空间标头。 构建并运行后，单击空白空间以添加彩色方块；然后单击彩色方块以拖动它。

### <a name="windows-console-application-cwinrt"></a>Windows 控制台应用程序 (C++/WinRT)
适用于 Windows 桌面版的 C++/WinRT 客户端应用程序的项目模板，具有控制台用户界面。

### <a name="windows-desktop-application-cwinrt"></a>Windows 桌面应用程序 (C++/WinRT)
适用于 Windows 桌面版的 C++/WinRT 客户端应用程序的项目模板，可在 Win32 **MessageBox** 中显示 Windows 运行时 [Windows.Foundation.Uri](/uwp/api/windows.foundation.uri)。

### <a name="windows-runtime-component-cwinrt"></a>Windows 运行时组件 (C++/WinRT)
组件的项目模板；通常可以在通用 Windows 平台 (UWP) 中使用。

此模板演示了 `midl.exe` > `cppwinrt.exe` 工具链：首先从 IDL 生成 Windows 运行时元数据 (`.winmd`)，然后从 Windows 运行时元数据生成实现和标头存根。

在 IDL 文件中，在组件、组件的默认接口和组件实现的任何其他接口中定义运行时类。 构建项目一次以生成 `module.g.cpp` 和 `module.h.cpp`，同时生成 `Generated Files` 中的实现模板和 `Generated Files\sources` 中的存根类型定义。 然后使用这些存根类型定义作为参考，在组件中实现运行时类。 请参阅[将运行时类重构到 Midl 文件 (.idl) 中](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)。

将生成的 Windows 运行时组件二进制文件及其 `.winmd` 与使用它们的 UWP 应用绑定。

## <a name="earlier-versions-of-the-vsix-extension"></a>早期版本的 VSIX 扩展
建议安装（或更新到）最新版 [VSIX 扩展](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)。 默认情况下，它已配置为自行更新。 如果你这样做，并且项目是使用 1.0.190128.4 之前的 VSIX 扩展版本创建的，则请参阅此部分，因为其中包含重要信息，介绍了如何升级这些项目，使之兼容新版本。 当然，如果你不进行更新，此部分的内容仍然对你有用。

就支持的 Windows SDK 和 Visual Studio 版本以及 Visual Studio 配置来说，上面的[针对 C++/WinRT、XAML、VSIX 扩展和 NuGet 包的 Visual Studio 支持](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)部分的信息适用于早期版本的 VSIX 扩展。 下面的内容介绍了在使用早期版本创建项目（或将项目升级，使之兼容早期版本）的情况下，在行为和配置方面存在的重要差异。

### <a name="created-earlier-than-101810022"></a>在 1.0.181002.2 之前创建
如果项目是使用 1.0.181002.2 之前的 VSIX 扩展版本创建的，则 C++/WinRT 生成支持已内置到该 VSIX 扩展版本中。 项目在 `.vcxproj` 文件中设置 `<CppWinRTEnabled>true</CppWinRTEnabled>` 属性。

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

可通过手动安装 **Microsoft.Windows.CppWinRT** NuGet 包来升级项目。 在安装（或升级到）最新版 VSIX 扩展以后，请在 Visual Studio 中打开项目，然后单击“项目”\>“管理 NuGet 包...”   \>“浏览”，在搜索框中键入或粘贴 **Microsoft.Windows.CppWinRT**，在搜索结果中选择该项，然后单击“安装”以安装项目的包。  

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>使用 1.0.181002.2 和 1.0.190128.3 之间的版本创建（或升级到该版本）
如果项目是使用 1.0.181002.2 和 1.0.190128.3 之间的 VSIX 扩展版本（含 1.0.181002.2 和 1.0.190128.3）创建的，则 **Microsoft.Windows.CppWinRT** NuGet 包已通过项目模板自动安装到项目中。 另外，你可能已经升级了旧项目，因此可以使用此范围中的 VSIX 扩展版本。 如果你这样做了，那么，由于生成支持也存在于此范围的 VSIX 扩展版本中，升级的项目可能安装了 **Microsoft.Windows.CppWinRT** NuGet 包，也可能没有。

若要升级项目，请按上一部分的说明操作，确保项目安装了 **Microsoft.Windows.CppWinRT** NuGet 包。

### <a name="invalid-upgrade-configurations"></a>无效的升级配置
使用最新版 VSIX 扩展时，项目在尚未安装 **Microsoft.Windows.CppWinRT** NuGet 包的情况下不能有 `<CppWinRTEnabled>true</CppWinRTEnabled>` 属性。 使用此配置的项目会出现一个生成错误消息：“C++/WinRT VSIX 不再提供项目生成支持。  请添加对 Microsoft.Windows.CppWinRT Nuget 包的项目引用。”

如上所述，现在需要在 C++/WinRT 项目中安装 NuGet 包。

由于 `<CppWinRTEnabled>` 元素现已过时，你可以选择编辑 `.vcxproj`，删除该元素。 严格说来这不是必需的，只是一种选择。

另外，如果 `.vcxproj` 包含 `<RequiredBundles>$(RequiredBundles);Microsoft.Windows.CppWinRT</RequiredBundles>`，则可将其删除，这样就可以在不需安装 C++/WinRT VSIX 扩展的情况下进行构建。

## <a name="sdk-support-for-cwinrt"></a>对 C++/WinRT 的 SDK 支持
从版本 10.0.17134.0（Windows 10 版本 1803）起，Windows SDK 包含基于标头文件的标准 C++ 库，因此可以使用第一方 Windows API（Windows 命名空间中的 Windows 运行时 API），但现在其存在只是出于兼容性原因。 这些标头位于文件夹 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt` 中。 从 Windows SDK 版本 10.0.17763.0（Windows 10 版本 1809）开始，将会在项目的 *$(GeneratedFilesDir)* 文件夹中生成这些标头。

Windows SDK 也附带 `cppwinrt.exe` 工具，这同样是出于兼容性原因。 但是，建议你改为安装并使用 **Microsoft.Windows.CppWinRT** NuGet 包随附的最新版 `cppwinrt.exe`。 该包和 `cppwinrt.exe` 在上述部分进行了介绍。

## <a name="custom-types-in-the-cwinrt-projection"></a>C++/WinRT 投影中的自定义类型
在 C++/WinRT 编程中，你可以使用标准 C++ 语言功能以及[标准 C++ 数据类型和 C++/WinRT](std-cpp-data-types.md)&mdash;包括一些 C++ 标准库数据类型。 但你还会在投影中发现一些自定义数据类型，并且可以选择使用它们。 例如，我们使用 [C++/WinRT 入门](get-started.md)中快速入门代码示例中的 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)。

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array) 是你可能在某个时间使用的另一个类型。 但你不太可能直接使用 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) 之类的类型。 或者，你可以选择不使用它，这样就可以在等效类型出现在 C++ 标准库中时不用更改任何代码。

> [!WARNING]
> 如果仔细研究 C++/WinRT Windows 命名空间标头，你可能也会发现一些类型。 一个示例是 **winrt::param::hstring**，另外还有一些示例。 这些类型只是为了优化输入参数的绑定，它们实现了显著的性能改进，并使得大多数调用模式“只适用于”相关的标准 C++ 类型和容器。 在增加最大价值的情况下，这些类型只被投影使用过。 它们被高度优化且不用于一般用途；请不要尝试自行使用。 也不应该使用来自 `winrt::impl` 命名空间的任何内容，因为这些属于实现类型，因此可能更改。 你应该继续使用标准类型，或来自 [winrt 命名空间](/uwp/cpp-ref-for-winrt/winrt)的类型。
>
> 另请参阅[将参数传递到 ABI 边界](/windows/uwp/cpp-and-winrt-apis/pass-parms-to-abi)。

## <a name="important-apis"></a>重要的 API
* [winrt::hstring 结构](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 命名空间](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相关主题
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT Visual Studio 扩展 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)
* [C++/WinRT 入门](get-started.md)
* [标准 C++ 数据类型和 C++/WinRT](std-cpp-data-types.md)
* [C++/WinRT 中的字符串处理](strings.md)
* [Windows 运行时 API](https://docs.microsoft.com/uwp/api/)

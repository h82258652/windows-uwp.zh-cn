---
description: 本指南将帮助您直接在 WPF 和 Windows 窗体应用程序中创建基于 Fluent 的 UWP UI
title: 桌面应用中的 UWP 控件
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10、uwp、windows 窗体、wpf、xaml 孤岛
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: bd49417d110759dc9fec4ff4c9003e842bf1d7bb
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643349"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>在桌面应用中托管 UWP XAML 控件 (XAML 孤岛)

从 Windows 10 版本1903开始, 你可以使用一个名为*XAML 孤岛*的功能在非 UWP 桌面应用程序中托管 UWP 控件。 利用此功能, 你可以使用仅可通过 UWP 控件获取的最新 Windows 10 UI 功能增强C++现有 WPF、Windows 窗体和 Win32 应用程序的外观和功能。 这意味着, 可以使用 UWP 功能, 如[Windows 墨迹](/windows/uwp/design/input/pen-and-stylus-interactions)和控件, 这些功能支持现有 WPF、Windows 窗体和C++ Win32 应用程序中的[熟知设计系统](/windows/uwp/design/fluent-design-system/index)。

你可以承载从[Windows](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)派生的任何 UWP 控件, 其中包括:

* Windows SDK 或 WinUI 库提供的任何第三方 UWP 控件。
* 任何自定义 UWP 控件 (例如, 包含多个可一起工作的 UWP 控件的用户控件)。 您必须具有自定义控件的源代码, 以便可以使用您的应用程序进行编译。

从根本上讲, 使用*UWP xaml 宿主 API*创建了 xaml 孤岛。 此 API 包含 Windows 10 版本 1903 SDK 中引入的几个 Windows 运行时类和 COM 接口。 我们还在[Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)中提供一组 XAML 岛 .net 控件, 这些控件在内部使用 UWP XAML 宿主 API 并为 WPF 和 Windows 窗体应用提供更方便的开发体验。

使用 XAML 孤岛的方式取决于您的应用程序类型和要承载的 UWP 控件类型。

> [!NOTE]
> 如果你有关于 XAML 孤岛的反馈, 请在[Microsoft 工具包](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)存储库中创建一个新问题, 并在此处留下你的意见。 如果您想要私下提交您的反馈, 则可以将其XamlIslandsFeedback@microsoft.com发送到。 你的见解和方案对我们至关重要。

## <a name="wpf-and-windows-forms-applications"></a>WPF 和 Windows 窗体应用程序

建议 WPF 和 Windows 窗体应用程序使用 Windows 社区工具包中提供的 XAML 岛 .NET 控件。 这些控件提供一个对象模型, 用于模拟 (或提供对) 相应 UWP 控件的属性、方法和事件的访问。 它们还处理键盘导航和布局更改等行为。

WPF 和 Windows 窗体应用程序有两组 XAML 岛控件:*包装控件*和*宿主控件*。 从 Windows 10 版本 1903, 这些控件[作为开发人员预览版提供](#feature-roadmap)。

### <a name="wrapped-controls"></a>包装控件

WPF 和 Windows 窗体应用程序可以使用包装特定 UWP 控件的界面和功能的 XAML 岛控件选择。 可以直接将这些控件添加到 WPF 或 Windows 窗体项目的设计图面上, 然后在设计器中将其与任何其他 WPF 或 Windows 窗体控件一起使用。

下面的已包装 UWP 控件目前在 Windows 社区工具包中可用。 

| 控件 | 支持的最低操作系统 | 描述 |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 版本 1903 | 为 Windows 窗体或 WPF 桌面应用程序中的基于 Windows Ink 的用户交互提供一个表面和相关的工具栏。 |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 版本 1903 | 嵌入用于在 Windows 窗体或 WPF 桌面应用程序中流式传输和呈现媒体内容 (如视频) 的视图。 |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10 版本 1903 | 使您能够在 Windows 窗体或 WPF 桌面应用程序中显示符号或照片的照片。 |

有关演示如何使用包装的 UWP 控件的演练, 请参阅[在 WPF 应用中托管标准 UWP 控件](host-standard-control-with-xaml-islands.md)。

### <a name="host-controls"></a>主机控件

对于超出可用包装控件所涵盖的方案, WPF 和 Windows 窗体应用程序还可以使用 Windows 社区工具包中提供的[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控件。

| 控件 | 支持的最低操作系统 | 描述 |
|-----------------|-------------------------------|-------------|
| [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10 版本 1903 | 可以承载从[Windows](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)中派生的任何 uwp 控件, 包括 Windows SDK 提供的所有第一方 uwp 控件以及自定义控件。 |

有关演示如何使用**WindowsXamlHost**控件的演练, 请参阅[在 wpf 应用程序中托管标准 uwp 控件](host-standard-control-with-xaml-islands.md)和[使用 XAML 孤岛在 wpf 应用程序中托管自定义 uwp 控件](host-custom-control-with-xaml-islands.md)。

> [!NOTE]
> 仅在 WPF 和面向 .NET Core 3 的 Windows 窗体应用中, 才支持使用**WindowsXamlHost**控件托管自定义 UWP 控件。 以 .NET Framework 或 .NET Core 3 为目标的应用程序支持承载 Windows SDK 提供的第一方 UWP 控件。

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>将项目配置为使用 XAML 岛 .NET 控件

XAML 孤岛 .NET 控件需要 Windows 10 版本1903或更高版本。 若要使用这些控件, 请安装下面列出的一个 NuGet 包。 这些包提供了使用 XAML 岛包装控件和宿主控件所需的一切, 还包括其他相关的 NuGet 包, 这些包也是必需的。

| 控件类型 | NuGet 包  | 相关文章 |
|-----------------|----------------|---------------------|
| [包装控件](#wrapped-controls) | 版本 6.0.0-preview7 或更高版本的这些包: <ul><li>WPF["Microsoft 工具包"。](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows 窗体:["Microsoft 工具包"。](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [在 WPF 应用程序中托管标准 UWP 控件](host-standard-control-with-xaml-islands.md)  |
| [主机控件](#host-controls) | 版本 6.0.0-preview7 或更高版本的这些包: <ul><li>WPF[XamlHost (& e)](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows 窗体:[XamlHost (& e)](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [在 WPF 应用程序中托管标准 UWP 控件](host-standard-control-with-xaml-islands.md)<br/>[在 WPF 应用程序中托管自定义 UWP 控件](host-custom-control-with-xaml-islands.md)  |

请注意以下详细信息:

* 宿主控件包也包含在包装的控件包中。 如果要使用这两组控件, 可以安装已包装的控件包。

* 如果承载的是自定义 UWP 控件, 则 WPF 或 Windows 窗体项目必须面向 .NET Core 3。 面向 .NET Framework 的应用程序中不支持承载自定义 UWP 控件。 还需要执行一些附加步骤来引用自定义控件。 有关详细信息, 请参阅[使用 XAML 孤岛在 WPF 应用中托管自定义 UWP 控件](host-custom-control-with-xaml-islands.md)。

* 这些说明的早期版本已将`maxversiontested`元素添加到 WPF 或 Windows 窗体项目中的应用程序清单。 只要你使用以上列出的 NuGet 包的最新预览版, 就不再需要将此元素添加到清单中。

### <a name="architecture-of-xaml-island-net-controls"></a>XAML 孤岛 .NET 控件的体系结构

下面简要介绍了不同类型的 XAML 岛控件如何组织在 UWP XAML 宿主 API 的顶层。

![托管控件体系结构](images/xaml-islands/host-controls.png)

此图表底部所示的 API 随 Windows SDK 提供。 已包装的控件和主机控件可通过 Windows 社区工具包中的 NuGet 包获得。

### <a name="web-view-controls"></a>Web 视图控件

Windows 社区工具包还提供了以下用于在 WPF 中承载 web 内容和 Windows 窗体应用程序的 .NET 控件。 这些控件通常用于类似的桌面应用现代化方案, 作为 XAML 岛控件, 并在与 XAML 岛控件相同的[Microsoft 工具包](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32)存储库存储库中进行维护。

| 控件 | 支持的最低操作系统 | 描述 |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10 版本 1803 | 使用 Microsoft Edge 呈现引擎显示 web 内容。 |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | 提供与更多操作系统版本兼容的**Web 视图**版本。 此控件使用 Microsoft 边缘呈现引擎在 Windows 10 版本1803及更高版本上显示 web 内容, 并使用 Internet Explorer 呈现引擎在早期版本的 Windows 10、Windows 8.x 和 Windows 7 上显示 web 内容。 |

若要使用这些控件, 请安装以下 NuGet 包之一:

* WPF[Microsoft 工具包. node.js。](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows 窗体:["Microsoft 工具包"。](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>C++Win32 应用程序

Win32 应用程序中C++不支持 XAML 岛 .net 控件。 这些应用程序必须使用 Windows 10 SDK (版本1903及更高版本) 提供的*UWP XAML 宿主 API* 。

UWP XAML 宿主 API 包含若干 Windows 运行时类和 COM 接口, C++ Win32 应用程序可使用这些接口来承载派生自[WINDOWS](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)的任何 UWP 控件。 可以在应用程序中具有关联窗口句柄 (HWND) 的任何 UI 元素中承载 UWP 控件。 有关此 API 的详细信息, 包括先决条件, 请参阅[ C++在 Win32 应用中使用 UWP XAML 宿主 API](using-the-xaml-hosting-api.md)。

> [!NOTE]
> Windows 社区工具包中的已包装控件和主机控件在内部使用 UWP XAML 宿主 API, 并实现你在自行使用 UWP XAML 宿主 API 时需要自行处理的所有行为, 包括键盘导航和布局更改。 对于 WPF 和 Windows 窗体应用程序, 我们强烈建议您直接使用这些控件而不是 UWP XAML 托管 API, 因为它们抽象地消除了使用 API 的许多实现细节。

## <a name="feature-roadmap"></a>功能路线图

截至 Windows 10 版本 1903, Windows 社区工具包中的 XAML 岛 .NET 控件仍处于开发人员预览版中, 直到版本1.0 版本的控件可用为止。

* .NET Framework 4.6.2 和更高版本的控件的1.0 版将在[工具包的6.0 版本](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones)中发布。
* 适用于 .NET Core 3 的控件版本1.0 计划在更高版本的工具包中。
* 如果要尝试对 .NET Framework 和 .NET Core 3 的这些控件的版本1.0 版本进行最新预览, 请参阅[UWP 社区工具包](https://dotnet.myget.org/gallery/uwpcommunitytoolkit)库中的版本 6.0.0-preview7 (或更高版本) NuGet 包。

有关更多详细信息，请参阅[这篇博客文章](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap)。

## <a name="additional-resources"></a>其他资源

有关使用 XAML 孤岛的更多背景信息和教程, 请参阅以下文章和资源:

* [现代化 WPF 应用教程](modernize-wpf-tutorial.md):本教程提供了使用 Windows 社区工具包中的包装控件和主机控件向现有 WPF 业务线应用程序添加 UWP 控件的分步说明。 本教程包含 WPF 应用程序的完整代码, 以及该过程中每个步骤的详细说明。
* [XAML 孤岛 v1-更新和路线图](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap):此博客文章讨论了有关 XAML 孤岛的许多常见问题, 并提供详细的开发路线图。

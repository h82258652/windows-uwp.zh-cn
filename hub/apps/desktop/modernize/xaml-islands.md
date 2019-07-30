---
description: 本指南将帮助您直接在 WPF 和 Windows 窗体应用程序中创建基于 Fluent 的 UWP UI
title: 桌面应用中的 UWP 控件
ms.date: 07/26/2019
ms.topic: article
keywords: windows 10、uwp、windows 窗体、wpf、xaml 孤岛
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 560d339476ef3cd45f30bfc678661fb0a4a11ee1
ms.sourcegitcommit: f6af7aeb8506379a184207035c8e43288cb31453
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2019
ms.locfileid: "68601536"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>在桌面应用中托管 UWP XAML 控件 (XAML 孤岛)

从 Windows 10 版本1903开始, 你可以使用一个名为*XAML 孤岛*的功能在非 UWP 桌面应用程序中托管 UWP 控件。 利用此功能, 你可以使用仅可通过 UWP 控件获取的最新 Windows 10 UI 功能来增强现有桌面应用程序的外观和功能。 这意味着, 可以使用 UWP 功能, 如[Windows 墨迹](/windows/uwp/design/input/pen-and-stylus-interactions)和控件, 这些功能支持现有 WPF、Windows 窗体和C++ Win32 应用程序中的[熟知设计系统](/windows/uwp/design/fluent-design-system/index)。

我们提供了多种方法来使用 WPF、Windows 窗体和C++ Win32 应用程序中的 XAML 孤岛, 具体取决于所使用的技术或框架。 

> [!NOTE]
> 如果你有关于 XAML 孤岛的反馈, 请在[Microsoft 工具包](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)存储库中创建一个新问题, 并在此处留下你的意见。 如果您想要私下提交您的反馈, 则可以将其XamlIslandsFeedback@microsoft.com发送到。 你的见解和方案对我们至关重要。

## <a name="how-do-xaml-islands-work"></a>XAML 孤岛如何工作？

从 Windows 10 版本1903开始, 我们提供了两种方法来使用 WPF、Windows 窗体和C++ Win32 应用程序中的 XAML 孤岛:

* Windows SDK 提供了若干 Windows 运行时类和 COM 接口, 应用程序可以使用这些接口来承载派生自[**Windows**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)的任何 UWP 控件。 这些类和接口共同称为*UWP XAML 宿主 API*, 它们使你能够在应用程序中将 uwp 控件托管为关联的窗口句柄 (HWND)。 有关此 API 的详细信息, 请参阅[使用 XAML 宿主 API](using-the-xaml-hosting-api.md)。

* [Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)还提供适用于 WPF 和 Windows 窗体的其他 XAML 岛控件。 这些控件在内部使用 UWP XAML 宿主 API, 并实现你在自行使用 UWP XAML 托管 API 时需要自行处理的所有行为, 包括键盘导航和布局更改。 对于 WPF 和 Windows 窗体应用程序, 我们强烈建议您直接使用这些控件而不是 UWP XAML 托管 API, 因为它们抽象地消除了使用 API 的许多实现细节。 请注意, 从 Windows 10 版本 1903, 这些控件[作为开发人员预览版提供](#feature-roadmap)。

> [!NOTE]
> C++Win32 桌面应用程序必须使用 UWP XAML 宿主 API 来承载 UWP 控件。 Windows 社区工具包中的 XAML 岛控件不适用于C++ Win32 桌面应用程序。

用于 WPF 的 Windows 社区工具包提供了两种类型的 XAML 岛控件, 并 Windows 窗体应用程序:*包装控件*和*宿主控件*。 

### <a name="wrapped-controls"></a>包装控件

WPF 和 Windows 窗体应用程序可以在[Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)中使用所选的包装 UWP 控件。 它们被称为*换行控制*, 因为它们包装特定 UWP 控件的界面和功能。 可以直接将这些控件添加到 WPF 或 Windows 窗体项目的设计图面上, 然后在设计器中将其与任何其他 WPF 或 Windows 窗体控件一起使用。

以下用于实现 XAML 孤岛的已包装 UWP 控件目前可用于 WPF (请参阅["")](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)和 "Windows 窗体应用程序" (请参阅[""](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)).

| 控件 | 支持的最低操作系统 | 描述 |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 版本 1903 | 为 Windows 窗体或 WPF 桌面应用程序中的基于 Windows Ink 的用户交互提供一个表面和相关的工具栏。 |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 版本 1903 | 嵌入用于在 Windows 窗体或 WPF 桌面应用程序中流式传输和呈现媒体内容 (如视频) 的视图。 |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10 版本 1903 | 使您能够在 Windows 窗体或 WPF 桌面应用程序中显示符号或照片的照片。 |

除了 XAML 孤岛的已包装控件外, Windows 社区工具包还提供以下控件, 用于在 WPF 中承载 web 内容 (请[参阅 "Windows 窗体](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)的应用程序) 和应用程序 (请参阅[Microsoft 工具包](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView). node.js. e。

| 控件 | 支持的最低操作系统 | 描述 |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10 版本 1803 | 使用 Microsoft Edge 呈现引擎显示 web 内容。 |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | 提供与更多操作系统版本兼容的**Web 视图**版本。 此控件使用 Microsoft 边缘呈现引擎在 Windows 10 版本1803及更高版本上显示 web 内容, 并使用 Internet Explorer 呈现引擎在早期版本的 Windows 10、Windows 8.x 和 Windows 7 上显示 web 内容。 |

### <a name="host-controls"></a>主机控件

对于超出可用包装控件所涵盖的方案, WPF 和 Windows 窗体应用程序也可以使用[Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)中的[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控件。 此控件可以承载从[**Windows**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)派生的任何 uwp 控件, 包括 Windows SDK 提供的任何 uwp 控件以及自定义用户控件。 此控件支持 Windows 10 Insider preview SDK build 17709 和更高版本。

这些控件在[XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) (适用于 Wpf) 和[XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) (适用于 Windows 窗体) 包中提供了此类控件。 这些包包括在包含已包装控件的[Microsoft 工具包](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)中的 "." 和 "." 和 " [microsoft](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) "。

### <a name="architecture-overview"></a>体系结构概述

下面是这些控件的组织结构速览。

![托管控件体系结构](images/xaml-islands/host-controls.png)

此图表底部所示的 API 随 Windows SDK 提供。 已包装的控件和主机控件可通过 Windows 社区工具包中的 Nuget 包获得。

<span id="requirements" />

## <a name="configure-your-project-to-use-xaml-islands"></a>将项目配置为使用 XAML 孤岛

XAML 孤岛需要 Windows 10 版本1903及更高版本。 若要在你的应用程序中使用 XAML 孤岛, 你必须首先设置你的项目:

1. 修改项目以使用 Windows 运行时 Api。 有关说明, 请参阅[此文](desktop-to-uwp-enhance.md#set-up-your-project)。
2. 在项目中安装其中一个 NuGet 包。 请确保安装版本 6.0.0-preview 6.4 或更高版本的包。
    * WPF安装[Microsoft 工具包](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)。
    * Windows 窗体:["Microsoft 工具包"。](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)
    * C++Win32[XamlApplication (& e)](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication)

> [!NOTE]
> 这些说明的早期版本已将**maxversiontested**元素添加到项目中的应用程序清单。 从 NuGet 包的最新预览版本中, 你不再需要将此元素添加到清单中。

## <a name="feature-roadmap"></a>功能路线图

从 Windows 10 版本1903起, Windows 社区工具包中的包装控件和主机控件仍处于开发人员预览版中, 直到版本1.0 版本的控件可用为止。

* .NET Framework 4.6.2 和更高版本的控件的1.0 版将在[工具包的6.0 版本](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones)中发布。
* 适用于 .NET Core 3 的控件版本1.0 计划在更高版本的工具包中。
* 若要尝试对 .NET Framework 和 .NET Core 3 的这些控件版本1.0 版本进行最新预览, 请参阅[UWP 社区工具包](https://dotnet.myget.org/gallery/uwpcommunitytoolkit)库中的**6.0.0-preview 6.4** NuGet 包。

有关更多详细信息，请参阅[这篇博客文章](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap)。

## <a name="additional-resources"></a>其他资源

有关使用 XAML 孤岛的更多背景信息和教程, 请参阅以下文章和资源:

* [现代化 WPF 应用教程](modernize-wpf-tutorial.md):本教程提供了使用 Windows 社区工具包中的包装控件和主机控件向现有 WPF 业务线应用程序添加 UWP 控件的分步说明。 本教程包含 WPF 应用程序的完整代码, 以及该过程中每个步骤的详细说明。
* [XAML 孤岛 v1-更新和路线图](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap):此博客文章讨论了有关 XAML 孤岛的许多常见问题, 并提供详细的开发路线图。

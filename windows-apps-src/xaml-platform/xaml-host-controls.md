---
author: mcleanbyron
description: 本指南将帮助您直接在 WPF 和 Windows 窗体应用程序中创建基于 Fluent 的 UWP UI
title: 桌面应用程序中的 UWP 控件
ms.author: mcleans
ms.date: 09/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp, windows forms, wpf
keywords: windows 10, uwp, windows 窗体, wpf
ms.localizationpriority: medium
ms.openlocfilehash: b9757466502283c673c7b2106b4a7775be412faf
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2018
ms.locfileid: "5167927"
---
# <a name="uwp-controls-in-desktop-applications"></a>桌面应用程序中的 UWP 控件

> [!NOTE]
> 作为开发人员预览版当前可用的 Api 和本文中讨论的控件。 尽管我们鼓励你试用它们在原型代码中现在，我们不建议你使用它们在生产代码中这一次。 这些 Api 和控件将继续成熟并在将来稳定的 Windows 版本。 Microsoft 对于此处提供的信息不作任何明示或默示的担保。

Windows 10 现在可以在非 UWP 桌面应用程序中使用 UWP 控件，以便你可以增强的外观、 体验和功能的现有桌面应用程序将仅可通过 UWP 控件的最新 Windows 10 UI 功能。 这意味着，你可以使用 UWP 功能，例如[Windows Ink](../design/input/pen-and-stylus-interactions.md)和[Fluent 设计系统](../design/fluent-design-system/index.md)支持你的现有 WPF、 Windows 窗体和 c + + Win32 应用程序中的控件。 此开发人员方案有时称为*XAML 群岛*。

我们提供几种方法，用于在 WPF、 Windows 窗体和 c + + Win32 应用程序，具体取决于技术或框架你使用的 XAML 群岛。

## <a name="wrapped-controls"></a>包装的控件

在[Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)，WPF 和 Windows 窗体应用程序可以使用所选的包装 UWP 控件。 我们称为这些控件*包装控件*因为它们换行的接口和特定 UWP 控件的功能。 你可以直接在 WPF 或 Windows 窗体项目的设计图面添加这些控件，并像使用任何其他 WPF 或 Windows 窗体控件中在设计器中使用它们。

> [!NOTE]
> 包装的控件不可用于 c + + Win32 桌面应用程序。 这些类型的应用程序必须使用[UWP XAML 托管 API](#uwp-xaml-hosting-api)。

以下包装的 UWP 控件是当前可用的 WPF 和 Windows 窗体应用程序。 Windows 社区工具包未来版本计划详细包装的 UWP 控件。

| 控件 | 最低受支持的操作系统 | 说明 |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10 版本 1803 | 使用 Microsoft Edge 呈现引擎来显示 web 内容。 |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | 提供的**web 视图**与多个操作系统版本兼容的版本。 此控件使用 Microsoft Edge 呈现引擎，以显示在 Windows 10 版本 1803年及更高版本，web 内容和 Internet Explorer 呈现引擎，以显示 web 内容较早版本的 Windows 10，Windows 8.x 和 Windows 7。 |
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 Insider Preview SDK 生成 17709 | 为 surface 和相关的工具栏提供 Windows 窗体或 WPF 桌面应用程序中的基于 Windows Ink 的用户交互。 |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 Insider Preview SDK 生成 17709 | 嵌入流式处理和呈现媒体内容，例如 Windows 窗体或 WPF 桌面应用程序中的视频的视图。 |

## <a name="host-controls"></a>主机控件

除涵盖可用包装控件的情况下，WPF 和 Windows 窗体应用程序还可以使用[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控件在[Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)。 此控件可以承载派生自[**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)，包括由 Windows SDK 以及自定义用户控件提供的任何 UWP 控件的任何 UWP 控件。 此控件支持 Windows 10 Insider Preview SDK 生成 17709 及更高版本。

> [!NOTE]
> 主机控件不可用于 c + + Win32 桌面应用程序。 这些类型的应用程序必须使用[UWP XAML 托管 API](#uwp-xaml-hosting-api)。

## <a name="uwp-xaml-hosting-api"></a>UWP XAML 托管 API

如果你有了 c + + Win32 应用程序，你可以使用*UWP XAML 托管 API*来托管你有一个关联的窗口句柄 (HWND) 的应用程序中的任何 UI 元素中派生[**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)任何 UWP 控件。 在 Windows 10 Insider Preview SDK 生成 17709 引入了此 API。 有关使用此 API 的详细信息，请参阅[使用托管 API 中的桌面应用程序的 XAML](using-the-xaml-hosting-api.md)。

> [!NOTE]
> C + + Win32 桌面应用程序必须使用 UWP XAML 到托管 UWP 控件中承载 API。 包装的控件和主机控件不可用针对这些类型的应用程序。 对于 WPF 和 Windows 窗体应用程序，我们建议你使用的包装的控件和主机控件而不是 UWP XAML 在 Windows 社区工具包中托管 API。 这些控件使用 UWP XAML 内部托管 API，并且提供更简单的开发体验。 但是，你可以使用 UWP XAML 托管 API 直接在 WPF 和 Windows 窗体应用程序中，如果你选择。

## <a name="architecture-overview"></a>体系结构概述

下面是这些控件的组织结构速览。 此图表中使用的名称可能发生更改。  

![托管控件体系结构](images/host-controls.png)

此图表底部所示的 API 随 Windows SDK 提供。 将添加到设计器中的控件在 Windows 社区工具包中作为 Nuget 程序包提供。

这些新控件存在一些限制，因此在使用它们之前，请花一点时间了解尚不支持什么功能，以及什么功能仅仅作为临时的变通方法。

## <a name="limitations"></a>限制

### <a name="whats-supported"></a>支持的功能

大多数情况下，除非下表中明确排除，否则支持所有功能。

### <a name="whats-supported-only-with-workarounds"></a>仅作为变通方法支持的功能

:heavy_check_mark：在多个窗口中托管多个收件箱控件。 你必须将每个窗口放置在自己的线程中。

:heavy_check_mark：将 ``x:Bind`` 用于托管控件。 你必须在 .NET 标准库中声明数据模型。

:heavy_check_mark: 基于 C# 的第三方控件。 如果有第三方控件的源代码，可以编译该代码。

### <a name="whats-not-yet-supported"></a>尚不支持的功能

:no_entry_sign：跨应用程序和托管控件无缝工作的辅助功能工具。

:no_entry_sign：添加到应用程序中的控件中不包含 Windows 应用包的本地化内容。

:no_entry_sign：应用程序中以 XAML 作出的不包含 Windows 应用包的资产引用。

:no_entry_sign：正确响应 DPI 和缩放更改的控件。

:no_entry_sign：将 **WebView** 控件添加到自定义用户控件（无论是线程上、线程下还是进程外）。

:no_entry_sign: [突出显示](https://docs.microsoft.com/windows/uwp/design/style/reveal) Fluent 效果。

:no_entry_sign：输入控件的内联墨迹书写、@Places 和 @People。

:no_entry_sign：分配加速键。

:no_entry_sign：基于 C++ 的第三方控件。

:no_entry_sign：托管自定义用户控件。

随着我们继续改进在桌面引入 Fluent 的体验，此列表中的项目可能发生更改。  

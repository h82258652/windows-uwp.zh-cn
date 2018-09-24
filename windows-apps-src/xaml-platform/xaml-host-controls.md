---
author: normesta
description: 本指南将帮助您直接在 WPF 和 Windows 窗体应用程序中创建基于 Fluent 的 UWP UI
title: 在桌面应用程序的 UWP 控件
ms.author: normesta
ms.date: 09/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp, windows forms, wpf
keywords: windows 10, uwp, windows 窗体, wpf
ms.localizationpriority: medium
ms.openlocfilehash: d5a4865f403685752225a729bf68abb15237dd90
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "4156580"
---
# <a name="uwp-controls-in-desktop-applications"></a>在桌面应用程序的 UWP 控件

> [!NOTE]
> 作为开发人员预览版当前可用的 Api 和本文中讨论的控件。 尽管我们鼓励你试用它们在原型代码中现在，我们不建议你使用它们在生产代码中这一次。 这些 Api 和控件将继续成熟并在将来稳定的 Windows 版本。 Microsoft 对于此处提供的信息不作任何明示或默示的担保。

Windows 10 的下一个版本中我们正在 UWP 控件引入非 UWP 桌面应用程序，以便你可以增强的外观、 体验和功能的现有桌面应用程序将仅可通过 UWP 控件的最新 Windows 10 UI 功能. 这意味着，你可以使用 UWP 功能，例如[Fluent 设计系统](../design/fluent-design-system/index.md)和[Windows Ink](../design/input/pen-and-stylus-interactions.md)现有 WPF、 Windows 窗体，以及 C/c + + Win32 应用程序中。 此开发人员方案有时称为*XAML 群岛*。

我们将提供几种方法，用于在桌面应用程序，具体取决于技术或框架你使用的 XAML 群岛。

## <a name="wrapped-controls"></a>包装的控件

我们将为在[Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)中的 WPF 和 Windows 窗体应用程序提供包装 UWP 控件的选择。 你可以直接在 WPF 或 Windows 窗体项目的设计图面添加这些控件并将像任何其他 WPF 或 Windows 窗体控件在设计器中。 我们称为这些控件*包装控件*因为它们换行的接口和特定 UWP 控件的功能。

请尝试此操作，现在与[WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/webview)控件在 Windows 社区工具包中。 此控件使用 Microsoft Edge 呈现引擎在 WPF 或 Windows 窗体应用程序中显示 web 内容。  

我们还会规划其他包装的 UWP 控件 WPF 和 Windows 窗体应用程序在将来的版本的 Windows 社区工具包，其中包括：

* **WebViewCompatible**。 此控件是**WebView**与 Windows 10 兼容的版本和以前版本的 Windows。 此控件使用 Microsoft Edge 呈现引擎，以显示在 Windows 10 上的 web 内容和 Internet Explorer 呈现引擎来显示较早版本上的 web 内容。
* **InkCanvas**和**InkToolbar**。 这些控件提供对 Windows 窗体或 WPF 桌面应用程序中的基于 Windows Ink 的用户交互的 surface 和相关的工具栏。
* **MediaPlayerElement**。 此控件嵌入流式处理和呈现媒体内容，例如 Windows 窗体或 WPF 桌面应用程序中的视频的视图。

多个 UWP 的 WPF 换行控件和 Windows 窗体应用程序计划的 Windows 社区工具包未来版本。

> [!NOTE]
> 包装的控件不可用的 C/c + + Win32 桌面应用程序。 这些类型的应用程序必须使用[UWP XAML 托管 API](#uwp-xaml-hosting-api)。

## <a name="host-controls"></a>主机控件

除涵盖可用的包装控件的情况下，WPF 和 Windows 窗体应用程序还可以使用[WindowsXamlHost](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/docs/controls/WindowsXAMLHost.md)控件在[Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)。 此控件可以承载派生自[**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)，包括由 Windows SDK 以及自定义用户控件提供的任何 UWP 控件的任何 UWP 控件。

> [!NOTE]
> 主机控件不可用的 C/c + + Win32 桌面应用程序。 这些类型的应用程序必须使用[UWP XAML 托管 API](#uwp-xaml-hosting-api)。

## <a name="uwp-xaml-hosting-api"></a>UWP XAML 托管 API

如果你有一个 C/c + + WinRT 应用程序，你可以使用*UWP XAML 托管 API*托管派生[**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)你有一个关联的窗口句柄 (HWND) 的应用程序中的任何 UI 元素中的任何 UWP 控件。 有关使用此 API 的详细信息，请参阅[使用托管 API 的桌面应用程序的 XAML](using-the-xaml-hosting-api.md)。

> [!NOTE]
> C/c + + Win32 桌面应用程序必须使用 UWP XAML 到托管 UWP 控件中承载 API。 包装的控件和主机控件不可用针对这些类型的应用程序。 对于 WPF 和 Windows 窗体应用程序，我们建议你使用的包装的控件和主机控件而不是 UWP XAML 在 Windows 社区工具包中托管 API。 这些控件使用 UWP XAML 内部托管 API，并且提供更简单的开发体验。 但是，你可以使用 UWP XAML 托管 API 直接在 WPF 和 Windows 窗体应用程序中，如果你选择。

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

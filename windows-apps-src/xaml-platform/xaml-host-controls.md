---
description: 本指南将帮助您直接在 WPF 和 Windows 窗体应用程序中创建基于 Fluent 的 UWP UI
title: 桌面应用程序中的 UWP 控件
ms.date: 01/11/2019
ms.topic: article
keywords: windows 10, uwp, windows 窗体, wpf
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bf25fea6ca6e8809c12324ae57a42cc712ded2a5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619702"
---
# <a name="uwp-controls-in-desktop-applications"></a>桌面应用程序中的 UWP 控件

> [!NOTE]
> XAML 群岛是目前以开发者预览版形式提供。 尽管我们鼓励您试用这些原型代码中现在，但不建议你使用它们在生产代码中这一次。 这些 Api 和控件将继续成熟和稳定在将来的 Windows 版本。 Microsoft 不对此处提供的信息作任何明示或默示的担保。
>
> 如果您将得到 XAML 群岛反馈，创建中的一个新问题[WindowsCommunityToolkit 存储库](https://github.com/windows-toolkit/WindowsCommunityToolkit/issues)和那里留言。 如果想要私下提交您的反馈，您可以将其发送到XamlIslandsFeedback@microsoft.com。 你的见解和方案是对我们非常重要。

现在，Windows 10，您可以在非 UWP 桌面应用程序中使用 UWP 控件，以便可以增强的外观、 感受和功能的最新的 Windows 10 用户界面功能仅通过 UWP 控件提供的现有桌面应用程序。 这意味着可以使用如 UWP 功能[Windows 墨迹](../design/input/pen-and-stylus-interactions.md)和支持的控件[Fluent 设计系统](../design/fluent-design-system/index.md)现有 WPF、 Windows 窗体和 c + + Win32 应用程序中。 此开发人员有时被称为*XAML 群岛*。

我们提供多种方式来使用 WPF、 Windows 窗体和 c + + Win32 应用程序，具体取决于技术或正在使用的框架中的 XAML 岛。

## <a name="wrapped-controls"></a>已包装的控件

WPF 和 Windows 窗体应用程序可以使用一系列中的已包装 UWP 控件[Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)。 我们将为这些控件*包装控件*因为它们包装的接口和特定的 UWP 控件的功能。 可以直接向你的 WPF 或 Windows 窗体项目的设计图面添加这些控件和类似于任何其他 WPF 或 Windows 窗体控件中在您的设计器中使用它们。

> [!NOTE]
> 已包装的控件不是适用于 c + + Win32 桌面应用程序。 这些类型的应用程序必须使用[UWP XAML 承载 API](#uwp-xaml-hosting-api)。

以下的已包装的 UWP 控件是当前适用于 WPF 和 Windows 窗体应用程序。 计划未来版本的 Windows 社区工具包提供更多包装的 UWP 控件。

| 控件 | 支持的最低 OS | 描述 |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10 版本 1803 | 使用 Microsoft Edge 呈现引擎来显示 web 内容。 |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | 提供的版本**WebView**与多个 OS 版本兼容。 此控件使用 Microsoft Edge 呈现引擎，以显示在 Windows 10 版本 1803年和更高版本，web 内容和 Internet Explorer 呈现引擎，以显示 web 内容在早期版本的 Windows 10 中，Windows 8.x 和 Windows 7。 |
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10，版本 1809年 （内部 17763） | 提供在 Windows 窗体或 WPF 桌面应用程序中的基于 Windows 手写内容的用户交互图面上和相关的工具栏。 |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10，版本 1809年 （内部 17763） | 将嵌入的流式处理和呈现在 Windows 窗体或 WPF 桌面应用程序中的视频等媒体内容的视图。 |
| [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10，版本 1809年 （内部 17763） | 可以在 Windows 窗体或 WPF 桌面应用程序中显示的符号或图像逼真的映射。 |

## <a name="host-controls"></a>主机控件

对于超出可用的已包装控件所涵盖的那些情况下，WPF 和 Windows 窗体应用程序也可以使用[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制[Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)。 此控件可以托管任何 UWP 控件派生自[ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)，包括任何由 Windows SDK 提供的 UWP 控件以及自定义用户控件。 此控件支持 Windows 10 Insider Preview SDK 生成 17709 和更高版本。

> [!NOTE]
> 主机控件不是适用于 c + + Win32 桌面应用程序。 这些类型的应用程序必须使用[UWP XAML 承载 API](#uwp-xaml-hosting-api)。

## <a name="uwp-xaml-hosting-api"></a>托管 API 的 UWP XAML

如果已将 c + + Win32 应用程序，则可以使用*托管 API 的 UWP XAML*承载任何 UWP 控件派生自[ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)中的任何 UI 元素中应用有一个关联的窗口句柄 (HWND) 的应用程序。 在 Windows 10 Insider Preview SDK 生成 17709 中引入此 API。 有关使用此 API 的详细信息，请参阅[使用桌面应用程序中托管 API XAML](using-the-xaml-hosting-api.md)。

> [!NOTE]
> C + + Win32 桌面应用程序必须使用托管 API 主机 UWP 控件到 UWP XAML。 已包装的控件和宿主控件不是可用于这些类型的应用程序。 对于 WPF 和 Windows 窗体应用程序，我们建议你使用已包装的控件和宿主控件而不是 UWP XAML 的 Windows 社区工具包中托管 API。 这些控件使用在内部托管 API UWP XAML，并提供更简单的开发体验。 但是，可以使用托管 API 直接在 WPF 和 Windows 窗体应用程序中，如果你选择 UWP XAML。

## <a name="architecture-overview"></a>体系结构概述

下面是这些控件的组织结构速览。 此图表中使用的名称可能发生更改。  

![托管控件体系结构](images/host-controls.png)

此图表底部所示的 API 随 Windows SDK 提供。 将添加到设计器中的控件在 Windows 社区工具包中作为 Nuget 程序包提供。

这些新控件存在一些限制，因此在使用它们之前，请花一点时间了解尚不支持什么功能，以及什么功能仅仅作为临时的变通方法。

## <a name="limitations"></a>限制

### <a name="whats-supported"></a>支持的功能

大多数情况下，除非下表中明确排除，否则支持所有功能。

### <a name="whats-supported-only-with-workarounds"></a>仅作为变通方法支持的功能

:heavy_check_mark:托管多个窗口内的多个收件箱控件。 你必须将每个窗口放置在自己的线程中。

:heavy_check_mark:使用``x:Bind``托管控件。 你必须在 .NET 标准库中声明数据模型。

:heavy_check_mark:C#-基于第三方控件。 如果有第三方控件的源代码，可以编译该代码。

### <a name="whats-not-yet-supported"></a>尚不支持的功能

:no_entry_sign:在应用程序可以无缝和承载控件的可访问性工具。

:no_entry_sign:在您将添加到不包含 Windows 应用包的应用程序的控件中的本地化的内容。

:no_entry_sign:在 XAML 中进行的应用程序不包含 Windows 应用包中的资产引用。

:no_entry_sign:正确响应 DPI 和规模中的更改的控件。

:no_entry_sign:添加**WebView**到自定义用户控件，（线程上，关闭线程或进程超出） 的控件。

:no_entry_sign:[显示突出显示](https://docs.microsoft.com/windows/uwp/design/style/reveal)Fluent 效果。

:no_entry_sign:内联墨迹书写， @Places，和@People输入控件。

:no_entry_sign:分配的快捷键。

:no_entry_sign:基于 c + + 的第三方控件。

:no_entry_sign:承载自定义用户控件。

随着我们继续改进在桌面引入 Fluent 的体验，此列表中的项目可能发生更改。  

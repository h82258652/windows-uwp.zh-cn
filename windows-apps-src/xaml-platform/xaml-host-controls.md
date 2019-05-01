---
description: 本指南将帮助您直接在 WPF 和 Windows 窗体应用程序中创建基于 Fluent 的 UWP UI
title: 桌面应用程序中的 UWP 控件
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10、 uwp、 windows 窗体、 wpf、 xaml 群岛
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: df2bb23f3934b7629f2fec408f8bde713e5f69d9
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63812712"
---
# <a name="uwp-controls-in-desktop-applications-xaml-islands"></a>桌面应用程序 （XAML 群岛） 中的 UWP 控件

从 Windows 10，版本 1903，开始可以托管在非 UWP 桌面应用程序使用名为的功能中的 UWP 控件*XAML 群岛*。 此功能可以增强的外观、 感受和功能的最新的 Windows 10 用户界面功能仅通过 UWP 控件提供的现有桌面应用程序。 这意味着可以使用如 UWP 功能[Windows 墨迹](../design/input/pen-and-stylus-interactions.md)和支持的控件[Fluent 设计系统](../design/fluent-design-system/index.md)在你现有 WPF 中，Windows 窗体和C++的 Win32 应用程序。

我们提供多种方式来使用 XAML 在 WPF 中，Windows 窗体中的群岛和C++Win32 应用程序，具体取决于技术或正在使用的框架。

> [!NOTE]
> 如果您将得到 XAML 群岛反馈，创建中的一个新问题[Microsoft.Toolkit.Win32 存储库](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)和那里留言。 如果想要私下提交您的反馈，您可以将其发送到XamlIslandsFeedback@microsoft.com。 你的见解和方案是对我们非常重要。

## <a name="how-do-xaml-islands-work"></a>XAML 群岛是如何工作的？

从 Windows 10，版本 1903，开始我们提供两种方法用于在 WPF 中，Windows 窗体中的 XAML 群岛和C++Win32 应用程序：

* Windows SDK 提供了几个 Windows 运行时类和应用程序可用于托管任何 UWP 控件派生的 COM 接口[ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)。 这些类和接口统称为*UWP XAML 承载 API*，并且它们使你的应用程序有一个关联的窗口句柄 (HWND) 中的任何 UI 元素中的主机 UWP 控件。 有关此 API 的详细信息，请参阅[使用托管 API XAML](using-the-xaml-hosting-api.md)。

* [Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)还提供其他 XAML 岛控件，用于 WPF 和 Windows 窗体。 这些控件使用在内部托管 API UWP XAML，并实施所有您需要自己进行处理，如果使用托管 API，包括键盘导航和布局更改 UWP XAML 的行为。 对于 WPF 和 Windows 窗体应用程序，我们强烈建议你使用这些控件，而不是 UWP XAML 直接托管 API，因为它们抽象出许多使用 API 的实现细节。 请注意，截至 Windows 10，版本 1903，这些控件都[可作为开发者预览版](#feature-roadmap)。

> [!NOTE]
> C++Win32 桌面应用程序必须使用托管 API 主机 UWP 控件到 UWP XAML。 Windows 社区工具包中的 XAML 岛控件不可用于C++Win32 桌面应用程序。

有两种类型的 XAML 岛控件提供的 Windows 社区工具包用于 WPF 和 Windows 窗体应用程序：*包装控件*并*主机控件*。

### <a name="wrapped-controls"></a>已包装的控件

WPF 和 Windows 窗体应用程序可以使用一系列中的已包装 UWP 控件[Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)。 这些被称为*包装控件*因为它们包装的接口和特定的 UWP 控件的功能。 可以直接向你的 WPF 或 Windows 窗体项目的设计图面添加这些控件和类似于任何其他 WPF 或 Windows 窗体控件中在您的设计器中使用它们。

用于实现 XAML 群岛以下已包装的 UWP 控件是当前适用于 WPF 和 Windows 窗体应用程序。

| 控件 | 支持的最低 OS | 描述 |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 版本 1903 | 提供在 Windows 窗体或 WPF 桌面应用程序中的基于 Windows 手写内容的用户交互图面上和相关的工具栏。 |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 版本 1903 | 将嵌入的流式处理和呈现在 Windows 窗体或 WPF 桌面应用程序中的视频等媒体内容的视图。 |
| [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10 版本 1903 | 可以在 Windows 窗体或 WPF 桌面应用程序中显示的符号或图像逼真的映射。 |

XAML 岛的已包装控件，除了 Windows 社区工具包还提供以下控件用于承载 web 内容。

| 控件 | 支持的最低 OS | 描述 |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10 版本 1803 | 使用 Microsoft Edge 呈现引擎来显示 web 内容。 |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | 提供的版本**WebView**与多个 OS 版本兼容。 此控件使用 Microsoft Edge 呈现引擎，以显示在 Windows 10 版本 1803年和更高版本，web 内容和 Internet Explorer 呈现引擎，以显示 web 内容在早期版本的 Windows 10 中，Windows 8.x 和 Windows 7。 |

### <a name="host-controls"></a>主机控件

对于超出可用的已包装控件所涵盖的那些情况下，WPF 和 Windows 窗体应用程序也可以使用[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制[Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)。 此控件可以托管任何 UWP 控件派生自[ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)，包括任何由 Windows SDK 提供的 UWP 控件以及自定义用户控件。 此控件支持 Windows 10 Insider Preview SDK 生成 17709 和更高版本。

### <a name="architecture-overview"></a>体系结构概述

下面是这些控件的组织结构速览。

![托管控件体系结构](images/host-controls.png)

此图表底部所示的 API 随 Windows SDK 提供。 已包装的控件和宿主控件均可通过 Windows 社区工具包中的 Nuget 包。

## <a name="feature-roadmap"></a>功能路线图

从发布的 Windows 10，版本 1903，开始包装的控件和 Windows 社区工具包中的主机控件是仍在开发者预览版可用控件的版本 1.0 发行之前。

* 在.NET Framework 4.6.2 的控件的 1.0 版和更高版本计划在中发布[6.0 版本的工具包](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones)。
* 该工具包的更高版本计划 1.0 版的.NET Core 3 的控件。
* 如果你想要尝试这些控件的版本 1.0 版本的最新预览版的.NET Framework 和.NET Core 3，请参阅**6.0.0-preview3**中的 NuGet 包[UWP 社区工具包](https://dotnet.myget.org/gallery/uwpcommunitytoolkit)库。

## <a name="requirements"></a>要求

XAML 群岛需要 Windows 10，版本 1903，及更高版本。 若要在应用程序中使用 XAML 群岛，必须先设置你的项目。

### <a name="step-1-modify-your-project-to-use-windows-runtime-apis"></a>第 1 步：修改项目以使用 Windows 运行时 Api

有关说明，请参阅[这篇文章](../porting/desktop-to-uwp-enhance.md#first-set-up-your-project)。

### <a name="step-2-enable-xaml-island-support-in-your-project"></a>步骤 2：在项目中启用 XAML 岛支持

对您的项目以启用 XAML 岛支持进行以下更改之一。 有关更多详细信息，请参阅[这篇博客文章](https://techcommunity.microsoft.com/t5/Windows-Dev-AppConsult/Using-XAML-Islands-on-Windows-10-19H1-fixing-the-quot/ba-p/376330#M117)。

#### <a name="option-1-package-your-application-in-an-msix-package"></a>选项 1：MSIX 包中的将应用程序打包  

安装 Windows 10，版本 1903 SDK （或更高版本）。 然后，通过添加打包应用程序，MSIX 封装[Windows 应用程序打包项目](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-packaging-dot-net)到你的解决方案和添加到 WPF 或 Windows 窗体项目的引用。

#### <a name="option-2-set-the-maxversiontested-value-in-your-assembly-manifest"></a>选项 2：在程序集清单中设置 maxVersionTested 值

如果不想要打包应用程序，MSIX 包中的，您可以添加[-并行程序集清单](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)到你的项目并添加**maxVersionTested**到清单中指定的值在应用程序是与 Windows 10，1903年或更高版本兼容。

1. 如果还没有你的项目中的清单，将新的 XML 文件添加到你的项目并将其命名程序集**app.manifest**。 对于 WPF 或 Windows 窗体的应用程序，请确保也将分配**清单**属性设置为 **。 应用程序清单**中**应用程序**页你[项目属性](https://docs.microsoft.com/en-us/visualstudio/ide/reference/application-page-project-designer-csharp?view=vs-2019#resources)。
2. 在程序集清单中，包括**兼容性**元素，并在下面的示例所示的子元素。 替换**Id**的属性**maxVersionTested**您面向的 Windows 10 的版本号的元素 （这必须是 Windows 10，版本 1903年或更高版本）。 

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
        <compatibility xmlns="urn:schemas-microsoft-com:compatibility.v1">
            <application>
                <!-- Windows 10 -->
                <maxversiontested Id="10.0.18362.0"/>
                <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
            </application>
        </compatibility>
    </assembly>
    ```

## <a name="additional-resources"></a>其他资源

有关详细背景信息和有关使用 XAML 岛的教程，请参阅以下文章和资源：

* [XAML 群岛实验室](https://github.com/Microsoft/Windows-AppConsult-XAMLIslandsLab/tree/microsoftlearn)。 此综合实验室提供了有关使用 Windows 社区工具包中的已包装的控件和宿主控件，将 UWP 控件添加到现有的 WPF 业务线应用程序的分步说明。 此实验室还包括[WPF 应用程序的完整代码](https://github.com/Microsoft/Windows-AppConsult-XAMLIslandsLab/tree/microsoftlearn/Lab)，以及[的详细说明](https://github.com/Microsoft/Windows-AppConsult-XAMLIslandsLab/blob/microsoftlearn/Manual/README.md)过程中每个步骤。

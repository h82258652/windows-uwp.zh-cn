---
Description: 添加新式 XAML 用户界面、创建 MSIX 包，以及将其他新式组件并入桌面应用程序中。
title: 实现 Windows 桌面应用的现代化
ms.topic: article
ms.date: 04/17/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: b966d00455bce390457e148c60b57296375ac2fa
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730244"
---
# <a name="modernize-your-desktop-apps"></a>实现桌面应用的现代化

Windows 10 和通用 Windows 平台 (UWP) 提供的许多功能可以用来在桌面应用中提供新式的体验。 你可以按照自己的进度，将大多数此类功能作为模块化组件在桌面应用中使用，不需为其他平台重新编写应用程序。 可以选择要采用的 Windows 10 和 UWP 具体部件，增强现有的桌面应用。

本文介绍目前可以在桌面应用中使用的 Windows 10 和 UWP 功能。 有关演示如何实现现有应用的现代化以使用本文中所述的许多功能的教程，请参阅[实现 WPF 应用现代化](modernize-wpf-tutorial.md)教程。

> [!NOTE]
> 在将桌面应用迁移到 Windows 10 的过程中，你是否需要帮助？ [桌面应用保证](https://docs.microsoft.com/FastTrack/win-10-desktop-app-assure)服务为需要将应用移植到 Windows 10 的开发人员提供直接的免费支持。 该计划适用于所有 ISV 和合格的企业。 若要更详细地了解相关资格和计划本身，请访问 [https://docs.microsoft.com/fasttrack/win-10-app-assure-assistance-offered](https://docs.microsoft.com/fasttrack/win-10-app-assure-assistance-offered)。 若要立即开始体验，请[提交请求](https://fasttrack.microsoft.com/dl/daa)。

## <a name="msix-packages"></a>MSIX 包

MSIX 是一种新式的 Windows 应用包格式，提供所有 Windows 应用（包括 UWP、WPF、Windows 窗体和 Win32 应用）的通用打包体验。 MSIX 汇集了 MSI、.appx、App-V 和 ClickOnce 安装技术的最佳方面，以便提供新式的可靠打包体验。

将桌面 Windows 应用打包到 MSIX 包中即可访问可靠的安装和更新体验、功能系统灵活的托管安全模型、对 Microsoft Store 的支持、企业管理以及许多自定义分发模型。

有关详细信息，请参阅 MSIX 文档中的[打包桌面应用程序](/windows/msix/desktop/desktop-to-uwp-root)。

## <a name="net-core-3"></a>.NET Core 3

.NET Core 3 是 .NET Core 的最新主要版本。 这个版本的主要特点是支持 Windows 桌面应用，包括 Windows Forms 应用和 WPF 应用。 你可以在 .NET Core 3 上运行新的和现有的 Windows 桌面应用并体验 .NET Core 提供的所有优势。 托管在 [XAML 岛](xaml-islands.md)中的 UWP 控件也可在面向 .NET Core 3 的 Windows 窗体和 WPF 应用中使用。

有关详细信息，请参阅 [.NET Core 3.0 中的新增功能](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0)。

## <a name="windows-runtime-apis"></a>Windows 运行时 API

可以在 WPF、Windows 窗体或 C++ Win32 桌面应用中直接调用许多 Windows 运行时 API，以便集成对 Windows 10 用户来说焕然一新的体验。 例如，可以调用 Windows 运行时 API，以便将 Toast 通知添加到桌面应用。

有关详细信息，请参阅[在桌面应用中使用 Windows 运行时 API](desktop-to-uwp-enhance.md)。

## <a name="host-uwp-controls-xaml-islands"></a>托管 UWP 控件（XAML 岛）

从 Windows 10 版本 1903 开始，可以将 [UWP XAML 控件](/windows/uwp/design/controls-and-patterns/controls-by-function)直接添加到与窗口句柄 (HWND) 关联的 WPF、Windows 窗体或 C++ Win32 应用中的任何 UI 元素。 这意味着，你可以将最新的 UWP 功能（例如 [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) 和支持 [Fluent Design System](/windows/uwp/design/fluent-design-system/index) 的控件完全集成到 Windows 以及桌面应用的其他显示表面中去。 此开发人员方案有时称为“XAML 岛”。 

有关详细信息，请参阅[桌面应用中的 UWP 控件](xaml-islands.md)

## <a name="use-the-visual-layer-in-desktop-apps"></a>在桌面应用中使用可视化层

现在可以在非 UWP 桌面应用中使用 Windows 运行时 API 来增强 WPF、Windows 窗体和 C++ Win32 应用的外观和功能，并充分利用只能通过 UWP 使用的最新 Windows 10 UI 功能。 在需要创建自定义体验且这些体验超出可以通过 XAML 岛托管的内置 UWP 控件的范畴时，这很有用。

有关详细信息，请参阅[使用可视化层实现桌面应用的现代化](visual-layer-in-desktop-apps.md)。

## <a name="additional-features-available-to-apps-with-package-identity"></a>适用于具有包标识的应用的其他功能

某些新式 Windows 10 体验仅适用于具有 [包标识](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)的桌面应用。 这些功能包括一些 Windows 运行时 API、包扩展和 UWP 组件。 有关详细信息，请参阅 [Features that require package identity](modernize-packaged-apps.md)（需要包标识的功能）。

可通过多种方式向桌面应用授予标识：

* 将其打包到 [MSIX 包](/windows/msix/desktop/desktop-to-uwp-root)中。 MSIX 是一种新式应用包格式，提供适合所有 Windows 应用、WPF、Windows 窗体和 Win32 应用的通用打包体验。 它提供了可靠的安装和更新体验、功能系统灵活的托管安全模型、对 Microsoft Store 的支持、企业管理以及许多自定义分发模型。 有关详细信息，请参阅 MSIX 文档中的[打包桌面应用程序](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)。
* 如果无法采用 MSIX 打包来部署桌面应用，那么，自 Windows 10 版本 2004 起，你可通过创建一个仅包含程序包清单的稀疏 MSIX 包来授予包标识  。 有关详细信息，请参阅[向未打包的桌面应用授予标识](grant-identity-to-nonpackaged-apps.md)。

<a id="desktop-uwp-controls"/>

## <a name="uwp-controls-optimized-for-desktop-apps"></a>针对桌面应用优化的 UWP 控件

不管你是要构建专门面向桌面设备系列的 UWP 应用，还是要在 WPF、Windows 窗体或 C++ Win32 桌面应用中使用 UWP 控件，均可利用下面这些全新的和更新的 UWP 控件。这些控件旨在提供可以与 [Fluent Design System](/windows/uwp/design/fluent-design-system/index) 配合使用的桌面优化体验。 这些控件是在 Windows 10 版本 1809（2018 年 10 月更新，或者版本 10.0.17763）中引入的。

| 控件 |  说明 |
|------ |--------------|
| [MenuBar](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus#create-a-menu-bar) | 为那些需要的组织或分组功能可能无法通过 **CommandBar** 来满足的应用提供一种公开命令集的方式，这种方式既快速又简单。 |
| [DropDownButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button) | 显示一个 V 形图标作为视觉指示器，表明其附加的浮出控件包含更多选项。  |
| [SplitButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button) | 提供的按钮有两个部分，可以分别调用。 一个部分的行为类似于标准按钮，可以调用即时操作。 另一个部分调用浮出控件，该控件包含可供用户选择的其他选项。|
| [ToggleSplitButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-toggle-split-button) | 提供的按钮有两个部分，可以分别调用。 一个部分的行为类似于可以打开或关闭的切换按钮。 另一个部分调用浮出控件，该控件包含可供用户选择的其他选项。 |
| [CommandBarFlyout](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/command-bar-flyout) |  用于在 UI 画布的项上下文中显示常见用户任务。 |
| [ComboBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/combo-box#make-a-combo-box-editable) | 现在可以将组合框设置为可编辑，这样用户就能输入控件中未列出的值。  |
| [TreeView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view) | 现在可以配置树视图，以便启用数据绑定、项模板和拖放功能。  |
| [DataGridView](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid) |   用于在行和列中灵活地显示数据集合。 此控件在 [Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)中可用。  |

## <a name="windows-ui-library"></a>Windows UI 库

Windows UI 库是一组 NuGet 包，它们提供了用于 UWP 应用的新控件和其他用户界面元素。 Windows UI 库 API 可以在早期版本的 Windows 10 上使用。因此，即使用户没有运行最新版 Windows 10，你的应用也可以使用。 这样，当新控件在 Windows UI 库中发布时，你就可以采用它们，不需担心是否要添加版本检查或条件 XAML。

请参阅 [Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/)（Windows UI 库）。

## <a name="other-technologies-for-modern-desktop-apps"></a>新式桌面应用的其他技术

### <a name="microsoft-graph"></a>Microsoft Graph

Microsoft Graph 是一系列 API，这些 API 可以用来为组织和消费者构建与数百万用户的数据交互的应用。 Microsoft Graph 公开的 REST API 和客户端库可以用来访问以下服务的数据：
* Azure Active Directory
* Office 365 服务：SharePoint、OneDrive、Outlook/Exchange、Microsoft Teams、OneNote、Planner、Excel
* 企业移动性和安全性服务：Identity Manager、Intune、高级威胁分析、高级威胁防护。
* Windows 10 服务：活动和设备

有关详细信息，请参阅 [Microsoft Graph 文档](https://developer.microsoft.com/graph/docs/concepts/overview)。

### <a name="adaptive-cards"></a>自适应卡片

自适应卡片是一种开放式跨平台框架，可以通过一种常见且一致的方法跨设备和平台交换基于卡的 UI 内容。

有关详细信息，请参阅[自适应卡片文档](https://docs.microsoft.com/adaptive-cards/)。

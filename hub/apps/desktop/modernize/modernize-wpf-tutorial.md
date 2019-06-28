---
description: 本教程演示如何添加 UWP XAML 用户界面、 创建 MSIX 包和其他新式组件合并到 WPF 应用。
title: 教程：使 WPF 应用现代化
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、 uwp、 windows 窗体、 wpf、 xaml 群岛
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 5e7179d4aeb66cad547e31e2456da2e8264ebbcd
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420075"
---
# <a name="tutorial-modernize-a-wpf-app"></a>教程：使 WPF 应用现代化 

有很多方面与[实现现代化](index.md)通过将最新的 Windows 功能集成到现有的现有桌面应用程序而不是重写从零开始的应用的源代码。 在本教程中我们将探讨几种方法实现现代化现有 WPF 业务线应用程序，通过使用这些功能：

* .NET Core 3
* 使用 XAML 群岛 UWP XAML 控件
* 自适应卡和 Windows 10 通知
* MSIX 部署

本教程需要以下开发技能：

* 在开发使用 WPF Windows 桌面应用程序体验。
* 基础知识C#和 XAML。
* UWP 的基本知识。

## <a name="overview"></a>概述

本教程提供了名为 Contoso 费用的简单 WPF 业务线应用的代码。 在本教程的虚构方案中，Contoso 费用是内部应用的 Contoso Corporation 的管理器用于跟踪的提交其报表的节省的费用。 管理器现在配备有触摸的设备，他们想要使用不带鼠标或键盘 Contoso Expenses 应用程序。 遗憾的是，应用程序的当前版本不是触摸友好。

Contoso 希望实现新的 Windows 功能，使员工能够更有效地创建支出报表使用此应用程序的现代化。 许多功能可轻松地实现通过生成新的 UWP 应用。 但是，现有的应用复杂且是由不同的团队开发的年数的结果。 在这种情况下，重写从零开始的一项新技术不是一个选项。 团队正在寻求将新功能添加到现有的基本代码的最佳方法。

在本教程开始时，Contoso 费用面向.NET Framework 4.7.2，并使用以下第三方库：

* MVVM Light，MVVM 模式的基本实现。
* Unity 中，依赖关系注入容器。
* LiteDb，嵌入的 NoSQL 解决方案，用于存储数据。
* 虚假，生成模拟数据的工具。

在本教程中，将使用新的 Windows 功能来增强 Contoso 费用：

* 将现有的 WPF 应用程序迁移到.NET Core 3.0。 这会打开新的和重要方案将来。
* 为主机使用 XAML 群岛**InkCanvas**并**MapControl**包装 Windows 社区工具包提供的控件。
* 使用 XAML 群岛，以承载任何标准的 UWP XAML 控件 (在这种情况下， **CalendardView**)。
* 将自适应卡和 Windows 10 通知集成到应用程序。
* 应用程序与 MSIX 并设置 CI/CD 管道 Azure DevOps，以便您可以自动传送新版本的应用程序到测试人员和用户立即可用的包。

## <a name="prerequisites"></a>先决条件

若要执行本教程中，在开发计算机必须安装这些必备组件：

* Windows 10，版本 1903年 （内部 18362） 或更高版本。
* [Visual Studio 2019](https://www.visualstudio.com)。
* [.NET core 3 预览版 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) （安装最新可用的预览版本）。

请确保使用 Visual Studio 2019 安装以下工作负荷和可选功能：

* .NET 桌面开发
* 通用 Windows 平台开发
* Windows 10 SDK (10.0.18362.0 或更高版本)

## <a name="get-the-contoso-expenses-sample-app"></a>获取 Contoso 支出示例应用

在开始本教程之前，下载 Contoso Expenses 应用程序的源代码，并确保可以生成 Visual Studio 中的代码。

1. 下载中的应用程序源代码**版本**选项卡[AppConsult WinAppsModernization 研讨会存储库](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop)。 直接链接[ https://aka.ms/wamwc ](https://aka.ms/wamwc)。
2. 打开 zip 文件，并解压缩到的根的所有内容您**c:\\** 驱动器。 它将创建名为的文件夹**C:\WinAppsModernizationWorkshop**。
3. 打开 Visual Studio 2019 并双击**C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln**文件以打开该解决方案。
4. 验证你可以生成、 运行和调试 Contoso 费用 WPF 项目通过按**启动**按钮或按 CTRL + F5。

## <a name="get-started"></a>立即开始行动

已为 Contoso Expenses 示例应用程序的源代码，并且你可以确认，您可以在 Visual Studio 中生成它后，就可以开始本教程：

* [第 1 部分：将 Contoso 迁移到.NET Core 3 Expenses 应用程序](modernize-wpf-tutorial-1.md)
* [第 2 部分：添加 UWP InkCanvas 控件使用 XAML 群岛](modernize-wpf-tutorial-2.md)
* [第 3 部分：添加 UWP 日历视图控件使用 XAML 群岛](modernize-wpf-tutorial-3.md)
* [第 4 部分：添加 Windows 10 用户活动和通知](modernize-wpf-tutorial-4.md)
* [第 5 部分：打包和部署与 MSIX](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>重要的概念

以下各节提供一些在此教程中讨论的关键概念背景。 如果您已经熟悉这些概念，则可以跳过本部分中。

### <a name="universal-windows-platform-uwp"></a>通用 Windows 平台 (UWP)

在 Windows 8 中，Microsoft 引入了一个称为 Windows 运行时 (WinRT) 的新框架。 与.NET Framework 中，不同 WinRT 是直接向应用程序公开的 Api 的本机层。 WinRT 还引入了语言投射，是在运行时，使开发人员可以使用类似于语言的与其进行交互之上添加层C#和除了 JavaScript C++。 投影使开发人员能够构建基于 WinRT 应用程序利用相同的C#以及它们在使用.NET Framework 构建应用程序中获得的 XAML 知识。 

在 Windows 10 中，Microsoft 引入了[通用 Windows 平台 (UWP)](/windows/uwp/get-started/universal-application-platform-guide)，其构建于 WinRT 之上。 UWP 的最重要功能是，它提供了一组在每个设备平台之间通用的 Api： 无论应用程序运行在桌面上、 Xbox One 上或在 HoloLens 上你是否能够使用相同的 Api。

今后，最新的 Windows 10 通过 WinRT Api，包括时间线、 项目罗马和 Windows Hello 等功能来公开功能。

### <a name="msix-packaging"></a>MSIX 打包

[MSIX](http://aka.ms/msix) （以前称为 AppX） 是用于 Windows 应用的现代打包模型。 MSIX 支持 UWP 应用和桌面应用程序构建使用 Win32、 WPF、 Windows 窗体、 Java、 Electron，和的详细信息等技术。 当包 MSIX 包中的桌面应用时，你可以将应用发布到 Microsoft Store。 桌面应用程序还安装它，从而使桌面应用程序使用一组更广泛的 WinRT Api 时获得的包标识。

有关详细信息，请参阅以下文章：

* [打包桌面应用程序](/windows/uwp/porting/desktop-to-uwp-root)
* [打包的桌面应用程序在后台](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>XAML 群岛

从 Windows 10，版本 1903，开始可以托管在非 UWP 桌面应用程序使用名为的功能的 UWP 控件*XAML 群岛*。 此功能可以增强的外观、 感受和功能的最新的 Windows 10 用户界面功能仅通过 UWP 控件提供的现有桌面应用。 这意味着您可以使用 UWP 功能，例如 Windows 墨迹和 Fluent 设计系统支持在你现有 WPF 中，Windows 窗体的控件和C++Win32 应用。

有关详细信息，请参阅[桌面应用程序 （XAML 群岛） 中的 UWP 控件](/windows/uwp/xaml-platform/xaml-host-controls)。 本教程将指导您完成使用两个不同类型的 XAML 岛控件的过程：

* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)并[MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) Windows 社区工具包中。 这些 WPF 控件包装的接口和相应的 UWP 控件的功能，并可以像在 Visual Studio 设计器中的任何其他 WPF 控件使用。

* UWP[日历视图](/windows/uwp/design/controls-and-patterns/calendar-view)控件。 这是标准的 UWP 控件，将使用托管[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) Windows 社区工具包中的控件。

### <a name="net-core-3"></a>.NET Core 3

[.NET core](https://docs.microsoft.com/dotnet/core/)是一种开放源代码框架，实现完整的.NET Framework 的跨平台、 轻型且易于扩展版本。 与完整的.NET Framework 相比，.NET Core 的启动时间更快并且已经过优化，这样的许多 Api。

通过其第一个的多个版本，.NET Core 的重点是用于支持 web 或后端应用。 使用.NET Core，您可以轻松构建可缩放的 web 应用或 Windows，Linux，或在微服务体系结构，例如 Docker 容器可托管的 Api。

.NET Core 3 是 .NET Core 的下一主要版本。 这个即将发行的版本的主要特点是支持 Windows 桌面应用，包括 Windows 窗体和 WPF 应用。 可以在.NET Core 3 上运行新的和现有 Windows 桌面应用程序和享受必须要提供的.NET Core 的所有权益。 托管在 [XAML 岛](xaml-islands.md)中的 UWP 控件也可在面向 .NET Core 3 的 Windows 窗体和 WPF 应用中使用。

> [!NOTE]
> WPF 和 Windows 窗体不会变得跨平台，您不能在 Linux 和 MacOS 上运行 WPF 或 Windows 窗体。 WPF 和 Windows 窗体的 UI 组件 Windows 呈现系统上仍有依赖关系。

有关详细信息，请参阅以下文章：

* [.NET Core 3.0 Preview 1 公告](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-1-and-open-sourcing-windows-desktop-frameworks/)
* [.NET Core 3.0 Preview 2 公告](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-2/)
* [.NET Core 3.0 Preview 3 公告](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-3/)
* [.NET Core 3.0 Preview 4 公告](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-4/)
* [.NET Core 3.0 新增功能](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0)。
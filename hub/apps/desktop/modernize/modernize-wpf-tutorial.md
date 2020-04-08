---
description: 本教程演示如何添加 UWP XAML 用户界面、创建 MSIX 包以及将其他新式组件合并到 WPF 应用中。
title: 教程：实现 WPF 应用现代化
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows 窗体, wpf, xaml 岛
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 21049c995d467209b22fe8ea5c40d303911f2c2c
ms.sourcegitcommit: 0a319e2e69ef88b55d472b009b3061a7b82e3ab1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2020
ms.locfileid: "77521272"
---
# <a name="tutorial-modernize-a-wpf-app"></a>教程：实现 WPF 应用现代化 

有多种方法可以通过将最新的 Windows 功能集成到现有源代码（而不是从头开始重新编写应用）来[现代化](index.md)现有桌面应用。 在本教程中，我们将探讨使用这些功能实现现有 WPF 业务线应用现代化的多种方法：

* .NET Core 3
* 带有 XAML 岛的 UWP XAML 控件
* 自适应卡和 Windows 10 通知
* MSIX 部署

本教程需要以下开发技能：

* 具有使用 WPF 开发 Windows 桌面应用的经验。
* 具备 C# 和 XAML 的基础知识。
* 具备 UWP 的基础知识。

## <a name="overview"></a>概述

本教程提供名为 Contoso Expenses 的简单 WPF 业务线应用的代码。 在本教程的虚构方案中，Contoso Expenses 是 Contoso Corporation 管理员使用的内部应用，用于跟踪报表提交的支出。 管理员现在配备有支持触摸的设备，他们希望无需鼠标或键盘即可使用 Contoso Expenses 应用。 遗憾的是，当前版本的应用不兼容触摸功能。

Contoso 希望使用新的 Windows 功能实现此应用现代化，使员工能够更高效地创建支出报表。 许多功能可通过生成新的 UWP 应用轻松实现。 但是，现有应用很复杂，这是不同团队多年开发的结果。 因此，不会选择使用新技术从头开始重新编写应用。 团队正在寻找将新功能添加到现有代码库的最佳方法。

在本教程开头，Contoso Expenses 面向 .NET Framework 4.7.2 并使用以下第三方库：

* MVVM Light，表示 MVVM 模式的基本实现。
* Unity，表示依赖关系注入容器。
* LiteDb，用于存储数据的嵌入式 NoSQL 解决方案。
* Bogus，用于生成虚假数据的工具。

在本教程中，你将使用新的 Windows 功能增强 Contoso Expenses：

* 将现有 WPF 应用迁移到 .NET Core 3.0。 将来会打开全新而重要的方案。
* 使用 XAML 岛托管 Windows 社区工具包提供的 InkCanvas 和 MapControl 包装控件   。
* 使用 XAML 岛托管任何标准 UWP XAML 控件（在本例中为 CalendardView  ）。
* 将自适应卡和 Windows 10 通知集成到应用中。
* 使用 MSIX 打包应用并在 Azure DevOps 上设置 CI/CD 管道，以便你可以在新版本的应用可用后立即向测试人员和用户自动交付。

## <a name="prerequisites"></a>必备条件

若要执行本教程，开发计算机必须安装以下必备项：

* Windows 10 版本 1903（版本 18362）或更高版本。
* [Visual Studio 2019](https://www.visualstudio.com)。
* [.NET Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)（安装最新版本）。

确保随 Visual Studio 2019 一起安装以下工作负载和可选功能：

* .NET 桌面开发
* 通用 Windows 平台开发
* Windows 10 SDK（10.0.18362.0 或更高版本）

## <a name="get-the-contoso-expenses-sample-app"></a>获取 Contoso Expenses 示例应用

在开始本教程之前，请下载 Contoso Expenses 应用的源代码，并确保可以在 Visual Studio 中生成代码。

1. 从 [AppConsult WinAppsModernization 研讨会存储库](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop)的“版本”选项卡中下载应用源代码  。 直接链接是 [https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases](https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases)。
2. 打开 zip 文件并将所有内容提取到 C:\\  驱动器的根目录。 此时将创建一个名为 C:\WinAppsModernizationWorkshop 的文件夹  。
3. 打开 Visual Studio 2019，然后双击 C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln 文件以打开解决方案  。
4. 验证你是否可以通过按“开始”按钮或按 CTRL + F5 来生成、运行和调试 Contoso Expenses WPF 项目  。

## <a name="get-started"></a>入门

获得 Contoso Expenses 示例应用的源代码，并且可以确认是否能够在 Visual Studio 中生成该应用后，即可开始本教程：

* [第 1 部分：将 Contoso Expenses 应用迁移到 .NET Core 3](modernize-wpf-tutorial-1.md)
* [第 2 部分：使用 XAML 岛添加 UWP InkCanvas 控件](modernize-wpf-tutorial-2.md)
* [第 3 部分：使用 XAML 岛添加 UWP CalendarView 控件](modernize-wpf-tutorial-3.md)
* [第 4 部分：添加 Windows 10 用户活动和通知](modernize-wpf-tutorial-4.md)
* [第 5 部分：使用 MSIX 打包和部署](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>重要概念

以下部分为本教程中讨论的部分关键概念提供了背景。 如果你已熟悉这些概念，则可以跳过本部分。

### <a name="universal-windows-platform-uwp"></a>通用 Windows 平台 (UWP)

在 Windows 8 中，Microsoft 引入了称为“Windows 运行时 (WinRT)”的新框架。 与 .NET Framework 不同，WinRT 是直接向应用公开的 API 的本机层。 WinRT 还引入了语言投影（这是基于运行时添加的层），允许开发人员使用除 C++ 之外的语言（如 C# 和 JavaScript）与之交互。 通过投影，开发人员可以基于 WinRT（利用使用 .NET Framework 生成应用时所获得的同一 C# 和 XAML 知识）生成应用。 

在 Windows 10 中，Microsoft 引入了[通用 Windows 平台 (UWP)](/windows/uwp/get-started/universal-application-platform-guide)，这是基于 WinRT 生成的。 UWP 的最重要功能是跨每个设备平台提供一组通用 API：无论应用是在桌面上、在 Xbox 上还是在 HoloLens 上运行，都可以使用相同的 API。

今后，大多数新的 Windows 10 功能都是通过 WinRT API 公开的，包括时间线、Project Rome 和 Windows Hello 等功能。

### <a name="msix-packaging"></a>MSIX 打包

[MSIX](/windows/msix/) 是适用于 Windows 应用的新式打包模型。 MSIX 支持 UWP 应用以及使用 Win32、WPF、Windows 窗体、Java、Electron 等技术生成的桌面应用。 在 MSIX 包中打包桌面应用时，可以将应用发布到 Microsoft Store。 桌面应用在安装后也会获得包标识符，这使桌面应用能够使用更广泛的一组 WinRT API。

有关详细信息，请参阅以下文章：

* [打包桌面应用程序](/windows/uwp/porting/desktop-to-uwp-root)
* [在打包桌面应用程序幕后](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>XAML 岛

从 Windows 10 版本 1903 开始，可以使用称为“XAML 岛”的功能在非 UWP 桌面应用中托管 UWP 控件  。 可以通过此功能来增强现有桌面应用的外观和功能，并使用只能通过 UWP 控件使用的最新 Windows 10 UI 功能。 这意味着，可以在现有的 WPF、Windows 窗体和 C++ Win32 应用中使用 UWP 功能（例如 Windows Ink）和支持 Fluent Design System 的控件。

有关详细信息，请参阅[桌面应用程序中的 UWP 控件（XAML 岛）](/windows/uwp/xaml-platform/xaml-host-controls)。 本教程将指导你完成使用两种不同类型的 XAML 岛控件的过程：

* Windows 社区工具包中的 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 和 [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol)。 这些 WPF 控件包装相应 UWP 控件的界面和功能，并可像 Visual Studio 设计器中的任何其他 WPF 控件一样使用。

* UWP [日历视图](/windows/uwp/design/controls-and-patterns/calendar-view)控件。 这是使用 Windows 社区工具包中的 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控件托管的标准 UWP 控件。

### <a name="net-core-3"></a>.NET Core 3

[.NET Core](https://docs.microsoft.com/dotnet/core/) 是一种开放源代码框架，可实现完整 .NET Framework 的跨平台、轻量级且可轻松扩展的版本。 与完整 .NET Framework 相比，.NET Core 启动时间要快得多，并且许多 API 已经过优化。

在前几个版本中，.NET Core 关注的是支持 Web 或后端应用。 借助 .NET Core，你可以轻松地生成可在 Windows、Linux 上或在微服务体系结构（如 Docker 容器）中托管的可缩放 Web 应用或 API。

.NET Core 3 是 .NET Core 的最新版本。 这个版本的主要特点是支持 Windows 桌面应用，包括 Windows Forms 应用和 WPF 应用。 你可以在 .NET Core 3 上运行新的和现有的 Windows 桌面应用并体验 .NET Core 提供的所有优势。 托管在 [XAML 岛](xaml-islands.md)中的 UWP 控件也可在面向 .NET Core 3 的 Windows 窗体和 WPF 应用中使用。

> [!NOTE]
> WPF 和 Windows 窗体不是跨平台的，因此不能在 Linux 和 MacOS 上运行 WPF 或 Windows 窗体。 WPF 和 Windows 窗体的 UI 组件仍依赖于 Windows 呈现系统。

有关详细信息，请参阅 [.NET Core 3.0 中的新增功能](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0)。

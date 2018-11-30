---
title: 2017 年 12 月 Windows 文档中的新增功能 - 开发 UWP 应用
description: 新增功能、视频和开发人员指南已添加到 2017 年 12 月 Windows 10 开发人员文档
keywords: 新增功能, 更新, 功能, 开发人员指南, Windows 10, 12 月
ms.date: 12/14/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c9b4c834c646aed7953dc2b0f992dea579d8b9fd
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "8214438"
---
# <a name="whats-new-in-the-windows-developer-docs-in-december-2017"></a>2017 年 12 月 Windows 开发人员文档中的新增功能

Windows 开发人员文档持续更新对整个 Windows 平台的开发人员提供的新功能的信息。 以下功能概述、开发人员指南和示例在发布 Fall Creators Update 之后提供，包含了面向 Windows 开发人员的新增和更新的信息。

只需在 Windows10 上[安装工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="features"></a>功能

### <a name="windows-mixed-reality-enthusiasts-guide"></a>Windows Mixed Reality：发烧友指南

[发烧友指南](https://docs.microsoft.com/en-us/windows/mixed-reality/enthusiast-guide/)面向探索混合现实领域的技术发烧友，回答了大家对 Windows Mixed Reality 的主要问题。 

在该指南中，你将了解： 
- 购买前常见问题解答， 
- 如何检查电脑的兼容性， 
- 设置说明， 
- 如何使用头戴显示设备和控制器， 
- 如何下载和畅玩沉浸式游戏、360 视频、2D 应用、WebVR 和 SteamVR， 
- 如何排查问题，等等。

![Windows Mixed Reality 头戴显示设备用户和运动控制器](images/BeforeYouBegin-tile.jpg)

### <a name="keyboard-interactions"></a>键盘交互

设计和优化你的 UWP 应用，以便使用更新的[键盘交互](../design/input/keyboard-interactions.md)为高级用户提供更轻松的访问体验和强大的功能。 我们更新了相关建议和指南，以反映 Fall Creators Update 中交互方面的新增改进。

请参阅[键盘快捷方式](../design/input/keyboard-accelerators.md)和[自定义键盘交互](../design/input/custom-keyboard-interactions.md)以扩展你的应用的键盘功能。

在支持触摸交互的设备上，根据[响应触摸键盘的状态](../design/input/respond-to-the-presence-of-the-touch-keyboard.md)以及[使用输入范围更改触摸键盘](../design/input/use-input-scope-to-change-the-touch-keyboard.md)文章添加键盘功能。

### <a name="microsoft-collaborate"></a>Microsoft Collaborate

Microsoft Collaborate 门户提供广泛的工具和服务，旨在通过分享工程系统工作项（bug、功能请求等）以及分配内容（版本、文档、规范）来简化 Microsoft 生态系统中的工程协作。 [了解详细信息](https://docs.microsoft.com/en-us/collaborate)。

![在合作伙伴中心中的 Microsoft Collaborate](images/microsoft_collaborate_screenshot.PNG)

### <a name="package-desktop-applications-with-uwp-projects"></a>打包桌面应用程序以及 UWP 项目

Visual Studio 2017 第 15.5 版已更新 **Windows 应用程序包项目**模板，因此更方便添加 UWP 项目。 你不需要再使用基于 JavaScript 的程序包项目，然后手动调整程序包清单。  

请参阅[使用 Visual Studio 创建应用包](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-packaging-dot-net)获取有关如何使用新模板打包桌面应用程序的指南。

请参阅[使用现代 UWP 组件扩展桌面应用程序](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-extend)获取有关如何将 UWP 项目添加到程序包的指南。

### <a name="subscription-add-ons-are-now-available-to-developers-in-the-windows-dev-center-insider-program"></a>参加 Windows 开发人员中心会员计划的开发人员现在可以使用订阅加载项

所有参加[开发人员中心会员计划](../publish/dev-center-insider-program.md)的开发人员现在都可以使用订阅加载项，以便在自动定期计费周期内销售自主应用中的数字产品（比如应用功能或数字内容）。 如需了解更多的详细信息，可参阅[启用应用加载项订阅](../monetize/enable-subscription-add-ons-for-your-app.md)。

## <a name="developer-guidance"></a>开发人员指南

### <a name="color"></a>颜色

为了尽可能地提高用户体验，我们添加了一些有关如何在应用中使用颜色的新指南。 这包括 API 使用情况，以及有关 UI 设计和辅助功能的一般指南。 我们还更新了可供用户在 Xbox 上使用的主题色列表。 [在此查看更新的颜色使用指南。](../design/style/color.md)

![通用 Windows 调色板](../design/basics/images/colors.png)

### <a name="data-access-guides"></a>数据访问指南

我们添加了 [SQL Server 指南](../data-access/sql-server-databases.md)以说明你的应用可以如何直接访问 SQL Server 数据库。 无需服务层。

我们对 [SQLite 指南](../data-access/sqlite-databases.md)进行了全面整改，使外观更加简洁，我们还加入了有关在用户设备上的轻量级数据库中存储和检索数据的最新优秀实践。

### <a name="forms"></a>表单

我们添加了有关[如何在应用中构建表单](../design/controls-and-patterns/forms.md)的新文章，以便收集和提交用户数据。 这包括有关实施表单的具体信息，以及有关在何时何处使用表单的一般指南。

### <a name="intro-to-app-design"></a>应用设计简介

通用 Windows 平台 (UWP) 设计指南可帮助你设计和构建美观、优化的应用。 [我们的新引言](../design/basics/design-and-ui-intro.md)概述了各项 UWP 应用中包含的通用设计功能，以及借助相关文档构建可在多台设备中完美扩展的用户界面 (UI) 的方法。


### <a name="request-ratings-and-reviews"></a>请求评分和评价

我们添加了一篇说明你可以如何[请求应用评分和评价](../monetize/request-ratings-and-reviews.md)的新文章。 你可以在应用中显示评分和评价对话框，也可以在 Store 中打开应用的评分和评价页面。

## <a name="samples"></a>示例

### <a name="customer-orders"></a>客户订单

[客户订单数据库](https://github.com/Microsoft/Windows-appsample-customers-orders-database)示例已更新以便显示有关数据访问权限的优秀实践，比如使用存储库模式以及如何连接到多个数据源（包括 Sqlite、SQL Azure 以及 REST 服务）。

## <a name="videos"></a>视频

### <a name="package-a-net-app-in-visual-studio"></a>在 Visual Studio 中创建 .NET 应用程序包

现在能够比以往更加轻松地将桌面应用添加到通用 Windows 平台。 [观看视频](https://www.youtube.com/watch?v=fJkbYPyd08w)了解如何打包要分发的 .NET 应用，然后[查看此页面](../porting/desktop-to-uwp-packaging-dot-net.md)了解更多详细信息。
---
title: 创建新主题作品
author: KevinAsgari
description: 了解如何使用 Windows 通用开发人员中心 (UDC) 为 Xbox Live 创建新主题作品。
ms.assetid: b8bd69e6-887a-4b1f-a42d-8affdbec0234
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 72126b51c4e155babad6cfee737e21d6102b03e5
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2018
ms.locfileid: "4680143"
---
# <a name="create-a-new-title-for-xbox-live"></a>为 Xbox Live 创建新主题作品

## <a name="introduction"></a>简介

编写任意代码之前，必须在你的服务配置门户上设置新主题作品。  你可以在 [Xbox Live 服务配置](../xbox-live-service-configuration.md)中了解有关服务配置的详细信息

本文将通过以下假设引导你完成这一过程

1. 你正在开发通用 Windows 平台 (UWP) 主题作品。  UWP 主题作品在 Xbox One、Windows 10 台式电脑和移动设备上运行
2. 要在 Windows 开发人员中心上配置你的游戏[http://dev.windows.com/](http://dev.windows.com)。  如有疑问，应使用 Windows 开发人员中心。
3. 你正在使用带有自定义游戏引擎的 Visual Studio 或 Unity。
4. 开发计算机运行的是 Windows 10。

假如以上条件均成立，本文的其余部分将介绍在 Windows 开发人员中心上配置主题作品、创建新项目以及编写和测试 Xbox Live 登录代码需要的所有内容。

> [!NOTE]
> 如果你是 Xbox Live 创意者计划的一员，则上述假设适用你，并应遵循本文中的内容。

## <a name="dev-center-setup"></a>开发人员中心设置

你需要在 [Windows 开发人员中心](http://dev.windows.com)上创建一个支持 Xbox Live 的主题作品，作为所有可用的 Xbox Live 功能的先决条件。

### <a name="create-a-microsoft-account"></a>创建 Microsoft 帐户
如果你没有 Microsoft 帐户 (也称为 MSA)，你将需要先创建一个在[https://go.microsoft.com/fwlink/p/?LinkID=254486](https://go.microsoft.com/fwlink/p/?LinkID=254486)。  如果你有 Office 365 帐户、使用 Outlook.com 或拥有 Xbox Live 帐户，则你可能已经有 MSA 了。

### <a name="register-as-an-app-developer"></a>注册成为应用开发人员。
你需要先注册成为应用开发人员，然后才可以在开发人员中心创建新主题作品。

若要注册，请转到https://developer.microsoft.com/en-us/store/register并按照注册流程。

### <a name="create-a-new-uwp-title"></a>创建新的 UWP 主题作品
接下来，你需要一个在开发人员中心上定义了的 UWP 主题作品。  需要先转到仪表板才能执行此操作

![](../images/getting_started/first_xbltitle_dashboard.png)

<p>
</p>
<br>
<p>
</p>

在仪表板上单击后，创建一个新主题作品。  你需要保留一个名称。

![](../images/getting_started/first_xbltitle_newapp.png)

然后，你将转到应用的*应用概述*页。  要在其中配置 Xbox Live 的主要页面位于“服务”->“Xbox Live”菜单下，如下所示。

![](../images/getting_started/first_xbltitle_leftnav.png)

<div id="createxblaccount"></div>

## <a name="create-an-xbox-live-account"></a>创建 Xbox Live 帐户
你需要一个 Xbox Live 帐户才能登录 Xbox Live。  如果你已经有一个用于登录 Xbox One 主机（或 Windows 10 上的 Xbox 应用）的帐户，则可以使用该帐户。

否则，你应打开电脑上的 Xbox 应用，然后使用 Microsoft 帐户登录。  然后，将启用该帐户，以用于 Xbox Live。

你可以通过进入*开始菜单*并键入“Xbox”查找 Xbox 应用，如下所示。

![](../images/getting_started/first_xbltitle_xboxapp.png)

## <a name="next-steps"></a>后续步骤
至此，你已经创建了一个新主题作品，接下来可以在游戏引擎、Visual Studio 或选择的生成环境中设置支持 Xbox Live 的主题作品。

请参阅[集成 Xbox Live 的分步指南](partners-step-by-step-guide.md)

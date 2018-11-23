---
title: 创建新主题作品
author: KevinAsgari
description: 了解如何通过使用合作伙伴中心为 Xbox Live 创建新主题作品。
ms.assetid: b8bd69e6-887a-4b1f-a42d-8affdbec0234
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b072ba3112a363111bce751069bccebfaf7465da
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2018
ms.locfileid: "7554555"
---
# <a name="create-a-new-title-for-xbox-live"></a>为 Xbox Live 创建新主题作品

## <a name="introduction"></a>简介

编写任意代码之前，必须在你的服务配置门户上设置新主题作品。  你可以在 [Xbox Live 服务配置](../xbox-live-service-configuration.md)中了解有关服务配置的详细信息

本文将通过以下假设引导你完成这一过程

1. 你正在开发通用 Windows 平台 (UWP) 主题作品。  UWP 主题作品在 Xbox One、Windows 10 台式电脑和移动设备上运行
2. 你要在[合作伙伴中心](https://partner.microsoft.com/dashboard)配置你的游戏。
3. 你正在使用带有自定义游戏引擎的 Visual Studio 或 Unity。
4. 开发计算机运行的是 Windows 10。

假如上述条件均成立，本文的其余部分将介绍通过获取在合作伙伴中心、 创建新项目和 Xbox Live 登录代码编写和测试配置主题作品所需的所有内容。

> [!NOTE]
> 如果你是 Xbox Live 创意者计划的一员，则上述假设适用你，并应遵循本文中的内容。

## <a name="partner-center-setup"></a>合作伙伴中心设置

你需要作为先决条件任何 Xbox Live 功能在[合作伙伴中心](https://partner.microsoft.com/dashboard)中创建一个支持 Xbox Live 的主题作品。

### <a name="create-a-microsoft-account"></a>创建 Microsoft 帐户
如果你没有 Microsoft 帐户 (也称为 MSA)，你将需要首先创建一个在[https://go.microsoft.com/fwlink/p/?LinkID=254486](https://go.microsoft.com/fwlink/p/?LinkID=254486)。  如果你有 Office 365 帐户、使用 Outlook.com 或拥有 Xbox Live 帐户，则你可能已经有 MSA 了。

### <a name="register-as-an-app-developer"></a>注册成为应用开发人员。
你将需要先注册成为应用开发人员，然后允许你在合作伙伴中心中创建新主题作品。

若要注册，请转到https://developer.microsoft.com/en-us/store/register并按照注册流程。

### <a name="create-a-new-uwp-title"></a>创建新的 UWP 主题作品
接下来，你需要在合作伙伴中心中定义一个 UWP 主题作品。  需要先转到仪表板才能执行此操作

![](../images/getting_started/first_xbltitle_dashboard.png)

<p>
</p>
<br>
<p>
</p>

然后，创建新主题作品。  你需要保留一个名称。

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

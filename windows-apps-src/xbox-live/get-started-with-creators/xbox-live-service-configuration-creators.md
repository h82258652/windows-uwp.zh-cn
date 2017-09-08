---
title: "Xbox Live 创意者服务配置"
author: KevinAsgari
description: "了解创意者计划的 Xbox Live 服务配置。"
ms.assetid: 22b8f893-abb3-426e-9840-f79de0753702
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "xbox live, xbox, 游戏, uwp, windows 10, xbox one"
ms.openlocfilehash: 5cb146f3cd316a88a66bcc3057e30d11baa3aab1
ms.sourcegitcommit: a7a1b41c7dce6d56250ce3113137391d65d9e401
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2017
---
# <a name="xbox-live-service-configuration-for-the-creators-program"></a>创意者计划的 Xbox Live 服务配置

## <a name="what-is-service-configuration"></a>什么是服务配置？

你可能熟悉 Xbox Live 的一些功能，例如[成就](../achievements-2017/achievements.md)、[排行榜](../leaderboards-and-stats-2017/leaderboards.md)和[匹配](../multiplayer/multiplayer-concepts.md#SmartMatch)。

如果不熟悉的话，我们将以排行榜为例简要介绍一下。  排行榜让玩家可以查看表示自己相比其他玩家取得的成就的值。  例如，街机游戏中的高分、赛车游戏中的圈速，或第一人称射击游戏中的爆头数。  但不同的是，街机只显示在该物理计算机上玩过的玩家所取得的最高分，而 Xbox Live 可以显示来自世界各地的高分。

不过，要做到这一点，你需要执行某种一次性配置，使 Xbox Live 了解你的排行榜。  例如，值应以升序还是降序排序，以及应对哪部分数据进行排序。

可在 Xbox Live 创意者计划的[开发人员中心](http://dev.windows.com)中执行此配置，并请参阅 [ Xbox Live 入门](get-started-with-xbox-live-creators.md)了解如何进行设置。

## <a name="get-your-ids"></a>获取你的 ID

若要启用 Xbox Live 服务，你需要获取多个 ID，以配置你的开发工具包和主题作品。 这些 ID 可通过执行 Xbox Live 服务配置获得。

如果你当前在开发人员中没有主题作品，请参阅[创建和测试新的创意者主题作品](create-and-test-a-new-creators-title.md)获取指导信息。

### <a name="critical-ids"></a>重要 ID

对于开发适用于 Xbox One 的标题和应用程序，下面 3 个 ID 至关重要：沙盒 ID、TitleID 和服务配置 ID (SCID)。

虽然需要拥有沙盒 ID 才能使用开发工具包，但是，初始开发时不需要 TitleID 和 SCID，但是，只要使用 Xbox Live 服务就需要用到。 因此，我们建议你一次性获取所有三个 ID。

#### <a name="sandbox-ids"></a>沙盒 ID

沙盒在开发期间为你的开发工具包提供了内容隔离，可确保你拥有一个全新的环境，用于开发和测试你的主题作品。 沙盒 ID 用于标识沙盒。 主机一次只能访问一个沙盒，尽管一个沙盒可供多个主机访问。

沙盒 ID 区分大小写。

你可以在“Xbox Live”根配置页面上获取你的沙盒 ID，如下所示：

![](../images/getting_started/devcenter_sandbox_id.png)

#### <a name="service-configuration-id-scid"></a>服务配置 ID (SCID)

在开发过程中，你将创建事件、成就和其他一系列联机功能。 这些内容都属于你的服务配置，需要 SCID 才能访问。 给定的主题作品可能有多个服务配置；每个服务配置都有其自己的 SCID。

SCID 区分大小写。

若要在开发人员中心上检索你的 SCID，请导航到“Xbox Live 服务”部分，然后转到 *Xbox Live 设置*。  你的 SCID 将显示在下表中：

![](../images/getting_started/devcenter_scid.png)

#### <a name="titleid"></a>TitleID

TitleID 用于向 Xbox Live 服务唯一地标识你的标题。 该 ID 用于整个服务中，使你的用户可以访问你的主题作品的实时内容、其用户统计数据和成就等内容，并支持实时多人游戏功能。

主题作品 ID 可以区分大小写，具体视使用方式和位置而定。

开发人员中心上的主题作品 ID 与 *Xbox Live 设置*页上的 SCID 位于同一个表中。

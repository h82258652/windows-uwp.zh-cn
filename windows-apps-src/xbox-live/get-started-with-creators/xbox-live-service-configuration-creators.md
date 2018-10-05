---
title: Xbox Live 创意者服务配置
author: KevinAsgari
description: 了解创意者计划的 Xbox Live 服务配置。
ms.assetid: 22b8f893-abb3-426e-9840-f79de0753702
ms.author: kevinasg
ms.date: 10/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f42379b5225553f03b9b9af8149d6cb888377a36
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4358717"
---
# <a name="xbox-live-service-configuration-for-the-creators-program"></a>创意者计划的 Xbox Live 服务配置

## <a name="what-is-service-configuration"></a>什么是服务配置？

你可能熟悉 Xbox Live 的一些功能，例如[排行榜](../leaderboards-and-stats-2017/leaderboards.md)和[连接存储](../storage-platform/connected-storage/connected-storage-technical-overview.md)。

如果不熟悉的话，我们将以排行榜为例简要介绍一下。 排行榜让玩家可以查看表示自己相比其他玩家取得的成就的值。 例如，街机游戏中的高分、赛车游戏中的圈速，或第一人称射击游戏中的爆头数。 但不同的是，街机只显示在该物理计算机上玩过的玩家所取得的最高分，而 Xbox Live 可以显示来自世界各地的高分。

不过，要做到这一点，你需要执行某种一次性配置，使 Xbox Live 了解你的排行榜。 例如，值应以升序还是降序排序，以及应对哪部分数据进行排序。

可在 Xbox Live 创意者计划的[开发人员中心](http://dev.windows.com)中执行此配置，并请参阅 [ Xbox Live 入门](get-started-with-xbox-live-creators.md)了解如何进行设置。

## <a name="get-your-ids"></a>获取你的 ID

若要启用 Xbox Live 服务，你需要获取多个 ID，以配置你的开发环境和作品。 这些 ID 可通过更新你的 Xbox Live 服务配置获得。

如果你当前在开发人员中没有主题作品，请参阅[创建和测试新的创意者主题作品](create-and-test-a-new-creators-title.md)获取指导信息。

### <a name="critical-ids"></a>重要 ID

对于开发适用于 Xbox One 的作品和应用程序，下面 3 个 ID 至关重要：沙盒 ID、作品 ID 和服务配置 ID (SCID)。

虽然需要拥有沙盒 ID 才能设置你的开发环境，但是，初始开发时不需要作品 ID 和 SCID，只要使用 Xbox Live 服务时才需要用到这两个 ID。 因此，我们建议你一次性获取所有三个 ID。 你可以在“Xbox Live”根配置页面上查看所有 ID，如下所示：

![](../images/getting_started/devcenter_sandbox_id.png)

#### <a name="sandbox-ids"></a>沙盒 ID

沙盒在开发期间为你的环境提供内容隔离，确保它是全新的环境，适合开发和测试你的作品。 沙盒 ID 用于标识你的沙盒。 主机一次只能访问一个沙盒，尽管一个沙盒可供多个主机访问。

沙盒 ID 区分大小写。

#### <a name="service-configuration-id-scid"></a>服务配置 ID (SCID)

在开发过程中，你将创建统计数据、排行榜和其他一系列在线功能。 这些内容都属于你的服务配置，需要 SCID 才能访问。 SCID 区分大小写。

#### <a name="title-id"></a>作品 ID

作品 ID 用于向 Xbox Live 服务唯一地标识你的作品。 该 ID 用于整个服务中，使你的用户可以访问你的主题作品的实时内容、其用户统计数据和成就等内容，并支持实时多人游戏功能。

作品 ID 可以区分大小写，具体视使用方式和位置而定。

## <a name="publish-your-xbox-live-service-configuration"></a>发布你的 Xbox Live 服务配置

在对游戏的 Xbox Live 配置进行更改时，你需要先发布更改，然后再由 Xbox Live 的其余部分进行选取，并在游戏中看到。 如果你的游戏还处于调试阶段，则可以将其发布到你自己的开发沙盒。 利用开发沙盒，可让你在隔离环境中运行对游戏的更改。 当游戏发布给公众后，Xbox Live 配置将自动发布到 RETAIL 沙盒。
默认情况下，Xbox One 主机和 Windows 10 电脑均位于 RETAIL 沙盒中。

在 Xbox Live 配置页中，单击**测试**按钮，将当前 Xbox Live 配置发布到开发沙盒。

![](../images/creators_udc/creators_udc_xboxlive_config_test.png)
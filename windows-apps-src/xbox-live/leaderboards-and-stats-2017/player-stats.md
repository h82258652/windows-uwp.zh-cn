---
title: 玩家统计数据
author: KevinAsgari
description: 了解如何在 Xbox Live 中设置玩家统计数据。
ms.assetid: 5ec7cec6-4296-483d-960d-2f025af6896e
ms.author: kevinasg
ms.date: 11/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 玩家统计数据, 排行榜
ms.localizationpriority: low
ms.openlocfilehash: c3a6f5ec0cbe983d07bc98f5cf4fd7c92f10614c
ms.sourcegitcommit: 929fa4b3273862dcdc76b083bf6c3b2c872dd590
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2018
ms.locfileid: "1935608"
---
# <a name="player-stats"></a>玩家统计数据

如[数据平台概述](../data-platform/data-platform.md)中所述，统计数据是要跟踪的玩家相关信息的重要组成部分。 例如*爆头数*或*单圈最快时间*。

这些统计数据可以显示在[游戏中心](../data-platform/designing-xbox-live-experiences.md)内，也可以用于填充排行榜。 例如，如果你拥有*单圈最快时间*的统计数据，则可以拥有相应的排行榜，以显示全球玩家的单圈时间。

## <a name="defining-stats"></a>定义统计数据

对于某些类型的统计数据，你将需要在开发人员中心上对其进行配置。 我们将在下面列出这些类型。

### <a name="what-needs-to-be-configured"></a>需要配置的内容

一般情况下，统计数据不需要配置，但是，如果你的统计数据是“特别推荐的统计数据”，则你需要在开发人员中心上对此统计数据进行定义。 特别推荐的统计数据将显示在你的作品的游戏中心上。 这些统计数据将使你的游戏中心看起来更有趣，并自动生成排行榜，以确保玩家始终参与其中。 你还可以在自己的作品中使用它们来引起玩家对某些游戏机制的关注，因为这些机制显示给 Xbox Live 上的所有玩家，即使他们没有你的作品。

将为特别推荐的统计数据自动提供全球历史排行榜和每月排行榜。 这些排行榜显示在游戏中心内，并将说明玩家与其好友的进度对比情况。 它们也可以像常规排行榜一样显示在你的作品中。 例如：

![](../images/omega/gamehub_featuredstats.png)

如果你的统计数据对应于*全球排行榜*，则你还需要在开发人员中心上定义它。 [排行榜](leaderboards.md)一文将更详细地介绍*全球排行榜*。 简而言之，全球排行榜就是将玩家与全球的其他所有玩家放在一起进行排名。

如果你的统计数据仅对应于*社交排行榜*，则你不需要在开发人员中心上执行任何配置。 [排行榜](leaderboards.md)一文将更详细地介绍*社交排行榜*。 简而言之，社交排行榜就是只将玩家与其好友（关注他们的人，以及他们关注的人）放在一起进行排名。

### <a name="configured-on-dev-center"></a>在开发人员中心上配置

配置体验取决于你所使用的数据平台的版本。

> **注意** 如果要让作品成为 Xbox Live 创意者计划的一部分，你需要使用 Data Platform 2017。

如果你已加入 Xbox Live 创意者计划，或使用了 Data Platform 2017，请参阅[在 Windows 开发人员中心上配置特别推荐的统计数据或排行榜](../configure-xbl/dev-center/featured-stats-and-leaderboards.md)

如果是在 XDK 和更早 UWP 作品中使用的 Data Platform 2013，则可通过统计数据进一步赢取成就。 如果你是托管合作伙伴，并且需要更多信息，请参阅 [Data Platform 2013 的用户统计数据](https://developer.microsoft.com/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/user-stats)。  

## <a name="next-steps"></a>后续步骤

现在，你已配置了一些玩家统计数据和特别推荐的统计数据。 你可以随时创建排行榜。 请参阅[排行榜](leaderboards.md)以开始操作。

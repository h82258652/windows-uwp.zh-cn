---
title: Xbox Live 数据平台
description: Xbox Live 数据平台概述，包括用于管理成就、玩家统计信息和排行榜的各项服务。
ms.assetid: a8bb7c4f-09fe-4dba-b3ce-1fab60453831
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 统计信息, 成就, 排行榜, 数据平台
ms.localizationpriority: medium
ms.openlocfilehash: f9a14c508e265cdfc510896eb621466d03c66776
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634832"
---
# <a name="xbox-live-data-platform---stats-leaderboards-achievements"></a>Xbox Live 数据平台 - 统计信息、排行榜、成就

将游戏数据写入 Xbox Live 数据平台让你的作品可以作为一项服务运行。 此外，Xbox Live 数据平台还使用统计信息、排行榜和成就来推动用户参与你的作品，并在主机 shell 和 Xbox 应用中呈现特别推荐的统计信息。

赛车游戏状态的一个示例可能是总的争用 （例如运行在拖动条模式下，这可用于创建排行榜进行比较您的分数针对社区，并且会反映在你在真实时间中的丰富的状态字符串"GoTeamEmily 播放拖动条形模式：523 争用已完成"）。 双车短程赛车道模式中的比赛总数还可以用作匹配条件，以确保拥有相似经验的玩家被匹配在一起的可能性更大。

你的游戏可以根据玩家统计信息显示排行榜。例如，排行榜可能是所完成比赛数的全球排名。 你可以直接使用 Xbox Live API 来调用这些服务，或使用游戏引擎（如 Unity）中的包装器。

你可以将某些统计信息指定为特别推荐的统计信息。特别推荐的统计信息显示在游戏的 GameHub 中，将玩家与其好友进行比较。

成就不是由统计信息支持，你的作品决定何时解锁成就。 例如，你的作品可能有成就：在一分钟内完成比赛。 你的作品将跟踪解锁此成就所需要的参数。 在此示例中，将由作品决定是否测量比赛使用的时间，然后对成就给予奖励。 通常会在成就完成时奖励玩家分数。 每个成就所奖励的玩家分数多少由你决定。

## <a name="features"></a>功能 ##
以下主题提供有关 Xbox Live 数据平台功能的详细信息：

* [播放机统计信息](../leaderboards-and-stats-2017/player-stats.md)
* [获得的成就](../achievements-2017/achievements.md)
* [排行榜](../leaderboards-and-stats-2017/leaderboards.md)

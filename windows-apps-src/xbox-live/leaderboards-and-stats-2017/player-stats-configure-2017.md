---
title: 配置 Stats and Leaderboards 2017
author: KevinAsgari
description: 了解如何使用 Data Platform 2017 在通用开发人员中心上配置 Xbox Live 的特别推荐的统计数据和排行榜。
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7b2516987d1f56ba1da482854b6c6a390e1001db
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2018
ms.locfileid: "4534918"
---
# <a name="configuring-featured-stats-or-leaderboards-on-universal-dev-center-with-data-platform-2017"></a>使用 Data Platform 2017 在通用开发人员中心上配置 Xbox Live 的特别推荐的统计数据和排行榜

有了 Data Platform 2017，你只需在下面两种情况下配置统计数据即可：

* 统计数据用作全球排行榜的基础，或者
* 统计数据是要显示在游戏中心页的特别推荐的玩家统计数据。

无论在哪种情况下，你都必须同时配置统计数据和排行榜。 每个排行榜都基于统计数据，因此，如果不能配置统计数据，也就无法配置关联的全球排行榜，即使你不打算将排行榜用于特别推荐的玩家统计数据。

你不需要配置不是特别推荐的玩家统计数据的统计数据，也不需要配置全球排行榜不使用的统计数据。

## <a name="configure-a-global-leaderboard-and-an-associated-player-stat"></a>配置全球排行榜和关联的玩家统计数据

首先，转到作品中的“玩家统计数据”部分，如下所示。

![](../images/omega/dev_center_player_stats_creators.png)

然后，单击 `Create your first featured stat/leaderboard` 按钮，填写此对话框，并单击“保存”。

![](../images/omega/dev_center_player_stats_creators_leaderboard.png)

用户将在游戏中心内看到 `Display name` 字段。  此字符串可在 `Localize strings` 部分中进行本地化。  单击 `Localize strings` 部分中的 `Show Options`，以查看有关如何本地化这些字符串的详细信息。

`ID` 字段是统计数据名称，将作为你通过作品代码更新统计数据时对其引用的方式。   有关更多详细信息，请参阅[更新统计数据](player-stats-updating.md)部分。

`Format`是统计数据的数据格式。

`Display Logic` 让你可在 `Player progression`、`Personal best` 和 `Counter` 之间进行选择：
- 玩家进度：表示各个玩家在游戏中的等级或级数。  用户将会看到最后一个值集。  例如，本局中的位置。
- 个人最佳成绩：表示玩家发布过的当前最佳分数。 用户将会看到最大或最小值集，这取决于排序顺序。  例如，单圈最快时间。
- 计数器：可添加到其他玩家的统计数据中以计算总计数。  

`Sort` 字段可用于更改排行榜的排序顺序。

你还可以选择让此统计数据成为 `Featured Stat`，只需单击 `Feature on players' profiles` 即可。  

## <a name="configure-featured-stats"></a>配置特别推荐的统计数据

定义玩家统计数据时，你可以选择选中 `Featured Stat`。  请注意以下要求

| 开发人员类型 | 要求 |
|----------------|-------------|
| Xbox Live 创意者计划 | 不要求将任何统计数据指定为特别推荐的统计数据。尽管你最多只能拥有 10 个 |
| ID@Xbox 和 Microsoft 合作伙伴 | 你必须在 3 到 10 之间指定特别推荐的统计数据 |

## <a name="next-steps"></a>后续步骤

接下来，你将需要通过作品代码更新统计数据。  有关更多详细信息，请参阅[更新统计数据](player-stats-updating.md)。
---
title: "向 Unity 项目添加玩家统计数据和排行榜"
author: KevinAsgari
description: "了解如何使用 Xbox Live Unity 插件向 Unity 项目添加玩家统计数据和排行榜。"
ms.assetid: 756b3c31-a459-4ad2-97af-119adcd522b5
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "xbox live, xbox, 游戏, uwp, windows 10, xbox one, Unity, 创意者"
ms.openlocfilehash: f27b909f3176e1e896dde39b117bb49e8ecf6fdc
ms.sourcegitcommit: 90fbdc0e25e0dff40c571d6687143dd7e16ab8a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2017
---
# <a name="add-player-stats-and-leaderboards-to-your-unity-project"></a>向 Unity 项目添加玩家统计数据和排行榜

> **注意：**只建议 [Xbox Live 创意者计划](../developer-program-overview.md)成员使用 Xbox Live Unity 插件，因为当前不支持成就或多人游戏。

向 Unity 项目添加 [Xbox Live 登录](sign-in-to-xbox-live-in-unity.md)后，下一步是添加玩家统计数据和基于其玩家统计数据的排行榜。

通过 Xbox Live Unity 插件，你可以轻松地在 Unity 项目中添加玩家统计数据和排行榜。 与登录步骤类似，你可以使用包含的 prefabs，也可以将包含的脚本附加到你的自定义对象中。

> **注意：**此主题假设你已在 Unity 项目中设置了 Xbox Live 插件。 有关如何执行此操作的信息，请参阅[在 Unity 中配置 Xbox Live](configure-xbox-live-in-unity.md)。

## <a name="player-stats"></a>玩家统计数据

玩家统计数据是你希望为玩家跟踪的任何有意义的统计信息。 使用 Xbox Live 跟踪的统计数据应该是与玩家相关、并用某种方式显示的统计数据。 这些玩家统计数据最常用于生成排行榜，玩家可以查看排行榜来确定他们相对于其他玩家的表现排名。 某些玩家数据可以标记为“特别推荐的玩家统计数据”，这意味着玩家统计数据将在游戏的“游戏中心”页显示。

个人统计数据不能是复杂对象，但必须代表单个值。 Xbox Live Unity 插件包含整数、双精度和字符串统计数据的 prefabs。 此外，还为可以扩展到其他数据类型的基本统计数据对象提供脚本。

有关玩家统计数据的详细信息，请参阅[玩家统计数据](../leaderboards-and-stats-2017/player-stats.md)。

> **注意：**为通过 Xbox Live 服务使用玩家统计数据或排行榜，你必须拥有成功登录的用户才能访问所有数据。

## <a name="using-the-player-stat-prefabs"></a>使用玩家统计数据 prefabs

Xbox Live Unity 插件中提供了多个与玩家统计数据相关的 prefabs 供你使用：

* IntegerStat - 一个可以表示为整数值的统计数据，例如，一轮中的总击杀数。
* DoubleStat - 一个可表示为浮点值的统计数据，例如击杀/死亡比。
* StringStat - 可表示为字符串值（通常是枚举）的统计数据，例如，一轮中获得的排名（例如，“金牌”、“银牌”或“铜牌”）。
* StatPanel - 可以用来显示统计数据的当前值的示例 UI。

要添加玩家统计数据，只需将与统计数据的数据类型匹配的 prefab 拖动到场景上。 在统计数据的 Unity 检查器中，你可以指定三个值：

* 统计数据名称。
* 统计数据的显示名称（此名称在 StatPanel prefab UI 中显示）。
* 场景启动时，统计数据的初始值。

你可以使用 **StatPanel** prefab 显示统计数据的值。 只需将 **StatPanel** prefab 拖动到场景中，并通过使用 **StatPanel** 对象的 Unity 检查器指定要显示的统计数据。

### <a name="manipulating-the-player-stat-values"></a>操作玩家统计数据值

玩家统计数据对象有多个函数，你可以调用这些函数来调整统计数据的值。 可以从其他例程调用这些函数，或将函数绑定到 UI 元素。 你可以查看 **DoubleStat**、**IntegerStat** 和 **StringStat** 脚本中的函数示例，以更改统计数据的值。 你可以修改或创建新脚本，用更复杂的函数和逻辑表示统计数据。 新的统计数据类应扩展在 **StatBase** 脚本中定义的 **StatBase** 类。

例如，作为一个简单的测试，你可以向场景中添加 UI 按钮，并在按钮的 **OnClick** 方法中的 Unity 检查器中添加 **IntegerStat** 对象，然后，调用 **Increment()** 函数，在每次单击按钮时将统计数据的值增加一。

如果你还将统计数据绑定到了 **StatPanel** 对象，你可以看到，统计数据值会在你每次单击按钮时更新。

## <a name="leaderboards"></a>排行榜

排行榜表示一份对已获得统计数据的“最佳”值的玩家进行排序的编号列表。 例如，排行榜可能会列出在赛圈时用时最快的用户，以便玩家可以将他们的最好比赛时间与其他玩家获得的最好比赛时间进行对比。

排行榜基于由游戏发送给 Xbox Live 服务的玩家统计数据。 因此，排行榜数据为只读数据，因为你无法直接进行修改。

Xbox Live Unity 插件提供了一个示例排行榜 prefab，你可以用它来理解如何在游戏中实现排行榜。

有关排行榜的详细信息，请参阅[排行榜](../leaderboards-and-stats-2017/leaderboards.md)。

## <a name="using-the-leaderboard-prefabs"></a>使用排行榜 prefabs

Xbox Live Unity 插件包含两个排行榜 prefabs：

* LeaderBoard - 一个表示排行榜的对象，并包含简单 UI 以显示排行榜中的值。
* LeaderboardEntry - 一个表示单行排行榜的对象。

你可以将**排行榜** prefab 拖动到场景上。 在 Unity 检查器中，你可以设置以下属性：

* 排行榜名称 - 如果你在服务配置中有定义的全局排行榜，此名称应该与配置的排行榜的名称匹配。 否则，此名称应该与玩家统计数据的值匹配。
* 显示名称 - 在 prefabde UI 中显示的名称
* 条目计数 - 每页要显示的数据的行数。

在 Unity 编辑器中，**排行榜** prefab 将始终显示相同的 mock 数据，无论检查器设置为何。 必须生成项目并将其导出到 Visual Studio，并使用授权的用户登录，以查看真实的数据值。 有关详细信息，请参阅[在 Unity 中配置 Xbox Live](configure-xbox-live-in-unity.md)。

## <a name="see-also"></a>另请参阅

* [在 Xbox Live 中登录 Unity](sign-in-to-xbox-live-in-unity.md)
* [在 Unity 中配置 Xbox Live](configure-xbox-live-in-unity.md)

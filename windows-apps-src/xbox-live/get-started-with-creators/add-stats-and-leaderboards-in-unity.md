---
title: 向 Unity 项目添加玩家统计数据和排行榜
description: 了解如何使用 Xbox Live Unity 插件向 Unity 项目添加玩家统计数据和排行榜。
ms.assetid: 756b3c31-a459-4ad2-97af-119adcd522b5
ms.date: 10/19/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, Unity, 创意者
ms.localizationpriority: medium
ms.openlocfilehash: 17af9afc8a9048e7222115d2afdc6108d25df1d7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658132"
---
# <a name="add-player-stats-and-leaderboards-to-your-unity-project"></a>向 Unity 项目添加玩家统计数据和排行榜

> [!IMPORTANT]
> Xbox Live Unity 插件不支持成就或多人在线游戏，只建议 [Xbox Live 创意者计划](../developer-program-overview.md)成员使用。

向 Unity 项目添加 [Xbox Live 登录](unity-prefabs-and-sign-in.md)后，下一步是添加玩家统计数据和基于其玩家统计数据的排行榜。

通过 [Xbox Live Unity 插件](https://github.com/Microsoft/xbox-live-unity-plugin)，你可以轻松地在 Unity 项目中添加玩家统计数据和排行榜。 与登录步骤类似，可以选择使用包含的 prefab，也可以将包含的脚本附加到自定义游戏对象中。

## <a name="prerequisites"></a>必备条件
1. [配置 Xbox Live 在 Unity 中](configure-xbox-live-in-unity.md)
2. [在 Unity 中登录到 Xbox Live](unity-prefabs-and-sign-in.md)

## <a name="player-stats"></a>玩家统计数据

玩家统计数据是你希望为玩家跟踪的任何有意义的统计信息。 使用 Xbox Live 跟踪的统计数据应该是与玩家相关、并用某种方式显示的统计数据。 这些玩家统计数据最常用于生成排行榜，玩家可以查看排行榜来确定他们相对于其他玩家的表现排名。 所有玩家统计数据都将被视为“特别推荐的统计数据”，这意味着玩家统计数据将在游戏的“游戏中心”页显示。

玩家统计数据必须表示单个值。 Xbox Live Unity 插件包含整数、双精度和字符串统计数据的 prefabs。此外，可以扩展到其他数据类型的基本统计信息对象提供了一个脚本。

有关玩家统计数据的详细信息，请参阅[玩家统计数据](../leaderboards-and-stats-2017/player-stats.md)。

> [!NOTE]
> 为通过 Xbox Live 服务使用玩家统计数据或排行榜，你必须拥有成功登录的用户才能访问所有数据。

## <a name="using-the-player-stat-prefabs"></a>使用玩家统计数据 prefabs

Xbox Live Unity 插件中提供了多个与玩家统计数据相关的 prefabs 供你使用：

* IntegerStat:可表示为整数值，例如会有一轮中的总行数统计信息。
* DoubleStat:状态可以表示为浮点值，如 kill/死亡比率。
* StringStat:可以表示为一个字符串值，通常一个枚举，如获得一轮的排名，例如"黄金"、"白银"或"青铜"状态。
* StatPanel:示例 UI，可用于显示状态的当前值。

要添加玩家统计数据，只需将与统计数据的数据类型匹配的 prefab 拖动到场景上。 在统计数据的 Unity 检查器中，你可以指定三个值：

* 统计信息的 ID。这必须匹配在合作伙伴中心中配置的 ID，区分大小写。
* 统计数据的显示名称（此名称在 StatPanel prefab UI 中显示）。
* 场景启动时，统计数据的初始值。

你可以使用 **StatPanel** prefab 显示统计数据的值。若要执行此操作，将拖动**StatPanel**拖到场景预设。 可以通过在 Unity 检查器中将统计数据 gameobject 拖动到 **StatPanel** 对象的 **Stat** 字段中指定要显示的统计数据。

### <a name="manipulating-the-player-stat-values"></a>操作玩家统计数据值

玩家统计数据对象有多个函数，你可以调用这些函数来调整统计数据的值。可以从其他例程调用这些函数，或将函数绑定到 UI 元素。 你可以查看 **DoubleStat**、**IntegerStat** 和 **StringStat** 脚本中的函数示例，以更改统计数据的值。可以修改或创建新脚本，用于表示使用更复杂的函数和逻辑的统计信息。 新的统计数据类应扩展在 `StatBase` 脚本中定义的 `StatBase` 类。

例如，作为简单的测试，你可以向场景中添加 UI 按钮，并在 Unity 检查器中在该按钮的 `OnClick` 事件中添加 **IntegerStat** 对象，然后，调用 `Increment()` 函数来在每次单击该按钮时将统计数据的值增加一。

如果你还将统计数据绑定到了 **StatPanel** 对象，你可以看到，统计数据值会在你每次单击按钮时更新。

每次你更新统计数据（如递增、递减等）时，值都会在本地更新。 要将这些统计更新反映在 Xbox Live 中，须执行两个操作。 首先，需要使用某个 StatisticManager.SetStatistic 函数设置统计值。 有三个 `StatisticManager` 函数可用于设置统计值，分别是 `StatisticManager.SetStatisticIntegerData(XboxLiveUser user, String statName, Int64 value)`、`StatisticManager.SetStatisticNumberData(XboxLiveUser user, String statName, Double value)` 和 `StatisticManager.SetStatisticStringData(XboxLiveUser user, String statName, String value)`。 这些函数的每个用于设置在统计信息的数据类型的相应值。您必须执行更新的服务器上，在统计信息的第二个操作是对*刷新*的本地数据。 数据通过 `StatManagerComponent` 脚本每 5 分钟自动刷新。  如果游戏在 5 分钟前结束，则需要确保先手动刷新数据以保证不会丢失进度。 为此，需要调用 `statManagerComponent.RequestFlushToService()` 方法，确保为写入统计数据的 **XboxLiveUser** 调用该方法。

> [!TIP]
> 最佳做法是始终在游戏结束前刷新数据以确保不会丢失进度。

### <a name="checking-and-verifying-stats"></a>检查并验证统计数据

`StatisticManager` 类有两个的函数，可用于检查为 `XboxLiveUser`、`StatisticManager.GetStatisticNames(XboxLiveUser user)` 和 `StatisticManager.GetStatistic(XboxLiveUser user, String statName)` 配置的统计数据。 `GetStatisticNames()` 将提供`list<string>`提供 XboxLiveUser 填充的统计信息名称。 可以使用这些名称通过调用 `GetStatistic()` 函数来调用某项统计数据的当前值。 请务必注意，虽然你可以从 Xbox Live 统计数据服务读取统计数据，但对于游戏逻辑不建议这样做，而应在推送统计数据之后检查统计数据的状态。 该服务仅用于帮助运行排行榜等其他服务，而不作为游戏中统计数据的资料来源。 在你的作品中处理所有统计数据逻辑十分重要，因为 Xbox 服务不会检查你的统计数据，只是简单地接受统计数据的任意赋值作为当前统计数据。

## <a name="leaderboards"></a>排行榜

排行榜表示一份对已获得统计数据的“最佳”值的玩家进行排序的编号列表。例如，排行榜可能会列出在赛圈时用时最快的用户，以便玩家可以将他们的最好比赛时间与其他玩家获得的最好比赛时间进行对比。

排行榜基于由游戏发送给 Xbox Live 服务的玩家统计数据。 因此，排行榜数据为只读数据，因为你无法直接进行修改。

Xbox Live Unity 插件提供了一个示例排行榜 prefab，你可以用它来理解如何在游戏中实现排行榜。

有关排行榜的详细信息，请参阅[排行榜](../leaderboards-and-stats-2017/leaderboards.md)。

## <a name="using-the-leaderboard-prefabs"></a>使用排行榜 prefabs

Xbox Live Unity 插件包含两个排行榜 prefab：

* 排行榜：一个对象，表示排行榜，并包含用于显示排行榜中的值的简单 UI。
* LeaderboardEntry:一个对象，表示单行排行榜。

你可以将**排行榜** prefab 拖动到场景上。 在 Unity 检查器中，你可以设置以下属性：

* 统计信息：与关联此排行榜 stat gameobject。
* 排行榜类型：应为排行榜条目返回结果的范围。
* 条目计数：要每页显示的数据行数。

> [!NOTE]
> 排行榜 prefab 的 Stat 部分最初为空白。 请尝试将上述某个统计数据 prefab 拖放到 gameobject 空位来测试此功能。

在 Unity 编辑器中，**排行榜** prefab 将始终显示相同的 mock 数据，无论检查器设置为何。 必须生成项目并将其导出到 Visual Studio，并使用授权的用户登录才能查看真实的数据值。 有关详细信息，请参阅[在 Unity 中配置 Xbox Live](configure-xbox-live-in-unity.md)。

## <a name="see-also"></a>另请参阅

* [在 Unity 中登录到 Xbox Live](unity-prefabs-and-sign-in.md)
* [配置 Xbox Live 在 Unity 中](configure-xbox-live-in-unity.md)
* [排行榜示例场景](setup-leaderboard-example-scene.md)
* [获取排行榜数据](unity-leaderboard-from-scratch.md)

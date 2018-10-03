---
title: 排行榜
author: KevinAsgari
description: 了解如何使用 Xbox Live 排行榜对玩家做以比较。
ms.assetid: 132604f9-6107-4479-9246-f8f497978db7
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 190d54fb53192a1cc798b46a0a4b76d7bdd3e074
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2018
ms.locfileid: "4267535"
---
# <a name="leaderboards"></a>排行榜

## <a name="introduction"></a>简介

如[数据平台概述](../data-platform/data-platform.md)中所述，排行榜不仅是鼓励玩家进行竞争的好办法，也是确保玩家积极参与尝试突破个人及好友的历史最好成绩的绝佳方式。

[特别推荐的统计数据](stats2017.md#configured-stats-and-featured-leaderboards)的排行榜始终显示在游戏的游戏中心并时它已固定到主页，有时显示为游戏的 UI 的一部分。 你还可以使用你配置的特别推荐的统计数据以你的游戏内创建排行榜。

## <a name="choosing-good-leaderboards"></a>选择合适的排行榜

正如[玩家统计数据](player-stats.md)中所讨论的，排行榜与所定义的统计数据相对应。  你应该选择与玩家可以努力提升的成就相对应的排行榜。

例如，在赛车游戏中，“单圈最快时间”就是合适的排行榜，因为玩家想要努力缩短其“单圈最快时间”。  其他示例是格斗游戏中的“击杀/死亡”比，或“最大组合大小”。

## <a name="when-to-display-leaderboards"></a>何时显示排行榜

你可以在作品中随时显示排行榜。  你应该选择排行榜不会影响玩游戏或者作品流的时间。  在各局之间或匹配后等，都是合适的时间。

## <a name="how-to-display-leaderboards"></a>如何显示排行榜

有很多选项都可以用于显示 Xbox Live SDK 中提供的排行榜。  如果你要通过 Xbox Live 创意者计划使用 Unity，则可以从使用排行榜 Prefab 入手，以显示你的排行榜数据。  请参阅[在 Unity 中配置 Xbox Live](../get-started-with-creators/configure-xbox-live-in-unity.md) 文章以了解具体信息。

如果你直接对 Xbox Live SDK 进行编码，请阅读下文，以了解你可以使用的 API。

### <a name="programming-guide"></a>编程指南

有几个排行榜 API 可用于获取排行榜的当前状态。  所有 API 都是异步的，不能阻止。  你可以发出获取排行榜数据的请求，并继续执行你的常规游戏处理。  在该服务返回排行榜结果后，你可以在适当的时间显示这些结果。

你应该从该服务请求排行榜数据，略早于要显示该数据的时间，以便玩家不会因为等待显示排行榜而受阻。

你可以在 `leaderboard_service` 命名空间中查看所有排行榜 API。

<table>

<tr>
<td>C++ API</td><td>描述</td>
</tr>

<tr>
<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard(
        const string_t& scid,
        const string_t& name
        );
```

</td>

<td>API 的最基本版本。  这将返回给定排行榜的排行榜值，从排行榜最上方的玩家开始。</td>

</tr>

<tr>
<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard(
    _In_ const string_t& scid,
    _In_ const string_t& name,
    _In_ uint32_t skipToRank,
    _In_ uint32_t maxItems = 0
    );

```

</td>

<td>此 API 提供了某种更大的灵活性，你可以指定要显示的排名（位置），以及要返回的项目的最大值。  例如，如果你想要从位置 1000 开始显示排行榜，则可以使用此 API。</td>

</tr>

<tr>

<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard_skip_to_xuid(
    _In_ const string_t& scid,
    _In_ const string_t& name,
    _In_ const string_t& skipToXuid = string_t(),
    _In_ uint32_t maxItems = 0
    );

```

</td>

<td>

如果你要在排行榜中跳到某个用户，请使用此项。  对于每个 Xbox 用户，`XUID` 是唯一的标识符。  你可以为登录用户或者其任一好友获取 ID，并将该 ID 传入此函数。

</td>

</tr>

</table>

然后，你可以设置要在该服务返回排行榜结果后调用的回调。  我们将在下面介绍此过程的示例。

如果你不熟悉要从这些 API 返回的 `pplx::task`，则这是来自 Microsoft 并行编程库 (PPL) 的异步任务对象。  你可以到 [https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks](https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks) 了解更多。

以下部分介绍如何检索和使用排行榜结果。

### <a name="example"></a>示例

#### <a name="1-create-an-async-task-to-retrieve-leaderboard-results"></a>1. 创建异步任务以检索排行榜结果

第一步是调用排行榜服务，以检索特定排行榜的结果。

```cpp
pplx::task<xbox_live_result<leaderboard_result>> asyncTask;
auto& leaderboardService = xboxLiveContext->leaderboard_service();

asyncTask = leaderboardService.get_leaderboard(m_liveResources->GetServiceConfigId(), LeaderboardIdEnemyDefeats);
```

#### <a name="2-setup-a-callback"></a>2. 设置回调

你可以设置要在返回排行榜结果后调用的[延续任务](https://msdn.microsoft.com/en-us/library/dd492427(v=vs.110).aspx#continuations)。  你可以按照以下方式执行该操作。

```cpp
asyncTask.then([this](xbox::services::xbox_live_result<xbox::services::leaderboard::leaderboard_result> result)
{
    // Handle result here
});
```

此延续任务在最初调用它的对象上下文中调用，将检索可以适合作品的方式显示的 ```leaderboard_result```。


#### <a name="3-display-leaderboard"></a>3. 显示排行榜

排行榜数据包含在 ```leaderboard_result``` 中，并且字段具有自解释性。  有关示例，请参阅下文。

```cpp
auto leaderboard = result.payload();

for (const xbox::services::leaderboard::leaderboard_row& row : leaderboard.rows())
{
    string_t colValues;
    for (auto columnValue : row.column_values())
    {
        colValues = colValues + L" ";
        colValues = colValues + columnValue;
    }
    m_console->Format(L"%18s %8d %14f %10s\n", row.gamertag().c_str(), row.rank(), row.percentile(), colValues.c_str());
}

```
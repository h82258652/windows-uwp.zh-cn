---
title: 排行榜
author: aablackm
description: 了解如何使用 Xbox Live 排行榜对玩家做以比较。
ms.assetid: 132604f9-6107-4479-9246-f8f497978db7
ms.author: aablackm
ms.date: 09/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0e65002e173922a989c8194266a1ef109d24e7e4
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5440313"
---
# <a name="leaderboards"></a>排行榜

## <a name="introduction"></a>简介

如[数据平台概述](../data-platform/data-platform.md)中所述，排行榜不仅是鼓励玩家进行竞争的好办法，也是确保玩家积极参与尝试突破个人及好友的历史最好成绩的绝佳方式。

[特别推荐的统计数据](stats2017.md#configured-stats-and-featured-leaderboards)的排行榜始终显示在游戏的游戏中心和时它已固定到主页，有时显示为游戏的 UI 的一部分。 你还可以使用你配置的特别推荐的统计数据你的游戏内创建排行榜。

## <a name="choosing-good-leaderboards"></a>选择合适的排行榜

正如[玩家统计数据](player-stats.md)中所讨论的，排行榜与所定义的统计数据相对应。  你应该选择与玩家可以努力提升的成就相对应的排行榜。

例如，在赛车游戏中的最快时间是合适的排行榜，因为玩家想要努力缩短其最快时间。  其他示例是格斗游戏中的击杀/死亡比射击游戏，或最大组合大小。

## <a name="when-to-display-leaderboards"></a>何时显示排行榜

你可以在作品中随时显示排行榜。  你应该选择排行榜不会影响玩游戏或者作品流的时间。  在各局之间和匹配后这两个合适的时间。

## <a name="how-to-display-leaderboards"></a>如何显示排行榜

有很多选项都可以用于显示 Xbox Live SDK 中提供的排行榜。  如果你要使用 Xbox Live 创意者计划使用 Unity，则可以从使用排行榜 Prefab 来显示你的排行榜数据。  请参阅[在 Unity 中配置 Xbox Live](../get-started-with-creators/configure-xbox-live-in-unity.md) 文章以了解具体信息。

如果你直接对 Xbox Live SDK 进行编码，请阅读下文，以了解你可以使用的 API。

## <a name="programming-guide"></a>编程指南

有几个排行榜 API 可用于获取排行榜的当前状态。  所有 API 都是异步的，不能阻止。  你可以发出获取排行榜数据的请求，并继续执行你的常规游戏处理。  在该服务返回排行榜结果后，你可以在适当的时间显示这些结果。

你应该从该服务请求排行榜数据，略早于要显示该数据的时间，以便玩家不会因为等待显示排行榜而受阻。

## <a name="leaderboards-2013-apis"></a>排行榜 2013 Api

你可以看到`leaderboard_service`所有统计数据 2013年排行榜 api 的命名空间。

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

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardAsync (
        _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName
        ) 
```

</td>

<td>WinRT C# 代码-获取给定服务配置 ID 和排行榜名称的单个排行榜的排行榜。</td>

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

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardAsync (
         _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName,
         _In_ uint32 skipToRank,
         _In_ uint32 maxItems
        ) 
```

</td>

<td>WinRT C# 代码-获取排行榜的页面的单个排行榜结果为服务配置 ID 和排行榜名称，在"skipToRank"排名的排行榜结果将开始。</td>

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

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardWithSkipToUserAsync (
         _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName,
         _In_opt_ Platform::String^ skipToXboxUserId,
         _In_ uint32 maxItems
        )
```

</td>

<td>WinRT C# 代码-获取开始在指定的玩家，无论玩家的排名的排行榜或分数，按玩家的百分比排名排序</td>

</tr>

</table>

## <a name="2013-c-example"></a>2013 c + + 示例

使用 c + + API 层时然后，你可以设置要从服务返回排行榜结果后调用的回调。  我们将在下面介绍此过程的示例。

如果你不熟悉要从这些 API 返回的 `pplx::task`，则这是来自 Microsoft 并行编程库 (PPL) 的异步任务对象。  你可以到 [https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks](https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks) 了解更多。

以下部分介绍如何检索和使用排行榜结果。

### <a name="1-create-an-async-task-to-retrieve-leaderboard-results"></a>1. 创建异步任务以检索排行榜结果

第一步是调用排行榜服务，以检索特定排行榜的结果。

```cpp
pplx::task<xbox_live_result<leaderboard_result>> asyncTask;
auto& leaderboardService = xboxLiveContext->leaderboard_service();

asyncTask = leaderboardService.get_leaderboard(m_liveResources->GetServiceConfigId(), LeaderboardIdEnemyDefeats);
```

### <a name="2-setup-a-callback"></a>2. 设置回调

你可以设置要返回排行榜结果后调用的[延续任务](https://msdn.microsoft.com/en-us/library/dd492427(v=vs.110).aspx#continuations)。  你可以按照以下方式执行该操作。

```cpp
asyncTask.then([this](xbox::services::xbox_live_result<xbox::services::leaderboard::leaderboard_result> result)
{
    // Handle result here
});
```

此延续任务在最初调用它的对象上下文中调用，将检索可以适合作品的方式显示的 ```leaderboard_result```。


### <a name="3-display-leaderboard"></a>3. 显示排行榜

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

## <a name="2013-winrt-c-example"></a>2013 WinRT C# 示例

使用 WinRT C# 层时将不需要进行单独回调任务并将只需使用`await`关键字调用排行榜服务时。

### <a name="1-access-the-leaderboardservice"></a>1.访问 LeaderboardService

`LeaderboardService`可从检索`XboxLiveContext`创建游戏用户登录时，你将需要该调用排行榜数据。

```csharp
XboxLiveContext xboxLiveContext = idManager.xboxLiveContext;
LeaderboardService boardService = xboxLiveContext.LeaderboardService;
```

### <a name="2-call-the-leaderboardservice"></a>2.调用 LeaderboardService

```csharp
LeaderboardResult boardResult = await boardService.GetLeaderboardAsync(
     xboxLiveConfig.ServiceConfigurationId,
     leaderboardName
     );
```

### <a name="3-retrieve-leaderboard-data"></a>3.检索排行榜数据

`GetLeaderboardAsync()` 返回`LeaderboardResult`这将包含填充命名的排行榜的统计数据。

`LeaderboardResult` 有多个函数和属性来促进排行榜数据的读数。

|属性  |说明  |
|---------|---------|
|公共 IAsyncOperation<LeaderboardResult> GetNextAsync (uint maxItems);     |检索下一组提高排名达到 maxItems 参数的数量。 这是本质上再次调用 `GetLeaderboard()`         |
|公共 LeaderboardQuery GetNextQuery();     |检索可用于进行排行榜调用来检索数据的下一组 LeaderboardQuery。         |
|公共 bool HasNext {get;}    |指定有要检索的多个排行榜行         |
|公共 IReadOnlyList<LeaderboardRow>行 {get;}     | 包含每个排名的排行榜数据行        |
|公共 IReadOnlyList<LeaderboardColumn>列 {get;}     | 构成排行榜的列的列表        |
|公共 uint TotalRowCount {get;}     | 排行榜中的行的总金额        |
|public 字符串 DisplayName {get;}     | 名称以显示排行榜       |

排行榜数据将提供一个页面一次。 你可能会循环访问`LeaderboardResult`若要检索数据的行和列。  
使用`HasNext`布尔和`GetNextAsync()`函数以检索排行榜数据的更高版本页。

```csharp
if (boardResult != null)
{
    foreach (LeaderboardRow row in boardResult.Rows)
    {
        Debug.Write(string.Format("Rank: {0} | Gamertag: {1} | {2}\n", row.Rank, row.Gamertag, row.Values.ToString()));
    }
}
```

## <a name="leaderboard-2017"></a>排行榜 2017

若要使你将使用的统计数据 2017年排行榜服务的调用`StatisticManager`而不是排行榜 api`LeaderboardService`排行榜 api。  

`xbox::services::stats:manager::stats_manager::get_leaderboard`  

```cpp
xbox_live_result< void >  get_leaderboard (
     _In_ const xbox_live_user_t &user,
     _In_ const string_t &statName,
     _In_ leaderboard::leaderboard_query query
     ) 
```  

`xbox::services::stats:manager::stats_manager::get_leaderboard`  

```cpp
xbox_live_result< void >  get_social_leaderboard (_In_ const xbox_live_user_t &user,
     _In_ const string_t &statName,
     _In_ const string_t &socialGroup,
     _In_ leaderboard::leaderboard_query query
)
```  

`Microsoft.Xbox.Services.Statistics.Manager.StatisticManager.GetLeaderboard`  

```csharp
public void GetLeaderboard(
    XboxLiveUser user,
    string statName,
    LeaderboardQuery query
    )
```  

`Microsoft.Xbox.Services.Statistics.Manager.StatisticManager.GetSocialLeaderboard`  

```csharp
public void GetSocialLeaderboard(
    XboxLiveUser user,
    string statName,
    string socialGroup,
    LeaderboardQuery query
    )
```  

## <a name="2017-c-example"></a>2017 c + + 示例

### <a name="1-get-a-singleton-instance-of-the-statsmanager"></a>1.获取 stats_manager 单一实例

你可以调用前`stats_manager`你将需要将变量设置为单一实例的函数。

```csharp
m_statsManager = stats_manager::get_singleton_instance();
```

### <a name="2-create-a-leaderboardquery"></a>2.创建 LeaderboardQuery

`leaderboard_query`将听写量，顺序，以及从排行榜调用返回的数据的起点。

A`leaderboard_query`具有几个属性可以设置这将影响返回的数据：

|属性 |说明  |
|---------|---------|
|m_skipResultToRank     |此 uint 变量将确定哪些排名的排行榜数据将从开始时返回。 排名开始排名 1。         |
|m_skipResultToMe     |如果设置为 true，此布尔值将导致返回从开始的排行榜数据`XboxLiveUser`中使用`get_leaderboard()`调用。  |
|m_order     |枚举类型的`xbox::services::leaderboard::sort_order`有两个可能的值，升序，和降序。 设置你的查询此变量将确定你的排行榜的排序顺序。        |
|m_maxItems     |此 uint 确定最大行数的每个调用返回`get_leaderboard`或`get_social_leaderboard()`。         |

`leaderboard_query` 具有多个组功能，你可以使用对这些属性分配值。 以下代码将向你介绍如何设置你 `leaderboard_query`

```cpp
leaderboard::leaderboard_query leaderboardQuery;
leaderboardQuery.set_skip_result_to_rank(10);
leaderboardQuery.set_max_items(10);
leaderboardQuery.set_order(sort_order::descending);
```

此查询将返回排名的排行榜 100th 从开始的十个行单个。

> [!WARNING]
> 设置 SkipResultToRank 高于排行榜中包含的玩家数量将导致排行榜数据返回零行。

### <a name="3-call-getleaderboard"></a>3.调用 get_leaderboard

```cpp
leaderboard::leaderboard_query leaderboardQuery;
m_statsManager->get_leaderboard(user, statName, leaderboardQuery);
```

> [!IMPORTANT]
> `statName`中使用`GetLeaderboard()`调用必须配置为你的游戏在其[Windows 开发人员中心仪表板](https://developer.microsoft.com/en-us/dashboard/windows/overview)中，这是区分大小写的统计数据名称相同。

### <a name="4-read-the-leaderboard-data"></a>4.读取排行榜数据

若要读取的排行榜数据，你将需要调用`stats_manager::do_work()`函数将返回的列表`stat_event`值。 排行榜数据将包含在`stat_event`类型的`stat_event_type::get_leaderboard_complete`。 当遇到这种类型的列表中的事件`stat_event`你可能看起来通过 s`leaderboard_result`中包含`stat_event`访问数据。

示例`do_work()`处理程序

```cpp
void Sample::UpdateStatsManager()
{
    // Process events from the stats manager
    // This should be called each frame update

    auto statsEvents = m_statsManager->do_work();
    std::wstring text;

    for (const auto& evt : statsEvents)
    {
        switch (evt.event_type())
        {
            case stat_event_type::local_user_added: 
                text = L"local_user_added"; 
                break;

            case stat_event_type::local_user_removed: 
                text = L"local_user_removed"; 
                break;

            case stat_event_type::stat_update_complete: 
                text = L"stat_update_complete"; 
                break;

            case stat_event_type::get_leaderboard_complete: //leaderboard data is read here
                text = L"get_leaderboard_complete";
                auto getLeaderboardCompleteArgs = std::dynamic_pointer_cast<leaderboard_result_event_args>(evt.event_args());
                ProcessLeaderboards(evt.local_user(), getLeaderboardCompleteArgs->result());
                break;
        }

        stringstream_t source;
        source << _T("StatsManager event: ");
        source << text;
        source << _T(".");
        m_console->WriteLine(source.str().c_str());
    }
}
```

排行榜数据读取排行榜结果  

```cpp
void Sample::PrintLeaderboard(const xbox::services::leaderboard::leaderboard_result& leaderboard)
{
    if (leaderboard.rows().size() > 0)
    {
        m_console->Format(L"%16s %6s %12s %12s\n", L"Gamertag", L"Rank", L"Percentile", L"Values");
    }

    for (const xbox::services::leaderboard::leaderboard_row& row : leaderboard.rows())
    {
        string_t colValues;
        for (auto columnValue : row.column_values())
        {
            colValues = colValues + L" ";
            colValues = colValues + columnValue;
        }
        m_console->Format(L"%16s %6d %12f %12s\n", row.gamertag().c_str(), row.rank(), row.percentile(), colValues.c_str());
    }
}
```  

从排行榜检索进一步的数据的页面。  

```cpp
void Sample::ProcessLeaderboards(
    _In_ std::shared_ptr<xbox::services::system::xbox_live_user> user,
    _In_ xbox::services::xbox_live_result<xbox::services::leaderboard::leaderboard_result> result
    )
{
    if (!result.err())
    {
        auto leaderboardResult = result.payload();
        PrintLeaderboard(leaderboardResult);

        // Keep processing if there is more data in the leaderboard
        if (leaderboardResult.has_next())
        {
            if (!leaderboardResult.get_next_query().err())
            {               
                auto lbQuery = leaderboardResult.get_next_query().payload();
                if (lbQuery.social_group().empty())
                {
                    m_statsManager->get_leaderboard(user, lbQuery.stat_name(), lbQuery);
                }
                else
                {
                    m_statsManager->get_social_leaderboard(user, lbQuery.stat_name(), lbQuery.social_group(), lbQuery);
                }
            }
        }
    }
}
```  

## <a name="2017-winrt-c-example"></a>2017 WinRT C# 示例

### <a name="1-get-a-singleton-instance-of-the-statisticmanager"></a>1.获取 StatisticManager 的单一实例

你可以调用前`StatisticManager`你将需要将变量设置为单一实例的函数。

```csharp
statManager = StatisticManager.SingletonInstance;
```

### <a name="2-create-a-leaderboardquery"></a>2.创建 LeaderboardQuery

`LeaderboardQuery`将听写量，顺序，以及从排行榜调用返回的数据的起点。  

```csharp
public sealed class LeaderboardQuery : __ILeaderboardQueryPublicNonVirtuals
    {
        [Overload("CreateInstance1")]
        public LeaderboardQuery();

        public bool HasNext { get; }
        public string SocialGroup { get; }
        public string StatName { get; }
        public SortOrder Order { get; set; }
        public uint MaxItems { get; set; }
        public uint SkipResultToRank { get; set; }
        public bool SkipResultToMe { get; set; }
    }
```

A`LeaderboardQuery`具有几个属性可以设置这将影响返回的数据：

|属性 |说明  |
|---------|---------|
|SkipResultToRank     |此 uint 变量将确定哪些排名的排行榜数据将从开始时返回。 排名开始排名 1。         |
|SkipResultToMe     |如果设置为 true，此布尔值将导致返回从开始的排行榜数据`XboxLiveUser`中使用`GetLeaderboard()`调用。  |
|顺序     |枚举类型的`Microsoft.Xbox.Services.Leaderboard.SortOrder`有两个可能的值，升序，和降序。 设置你的查询此变量将确定你的排行榜的排序顺序。        |
|MaxItems     |此 uint 确定最大行数的每个调用返回`GetLeaderboard()`或`GetSocialLeaderboard()`。         |

形成你`LeaderboardQuery`可能如下所示：

```csharp
using Microsoft.Xbox.Services.Leaderboard;

LeaderboardQuery query = new LeaderboardQuery
        {
            SkipResultToRank = 100,
            MaxItems = 5
        };
```

此查询将返回五个行的排行榜 100th 从开始排名单个。

> [!WARNING]
> 设置 SkipResultToRank 高于排行榜中包含的玩家数量将导致排行榜数据返回零行。

### <a name="3-call-getleaderboard"></a>3.调用 GetLeaderboard()

现在，你可以调用`GetLeaderboard()`与你`XboxLiveUser`，你的统计数据的名称和`LeaderboardQuery`。

```csharp
statManager.GetLeaderboard(xboxLiveUser, statName, leaderboardQuery);
```

> [!IMPORTANT]
> `statName`中使用`GetLeaderboard()`调用必须配置为你的游戏在其[Windows 开发人员中心仪表板](https://developer.microsoft.com/en-us/dashboard/windows/overview)中，这是区分大小写的统计数据名称相同。

### <a name="4-read-leaderboard-data"></a>4.读取排行榜数据

若要读取的排行榜数据，你将需要调用`StatisticManager.DoWork()`函数将返回的列表`StatisticEvent`值。 排行榜数据将包含在`StatisticEvent`类型的`GetLeaderboardComplete`。 当遇到这种类型的列表中的事件`StatisticEvent`你可能看起来通过 s`LeaderboardResult`中包含`StatisticEvent`访问数据。

```csharp
IReadOnlyList<StatisticEvent> statEvents = statManager.DoWork(); //In practice this should be called every update frame

foreach(StatisticEvent statEvent in statEvents)
{
    if(statEvent.EventType == StatisticEventType.GetLeaderboardComplete
        && statEvent.ErrorCode == 0)
    {
        LeaderboardResultEventArgs leaderArgs = (LeaderboardResultEventArgs)statEvent.EventArgs;
        LeaderboardResult leaderboardResult = leaderArgs.Result;
        foreach(LeaderboardRow leaderRow in leaderboardResult.Rows)
        {
            Debug.WriteLine(string.Format("Rank: {0} | Gamertag: {1} | {2}:{3}\n\n", leaderRow.Rank, leaderRow.Gamertag, "test", leaderRow.Values[0]));
        }
    }
}
```

在你的作品代码中`StatisticManager.DoWork()`应该用于处理所有传入的统计数据管理器事件并不仅是个排行榜。 

> [!NOTE]
> 以检索`LeaderboardResultEventArgs`你将需要将转换`StatisticEvent.EventArgs`为`LeaderboardResultEventArgs`变量。

### <a name="5-retrieve-more-leaderboard-data"></a>5.检索更多的排行榜数据

以检索更高版本的页面的排行榜数据，你将需要使用`LeaderboardResult.HasNext`属性和`LeaderboardResult.GetNextQuery()`函数以检索`LeaderboardQuery`，将为你带来数据的下一页。

```csharp
while (leaderboardResult.HasNext)
{
    leaderboardQuery = leaderboardResult.GetNextQuery();
    statManager.GetLeaderboard(xboxLiveUser, leaderboardQuery.StatName, leaderboardQuery)
    // StatisticManager.DoWork() is called
    // Leaderboard results are read
}
```
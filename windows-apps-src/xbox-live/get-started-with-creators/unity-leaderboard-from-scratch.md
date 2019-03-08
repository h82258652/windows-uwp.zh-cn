---
title: 脚本在 Unity 中的排行榜
description: 有关构建您自己在 Unity 中的排行榜指导
ms.date: 04/24/2018
ms.topic: get-started-article
keywords: xbox live、 xbox、 游戏、 uwp、 windows 10 中，一个 xbox、 unity、 排行榜
ms.openlocfilehash: 6e73ffd9b55f3638eb3cf4245c6f7943fe92dc48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608752"
---
# <a name="script-a-leaderbaord-gameobject"></a>脚本 leaderbaord GameObject

对于那些希望自定义排行榜体验的用户，这篇文章将提供的工具来实现您自己排行榜回顾到 Unity 开发人员提供的 Api。 了解如何拉取排行榜数据后您将能够将其应用于所选的用户界面。

## <a name="call-for-leaderboard-data"></a>排行榜数据调用

有两个 API 调用以检索排行榜数据。

- `void GetLeaderboard(XboxLiveUser user, string statName, LeaderboardQuery query)`
- `void GetSocialLeaderboard(XboxLiveUSer user, string statName, string socialGroup, LeaderboardQuery query)`

为了成功地进行任一这些调用将返回你将需要获取数据`XboxLiveUser`由[登录](unity-prefabs-and-sign-in.md)，具有[配置 stat](add-stats-and-leaderboards-in-unity.md)值至少一个播放器和表单`LeaderboardQuery`. 如果您不知道如何登录用户或需要初始化你排行榜的统计信息，可以读取链接的文章。 Leaderboard 脚本后已初始化的统计信息将其与相关联的最简单方法是包含统计信息预设之一： `IntegerStat`， `DoubleStat`，或`StringStat`作为公共变量。 在统计信息将需要具有的 ID 属性配置为至少在这是我们将使用**statName**参数调用排行榜数据时。 最后你将需要向窗体`LeaderboardQuery`对象。
一个`LeaderboardQuery`具有几个可以设置这将影响返回的数据属性：

- **SkipResultToRank**： 如果设置，此 uint 变量将确定哪些排名排行榜数据将从开始时返回。 排名开始秩为 1。
- **SkipResultToMe**： 如果设置为 true，此布尔值将导致返回启动时开始的排行榜数据`XboxLiveUser`中使用`GetLeaderboard()`调用。
- **顺序**:枚举类型的`Microsoft.Xbox.Services.Leaderboard.SortOrder`具有两个可能的值，升序排序，并按降序。 设置此变量用于您的查询将确定你排行榜的排序顺序。
- **MaxItems**:此 uint 确定要在每次调用返回的行的最大数`GetLeaderboard()`或`GetSocialLeaderboard()`。

构成您 leaderboardQuery 可能看起来如下所示：

```csharp
using Microsoft.Xbox.Services.Leaderboard;

LeaderboardQuery query = new LeaderboardQuery
        {
            SkipResultToRank = 100,
            MaxItems = 5
        };
```

此查询将返回 5 行的开始 100th 排行榜排名单个。

> [!WARNING]
> 设置 SkipResultToRank 大于玩家在排行榜中包含的数将导致排行榜数据返回零行。

现在，我们共同拥有的所有部分我们可以调用`GetLeaderboard(XboxLiveUser user, string statName, LeaderboardQuery query)`函数。

`GetSocialLeaderboard(XboxLiveUSer user, string statName, string socialGroup, LeaderboardQuery query)`函数具有一个名为 socialGroup 的额外参数。 此字符串将用作关系筛选器对返回的数据。 可接受的 socialGroup 值如下所示：

- "全部": 此操作将返回已筛选到 XboxLiveUser 的友元的排行榜
- "收藏": 此操作将返回已筛选到 XboxLiveUser 最喜欢的友元的排行榜

可以使用`LeaderboardTypes`中的枚举`Microsoft.Xbox.Services.Client`命名空间标记排行榜 socialGroup，然后使用`LeaderboardHelper`类函数`GetSocialGroupFromLeaderboardType(LeaderboardTypes leaderboardType)`拉出适当的字符串。

> [!NOTE]
> 空字符串传递 socialGroup 参数将返回相同的结果与调用`GetLeaderboard()`函数。 你将收到未经筛选*全局*排行榜显示级别为每个人都扮演游戏排行榜中。

```csharp
using Microsoft.Xbox.Services.Leaderboard;
using Microsoft.Xbox.Services.Statistics.Manager;
using Microsoft.Xbox.Services;

public void LoadLeaderboard()
{

    if (this.stat == null)
    {
        // TO DO: Display "Stat not specified" error message!
        return;
    }

    if (this.xboxLiveUser == null)
    {
        if (SignInManager.Instance.GetCurrentNumberOfPlayers() > 0)
        {
            this.xboxLiveUser = SignInManager.Instance.GetPlayer(1);
            this.isLocalUserAdded = true;
        }
        else
        {
            // TO DO: Display "No user signed-in" error message!
            return;
        }
    }

    LeaderboardQuery query = new LeaderboardQuery
    {
        MaxItems = 5,
    };

    XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, this.stat.ID, otherQuery);
}

```

现在您可能已经注意到我们的两个排行榜检索函数返回 void，并且因此不会返回我们正在寻找的排行榜数据。 我们实际上将检索下一节中讨论的事件函数中的排行榜数据。

## <a name="receive-the-leaderboard-data"></a>接收排行榜数据

若要检索你将需要添加到侦听函数的排行榜数据`StatsManagerComponent`在标题上的实例。 应添加以下代码行`Awake()`代码的函数： `StatsManagerComponent.Instance.GetLeaderboardCompleted += this.MyGetLeaderboardCompletedFunction`。 `StatsManagerComponent`在`Microsoft.Xbox.Services.Client`命名空间侦听排行榜完成事件。 通过运行这行代码，将函数添加到要排行榜完成事件发生时调用的函数列表。 在此示例中，函数称为 MyGetLeaderBoardCompletedFunction，可以根据需要在您自己的脚本中命名该函数。 "MyGetLeaderboardCompletedFunction"需要采用两个参数，一个对象，表示发件人，该函数和一个`Microsoft.Xbox.Services.Client.StatEventArgs`参数。 你的函数的 shell 可能如下所示：

```csharp
using Microsoft.Xbox.Services.Client;
using Microsoft.Xbox.Services.Statistics.Manager;

private void GetLeaderboardCompleted(object sender, StatEventArgs statArgs)
    {
        //Do Something;
    }
```

此函数应做的第一件事是检查是否有错误，可在`StatEventArgs`参数 statArgs。 StatArgs 包含`StatisticEvent`EventData，其中包含错误数据。 如果在检索排行榜数据时出错可以找到它在`statArgs.EventData.ErrorCode`或`statArgs.EventData.ErrorMessage`。 如果没有错误错误代码将为 0 和 ErrorMessage 将为空字符串""。 您可以添加使用简单的 if 语句与前面的代码以检查是否有错误。

```csharp
using Microsoft.Xbox.Services.Client;
using Microsoft.Xbox.Services.Statistics.Manager;

private void GetLeaderboardCompleted(object sender, StatEventArgs statArgs)
    {
        if (statArgs.EventData.ErrorCode != 0) //if there is an error
        {
            // TO DO: Display error message
            return;
        }
    }
```

在确认没有任何错误，存储排行榜请求中找到的结果`statArgs.EventData.EventArgs.Result`。 `Result` 是`LeaderBoardResult`对象，其中包含所需来填充你排行榜的数据。 在我们的示例代码中我们将提取此数据并将其发送到另一个函数调用`LoadResult()`。

```csharp
using Microsoft.Xbox.Services.Client;
using Microsoft.Xbox.Services.Statistics.Manager;

private void GetLeaderboardCompleted(object sender, StatEventArgs statArgs)
    {
        if (statArgs.EventData.ErrorCode != 0)
        {
            // TO DO: Display error message
            return;
        }

        LeaderboardResultEventArgs leaderboardArgs = (LeaderboardResultEventArgs)statArgs.EventData.EventArgs;
        this.LoadResult(leaderboardArgs.Result);
    }
```

`LeaderboardResult`我们将发送到的结果`LoadResult()`函数将具有需要同时读取返回的排行榜数据，以及进行更多的调用来检索排名尚不支持的原始调用返回的所有数据。 想要将结果存储在 leaderboard 脚本的类变量如下所示：

```csharp
using Microsoft.Xbox.Services.Leaderboard;

void LoadResult(LeaderboardResult result)
    {
        this.leaderboardData = result;
    }
```

这一点很重要因为`LeaderboardResult`包含`HasNext`确定是否有可以检索排行榜的后面部分的属性，结果也包含构成排行榜的行的总计数。 这些属性来说十分重要的导航你排行榜。 提取数据，从你`LeaderBoardResult`只需实现循环使用`LeaderboardResults`的列表`LeaderboardRow`调用`Rows`。 在示例代码中我们只需将在每个值串联`LeaderboardRow`为字符串，用于显示。


```csharp
using Microsoft.Xbox.Services.Leaderboard

void LoadResult(LeaderboardResult result)
{

    this.leaderboardData = result;

    this.leaderBoardText.text = "" // leaderBoardText is a UnityEngine.UI text box.

    foreach (LeaderboardRow row in this.leaderboardData.Rows)
    {
        leaderBoardText.text += string.Format("Rank: {0} | Gamertag: {1} | {2}:{3}\n\n", row.Rank, row.Gamertag, this.stat.DisplayName, row.Values[0]);
    }
}
```

在本示例中我们使用 LeaderBoardResult 的排名、 玩家代号、 和值属性来填充我们字符串，以及与排行榜相关联的状态的显示名称。

我敢肯定您将能够使用所有这些排行榜数据更具创造性的事情。

## <a name="navigating-the-leaderboard-data"></a>导航排行榜数据

在最常见的情况将不会加载每个单通道中你排行榜，并将需要能够导航以显示用户排行榜的不同部分的排名。 让我们假设使用 100 个排名播放器有排行榜。 在你首次调用`GetLeaderboard()`或`GetSocialLeaderboard`检索 10`LeaderboardRows`和显示的播放机。 播放机可能想要查看更多比前十个播放机。 若要获取下一组有十个用户的最简单方法是确定是否`LeaderboardResult`存储从在上一个查询具有多个要检索的行并调用`GetLeaderboard()`与该 LeaderboardResult 第二个查询。 若要使用 LeaderBoardResult *nextQuery*您必须使用函数`LeaderBoardResult.GetNextQuery()`。 如以下所示的代码来检索下一组排名。

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

void GetNextLeaders()
    {
        if(this.leaderboardData.HasNext)
        {
            LeaderboardQuery query = this.leaderboardData.GetNextQuery();
            XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, this.stat.ID, query);
        }
        else
        {
            //TO DO: Display an error message or go back to the beginning of the leaderboard as the situation demands.
        }
    }
```

在排行榜中向后移动，因为没有任何函数来提取上一个 X 数是从你排行榜多少有点更加困难。 若要检索以前必须编写您自己的逻辑的排名。 是一种方法存储在`MaxItems`每个`LeaderboardQuery`和计算需要跳过使用到哪些级别`SkipToRank`的属性在`LeaderboardQuery`。 该代码可能如下所示：

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

void GetPreviousLeader()
{
    if(leaderboardData == null || leaderboardData.Rows.Count < 1)
    {
        return;
    }

    uint topRank = leaderboardData.Rows[0].Rank; //get the first rank of the leaderboard.
    uint targetRank = topRank - this.maxItems > 0 ? topRank - this.maxItems : 0; //set your targetRank equal to the current top rank of your leaderboard minus the configured number of rows to display at a time.

    LeaderboardQuery query = new LeaderboardQuery // make a new query with the calculated targetRank
    {
        SkipResultToRank = targetRank,
        MaxItems = this.maxItems
    };

    XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, this.stat.ID, query); // call the GetLeaderboard() function with the new query
}
```

最后一个最常见方案是播放机可能只是想要在排行榜上看到自己的位置。 轻松地实现方法是调用`GetLeaderboard()`函数替换查询其中`SkipResultToMe`属性设置为 true。

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

    void GetRankForTag()
    {

        LeaderboardQuery query = new LeaderboardQuery // make a new query with the calculated targetRank
        {
            SkipResultToMe = true,
            MaxItems = this.maxItems
        };

        XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, this.stat.ID, query); // call the GetLeaderboard() function with the new query
    }
```

如果你想要深入了解更详细的 Leaderboard 示例始终能够读取下资产的 XboxLive 插件文件夹中的 Leaderboard.cs 脚本 >> XboxLive >> 脚本 >> Leaderboard.cs。
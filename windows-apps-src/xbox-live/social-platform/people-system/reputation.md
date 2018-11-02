---
title: 信誉
author: KevinAsgari
description: 了解 Xbox Live 如何使用信誉服务鼓励正面的游戏玩法。
ms.assetid: f8966184-5db7-4cab-93ca-9a0250a6077d
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 信誉, 社交平台
ms.localizationpriority: medium
ms.openlocfilehash: 5b3ff2d546dc142331406f198848e9d7a7077b3d
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5929561"
---
# <a name="reputation"></a>信誉

与任何其他信息一样，信誉是一项用户统计信息，它包含在调用中，用来检索所有用户统计信息，并使用这些信息来报告和跟踪。 信誉本身由 **Reputation 类**表示。 **ReputationService 类**表示信誉服务。 相应的 URI 在**信誉 URI** 中加以介绍。

> [!IMPORTANT]
> 信誉统计信息是跨整个服务的全局信息，不与特定标题关联。 服务的服务配置 ID (SCID) 是用于访问信誉统计信息的全局 SCID。


## <a name="features-of-the-reputation-service"></a>信誉服务的功能

信誉服务：

-   通过相同方式处理反馈和投诉。 实体提交关于用户的反馈，此反馈将影响用户的信誉。 反馈随后可以转发给执行团队以采取进一步操作。
-   允许用户提供有关其他用户的反馈。 作品可以自动提交反馈。
-   允许作品直接访问 API。 用户可以直接从游戏内，以及用户当时所在的游戏区域的背景内提交反馈。
-   处理为低信誉，因为这会影响用户可在 Xbox Live 上和作品内执行的操作。 因此用户必须留意其信誉，当进行在线游戏时让自己的行为更加得当。
-   允许正面的反馈和负面的反馈。 帮助 Xbox 社区或作品社区的用户所付出的努力可以获得奖励，并且他们将被授予特殊权限。
-   派生单一的整体信誉，由 **Reputation.OverallReputation 属性**表示。 它从以下信誉类型派生：

    -   公平比赛。 由 **Reputation.FairplayReputation 属性**表示。
    -   通信。 由 **Reputation.CommunicationsReputation 属性**表示。
    -   用户生成的内容 (UGC)。 由 **Reputation.UserGeneratedContentReputation 属性**表示。

有关详细信息，请参阅 **ResetReputation (JSON)**。


## <a name="usage-flow-for-the-reputation-service"></a>信誉服务的使用流

信誉服务的整体流有两个阶段：报告信誉和使用信誉筛选出的比赛。


## <a name="reporting-reputation"></a>报告信誉

当某个用户被报告了一定数量的负面反馈时，标题会设置 **Reputation.OverallReputation 属性**，以指示该用户信誉较差（JSON 属性 OverallReputationIsBad）。 如果给定时间内未收到投诉，用户信誉将缓慢提高，信誉曾经较差的用户有可能再次获得良好信誉。


## <a name="reputation-filtered-matchmaking"></a>使用信誉筛选出的比赛

对于由 Xbox 要求 (XR) 指定的使用信誉筛选出的比赛，游戏将检索玩家的 **Reputation.OverallReputation 属性**。 此值是指示玩家整体信誉状态的评分。

> [!NOTE]
> 如果你的标题在 JSON 文件中查找 OverallReputationIsBad 属性且未找到，则可以放心假定该用户信誉良好。 新帐户可能存在这种情况，直到处理了用户的反馈。 获得来自其他用户的反馈的玩家会始终具有信誉统计信息，以及 **Reputation.OverallReputation** 属性值。


## <a name="reputation-as-an-indicator-of-behavior"></a>充当行为指示器的信誉

信誉提供了指示用户在系统上的行为方式的指示器。 例如，该用户是否是友好玩家？ 来自其他会话成员的反馈确定玩家的信誉。 每个用户都有一个信誉评分，它会在 Xbox One 上随处跟随这个用户。 它会在好友能够看到的社交位置醒目地展示，以便向玩家施加同伴压力，让他们改良自己的行为。 例如：

-   位于每个用户的配置文件卡上。 系统中的任何人只需单击一下即可查看用户配置文件。
-   显示在多人游戏中。 当用户在线一起玩游戏时，组信誉等于组中信誉最低的玩家的信誉。 组仅与信誉相当的其他人匹配。 其他玩家使用信誉来确定该游戏中有哪类玩家，并决定是否要加入游戏。


## <a name="key-elements-of-reputation"></a>信誉的关键元素

标题可以确定用户是否已达到公平比赛、通信或用户生成的内容 (UGC) 类别的“避开我”级别。 如果以下任何一个属性被设置为 1，则说明用户已到达关联类别的“避开我”：

-   **Reputation.OverallReputation 属性**。 关联的 JSON 属性是 OverallReputationIsBad。
-   **Reputation.FairplayReputation 属性**。 关联的 JSON 属性是 FairplayReputationIsBad。
-   **Reputation.CommunicationsReputation 属性**。 关联的 JSON 属性是 CommsReputationIsBad。
-   **Reputation.UserGeneratedContentReputation 属性**。 关联的 JSON 属性是 UserContentReputationIsBad。


## <a name="good-game-reports"></a>良好游戏报告

除了报告用户的不良操作外，用户还可以相互发送良好游戏报告，这可以在游戏中创建为向最有价值的玩家投票。


## <a name="feedback-history"></a>反馈历史记录

反馈历史记录报告一些信息，如用户上次报告的时间、用户被报告的内容，例如，“最近收到有关通信风格的反馈”。


## <a name="xbox-requirement-for-reputation"></a>Xbox 的信誉要求

Xbox 要求 (XR) 068 按信誉筛选比赛，要求将信誉较低的玩家与信誉较高的玩家分开。 上述目标可以通过查看用户统计信息中玩家的 **Reputation.OverallReputation** 值和该玩家的 OverallReputationIsBad 属性来实现。

你的作品可以通过将 "OverallReputation" 传递到 **UserStatisticsService.GetSingleUserStatisticsAsync 方法（字符串，字符串，字符串）** 的 *statisticName* 参数来检索用户信誉。 WinRT 方法调用以下 JSON 正文中显示的 **GET (/users/xuid({xuid})/scids/{scid}/stats)**。

```json
    {
      "requestedusers": [
        "2533274792693551"
      ],
      "requestedscids": [
        {
          "scid": "7492baca-c1b4-440d-a391-b7ef364a8d40",
          "requestedstats": [
            "OverallReputationIsBad",
            "FairplayReputationIsBad",
            "CommsReputationIsBad",
            "UserContentReputationIsBad"
          ]
        }
      ]
    }
```

当来自其他玩家的用户反馈达到较低级别时，该用户的 OverallReputationIsBad 会被设置为 1。 **Reputation.OverallReputation** 为 1 的用户只会与其他 **OverallReputation** 设置为 1 的用户匹配。 默认情况下，当用户进入匹配时，他们通常无需接触信誉较低的用户。 游戏可以视情况允许信誉良好的玩家选择加入与低信誉玩家的匹配。

若要获取适用于信誉的最新 XR 版本，请参阅游戏开发人员网络 (GDN) 上的 [Xbox 要求](https://developer.xboxlive.com/en-us/platform/certification/requirements/Pages/home.aspx)。


## <a name="creating-users-with-bad-overall-reputation-for-testing"></a>为测试创建整体信誉较差的用户

若要进行测试，可以设置信誉极差的用户来测试标题的匹配筛选实现情况。 为设置特定用户值，测试标题或工具使用设置用户特定信誉值的 JSON 文件调用 **POST (/users/xuid({xuid})/resetreputation)**。 有关详细信息，请参阅 **ResetReputation (JSON)**。

例如，以下 JSON 正文将用户的公平比赛信誉设置为一个极差的值：

```json
    {
      "fairplayReputation": 5,
      "commsReputation": 75,
      "userContentReputation": 75
    }
```

合作伙伴可以对除零售沙盒以外的所有沙盒进行此调用。 此请求用于设置用户的基本信誉评分，玩家的正面反馈权重将全部归零。此调用后用户的实际信誉包含这些基本评分以及玩家的大使奖金和关注者奖金。 此机制创建低评分用户和设置为 1 的 **Reputation.OverallReputation** 来测试匹配筛选 XR。 作品可以从用户统计信息服务获取用户的用户信誉评分，如本主题的“Xbox 的信誉要求”部分所述。


## <a name="resetting-a-users-reputation-to-the-defaults"></a>将用户信誉重置为默认值

标题设置 OverallReputationIsBad 属性来指示用户拥有良好信誉。 它调用 **POST (/users/me/resetreputation)** 并将所有值都设置为 75。 对 **/users/xuid({xuid})/deleteuserdata** 的一次调用可以用来重置多个 Xbox 用户 ID。 请求正文是：

```json
    {
      "xuids": [
        "2814659110958830"
      ]
    }
```

## <a name="sending-feedback-from-titles"></a>从作品中发送反馈

游戏可以发送有关匹配中玩家的公平比赛反馈。 上述操作可以直接从作品或作品服务中分批实现。 作品可以使用 **ReputationService.SubmitReputationFeedbackAsync 方法**或以下直接 REST 方法：

|                      |                                                             |
|----------------------|-------------------------------------------------------------|
| 客户端 POST          | https://reputation.xboxlive.com/users/xuid{XUID}/feedback |
| 服务（批）POST | https://reputation.xboxlive.com:10443/users/batchfeedback   |

请参阅**反馈 (JSON)** 查看用于提交的 JSON 正文示例和允许字段的定义。


## <a name="types-of-feedback-allowed"></a>允许的反馈类型

可以发布的反馈类别在**反馈 (JSON)** 中定义。


## <a name="when-titles-should-send-feedback"></a>当作品应该发送反馈时

当事件在游戏中对玩家体验造成负面影响时，游戏应发送负面反馈。 一般情况下，在每一轮游戏中，对于每种类型，游戏只会发送一个反馈。 例如，作品应该：

1.  在一轮游戏中为杀死 3 名或更多队友的用户只发送一个 FairPlayKillsTeammates 反馈项目，而不是该用户每杀死一名队友就发送一个事件。
2.  当某人有目的地提前退出匹配时发送 FairplayQuitter 反馈项目。
3.  当用户在赛车游戏中向相反方向驾驶时发送 FairplayUnsporting 反馈项目，每次比赛发送一次。

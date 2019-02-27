---
title: Arena 游戏集成指南
description: 了解如何构建支持 Xbox Live Arena 平台的游戏。
ms.assetid: 470914df-cbb5-4580-b33a-edb353873e32
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, arena, 锦标赛
ms.localizationpriority: medium
ms.openlocfilehash: 891fa8da1ca6e26128e0a33d28a505a18e99662a
ms.sourcegitcommit: ff131135248c85a8a2542fc55437099d549cfaa5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2019
ms.locfileid: "9117563"
---
# <a name="arena-title-integration-guide"></a>Arena 游戏集成指南

## <a name="introduction"></a>简介

Xbox 的 Arena 平台便于锦标赛组织者 (TO) 创建和开展支持 Arena 的游戏的锦标赛。 Arena 支持启用发布者和用户运行的锦标赛的 Xbox Live TO 以及与 Arena 集成的第三方 TO。 锦标赛类型，如单淘汰制或瑞士制，由 TO 确定。

本主题将向你介绍，作为一个游戏开发人员，如何在你的游戏中实现对 Arena 的支持。 如果一款游戏支持 Arena，它将支持使用由 TO 选择的任何受支持的锦标赛格式。 TO 根据锦标赛的结构为你的游戏协调比赛。 支持 Arena 的游戏再将合适的用户放入游戏的每场比赛中，然后将比赛结果返回给 TO。

## <a name="overview"></a>概述

Xbox Arena 使用的概念与 Xbox 多人游戏开发相似。 如果你不熟悉 Xbox 多人游戏系统，我们建议你查看相关文档，然后再继续。 该流程类似于用户接受邀请加入多人游戏的流程。 当用户到了在 Arena 锦标赛中玩游戏的时间时，toast 会弹出通知以提醒用户。 接受 toast 会使你的游戏收到标准的 Arena 协议激活事件。 如果你的游戏尚未运行，则它会在此时启动。

锦标赛本身通过使用多人游戏会话目录 (MPSD) 会话进行协调。 就 Arena 而言，会话由 Arena 平台而不是你的游戏创建。 这可以通过使用会话模板以及你作为游戏发布者创建的其他游戏配置完成。 锦标赛参与者将已经加入此会话。 作为游戏发布者，你必须配置供 Arena 使用的会话模板以支持用于结果报告的仲裁。 此会话的完整要求包含在本文档的后面部分。 你的游戏也可以创建它需要的任何其他会话。

你的游戏使用会话设置比赛和报告结果。 响应协议激活和与 MPSD 会话互动是 Arena 游戏需要的最低集成级别。 支持通过 Xbox Arena UI 浏览和注册参加锦标赛。 或者，游戏可以从“锦标赛中心”服务中获取关于锦标赛的更多信息，实现更丰富的游戏内体验。 例如，通过使用“锦标赛中心”，你的游戏可以显示团队名称等背景信息，并为用户发现新的比赛会话。 此数据流以下图中的绿色箭头表示，并且在[体验要求和最佳做法](#experience-requirements-and-best-practices)部分进行了详细描述。 当用户从锦标赛中的一场比赛移到另一场比赛时，淡出的箭头表示团队对会话的引用随时间推移而变化。

![](../../images/arena/tournament-flow.png)


Arena 协议激活 URI 包含与锦标赛有关的信息、比赛会话以及当比赛结束时你的游戏可以调用的深层链接。 深层链接可使用户返回至 Xbox Arena UI。 这些 URI 组件在 [Arena 集成的基本要求](#basic-requirements-for-arena-integration)的[协议激活](#1protocol-activation)部分有详细的介绍。

## <a name="basic-requirements-for-arena-integration"></a>Arena 集成的基本要求

本部分提供有关集成你的游戏与支持 Arena 的最低要求的技术指导和详细信息。 此类集成利用如下概述图中以橙色箭头表示的数据流。

![](../../images/arena/arena-data-flow.png)

你的游戏从 Xbox Arena UI 进行协议激活。 这可能源自 toast 通知、锦标赛的详细信息页面或比赛的任何其他入口点。 当用户参加比赛时发生的步骤如下：

1.  你的游戏由 Xbox Arena UI 进行协议激活。
2.  你的游戏使用激活参数玩单次比赛。
3.  比赛结束后，你的游戏将结果报告至 MPSD 游戏会话。
4.  你的游戏使用户可以选择返回到 Arena UI。

以下部分对每个步骤进行了详细介绍。

### <a name="1--protocol-activation"></a>1. 协议激活

当比赛为用户准备就绪后，Arena UI 通过对你的游戏进行协议激活来启动。 这里有两种情况：你的游戏可能在此时启动，或者已经正在运行。 这两种情况类似于用户接受多人游戏邀请后游戏被激活时发生的情况。

#### <a name="if-your-title-is-launched-at-the-time-of-protocol-activation"></a>如果你的游戏在协议激活时启动

当你的游戏首次启动时，将触发**已激活**事件。 如果该初始激活为 Arena 协议激活，则你的游戏由尝试在锦标赛中比赛的用户启动。 让你的游戏跳过菜单和登录屏幕，尽快跳到比赛。 参与用户的 Xbox 用户 ID (XUID) 在激活 URI 中提供，并且应该已经登录。

#### <a name="if-your-title-is-already-running"></a>如果你的游戏正在运行

当你的游戏正在运行时，也可以使用 Arena 协议激活触发**已激活**事件。 **激活**事件仅通过显式用户操作（对指示比赛的 toast 执行操作或从 Xbox Arena UI 跳到比赛）触发。 如果你的游戏正在菜单屏幕上等待，则立即跳到锦标赛比赛。 如果你的游戏在玩游戏期间收到协议激活，则让游戏让用户选择离开游戏并以最适合的方式进入锦标赛比赛。 如有必要，让用户有机会保存他们的游戏。

协议激活通过 `CoreApplicationView.Activated` 事件传输到你的游戏。 如果事件参数的 `IActivatedEventArgs.Kind` 属性设置为 `Protocol`，则激活为协议激活，且参数可以转换为 `ProtocolActivatedEventArgs` 类，其中协议激活 URI 通过 `Uri` 属性提供。

让你的游戏检查协议计划 URI 上的 XUID。 如果它与当前玩家不匹配，你的游戏还将需要切换用户上下文。

#### <a name="the-protocol-activation-uri"></a>协议激活 URI

URI 的模板是：

```URI
ms-xbl-multiplayer://tournament?action=joinGame&joinerXuid={memberId}&organizer={organizerId}&tournamentId={tournamentId}&teamId={teamId}&scid={scid}&templateName={templateName}&name={name}&returnUri={returnUri}&returnPfn={returnPfn}
```

对于控制台上的 ERA 游戏，激活方案略有不同：

```
ms-xbl-{titleIdHex}://
```

这与邀请相同。 为此，游戏 ID 必须使用 8 个十六进制字符，因此必要时它将包括前导零。

首先，确保 `Uri.Host`（紧跟“://”分隔符的名称）为“tournament”。 这是 Arena 协议激活与其他功能（如游戏邀请）激活的区别。

查询字符串参数将使用 URL 编码并且不区分大小写。 你的游戏不应依赖于参数的顺序，并且应该忽略无法识别的参数。


参数 | 描述
--- | ---
**操作** | 唯一支持的操作是“joinGame”。 如果缺少**操作**或为其指定了另一个值，则忽略激活。
**joinerXuid** | **joinerXuid** 是尝试在锦标赛比赛中进行游戏的已登录用户的 XUID。 你的游戏必须允许切换到该用户的上下文。 如果 **joinerXuid** 未登录，你的游戏必须提示用户通过调出帐户选取器来进行登录。 XUID 始终以十进制表示。
**organizerId、tournamentId** | **organizerId** 和 **tournamentId** 一起使用时，构成与比赛关联的锦标赛的唯一标识符。 如果你选择在你的游戏中显示它，则可使用此标识符从“锦标赛中心”检索与锦标赛有关的更多详细信息。
**teamId** | **teamId** 是在用户（由 **joinerXuid** 参数指定）加入的锦标赛上下文中团队的唯一标识符。 与 **organizerId** 和 **tournamentId** 参数类似，你可以使用 **teamId** 选择性地从“锦标赛中心”检索与团队有关的信息。
**scid、templateName、name** | 这些参数一起用来标识会话。 这些也是在指向会话的 MPSD URI 路径中的三个参数：</br> </br>`https://sessiondirectory.xboxlive.com/serviceconfigs/{scid}/sessiontemplates/{templateName}/sessions{name}`.</br></br>在 XSAPI 中，它们是 `multiplayer_session_reference ` 构造函数的三个参数。
**returnUri、returnPfn** | **returnUri** 是使用户返回到 Xbox Arena UI 的协议激活 URI。 **returnPfn** 参数可能存在，也可能不存在。 如果存在，它是用于处理 **returnUri** 的应用的产品系列名称 (PFN)。 有关演示如何使用这些值的示例代码，请参阅[返回到 Xbox Arena UI](#4returning-to-the-xbox-arena-ui)。

### <a name="2--playing-the-match"></a>2. 进行比赛

当锦标赛组织者创建 MPSD 会话时，用户被设置为该会话的非活动成员。 你的游戏必须立即使用 `multiplayer_session::join()` 将玩家设置为活动状态。 这表明对于 Xbox Live 和比赛中的其他用户而言，该用户位于你的游戏中且已准备就绪。

比赛的开始时间为采用标准 UTC 格式（如“2009-06-15T13:45:30.0900000Z”）的日期时间值，位于会话的 `/servers/arbitration/constants/system/startTime` 位置。 （开始时间也作为 `multiplayer_session_arbitration_server::arbitration_start_time()` 在 XSAPI 中可用，从 1703 XDK 开始）。 TO 可以随意在开始时间之前尽量提前（包括与开始时间同时）创建会话。 比赛通知于开始时间发送至比赛参与者，因此游戏在开始时间前绝不会激活。 开始时间也是 MPSD 允许报告比赛结果的最早时间。

你的游戏查找每位成员的 `/member/{index}/constants/system/team` 属性 (`multiplayer_session_member::team_id()`) 以确定每位成员属于哪个团队。

你的游戏还使用会话确定比赛设置，如地图和模式。 这些特定于游戏的设置可以在会话模板中设置，也可以作为锦标赛定义的一部分设置为自定义常量。 （有关详细信息，请参阅[为 Arena 配置游戏](#configuring-a-title-for-arena)。）下面是一个示例：

```json
{
    "constants": {
        "custom": {
            "enableCheats": false,
            "bestOf": 3,
            "map": "winter-fall",
            "mode": "capture-the-throne"
        }
    }
}
```

你可以使用 `multiplayer_session_constants::session_custom_constants_json()` API 从作为 JavaScript 对象表示法 (JSON) 对象的会话检索这些设置。

一般情况下，你的游戏应将 Arena 会话与自己的 MPSD 会话一样同等对待。 例如，它可以创建句柄和订阅 RTA 通知。 但有几个区别。 例如，你的游戏无法更改游戏会话的名单或使用会话的服务质量 (QoS) 功能，且它必须参与仲裁。 此会话的完整要求包含在本文档的后面部分。

### <a name="3--reporting-results"></a>3. 报告结果

比赛结果使用仲裁功能通过会话报告回 Arena 和 TO。 仲裁是使用会话确保比赛安全和报告结果的框架。

在协议激活步骤中向你的游戏提供的会话将为仲裁的会话，这意味着它具有仲裁框架执行的固定时间线。 此图显示了仲裁时间线。

![](../../images/arena/arbitration-timeline.png)

在作废时间（即开始时间 (`multiplayer_session_arbitration_server::arbitration_start_time()`) 加上作废超时）之前，会话中必须至少有一个玩家处于活动状态。作废超时以毫秒为单位，在会话中位于 `/constants/system/arbitration/forfeitTimeout` (`multiplayer_session_constants::forfeit_timeout()`)。 如果在作废时间前没有人作为活动状态加入会话，则比赛取消。

仲裁超时以毫秒为单位，位于 `/constants/system/arbitration/arbitrationTimeout`(`multiplayer_session_constants::arbitration_timeout()`)。 仲裁时间是开始时间加上仲裁超时值，表示玩家必须完成比赛并且报告结果的截止时间。 此值由游戏发布者在会话模板或游戏模式中进行设置。 将它设置为允许你的游戏完成比赛所需的最长时间。

你的游戏可以在开始时间与仲裁时间之间的任何时间报告结果。 在会话的每位活动成员提交结果后，仲裁发生在作废时间与仲裁时间之间的任何时间。 例如，如果在作废时间只有一位成员在会话中处于活动状态，则他们可以（且应该）发布结果，且会发生仲裁。 无论在仲裁时间有多少结果可用，如果尚未发生仲裁，则会发生仲裁。 如果在到达仲裁时间时没有提交结果，则比赛中的所有参与者均被判输。

也有可能游戏服务器通过在任何时间写入仲裁结果的简单方法来强制仲裁。

如果用户处于已经仲裁的会话中（无论是由于仲裁超时过期、游戏服务器仲裁会话还是用户超时加入），你的游戏会结束比赛，并向用户显示仲裁的结果。

仲裁结果始终包括每个团队的结果。 当单个玩家将结果写入到会话时，它不仅包括团队的结果，也包括每个团队的完整的结果集。

JSON 中的结果如下所示。

```json
"results": {
    "red": {
        "outcome": "rank",
        "ranking": 1
    },
    "blue": {
        "outcome": "rank",
        "ranking": 2
    }
}
```

在本例中，团队 ID 为“红色”和“蓝色”。 红色团队出现在第一位，蓝色团队出现在第二位。 **结果**的其他可能值为：

* “赢”
* “输”
* “和局”
* “未出场”

如果**结果**是除“排名”以外的任何其他结果，则省略该团队的排名。

个别用户将比赛结果写入 `/members/{index}/properties/system/arbitration` (`multiplayer_session::set_current_user_member_arbitration_results()`)。 下面的示例说明了游戏可以如何构造和提交结果集，假设你的游戏不是团队游戏（即所有玩家都属于一个团队）。

```c++
void Sample::SubmitResultsForArbitration()
{
    std::unordered_map<string_t, tournament_team_result> results;

    for (auto& member : arbitratedSession->members())
    {
        tournament_team_result teamResult;
        teamResult.set_state(tournament_game_result_state::rank);
        teamResult.set_ranking(memberRank);

        results.insert(std::pair<string_t, tournament_team_result>(
            member->team_id(),
            teamResult));
    }

    arbitratedSession->set_current_user_member_arbitration_results(results);
    xboxLiveContext->multiplayer_service().write_session(
arbitratedSession,
multiplayer_session_write_mode::update_existing)
    .then([](xbox_live_result<std::shared_ptr<multiplayer_session>> sessionResult)
    {
        if (sessionResult.err())
        {
            // Handle error.
        }
        else
        {
            // Update local session cache.
        }
    });
}
```

发生仲裁后，MPSD 将最终结果以及此处显示的一些其他属性一起放在 `/servers/arbitration/properties/system` (`multiplayer_session::arbitration_server()`)。

```json
{
    "resultState": "completed”,
    "resultSource": "arbitration",
    "resultConfidenceLevel": 75,

    "results": { ... }
}
```

**resultState** 可能包括：

* “completed”：所有活动的玩家均已报告结果。
* “partialresults”：部分但不是所有活动的玩家在仲裁超时前报告了结果。
* “noresults”：没有活动的玩家在仲裁超时前报告结果。
* “canceled”：在作废超时前没有活动的玩家到达。

如果是“noresults”和“canceled”，则省略结果。

**resultSource** 是当 MPSD 基于个别玩家报告的结果执行仲裁的“仲裁”。 游戏服务器可以绕过仲裁，写入 /servers/arbitration/properties/system 本身。 在这种情况下，它表示“服务器”的 **resultSource**。 游戏服务器也有可能重写（即修正或调整）结果，在这种情况下，它将 **resultSource** 设置为“已调整”。

**resultConfidenceLevel** 是介于 0 与 100 之间的整数，表示结果中的共识级别。 100 意味着所有玩家都同意。 0 意味着没有任何共识，并且基本上是从提交的所有结果中随机选择一个结果。 当游戏服务器设置仲裁结果时，它将设置 **resultConfidenceLevel** - 通常是 100，除非出于某种原因，甚至连游戏服务器本身都不确定（例如，如果它报告自己的按玩家投票过程的结果）。 调整结果时也设置 **resultConfidenceLevel**，以反映调整的结果中的置信度。

当 **resultSource** 为“仲裁”（且 **resultState** 为“completed”或“partialresults”）时，提供的结果将与至少一个玩家报告的结果完全相同。

### <a name="4--returning-to-the-xbox-arena-ui"></a>4. 返回到 Xbox Arena UI

比赛结束（或可能响应玩家放弃正在进行的比赛的请求）时，显示一个供玩家返回到他们加入比赛的 Xbox Arena UI 位置的选项。 这可能从比赛后结果屏幕或任何其他游戏结束 UI 执行。

要返回到Arena UI，使用 `Windows::System::Launcher` 类调用从协议激活 URI 传入的 **returnUri**。 请务必包括用户上下文。

启动 API 对 ERA 游戏的公开与对通用 Windows 平台 (UWP) 游戏的公开略有不同。 API 的 ERA 版本不允许提供 PFN，因此即使在激活 URI 中存在 PFN，也可以将其忽略。

以下是将用户返回到Arena UI 的 ERA 的示例代码。

```c++
void Sample::LaunchReturnUi(Uri ^returnUri, Windows::Xbox::System::User ^currentUser)
{
    auto options = ref new LauncherOptions();
    options->Context = currentUser;

    Launcher::LaunchUriAsync(returnUri, options);
}
```

UWP 游戏可以将 **LauncherOptions** 上的 **TargetApplicationPackageFamilyName** 属性设置为当在协议激活 URI 上提供 PFN 时返回 PFN。 这确保启动特定应用，如果尚未安装应用，则将用户导航到 Microsoft Store。

以下是将用户返回到 Arena UI 的 UWP 应用的示例代码。

```c++
void Sample::LaunchReturnUi(Uri ^returnUri, String ^returnPfn, User ^currentUser)
{
    auto options = ref new LauncherOptions();

    if (returnPfn != nullptr)
    {
        options->TargetApplicationPackageFamilyName = returnPfn;
    }

    Launcher::LaunchUriForUserAsync(currentUser, returnUri, options);
}
```

调用 Arena UI 之后，你的游戏可能一边在该同一屏幕上继续运行，一边等待另一个协议激活事件。 这样一来，如果玩家有其他比赛要进行，你的游戏准备就绪。 这加快了玩家从一场比赛到另一场比赛时在你的游戏与 Xbox Arena UI 之间进行切换的用户体验。

### <a name="handling-errors-and-edge-cases"></a>处理错误和边缘案例

上一节中的流程介绍了进行比赛的黄金路线。 有许多地方可能看到意外行为或可能产生问题。 此部分介绍了大量可能错误或边缘案例，并提供在你的游戏中对其进行处理的建议。

#### <a name="game-session-not-found"></a>找不到游戏会话。

如果使用比赛的会话参考启动你的游戏，且随后客户端失败且从 MPSD 请求该会话时获取错误，则向用户显示错误，表明其无法加入比赛。

#### <a name="user-attempts-to-join-a-match-that-has-been-played"></a>用户尝试加入已经玩过的游戏

如果用户尝试在报告结果后加入比赛，则阻止该客户端启动新比赛并向用户显示错误。 你可以通过循环访问会话成员来查看是否有任何成员将 `/members/{index}/arbitrationStatus` (`multiplayer_session_member::tournament_arbitration_status`) 设置为“complete”的方式检查是否已报告结果。

#### <a name="game-clients-unable-to-establish-a-p2p-connection"></a>游戏客户端无法建立 p2p 连接

如果你的游戏在多人游戏的客户端之间建立一对一 (p2p) 连接，且不支持中继，可能存在一起比赛的用户无法建立 p2p 连接的情况。

如果客户端能够检索比赛会话但无法连接到其他客户端，则在 `/members/{index}/properties/system/arbitration/results` (`multiplayer_session::set_current_user_member_arbitration_results()`) 下为每个团队报告“和局”结果。 这表明对 Xbox Live 而言，没有发生比赛。 它还可阻止用户通过强制 p2p 连接失败的方式在锦标赛中前进的漏洞。

#### <a name="game-client-disconnects-mid-match"></a>游戏客户端在比赛中断开连接

最常见的错误方案之一是客户端在比赛过程中断开连接。 根据比赛的状态和你的实现，处理该情况有几种选项：

* 如果锦标赛比赛是一对一，或如果团队的所有成员从其他客户端和/或你的游戏服务断开连接，我们建议你为断开连接的团队报告“输”。 这可以防止用户从他们输掉的比赛中强制断开连接来阻止报告结果的情况。
* 如果比赛发生在具有多个成员的团队之间，且团队的客户端的子集在比赛中断开连接，你可以选择此时结束比赛或允许其余用户继续比赛。 如果你选择继续进行比赛，你也可以选择允许断开连接的用户重新加入比赛。

#### <a name="full-teams-not-present"></a>整个团队全部未出场

如果你的游戏支持两个或多个团队比赛，你可能遇到某个团队的一些成员无法启动你的游戏进行比赛的情况。 在此情况下，你可以选择允许团队的成员在无团队成员的情况下进行比赛，并选择允许该团队成员在比赛过程中加入比赛。

或者，你可以自动强制执行结果。 如果这样做，等到 forfeitTimeout 时间时让所有参与者有机会在你强制执行结果之前加入比赛。 这允许你使用锦标赛游戏模式更改调整窗口，无需更新你的游戏。

#### <a name="length-of-match-exceeds-arbitrationtimeout"></a>比赛时长超过 arbitrationTimeout

将 **arbitrationTimeout** 的值设置为大于比赛可能持续的最大时长。 也就是说，你的游戏可能具有可能比赛时长非常长或没有最大值的模式。 在这些情况下，你考虑：

* 将锦标赛限制为仅在具有固定最大时长的模式下比赛。
* 告知用户时间限制，并在 **arbitrationTimeout** 前基于比赛中的决胜局报告结果。

由于 **arbitrationTimeout** 到期将触发比赛自动生成结果，因此请在整个 **arbitrationTimeout** 期间结束前报告游戏结果。  可以在时间缓冲区（例如，**arbitrationTimeout** - 1000ms）中生成结果，或使用单独的值来控制最大匹配长度。

#### <a name="other-cases"></a>其他情况

对于任何其他失败的情况，一般指导原则是告知用户错误并使用户返回到 Xbox Arena UI。

## <a name="experience-requirements-and-best-practices"></a>体验要求和最佳做法

由于 Arena 仅显示你的游戏的单场比赛，因此可随着时间的推移而引入新的格式，无需更新你的游戏。 因此，Arena 集成的游戏的基准用户体验必须简单且能充分灵活地使用多种竞争格式。

（可选）可以利用游戏“锦标赛中心”中的数据让锦标赛更好地融入游戏体验。  可在“xbox::services::tournaments”下找到此功能。  借助这些 API，你可以：
* 在游戏 UI 中突出显示锦标赛详细信息（锦标赛名称、团队名称等）
* 在游戏中提供用于发现锦标赛的功能
* 让用户在 Arena 比赛间隙留在游戏中

## <a name="configuring-a-title-for-arena"></a>为 Arena 配置游戏

若要为 Arena 启用游戏，一些额外步骤都是必需时在 Xbox 开发人员门户 (XDP) 或[合作伙伴中心](https://partner.microsoft.com/dashboard)中对其进行配置。

### <a name="enabling-arena-for-your-title"></a>为你的游戏启用 Arena

若要启用 Arena ，请转到 XDP 中你的游戏的服务配置页面，并选择“Arena”。

![](../../images/arena/arena-configure-xdp.png)

在这里，你将有多个选项：

* **启用 Arena** - 选择此复选框为此沙盒启用 Arena。
* **Arena 功能** - 此部分包括在沙盒中启用用户生成的锦标赛的复选框和启用跨平台游戏，允许多个平台上的用户参加同一个锦标赛的复选框。
* **Arena 平台** - 你可以为你的游戏选择玩锦标赛的平台。
* **锦标赛资产** -（以前位于“多人游戏和匹配”部分。）这些是你的游戏的锦标赛图像。

Arena 还可以在启用在合作伙伴中心中下的 Xbox Live 服务的**锦标赛**菜单中。

![在合作伙伴中心中的 arena 菜单](../../images/arena/Arena_On_WDC.JPG)

您必须发布服务配置，你的更改才会生效。 目前不支持通过 UDC 进行自助式 Arena 配置。 如果你使用 UDC 进行服务配置，则与你的开发客户经理一起进行 Arena 入门培训。

### <a name="setting-up-game-modes"></a>设置游戏模式

游戏模式是一项 Xbox Live 功能，可使发布者预配置锦标赛比赛的设置。 这些游戏模式在创建锦标赛时使用，用来设置 Xbox Live 和你的游戏为锦标赛强制执行的规则。 作为游戏发布者，你需要至少一个游戏模式来为你的游戏启用用户生成的锦标赛。 游戏模式的要素包括：

#### <a name="supports-ugt"></a>支持 UGT？

可以启用游戏模式与用户生成的锦标赛 (UGT) 一起使用。 如果你的游戏被配置为支持 UGT，你可以标记游戏模式，使 Xbox Live 用户在为你的游戏创建锦标赛时可以选择它们。

#### <a name="team-size"></a>团队大小

这是可以作为团队参加锦标赛的用户数量。 对于单用户游戏或锦标赛，游戏模式的团队大小为一。 Arena 目前不支持可变的锦标赛团队大小。

#### <a name="forfeittimeout"></a>forfeitTimeout

这是在比赛开始时间后以及如果没有玩家出现时（即没有玩家在会话中将自己设置为活动状态）取消比赛前之间的毫秒数。 将此设置为至少足够玩家启动你的游戏并在他们看到 toast 通知后进入锦标赛比赛的时长。

#### <a name="arbitrationtimeout"></a>arbitrationTimeout

这是在比赛开始时间后超过一段时间将不再接受结果的毫秒数。 将其设置为作废超时加上比最长的比赛可能需要的时间更长的时间。 这样一来，即使玩家刚好在作废时间之前开始玩，他们仍然有足够的时间完成比赛。 如果在 **arbitrationTimeout** 时间之前没有报告任何结果，则比赛的参与者全部获得输。 Xbox Arena 还会等到所有活动成员报告结果的 **arbitrationTimeout** 后才会开始仲裁。

此超时作为当出现错误且未获得某个玩家的结果报告时的一种支持。 与其拖延整个锦标赛，此超时设定了锦标赛将等待的时间上限。 出于该原因，它不应设置得过高，因为它决定每一轮锦标赛的最大时长。

#### <a name="name"></a>名称

这是游戏模式面向用户的名称。 此值可本地化。

#### <a name="description"></a>描述

这是游戏模式面向用户的描述。 此值可本地化。

#### <a name="custom"></a>自定义

游戏模式的**自定义**部分是一个属性包，其中开发人员可以为锦标赛比赛插入任何特定于游戏的配置设置。 作为自定义部分的一部分定义的值在 `/properties/custom/` 下写入比赛 MPSD 会话。

目前不支持通过 XDP 或 UDC 进行游戏模式设置。 若要为你的游戏创建游戏模式，请与你的开发人员客户经理联系。

### <a name="requirements-for-the-session-template"></a>会话模板要求

作为游戏开发人员，你必须提供 TO 在为比赛创建会话时要使用的会话模板。 对模板的内容有一些期望。

```json
{
    "version": 1,
    "inviteProtocol": "tournamentGame",
    "memberInitialization": null,

    "capabilities": {
        "gameplay": true,
        "arbitration": true,

        "large": false,
        "broadcast": false,
        "blockBadMsaReputation": false
    },

    "maxMembersCount": {maxMembersCount},
}
```

可以根据你自己的想法设置此处未显示的其他属性。

Arena 会话始终是版本 1。 将 **inviteProtocol** 设置为“tournamentGame”允许将比赛就绪通知发送到锦标赛参与者。 **memberInitialization** 设置为空以禁用 QoS。 必须设置玩游戏功能，因为会话表示比赛，且报告结果需要具有仲裁功能。 必须禁用 **large**、**broadcast** 和 **blockBadMsaReputation** 功能，因为它们会干扰会话的运行。

对于使用某模板的所有锦标赛具有固定值的设置，你的游戏可以在该模板的自定义部分指定它自己的设置。 以下提供了一个示例。

```json
        "custom": {
            "enableCheats": false,
            "bestOf": 3
        }
```

最后，需要 **maxMembersCount** 系统设置。 将其设置为可以进行锦标赛比赛的玩家的总数量。 玩家数量可以进一步通过游戏模式设置进行限制，因此请确保在会话中设定的值表示你的游戏支持的最大玩家总数量。 例如，如果你的游戏支持比赛的最大玩家数量是 5 对 5，则将 **maxMembersCount** 设置为 10。 此 MPSD 模板可用来支持最多为 **maxMembersCount** 的任何玩家数量的比赛。

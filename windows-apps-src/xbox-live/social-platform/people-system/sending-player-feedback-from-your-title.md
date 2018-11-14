---
title: 从游戏中发送玩家反馈
author: KevinAsgari
description: 了解你的游戏如何通过向 Xbox Live 信誉服务发送玩家反馈来帮助提升积极的玩家体验。
ms.assetid: 49f8eb44-1e31-4248-b645-9123df6f8689
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 信誉, 玩家反馈
ms.localizationpriority: medium
ms.openlocfilehash: 6a0d6693fb1d97a408a8b6559bb7c317a0af0030
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6273217"
---
# <a name="sending-player-feedback-from-your-title"></a>从游戏中发送玩家反馈
Xbox Live 成员大部分都很出色，但有一小部分“坏苹果”损害了其他人的游戏体验。 我们通过用户和游戏提交的反馈发现了这一小部分用户。 我们通过确保让这些“坏苹果”只有有限的多玩家体验，从而无法干扰良好玩家玩游戏，来保护其余的用户。 Xbox 非常依赖用户报告其他用户行为来保持系统的准确性，但 Xbox One 中的游戏可以直接参与并显著改善用户信誉群体的准确性。

## <a name="steps-to-submit-feedback-from-title-or-title-service"></a>从游戏或游戏服务提交反馈的步骤
1. 将反馈时刻添加到游戏或游戏服务
2. 确定正确的反馈类型
3. 通过反馈调用信誉反馈 API

### <a name="adding-feedback-moments-to-title-or-title-service"></a>将反馈时刻添加到游戏或游戏服务
所有玩家都遇到过不愉快的体验：在自己一方搞破坏的队友，袖手旁观、不积极参与的玩家，还有破坏游戏规则的作弊者。 Xbox Live 允许用户直接报告这些问题玩家，但用户反馈并不理想。 游戏可以轻松确定一些简单情况，如玩家在游戏中无所事事、提前退出，有时甚至可以确定有人在作弊。 你可以随时在游戏中提交反馈，这将帮助改善所有良好玩家的体验。

### <a name="determining-the-correct-feedback-type"></a>确定正确的反馈类型
信誉系统有很多反馈类型，目的是捕获用户为保证反馈可能采取的各种方式。 它们已在下面的表 1 中全部列出。 其中大部分是负面的，但也有可能通过正面的反馈改善用户信誉。

Xbox 系统 UI 为玩家提供了提交有关游戏中其他用户的反馈的方式。 这种用户间反馈所占比重不大，因为用户在失败时很可能会互相恶意破坏。 游戏可以弥补该系统 UI 的不足，为用户提供直接提交有关他人的反馈的 UI，但是我们更希望游戏通过合作伙伴反馈代表游戏提交反馈。 合作伙伴反馈受到高度信任。

## <a name="example-partner-feedback-usage-scenarios"></a>合作伙伴反馈使用情境示例

### <a name="user-quitting-a-game-in-the-middle-of-a-match"></a>用户在匹配期间退出游戏
玩家丢失匹配，并使用游戏菜单退出游戏，放弃了她的队友。 当游戏玩家检测到此行为时，它们可以使用 **FairPlayQuitter** 报告此行为。

### <a name="idling-after-match-found-in-game"></a>被发现在游戏中匹配后空闲
玩家与其他玩家匹配一起玩游戏，但在游戏中一直空闲而不为团队帮忙。 游戏玩家可以使用 **FairplayIdler** 报告这种玩家行为。

### <a name="killing-teammates-in-game"></a>在游戏中杀死队友
游戏中的玩家经常通过杀死队友来取乐。 当游戏玩家检测到有用户一直在杀队友时，可以使用 **FairPlayKillsTeammates** 报告该玩家。

### <a name="title-has-community-kickvote-feature"></a>游戏提供了社区踢人/投票功能
某玩家在该轮游戏中因其恶劣行为被其他玩家投票后从会话中删除。 如果游戏玩家从会话中删除了该玩家，它可以使用 **FairPlayKicked** 报告该用户。

### <a name="helpful-player-community-vote"></a>有用的玩家社区投票
几轮游戏后，游戏将提供选择帮助最大用户的选择选项。 当游戏玩家发现此操作时，它们可以使用 **PositiveHelpfulPlayer** 报告此行为。

### <a name="high-quality-ugc-user-generated-content"></a>高质量 UGC（用户生成的内容）
游戏包含一个场景，玩家可以在其中为工具选择最佳设计。 当游戏玩家发现此操作时，它们可以使用 **PositiveHighQualityUGC** 报告此行为。

### <a name="skilled-player"></a>熟练的玩家
几轮游戏后，游戏将提供选择成为最佳玩家的 MVP 用户的选择选项。 当游戏玩家发现玩家持续获得 MVP 状态时，它可以使用 **PositiveSkilledPlayer** 报告此行为。

### <a name="user-reports-questionable-ugc-in-title"></a>用户在游戏中报告可疑 UGC
游戏包含一个场景，玩家可以在其中查看工具设计。 玩家注意到具有攻击性的设计，并且想要报告。 当游戏玩家发现此操作时，它们可以使用 **UserContentInappropriateUGC** 报告此冒犯者。

### <a name="title-wants-to-request-an-xbl-ban-review-of-a-player-in-their-title"></a>游戏玩家想要在游戏中请求某玩家的 XBL 禁止评价
游戏的社区管理员注意到在游戏中不断制造麻烦的低信誉玩家。 游戏玩家可以使用 **FairPlayUserBanRequest** 请求 XBL 策略和强制团队评价。

## <a name="complete-behavior-feedback-options"></a>完成行为反馈选项
下表列出了可用于代表你的游戏提交用户反馈的反馈类型。 信誉服务很灵活，并且可以在你认为某些行为不符合你的游戏需求时，轻松添加新反馈类型。 如果你想要看到添加的新反馈类型，请告知你的客户经理。

表 1：信誉服务支持的各种合作伙伴反馈类型。

**公平比赛反馈类型**               | **描述**
----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------
FairplayKillsTeammates                    | 报告故意杀死自己队友的玩家
FairplayCheater                           | 报告确定作弊的玩家
FairplayTampering                         | 报告已确定干扰了游戏磁盘或篡改游戏软件或硬件的玩家
FairplayUserBanRequest                    | 报告你认为引起暂停的玩家
FairplayConsoleBanRequest                 | 报告你认为应该禁止其连接到 Xbox Live 的主机
FairplayUnsporting                        | 报告确定参与了不道德行为的玩家
FairplayIdler                             | 报告进入了多玩家匹配但不积极加入游戏的玩家
FairplayLeaderboardCheater                | 报告确定经过作弊在排行榜上排名靠前的玩家
**通信反馈类型**         |
CommsInappropriateVideo                   | 报告在视频聊天中行为不当的玩家
**用户生成的内容反馈类型** |
UserContentInappropriateUGC               | 报告玩家在游戏中创建的不当内容
UserContentReviewRequest                  | 主动报告内容，以便 XBLPET 团队进行审阅
UserContentReviewRequestBroadcast         | 主动报告广播，以便 XBLPET 团队进行审阅
UserContentReviewRequestGameDVR           | 主动报告 GameDVR 剪辑，以便 XBLPET 团队进行审阅
UserContentReviewRequestScreenshot        | 主动报告屏幕截图，以便 XBLPET 团队进行审阅
**正面反馈**                     |
PositiveSkilledPlayer                     | 如果用户可以通过投票来确定 MVP，在确定玩家应获得积极反馈时报告熟练的玩家
PositiveHelpfulPlayer                     | 如果游戏为玩家提供了报告另一个有帮助用户的 UI，报告有帮助的玩家
PositiveHighQualityUGC                    | 如果游戏为玩家提供了称赞其他用户内容的 UI，主动报告该内容

## <a name="feedback-api-calls"></a>反馈 API 调用
游戏可以使用两个策略来调用信誉服务。 首选方法是使用用于进行身份验证的服务令牌从合作伙伴服务作为整体报告用户。 游戏也可以从客户端直接报告用户。 客户端 API 具有内置的反欺诈技术，它需要将多个有关用户的报告视为有效。 这两个 API 均为批处理，可以同时报告多个用户。

游戏可以使用以下 Xbox Live API 来提交玩家信誉反馈：

语言 | API
-------- | --------------------------------------------------------------------------------------------
WinRT    | Microsoft::Xbox::Services::Social::ReputationService.SubmitBatchReputationFeedbackAsync(...)
C++      | xbox::services::social::reputation_service.submit_batch_reputation_feedback(...)

此外，游戏还可以使用以下直接 REST 方法：

API          | URL                                                      | 身份验证要求
------------ | -------------------------------------------------------- | ---------------------------------------
服务 POST | https://reputation.xboxlive.com/users/batchfeedback      | 合作伙伴和沙盒要求的 S-token
客户端 POST  | https://reputation.xboxlive.com/users/batchtitlefeedback | 游戏和沙盒要求的 Xtoken

## <a name="feedback-object"></a>反馈对象
反馈对象具有以下最新版本 101 的规范。 两个 API 均应有下面的一批对象。

成员       | 类型   | 描述
------------ | ------ | -----------------------------------------------------------------------------------------------------------------------------------------------------------------
sessionRef   | 对象 | 描述此反馈与之相关的 MPSD 会话的对象，或为 null。
feedbackType | 字符串 | 反馈的类型。 在 ReputationFeedbackType 枚举中定义的可能值
textReason   | 字符串 | 发件人添加的用来说明反馈提交原因的用户提供的文本。 这对于我们的策略执行团队非常有价值。
evidenceId   | 字符串 | 可用作所提交反馈的证据的资源的 ID，例如，玩游戏时录制的视频文件或活动源项目。
titleID      | 字符串 | 所玩游戏的游戏 ID；仅当令牌不存在时需要。
targetXuid   | 字符串 | 你正在报告的玩家的 XUID。

示例：

```json
POST https://reputation.xboxlive.com/users/batchtitlefeedback
{
    "items" :
    [
        {
            "targetXuid": "33445566778899",
            "titleId" : null,
            "sessionRef": {
  "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
  "templateName": "CaptureFlag5",
  "name": "Title56932",
           },
            "feedbackType": "FairPlayKillsTeammates",
            "textReason": "Title detected this player killing team members 19 times",
            "evidenceId": null
        }
    ]
}
```

## <a name="feedback-qa"></a>反馈常见问题

### <a name="q-can-i-send-a-hint-to-the-system-to-help-with-humans-that-might-be-looking-at-the-player-report"></a>问：我能否向系统发送提示，以帮助可能在查看玩家报告的人？
答：可以，这会很有帮助！ 请使用 "textReason" 参数帮助最终将查看所提交反馈的强制执行人员。 例如，对于空闲玩家，你可以添加这样的文本原因：“游戏开始后的前五秒过后，我们没有收到来自该玩家的用户输入”。 此文本原因对于 XBLPET 强制执行代理可能非常有价值，因此请确保文本原因有用且是描述性的。

### <a name="q-should-i-worry-about-how-often-i-send-in-feedback-on-a-user"></a>问：在发送有关某个用户的反馈时是否有频率限制？
答：当游戏确信某个用户获得了反馈时，它们应会调用信誉服务。 系统具有多个安全措施来阻止游戏和用户过度影响其他用户。

### <a name="q-can-i-adjust-the-weight-of-the-feedback-being-sent"></a>问：我能否调整所发送反馈的权重？
答：不能，信誉服务将确定反馈的权重。 我们始终会调整权重以改进系统。

### <a name="q-can-i-undo-feedback-ive-sent-on-a-user"></a>问：我能否撤消已经发送的用户反馈？
答：不能，反馈是最终行为。 如果你认为你的游戏存在 bug，正在发送错误反馈，请告诉我们，我们会将该游戏放入黑名单，直到你修复了 bug。

### <a name="q-can-i-see-the-feedback-sent-for-my-title-from-users"></a>问：我能否看到其他用户发送的有关我的游戏的反馈？
答：该信息目前不能自助查看。 你可以询问客户经理，我们确实有每个游戏的数据，可以向你提供。 我们很快会让你直接看到这些信息，另外还会显示在使用你的游戏时信誉较低的一组用户。

### <a name="q-when-i-send-in-console-or-user-ban-review-request-how-do-i-know-what-happened"></a>问：当我在主机或用户禁止评价请求中发送时，我如何知道发生了什么？
答：目前，评价信息将发送给 XBL 策略和执行团队，但现在你看不到禁止评价的新进展。

### <a name="q-is-there-a-reputation-score-per-title"></a>问：各个游戏有信誉评分吗？
答：没有。 目前存在公平比赛、通信和用户生成的内容的信誉分项评分，但不是按游戏评分。 如果需求较多，我们可能会在将来增加此功能，因此，如果你需要该功能，请告知你的客户经理。

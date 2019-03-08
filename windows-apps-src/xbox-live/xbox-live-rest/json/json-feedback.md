---
title: Feedback (JSON)
assetID: 726117c1-f01b-18c0-3b75-a7a7d27d84a2
permalink: en-us/docs/xboxlive/rest/json-feedback.html
description: " Feedback (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9cc1cb4ecc12219d54af1c4ab420671f2bbfa81f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644762"
---
# <a name="feedback-json"></a>Feedback (JSON)
包含有关播放器的反馈信息。
<a id="ID4EN"></a>


## <a name="feedback"></a>反馈

反馈对象具有以下规范。

| 成员| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| sessionRef| 对象 | 描述此反馈与之相关的 MPSD 会话的对象，或为 null。 |
| feedbackType| 字符串 | 反馈的类型。 可能的值中定义<b>Microsoft.Xbox.Services.Social.ReputationFeedbackType</b>。 |
| textReason| 字符串| 发件人添加的用来说明反馈提交原因的用户提供的文本。 |
| voiceReasonId| 字符串| 发件人添加以解释反馈的原因是 Kinect 中的用户提供语音文件的 ID 提交 (Base-64)。 |
| evidenceId| 字符串| 可以用作要提交的反馈的证据的资源的 ID，例如，视频文件记录在玩游戏期间。 |

<a id="ID4EVC"></a>


### <a name="feedback-types"></a>反馈类型

"发送"列将指示谁可以提交反馈。

   * "用户"意味着它可以通过在控制台中的身份验证使用 XToken 提交，因此该 API 可以接受**SubmitFeedback**。
   * "合作伙伴"意味着它可以由合作伙伴提交使用声明证书，因此该 API 可以接受**SubmitBatchFeedback**。
   * "隐私"意味着只有 SLS 隐私服务可以发送反馈。
   * "None"意味着反馈在内部生成的审核的 SLS 信誉服务和不能发送的任何调用方。

| 在任务栏的搜索框中键入| 由发送| 注释|
| --- | --- | --- | --- | --- | --- |
| CommsAbusiveVoice| 用户| 用户将反馈发送到报表不合适的语音内的通信从标题从 Xbox 仪表板。 |
| CommsInappropriateVideo| 用户、 合作伙伴| 用户和合作伙伴发送反馈，报告来自不适合视频标题内以及从 Xbox 仪表板。 |
| CommsMuted| 隐私| 当用户使静音其他播放机时，隐私将此反馈发送到信誉服务。 |
| CommsPhishing| 用户| 用户发送此反馈，报告仿冒网站邮件。 |
| CommsPictureMessage| 用户| 收件箱服务调用更新基于图片的通信的发件人信誉，并向强制执行团队报告反馈的信誉服务。 |
| CommsSpam| 用户| 用户发送此反馈，报告垃圾邮件消息。 |
| CommsTextMessage| 用户| 收件箱服务调用的更新的发件人信誉，并向强制执行团队报告反馈的信誉服务。 **注意：** 收件箱 UI 应具有一个按钮，让用户能够将邮件标记。 |
  | CommsVoiceMessage | 用户 | 收件箱服务调用更新基于通信的语音消息的发件人信誉，并向强制执行团队报告反馈的信誉服务。  |
  | FairPlayBlock | 隐私 | 当用户阻止其他播放器时，隐私会将此反馈发送到信誉服务。  |
  | FairPlayCheater | 用户、 合作伙伴 | 确定用户不真实的标题可以发送此反馈而无需用户干预。  |
  | FairPlayConsoleBanRequest | 合作伙伴 | 合作伙伴发送此反馈建议禁止从 Xbox Live 的控制台。  |
  | FairPlayIdler | 用户、 合作伙伴 | 确定是否用户代表空闲有意在游戏中，通常在轮之后, 倒圆角的标题可以发送此反馈而无需用户干预。  |
  | FairPlayKicked | 用户、 合作伙伴 | 检测用户获赞成票数超出 （启动） 的游戏的标题可以发送此反馈而无需用户干预。  |
  | FairPlayKillsTeammates | 用户、 合作伙伴 | 可以在播放机 killls 时自动确定的标题他的队友可以发送此反馈而无需用户干预。  |
  | FairPlayQuitter | 用户、 合作伙伴 | 确定用户提前退出游戏的标题可以发送此反馈而无需用户干预。  |
  | FairPlayTampering | 用户、 合作伙伴 | 确定用户已篡改磁盘上内容的标题可以发送此反馈而无需用户干预。  |
  | FairPlayUnblock | 隐私 | 隐私将此反馈发送到信誉服务，当用户取消阻止其他播放器。  |
  | FairPlayUserBanRequest | 合作伙伴 | 合作伙伴将作为一个建议，禁止从 Xbox Live 用户发送此反馈。  |
  | InternalAmbassadorScoreUpdated | 无 | 这是不以供调用方内部的反馈类型。  |
  | InternalReputationReset | 无 | 这是不以供调用方内部的反馈类型。  |
  | InternalReputationUpdated | 无 | 这是不以供调用方内部的反馈类型。  |
  | PositiveHelpfulPlayer | 用户、 合作伙伴 | 用户和合作伙伴发送此反馈，提交正信息很有帮助的资深玩家通过中的游戏、 论坛和等等。  |
  | PositiveHighQualityUGC | 用户、 合作伙伴 | 用户和合作伙伴发送此反馈，指示标题应允许用户提交上从游戏中的共享 UGC 的正面反馈等优化 Forza 中的设置。  |
  | PositiveSkilledPlayer | 用户、 合作伙伴 | 用户和合作伙伴发送此反馈，指示标题可以允许用户对 MVP 投票 MPSD 会话结束时。  |
  | UserContentGamerpic | 用户 | 用户发送此反馈，报告的不适当的玩家图片直接从玩家卡。  |
  | UserContentGamertag | 用户 | 用户发送此反馈，报告的不适当的玩家标记直接从玩家的卡。  |
  | UserContentInappropriateUGC | 用户、 合作伙伴 | 用户和合作伙伴发送此反馈，指示标题应允许用户标记不适当的共享的 UGC 从内游戏时，例如，油漆 Forza 中的作业。  |
  | UserContentPersonalInfo | 用户 | 用户发送此反馈，报表作者简介和直接从玩家卡其他个人信息。  |

<a id="ID4EFEAC"></a>


## <a name="sample-json-syntax"></a>示例 JSON 语法


```json
{
"sessionRef": {
  "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
  "templateName": "CaptureFlag5",
  "name": "Halo556932",
  },
  "feedbackType": "CommsAbusiveVoice",
  "textReason": "He called me a doodoo-head!",
  "voiceReasonId": null,
  "evidenceId": null
}

```


<a id="ID4EOEAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EQEAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

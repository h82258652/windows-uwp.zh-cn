---
title: Feedback (JSON)
assetID: 726117c1-f01b-18c0-3b75-a7a7d27d84a2
permalink: en-us/docs/xboxlive/rest/json-feedback.html
author: KevinAsgari
description: " Feedback (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e9ef3bb19155199ae94b18b828fb40eb7a0a2ce6
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2018
ms.locfileid: "4507164"
---
# <a name="feedback-json"></a>Feedback (JSON)
包含有关玩家的反馈信息。
<a id="ID4EN"></a>


## <a name="feedback"></a>反馈

反馈对象具有以下规范。

| 成员| 类型| 描述|
| --- | --- | --- |
| sessionRef| 对象 | 描述此反馈与之相关的 MPSD 会话的对象，或为 null。 |
| feedbackType| 字符串 | 反馈的类型。 <b>Microsoft.Xbox.Services.Social.ReputationFeedbackType</b>中定义了可能的值。 |
| textReason| 字符串| 发件人添加的用来说明反馈提交原因的用户提供的文本。 |
| voiceReasonId| 字符串| 从 Kinect 发件人添加以解释原因反馈的用户提供语音文件的 ID 提交 (Base-64)。 |
| evidenceId| 字符串| 记录的 ID 可用作所提交反馈的证据的资源，例如，视频文件玩游戏过程。 |

<a id="ID4EVC"></a>


### <a name="feedback-types"></a>反馈类型

"发送"列指示谁可以提交反馈。

   * "用户"意味着它才能提交用于 XToken 身份验证，因此控制台 API 可以接受**SubmitFeedback**。
   * "合作伙伴"意味着可以提交它由合作伙伴使用声明证书，因此该 API 可接受**SubmitBatchFeedback**。
   * "隐私"意味着只有 SLS 隐私服务可以发送反馈。
   * "None"意味着反馈 SLS 信誉服务审核内部生成，并且无法发送的任何调用方。

| 类型| 由发送| 注释|
| --- | --- | --- | --- | --- | --- |
| CommsAbusiveVoice| 用户| 用户向报告不恰当的语音通信从游戏内和 Xbox 仪表板发送反馈。 |
| CommsInappropriateVideo| 用户合作伙伴| 用户和合作伙伴发送反馈报告不恰当视频从游戏内和 Xbox 仪表板。 |
| CommsMuted| 隐私| 当用户静音另一个玩家时，隐私向信誉服务发送此反馈。 |
| CommsPhishing| 用户| 用户发送此反馈报告的钓鱼消息。 |
| CommsPictureMessage| 用户| 收件箱服务调用信誉服务，它会更新发件人的信誉，具体取决于图片的通信并报告给执行团队的反馈。 |
| CommsSpam| 用户| 用户发送此反馈报告垃圾邮件。 |
| CommsTextMessage| 用户| 收件箱服务调用信誉服务，该更新发件人的信誉，报告给执行团队的反馈。 **注意：** 收件箱 UI 应该有一个按钮，以允许用户在标志一条消息。 |
  | CommsVoiceMessage | 用户 | 收件箱服务调用信誉服务，它会更新发件人的信誉，具体取决于语音消息的通信并报告给执行团队的反馈。  |
  | FairPlayBlock | 隐私 | 当用户阻止其他玩家，隐私向信誉服务发送此反馈。  |
  | FairPlayCheater | 用户合作伙伴 | 确定用户在作弊的游戏可以发送此反馈无需用户干预。  |
  | FairPlayConsoleBanRequest | 合作伙伴 | 合作伙伴将作为建议禁止从 Xbox Live 的主机发送此反馈。  |
  | FairPlayIdler | 用户合作伙伴 | 如果用户处于空闲目的游戏时，通常圆形后一轮，确定的游戏可以发送此反馈无需用户干预。  |
  | FairPlayKicked | 用户合作伙伴 | 检测用户已进行投票退出 （踢） 的游戏的游戏可以发送此反馈无需用户干预。  |
  | FairPlayKillsTeammates | 用户合作伙伴 | 可以在玩家 killls 时自动确定的游戏他名队友就可以发送此反馈无需用户干预。  |
  | FairPlayQuitter | 用户合作伙伴 | 确定用户提前退出游戏的游戏可以发送此反馈无需用户干预。  |
  | FairPlayTampering | 用户合作伙伴 | 确定用户已篡改磁盘上的内容的游戏可以发送此反馈无需用户干预。  |
  | FairPlayUnblock | 隐私 | 当用户取消阻止其他玩家，隐私向信誉服务发送此反馈。  |
  | FairPlayUserBanRequest | 合作伙伴 | 合作伙伴将作为建议禁止从 Xbox Live 用户发送此反馈。  |
  | InternalAmbassadorScoreUpdated | 无 | 这是不用于调用方内部反馈类型。  |
  | InternalReputationReset | 无 | 这是不用于调用方内部反馈类型。  |
  | InternalReputationUpdated | 无 | 这是不用于调用方内部反馈类型。  |
  | PositiveHelpfulPlayer | 用户合作伙伴 | 用户和合作伙伴发送此反馈提交有关从游戏、 论坛、 等内的有用同事的玩家的正面信息。  |
  | PositiveHighQualityUGC | 用户合作伙伴 | 用户和合作伙伴发送此反馈以指示游戏应允许用户提交的游戏内从共享 UGC 上的正面反馈例如，调节 Forza 中的设置。  |
  | PositiveSkilledPlayer | 用户合作伙伴 | 用户和合作伙伴发送此反馈以指示游戏可以让用户能够 MPSD 会话结束时对 MVP 投票。  |
  | UserContentGamerpic | 用户 | 用户发送此反馈报告直接通过玩家卡片不恰当的玩家图片。  |
  | UserContentGamertag | 用户 | 用户发送此反馈报告直接通过玩家卡片的不恰当玩家标记。  |
  | UserContentInappropriateUGC | 用户合作伙伴 | 用户和合作伙伴发送此反馈以指示游戏应允许用户标志在游戏中的不恰当共享的 UGC 例如，Forza 画图作业。  |
  | UserContentPersonalInfo | 用户 | 用户发送此反馈报告简介和玩家卡片直接从其他个人信息。  |

<a id="ID4EFEAC"></a>


## <a name="sample-json-syntax"></a>JSON 语法示例


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

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

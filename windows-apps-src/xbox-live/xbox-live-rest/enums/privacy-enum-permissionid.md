---
title: PermissionId 枚举
assetID: 3e7ee317-4946-1105-ecd7-1e26346deccb
permalink: en-us/docs/xboxlive/rest/privacy-enum-permissionid.html
description: " PermissionId 枚举"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: aff463e2268af972536984a00e29348bf0a6859e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594682"
---
# <a name="permissionid-enumeration"></a>PermissionId 枚举
详细介绍 PermissionId 枚举。
权限 Id 可以在权限验证 Url:

   * [GET (/users/{requestorId}/permission/validate)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidateget.md)
   * [POST (/users/{requestorId}/permission/validate)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidatepost.md)

这些 Id 包括直接检查针对的用户，例如检查单个隐私设置目标或单个特权执行组件的特定设置。 此外，还有权限可用于 API 的权限和合并检查针对特定的用户操作的多个设置的 Id。

<a id="ID4EIB"></a>


## <a name="permissions"></a>权限

这些是调用方可以使用它来检查是否可以执行特定操作的值。 与上述设置，这些封装由服务定义的策略并不能直接更改用户，但在大多数情况下，策略构建基于其值由用户定义的一个或多个设置。 这些是根据上面定义的多个设置通常复合检查。 示例：<b>ViewProfile</b>权限执行检查的目标的<b>ShareProfile</b>隐私设置和请求者的<b>AllowProfileViewing</b>特权。

一般情况下，建议调用方请求需要检查，而不是直接检查隐私设置和权限的操作的权限 ID。 这样，若要在服务之间一致地更改，因为新的检查，并且包含的隐私策略。

| 权限名称| 描述|
| --- | --- |
| CommunicateUsingText| 检查用户可以将包含文本内容的消息发送到目标用户|
| CommunicateUsingVideo| 检查用户可以通信视频中使用的目标用户|
| CommunicateUsingVoice| 检查用户能够与目标用户使用语音|
| ViewTargetProfile| 检查用户可以查看目标用户的配置文件|
| ViewTargetGameHistory| 检查用户可以查看目标用户的游戏历史记录|
| ViewTargetVideoHistory| 检查用户可以查看视频监视的详细历史记录目标用户|
| ViewTargetMusicHistory| 检查用户可以查看目标用户的历史记录详细的音乐侦听|
| ViewTargetExerciseInfo| 检查用户可以查看目标用户的练习信息|
| ViewTargetPresence| 检查用户可以查看目标用户的联机状态|
| ViewTargetVideoStatus| 检查用户可以查看目标视频状态 （扩展联机状态） 的详细信息|
| ViewTargetMusicStatus| 检查用户可以查看目标音乐状态 （扩展联机状态） 的详细信息|
| ViewTargetUserCreatedContent| 检查用户可以查看其他用户的用户创建内容|

---
title: PermissionId 枚举
assetID: 3e7ee317-4946-1105-ecd7-1e26346deccb
permalink: en-us/docs/xboxlive/rest/privacy-enum-permissionid.html
author: KevinAsgari
description: " PermissionId 枚举"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f58c2d0f68e1f65820104928e45a09ccfdb259cb
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2018
ms.locfileid: "4053184"
---
# <a name="permissionid-enumeration"></a>PermissionId 枚举
详细介绍 PermissionId 枚举。
权限 Id 可与权限验证 Url:

   * [GET (/users/{requestorId}/permission/validate)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidateget.md)
   * [POST (/users/{requestorId}/permission/validate)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidatepost.md)

这些 Id 包括针对用户，如检查目标或单个特权参与者的单个隐私设置的特定设置的直接检查。 此外，还有权限可授予权限 API 和将针对特定用户操作的多个设置的检查的 Id。

<a id="ID4EIB"></a>


## <a name="permissions"></a>权限

以下是调用方可以使用它来检查是否可以执行特定操作的值。 与上面的设置，这些封装的服务定义的策略，并不能直接更改由用户，但在大多数情况下，基于其值由用户定义的一个或多个设置将构建策略。 以下是针对多个上面定义的设置通常复合检查。 示例： <b>ViewProfile</b>权限执行操作目标的<b>ShareProfile</b>隐私设置和请求者的<b>AllowProfileViewing</b>权限的检查。

一般情况下，建议的调用方请求需要检查，而不是直接检查隐私设置和权限的操作的权限 ID。 这允许隐私策略，如新的检查，并且包含跨整个服务完全一致地更改。

| 权限名称| 说明|
| --- | --- |
| CommunicateUsingText| 检查用户可以向目标用户发送的邮件包含文本内容|
| CommunicateUsingVideo| 检查用户可以使用与目标用户的视频通信|
| CommunicateUsingVoice| 检查用户可以使用与目标用户的语音通信|
| ViewTargetProfile| 检查用户可以查看目标用户的配置文件|
| ViewTargetGameHistory| 检查用户可以查看目标用户的游戏历史记录|
| ViewTargetVideoHistory| 检查用户可以查看详细视频观看的历史记录目标用户|
| ViewTargetMusicHistory| 检查用户可以查看目标用户的详细的音乐侦听历史记录|
| ViewTargetExerciseInfo| 检查用户可以查看练习目标用户的信息|
| ViewTargetPresence| 检查用户可以查看目标用户的联机状态|
| ViewTargetVideoStatus| 检查用户可以查看目标视频状态 （扩展联机状态） 的详细信息|
| ViewTargetMusicStatus| 检查用户可以查看目标音乐状态 （扩展联机状态） 的详细信息|
| ViewTargetUserCreatedContent| 检查用户可以查看其他用户的用户创建内容|

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
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3880639"
---
# <a name="permissionid-enumeration"></a>PermissionId 枚举
详细介绍 PermissionId 枚举。
权限 Id 可以使用权限验证 Url:

   * [获取 （/users/ {requestorId} / 权限/验证）](../uri/privacy/uri-privacyusersrequestoridpermissionvalidateget.md)
   * [POST （/users/ {requestorId} / 权限/验证）](../uri/privacy/uri-privacyusersrequestoridpermissionvalidatepost.md)

这些 Id 包括对用户，如检查目标或单个特权参与者的单个隐私设置的特定设置的直接检查。 此外，还有权限可与权限 API 和将根据特定用户操作的多个设置的检查的 Id。

<a id="ID4EIB"></a>


## <a name="permissions"></a>权限

以下是调用方可以使用它来检查是否可以执行特定操作的值。 与上面的设置，这些封装的服务定义的策略，并不能直接更改由用户，但在大多数情况下，策略生成一个或多个设置的值由用户顶部。 这些是针对上面定义的多个设置通常复合检查。 示例： <b>ViewProfile</b>权限执行操作目标的<b>ShareProfile</b>隐私设置和请求者的<b>AllowProfileViewing</b>权限的检查。

一般情况下，建议的调用方请求需要检查，而不是直接检查隐私设置和权限的操作的权限 ID。 这允许隐私策略，如新的检查合并，完全一致地更改跨整个服务。

| 权限名称| 说明|
| --- | --- |
| CommunicateUsingText| 检查用户可以将具有文本内容的一条消息发送给目标用户|
| CommunicateUsingVideo| 检查用户可以使用目标用户的视频通信|
| CommunicateUsingVoice| 检查用户可以使用目标用户的语音通信|
| ViewTargetProfile| 检查用户可以查看目标用户的配置文件|
| ViewTargetGameHistory| 检查用户可以查看目标用户的游戏历史记录|
| ViewTargetVideoHistory| 检查用户可以查看的详细视频观看历史记录目标用户|
| ViewTargetMusicHistory| 检查用户可以查看目标用户的详细的音乐侦听历史记录|
| ViewTargetExerciseInfo| 检查用户可以查看目标用户的练习信息|
| ViewTargetPresence| 检查用户可以查看目标用户的联机状态|
| ViewTargetVideoStatus| 检查用户可以查看目标视频状态 （扩展联机状态） 的详细信息|
| ViewTargetMusicStatus| 检查用户可以查看目标音乐状态 （扩展联机状态） 的详细信息|
| ViewTargetUserCreatedContent| 检查用户可以查看其他用户的用户创建内容|

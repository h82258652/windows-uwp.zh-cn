---
title: "在 Unity 中检查用户权限设置"
author: StaceyHaffner
description: "了解如何检查已登录 Xbox Live 帐户的权限设置。"
ms.assetid: 
ms.author: sthaff
ms.date: 10/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 帐户, 测试帐户, 家长控制, 用户权限, 强制禁止, 升级销售"
ms.openlocfilehash: 45c5785899dcdda596fe66bb76930a781d50b77d
ms.sourcegitcommit: d5f74029f9844603914e8e46a8d4dfe078c2a90a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2017
---
# <a name="check-user-privilege-settings-in-unity"></a>在 Unity 中检查用户权限设置
在 Xbox Live 中，每个经过身份验证的用户帐户都具有相关权限。 权限控制着用户在给定时间点可以访问 Xbox Live 的哪些功能。 有些权限用于系统控制的功能，而其他权限可能与特定的游戏或扩展订阅相关。 此外，Xbox Live 执行团队颁发的家长控制和禁止可以限制用户的权限。 这些权限涵盖许多常见场景，包括多人游戏、访问用户生成的内容、聊天或流式传输视频。 游戏使用此信息来做出访问控制和个性化决策。 

## <a name="prerequisites"></a>系统必备
为了确定用户权限设置，您必须使用 Xbox Live 配置游戏的身份验证并使用户成功登录。 下面的文章概述了可以执行的步骤：

* [在 Unity 中配置 Xbox Live](check-user-privileges-in-unity.md)
* [在 Unity 中登录到 Xbox Live](sign-in-to-xbox-live-in-unity.md)

## <a name="determine-privileges"></a>确定权限
用户的权限包含在身份验证时收到的令牌中。 在 Unity 中，你可以在用户成功登录后在 `XboxLiveUser` 类中访问该用户所拥有权限的列表。 权限存储为单个字符串，以空格分隔。 例如，你可能会看到具有以下权限的用户：

```csharp
public XboxLiveUserInfo XboxLiveUser;

//sign in is done and the user has been successfully signed in

Debug.Log(XboxLiveUser.User.Privileges);

//Console would read:
// Privileges: "188 191 192 193 194 195 196 198 199 200 201 203 204 205 206 207 208 211 214 215 216 217 220 224 227 228 235 238 245 247 249 252 254 255"
```

如果需要查找特定权限，可检查 `Privileges` 属性是否包含相关代码：

```csharp
public XboxLiveUserInfo XboxLiveUser;

//sign in is done and the user has been successfully signed in

if (XboxLiveUser.User.Privileges.Contains("247"))
{
    Debug.Log("User has the user_created_content privilege");
}
```

## <a name="privilege-codes"></a>权限代码
下面是可能返回的权限代码的列表。

| 代码  | 权限  | 描述   |
|------ |-----------------------------  |-------------------    |
| 190   | 广播             | 可以广播实时的玩游戏过程。     |
| 197   | view_friends_list     | 可以查看其他用户的好友列表。   |
| 198   | game_dvr              | 可将已录制的游戏内视频上载到云中。      |
| 199   | share_kinect_content          | 可为用户将 Kinect 录制的内容上载到云中，并使其可供所有用户访问。 |
| 203   | multiplayer_parties           | 能加入群会话。     |
| 205   | communication_voice_ingame    | 可在多方和多人游戏会话期间参与语音聊天。    |
| 206   | communication_voice_skype     | 可以通过 Xbox One 上的 Skype 实现语音通信。   |
| 207   | cloud_gaming_manage_session   | 可为托管的游戏会话分配和管理云计算群集。    |
| 208   | cloud_gaming_join_session     | 可以加入云计算会话。     |
| 209   | cloud_saved_games     | 可在云游戏存储中保存游戏。    |
| 211   | share_content     | 可与他人共享内容。    |
| 214   | premium_content   | 可以购买、下载和启动通过 Xbox Live 金会员订阅提供的付费内容。     |
| 219   | subscription_content  | 可以购买并下载付费订阅内容和使用付费订阅功能。     |
| 220   | social_network_sharing    | 可在社交网络上共享进度信息。    |
| 224   | premium_video     | 可以访问付费视频服务。    |
| 235   | purchase_content  | 可以购买内容。     |
| 247   | user_created_content  | 可以下载和查看联机用户创建的内容。    |
| 249   | profile_viewing   | 可以查看其他用户的个人资料。   |
| 252   | 通信    | 可将异步短信用于任何人。    |
| 254   | multiplayer_sessions  | 可以加入游戏的多人会话。   |
| 255   | add_friend    | 可以关注其他 Xbox Live 用户，并添加 Xbox Live 好友。   |
 

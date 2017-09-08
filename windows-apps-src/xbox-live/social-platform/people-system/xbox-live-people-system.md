---
title: "Xbox Live 人脉系统"
author: KevinAsgari
description: "了解 Xbox Live 人脉系统。"
ms.assetid: f1881a52-8e65-4364-9937-d2b8b8476cbf
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 社交, 人脉系统, 好友"
ms.openlocfilehash: bddafe769bd00d311df0a54aac0bdc13638f1c3f
ms.sourcegitcommit: 90fbdc0e25e0dff40c571d6687143dd7e16ab8a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2017
---
# <a name="xbox-live-people-system"></a>Xbox Live 人脉系统

为了 Xbox One 启动，Xbox Live 从之前的 Xbox 360 时代的_好友_系统迁移到了新的_人脉_系统，该系统在 Xbox Live 中重新构建了社交结构。 人脉系统的目标包括：

- 让用户可以轻松地识别出他们认识的人。

  真实姓名可用于所有体验，并成为非匿名关系交互的基础。

- 现有的 Xbox Live 关系迁移到 Xbox One。

  已在 Xbox 360 中添加了好友的用户在使用现代体验时不需要重置他们的列表。

- 继续支持维护匿名玩家代号关系。

  通过由比赛服务进行的匹配开展了一次满意的游戏会话后，用户可以将此人添加到他或她的人脉列表中，以方便以后联系。

- 提供管理长人脉列表的简单方法。

  用户可以将某人标记为常用，以便 Xbox One 体验首先显示这些联系人。

- 改进 Xbox Live 隐私系统，支持用户为受信任的现实世界关系赋予更多权利，为匿名（仅玩家代号）关系赋予更少权利。
- 确保向后兼容性。

  旧客户端和旧服务代码继续针对以前的社交基础结构运行，无需移动到新系统。

## <a name="identity-and-display-of-real-names-on-xbox-one"></a>Xbox One 上真实姓名的身份和显示
Xbox One 上的用户通过 Microsoft 帐户身份进行身份验证。 用户使用他或她的 Microsoft 帐户登录到主机，关联的用户名称和图片在整个 shell 显示。 如果用户未将他或她的 Xbox Live 个人资料链接到提供真实姓名的外部社交网络，该用户在系统中将由他或她的玩家代号和玩家图片表示（之前的 Xbox Live 玩家代号关系迁移到 Xbox One）。

系统将提示链接到外部网络，不过链接是可选的。 如果用户已将他或她的个人资料链接到外部社交网络，当该网络上的其他好友有 Xbox Live 帐户，并且也链接了他们的个人资料时，他们将在主机上使用自己的 Microsoft 帐户名称和图片，被呈现给他们在外部网络中与之共享了连接的好友。 但是，通过这种方式连接的用户将继续使用他们的玩家代号和玩家图片向外部网络中未连接的人呈现。

在此部分后面的主题将详细介绍这些关系。 有关相关 API 的信息，请参阅 **Microsoft.Xbox.Services.Social 命名空间**。

## <a name="in-this-section"></a>本部分内容
[单向关系](one-way-relationships.md)  
介绍 Xbox Live 人脉系统中的单向关系模型。

[人脉系统中的玩家代号和真实姓名](gamertags-and-real-names.md)  
Xbox Live 人脉系统的目标是允许向用户认识的人显示真实姓名；始终显示在新 Xbox Live 体验的任何位置，不论哪个环境...甚至在 FPS 在线游戏的地图上四处跑时，显示在角色的头上。

[常用联系人](favorite-people.md)  
Xbox Live 人脉系统中的收藏夹。

[显示来自人脉系统的联系人](displaying-people-from-the-people-system.md)  
如何显示从 Xbox Live 人脉系统返回的联系人。

[Xbox 360 好友向后兼容性](xbox-360-friends-backward-compatibility.md)  
如何在新的 Xbox Live 人脉系统中使用 Xbox 360 好友。

[隐私和我认识的联系人](privacy-and-people-i-know.md)  
在人脉系统中保护隐私。

[信誉](reputation.md)  
介绍信誉系统如何用于聚合玩家的用户报告，以允许 Xbox 服务提供高质量的游戏体验，并奖励正面行为。

[从游戏中发送玩家反馈](sending-player-feedback-from-your-title.md)  
介绍你的游戏如何帮助提供会影响用户信誉的反馈。

[对 Xbox Live 社交服务进行编程](programming-social-services.md)  
演示如何使用 Xbox Live 提供的社交服务。

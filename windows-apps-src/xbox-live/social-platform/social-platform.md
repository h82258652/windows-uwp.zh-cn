---
title: Xbox Live 社交平台 - 针对游戏和玩家
author: aablackm
description: 提供了解 Xbox Live 社交平台服务的链接。
ms.assetid: 27b85218-60f3-4eb0-9f7e-fe90e027db5c
ms.author: aablackm
ms.date: 09/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2bc8856d4ea03adc39edaa2066f0dd3224f293c1
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2018
ms.locfileid: "5166054"
---
# <a name="xbox-live-social-platform---for-games-and-gamers"></a>Xbox Live 社交平台 - 针对游戏和玩家

对于采用你的游戏并继续参与的玩家，与他人玩游戏和比赛对于他们来说至关重要。 Xbox Live 提供了最好的游戏社交网络，拥有超过 5000 万活跃玩家，而且人数仍在增长。 我们创建了一套工具，将玩家聚集在一起，让玩家注意到你新创建的精彩游戏。 在你的游戏中集成 Xbox Live 社交平台非常容易，而且投资回报巨大，不论你是构建单人休闲游戏、伴侣应用，还是大型多人游戏。

## <a name="concepts-in-this-article"></a>本文中的概念
- [个人资料](#profile)
- [玩家分数和成就](#gamerscore-and-achievements)
- [人脉系统 - 你的 Xbox Live 社交网络](#the-people-system---your-social-network-on-xbox-live)
- [俱乐部](#clubs)
- [剪辑和屏幕截图](#clips-and-screenshots)
- [活动源](#the-activity-feed)
- [新潮](#trending)
- [完整状态](#rich-presence)
- [游戏中心](#game-hubs)
- [使用 Xbox Live API 社交 API](#use-the-xbox-live-social-apis)

## <a name="bringing-gamers-together"></a>将玩家汇聚起来
Xbox Live 致力于建立活跃的玩家社区。 因此我们提供了一组工具，帮助玩家找到其他玩家来分享经验，让游戏成为一种社交活动。 这样，在玩家玩你的游戏时，他们就一定会分享给他们的朋友。 

### <a name="profile"></a>个人资料
Xbox Live 上的每个玩家都有个人资料。 个人资料包含以下信息：

-   **玩家代号**：玩家的唯一昵称。 Xbox Live 上的每个用户都有玩家代号。

-   **真实姓名**：玩家的名字和姓氏。 如果用户不希望通过隐私设置共享真实姓名，则此项为空。

-   **玩家图片**：玩家选择来代表自己的图片或图标。

-   **状态**：玩家的当前状态（联机、脱机、玩游戏等）。

-   **玩家分数**：表示玩家玩过的所有 Xbox Live 游戏累积的总成就分数的单一值。

### <a name="gamerscore-and-achievements"></a>玩家分数和成就
每个玩家可以通过解锁游戏中的[成就](../achievements-2017/achievements.md)获得玩家分数。
成就是一项非常受欢迎的功能，能让玩家在炫耀自己的实力的同时，扩展游戏目标，让玩家参与到你的游戏之中。 玩家可将所取得的成就与个人资料中的好友进行比较。

> [!NOTE]
> 玩家分数和成就对于在 Xbox Live 创意者计划下创建的游戏不可用。

### <a name="the-people-system---your-social-network-on-xbox-live"></a>人脉系统 - 你的 Xbox Live 社交网络
每个 Xbox Live 成员可以保留个人好友列表，其中可能包含现实世界中的好友、在 Xbox Live 遇到的其他玩家，以及知名玩家和 VIP，如 Major Nelson。 

用户之间的关系是单向*关注者*关系，这就意味着你只能通过关注将某人添加到你的好友列表中。 这与其他系统相反，在其他系统中要关注的人需先确认该关系，之后你才能成为其系统中的好友。 你最多可在好友列表中添加 1000 位好友，而且可以有无限个关注者：

-   *好友*是你通过关注添加到好友列表中的人。

-   *关注者*是通过关注你将你添加到他们的好友列表中的人。

为了能够在好友列表中轻松找到常用玩家，每个玩家最多可以将 100 位好友标记为常用。

当玩家玩游戏时，Xbox Live 社交图片将仅显示同样玩过你的游戏的玩家好友。

[详细了解人脉系统。](people-system/xbox-live-people-system.md) 

### <a name="clubs"></a>俱乐部
玩家可根据共同的兴趣构建各种规模的社交组。 这些组称为俱乐部。
俱乐部可以是一小群好友，也可以是拥有上万玩家的大型社区，他们热爱社交，热爱一起玩游戏。
俱乐部也可能涵盖各种兴趣，不仅仅是游戏。 一些著名的俱乐部有：

- 出色屏幕截图俱乐部
- 复古怀旧游戏
- 至高米姆
- UFC 俱乐部

这只是 Xbox Live 玩家创建的众多俱乐部中的一部分。

在俱乐部中，你游戏的粉丝可以与其他人共享游戏的剪辑和屏幕截图、相互提问、一起聊天，并可以通过“查找组 (LFG)”查找玩你游戏的其他玩家。 加入俱乐部的玩家的参与度将提高 40%，并可以结交更多好友。

> [!NOTE]
> 俱乐部专门由玩家管理，自然就会出现在粉丝群中，无需开发人员参与。 但创建者在其中发言也无妨。 

## <a name="getting-eyes-on-games"></a>关注到游戏
Xbox 社交系统重要的部分就是活动分享。 这能让你 Xbox Live 上所有的朋友都知道你的动向，增加你的游戏在游戏玩家中的曝光度。 以下功能旨在帮助 Xbox 用户发现你的游戏，让他们在成为游戏的又一忠实粉丝后持续参与到游戏之中。 更好的是，玩家每次玩游戏时，会吸引他们所有的朋友加入你的游戏。 

### <a name="clips-and-screenshots"></a>剪辑和屏幕截图
剪辑和屏幕截图是玩家分享游戏内容最简单的方法。 玩家可记录玩游戏时的视频和截图，与 Xbox Live 上的朋友分享。 这些荣耀时刻会自动显示在玩家的活动源中。

### <a name="the-activity-feed"></a>活动源
玩家玩你的游戏时，会解锁、剪辑、截图成就，同时与好友共享的活动源中还会包含其他活动，这样他们的好友就能看到你游戏中的精彩之处。

### <a name="trending"></a>新潮
在 Xbox Live 中发布的最受欢迎的内容显示在 Xbox Live 的“热门”部分。 如果玩家在你的游戏中心发布了一个有趣的问题或共享了你的游戏的出色剪辑，你可以期待这些内容在 Xbox Live 上传播开来。 这是另一个提高你的游戏知名度的好方法。

### <a name="rich-presence"></a>完整状态
状态，即在线状态，这不仅仅是用于检查玩家是否在线， 还会显示玩家目前正在使用或参与什么应用或游戏。 此外，通过“完整状态”字符串，你还能了解玩家在游戏中的动向，无论他们是在浏览菜单、排队等待游戏，还是在进行游戏。 

[详细了解完整状态](rich-presence-strings/rich-presence-strings-overview.md)

> [!NOTE]
> 完整状态对于在 Xbox Live 创意者计划下创建的游戏不可用。

### <a name="game-hubs"></a>游戏中心
通过与 Xbox Live 集成，你的游戏将自动获得称为“游戏中心”的中心页面。 游戏中心是你的游戏在 Xbox Live 上的官方目标位置，可让玩家查找游戏相关活动、成就、组、俱乐部、锦标赛，等等。

你可以视情况为你的游戏中心设置社区管理员，以通过活动源与用户交互。 玩你游戏的任何玩家都会被自动订阅到你的游戏中心活动源，这向你提供了接触玩家的强大机制。 通常，游戏中心活动源中的帖子的参与度会比外界热门社交媒体平台上的类似帖子高出 10 倍。

##  <a name="use-the-xbox-live-social-apis"></a>使用 Xbox Live API 社交 API
若要让玩家和他们的朋友通过 Xbox Live 社交 API 参与到你的游戏中，你需要使用 `SocialManager` 类。  在[社交管理器简介](intro-to-social-manager.md)中开始了解使用方法。
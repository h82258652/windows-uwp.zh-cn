---
title: Achievements
author: KevinAsgari
description: Achievements
ms.assetid: 35e055c2-3c84-4d73-bb86-fc776327d901
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, games, uwp, windows 10, xbox one
ms.openlocfilehash: 3e1e91bd6bdb4740af71b403a4db9fe931cab319
ms.sourcegitcommit: b73a57142b9847b09ebb00e81396f2655bbc26ec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2017
---
# <a name="achievements"></a>Achievements

In 2005, Xbox LIVE introduced the gaming industry to the notion of an achievement, a system-wide mechanism for directing and rewarding users' in-game actions in a consistent manner across all games.

With Xbox One, the achievement system expands to engage users across a broader range of activities, devices, and scenarios. Achievements are now more inclusive, more social, and more engaging than ever before! 开发人员和发布者可以创建新的成就类型以及玩家分数范畴之外的奖励。 Gamerscore is still a key reward, however, so developers can now add more achievements and gamerscore over the lifetime of the title, even without a new content release. Xbox One 上的成就经过全面重新设计，能够让游戏实现如服务般的灵活性。

## <a name="feature-summary"></a>Feature Summary ##
This list provides a summary of the key features and components that contribute to the achievement system on Xbox One.

Feature | Description
--- | ---
Persistent Achievements (achievements) | 此类成就存在于游戏的整个生命周期中。 Users may unlock them at any time. A persistent achievement’s required activity must always be available for users to complete and its reward must always be earnable.
Challenge Achievements (challenges) | This type of achievement is only available for a limited time. The publisher determines the dates during which a challenge achievement is available to unlock, and users must unlock it within that predefined timeframe to receive the recognition and its reward.
启动后成就 | Add achievements after title launch with no additional code required.
Achievement Progression | Now users can see how far along they are toward unlocking the achievement, even from the dashboard, giving them more reasons to fire up your title.
解锁切实奖励 | 除了玩家分数（是 Xbox 游戏体验的重要部分）外，Xbox One 用户还可以解锁与游戏相关的特殊奖励，如数字插图、新地图、可解锁的字符和通过 Xbox Live 成就提供的临时状态宝物。
成就活动源 | 用户可以轻松发现好友中近期最为热门的成就以及可赢取的奖励。
一个玩家分数 | 用户在传统平台或现代平台上赢取的任何玩家分数均将计入单个玩家分数中。

## <a name="making-achievements-work-well-with-the-guide-ui"></a>实现成就与指南 UI 的完美配合 ##
Xbox One 主机上的指南 UI 突出显示用户的实时成就活动。 若要充分利用这一内置功能，你的游戏应该及时更新用户解锁成就的进度。 借助游戏的成就进度跟踪特性，用户能够更加详细并实用地了解距离解锁每个游戏成就的当前进度。 此外，指南 UI 还会按进度百分比自动为游戏成就排序，让用户可以更轻松地找出有可能快速突破下一个游戏会话的成就。

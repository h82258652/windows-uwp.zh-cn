---
title: Get started with Xbox Live as a partner
author: KevinAsgari
description: Provides links to help a managed partner or ID@Xbox member get started with Xbox Live development.
ms.assetid: 69ab75d1-c0e7-4bad-bf8f-5df5cfce13d5
ms.author: kevinasg
ms.date: 06-07-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, games, uwp, windows 10, xbox one, partner, ID@Xbox
ms.openlocfilehash: c39f0dfd07fd3e5d932c11391e58194268b64e6e
ms.sourcegitcommit: 2aa04e1ef925a6c2025b9ec2edc1d784d02eb94e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-xbox-live-as-a-managed-partner-or-an-idxbox-developer"></a>以托管合作伙伴或 ID@Xbox 开发人员的身份开始使用 Xbox Live

This section covers getting started with Xbox Live as a managed partner or as an ID@Xbox developer. Partners and ID developers can access the full range of Xbox Live features, including achievements, multiplayer, and more.

Managed partners and ID@Xbox developers can develop Xbox Live titles for both the Universal Windows Platform (UWP) or the Xbox Developer Kit (XDK) platform.

In addition to the content available here, there is also additional documentation that is available to partners with an authorized Dev Center account. You can access that content here: [Xbox Live partner content](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/partner-content).

## <a name="why-should-you-use-xbox-live"></a>Why should you use Xbox Live?

Xbox Live offers an array of features designed to help promote your game and keep gamers engaged:

- Xbox Live social platform lets gamers connect with friends and talk about your game.
- Xbox Live achievements helps your game get popular by giving your game free promotion to the Xbox Live social graph when gamers unlock achievements.
- Xbox Live leaderboards helps drive engagement of your game by letting gamers compete to beat their friends and move up the ranks.
- Xbox Live multiplayer lets gamers play with their friends or a get matched with other players to compete or cooperate in your game.
- Xbox Live connected storage offers free save game roaming between devices so gamers can easily continue game progress between Xbox One and Windows PC.

## <a name="1-choose-a-platform"></a>1. 选择平台
在制作 Xbox 开发工具包 (XDK)、通用 Windows 平台 (UWP) 或跨平台游戏之间作出选择：

- 基于 XDK 的游戏只能在 Xbox One 主机上运行
- UWP games can target any Windows platform such as Windows PC, Windows Phone, or Xbox One
  - For Xbox One, see [UWP on Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index) and specifically [System resources for UWP apps and games on Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation)
- 跨平台游戏通常是使用 XDK 和 UWP 路径的面向 Xbox One 和 Windows 电脑的游戏。

## <a name="2-ensure-that-you-have-a-title-created-on-dev-center-or-xdp"></a>2. 确保你已在开发人员中心或 XDP 上创建了一个主题作品
Every Xbox Live title must be defined on Dev Center or the Xbox Developer Portal (XDP) before you will be able to sign-in and make Xbox Live Service calls.  [创建新的主题作品](create-a-new-title.md)将为你介绍如何执行此操作。

## <a name="3-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>3. 遵循相应指南设置 IDE 或游戏引擎
You can follow the appropriate Getting Started guide for your platform and engine and learn the basics of Xbox Live as you go along.

* [Getting started using Visual Studio for UWP games](get-started-with-visual-studio-and-uwp.md) will show you how to link your Visual Studio project with your Xbox Live configuration on Dev Center.
* [Getting started using Unity for UWP games](partner-add-xbox-live-to-unity-uwp.md) will show you how to create a new Xbox Live enabled Unity title, add features such as Leaderboards to your title, and generate a native Visual Studio project.
* [Getting started using Visual Studio for XDK based games](xdk-developers.md) will show you how to get your Visual Studio project setup if you are making an Xbox One title using the XDK.
* [Getting started making a cross-play game](get-started-with-cross-play-games.md) explains how to make a product that is an XDK based game for Xbox One and a UWP based game for Windows 10 PC.

## <a name="4-xbox-live-concepts"></a>4. Xbox Live 概念
创建主题作品后，你应会了解一些将影响开发主题作品体验的 Xbox Live 概念：

- [Xbox Live 沙盒](../xbox-live-sandboxes.md)
- [Xbox Live test accounts](../xbox-live-test-accounts.md)
- [Introduction to Xbox Live APIs](../introduction-to-xbox-live-apis.md)

## <a name="5-add-xbox-live-features"></a>5. Add Xbox Live Features

Add Xbox Live features to your game:

- [Xbox Live Social Platform - Profile, Friends, Presence](../social-platform/social-platform.md)
- [Xbox Live Data Platform - Stats, Leaderboards, Achievements](../data-platform/data-platform.md)
- [Xbox Live Multiplayer Platform - Invite, Matchmaking, Tournaments](../multiplayer/multiplayer-intro.md)
- [Xbox Live Storage Platform - Connected Storage, Title Storage](../storage-platform/storage-platform.md)
- [Contextual Search](../contextual-search/introduction-to-contextual-search.md)

## <a name="6-release-your-title"></a>6. 发布主题作品

ID@Xbox 和托管合作伙伴在发布其主题作品前必须先通过认证过程。  这是因为这些主题作品有权限访问联机多人游戏、成就和完整状态等其他功能。  在准备好发布你的主题作品后，请联系 Microsoft 代表以了解有关提交和发布过程的更多详细信息。

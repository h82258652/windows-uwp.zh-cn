---
title: 多人游戏附录
author: KevinAsgari
description: 提供了可以了解更多 Xbox Live 多人游戏 2015 服务信息的链接。
ms.assetid: 412ae5f4-6975-4a8b-9cc2-9747e093ec4d
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a85df1f0d5e58a89edffe8afe192563eecbfb60b
ms.sourcegitcommit: 5dda01da4702cbc49c799c750efe0e430b699502
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "4110974"
---
# <a name="multiplayer-appendix"></a>多人游戏附录

Xbox One 中的多人游戏系统支持游戏执行，并支持将游戏玩家组成组。 此系统安全、易用、灵活，让你不仅可以快速构建简单功能，还可以构建更加复杂的功能并插入你自己的服务。

> **注意**  
本文面向高级 API 的使用。  作为起点，请看一下[多人游戏管理器 API](../multiplayer-manager.md)，其大大简化了开发。  如果你在多人游戏管理器中发现不受支持的场景，请告知你的 DAM。

多人游戏系统的当前版本是 2015 多人游戏。 此版本提炼了“游戏派对”概念，使用多人游戏会话目录 (MPSD) 来控制游戏会话。

> **注意**  
多人游戏系统的前一个版本是 2014 多人游戏。 此版本基于“游戏派对”以及通过派对参与游戏的概念。 虽然此版本的源代码仍通过 XDK 提供，但它现在已被弃用。 XDK 中不再包括 2014 多人游戏文档。 如果你需要此文档，请使用 XDK 的 2014 版本。


## <a name="in-this-section"></a>本部分内容

[简介](introduction-to-the-multiplayer-system.md)  
介绍多人游戏系统。

[多人游戏会话目录 (mpsd)](multiplayer-session-directory.md)  
介绍多人游戏会话目录 (MPSD)，它将所有游戏的多人游戏 API 元数据集中起来，并管理游戏会话。

[MPSD 会话详细信息](mpsd-session-details.md)  
提供 MPSD 会话的详细信息。

[为 2015 多人游戏调整游戏时的常见问题](common-issues-when-adapting-multiplayer.md)  
介绍在调整游戏以运行 2015 多人游戏时需要考虑的常见问题。

[SmartMatch 匹配](smartmatch-matchmaking.md)  
介绍 Xbox Live 使用的匹配系统。

[迁移仲裁程序](migrating-an-arbiter.md)  
介绍仲裁程序迁移的 MPSD 流程。

[使用 SmartMatch 匹配](using-smartmatch-matchmaking.md)  
介绍如何使用 SmartMatch 匹配。

[多人游戏操作指南](multiplayer-how-tos.md)  
提供在游戏中使用 2015 多人游戏的过程。

[多人游戏会话状态代码](multiplayer-session-status-codes.md)  
定义 Xbox One 的多人游戏会话状态代码。

[2015 多人游戏常见问题和疑难解答](multiplayer-2015-faq.md)  
定义多人游戏的常见问题和疑难解答。

[Xbox One 多人游戏会话目录](xbox-one-multiplayer-session-directory.md) 提供使用新的 Xbox One 多人游戏会话目录 (MPSD) 服务创建多人游戏会话的概述。

[多人游戏的邀请流程](flows-for-multiplayer-game-invites.md) 介绍邀请其他玩家加入多人游戏所涉及的体验的流程。

[游戏会话和游戏派对可见性与可加入性](game-session-and-game-party-visibility-and-joinability.md) 介绍与多人游戏会话有关的可见性与可加入性之间的区别。

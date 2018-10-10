---
title: 多人游戏服务配置
author: KevinAsgari
description: 了解如何配置 Xbox Live 多人游戏服务。
ms.assetid: d042d4d5-1c75-4257-8a6f-07eddd39ca7e
ms.author: kevinasg
ms.date: 07/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 多人游戏, 服务配置, 会话模板, 自定义邀请字符串, smartmatch hopper
ms.localizationpriority: medium
ms.openlocfilehash: fd4032152e2c4a110fcffd8e6a7a46dba25d8271
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2018
ms.locfileid: "4503431"
---
# <a name="multiplayer-service-configuration"></a>多人游戏服务配置
为了让你的作品充分利用 Xbox Live 提供的服务，你必须先定义服务配置。 此服务配置位于 Xbox Live 云服务中，可定义 Xbox Live 服务与任何运行你的作品/游戏的设备的交互方式。

对于多人游戏服务，你可以配置多人游戏的三个方面：
* 会话模板
* SmartMatch Hopper
* 自定义邀请字符串

## <a name="session-templates"></a>会话模板
使用 Xbox 多人游戏服务，玩家可以创建和加入会话，与同一会话中的其他玩家交换会话消息，并将他们的游戏结果发布到该会话。 （发布结果时会清除会话，还会更新会话中的所有玩家的排行榜。）

例如，多人游戏会话可以是两个玩家之间进行的单个象棋游戏。 也可能是大量玩家玩的动作和冒险类作品的连续会话。

创建新会话时，游戏会根据预定义的会话模板创建该会话。 此模板本质上是包含描述会话的属性的 JSON 对象。

创建新会话模板时，必须定义以下各项：

| 字段 | 描述 |
| --- | --- |
| 会话名称 | 输入一个名称，该名称既描述了多人游戏会话模板的特征，又便于你记忆和识别。 该名称必须是文本字符串，最多包含 100 个字符。 |
| 合约版本 | 此值由系统自动填充，表示 JSON 合约的当前系统版本。 不要编辑该值。 |
| 会话模板（JSON 文本） | 指定 JSON 数据，以描述与多人游戏会话关联的不同属性。 |

有关多人游戏会话模板的更多信息，包括可用作 JSON 文本的基础的若干预定义模板，请参阅[多人游戏会话模板](session-templates.md)。

> **重要提示：** 在作品通过最终认证后，该作品中现有的多人游戏会话将再也不能更改或删除。

## <a name="smartmatch-hoppers"></a>SmartMatch Hopper

除了 Xbox 多人游戏服务外，还有基于 Xbox 服务器的可选匹配服务，后者可用于根据作品提供的信息、用户统计数据中存储的信息、用户的首选项或服务质量对玩家进行分组。

由于 Xbox One 匹配基于服务器，因此，用户可以提供对该服务的请求，然后收到通知（如果找到匹配项）。 也就是说，在匹配过程中，用户不用被迫在你的作品中等待，他们可以随意地玩你的作品中的单人游戏，甚至可玩其他作品，并且仍是匹配的候选人。 这样，在找到匹配项之前，不需要达到玩家的“临界规模”。

匹配 hopper 必须基于以前定义的会话模板。

创建新的匹配 hopper 时，你必须定义以下各项：

| 字段 | 描述 |
|---|---|
|名称| 输入一个名称，该名称既描述了匹配 hopper 的特征，又便于你记忆和识别。 该名称必须是文本字符串，最多包含 140 个字符。 |
| 最小组大小 | 指定可接受的最小玩家数。 最小值为 1。 |
| 最大组大小 | 指定可接受的最大玩家数。 最大值为 256。 |
| 应控制扩展周期 | 默认值为 3。 不需要更改默认值即可执行常规玩家填充。 |
| 排名 hopper | 如果 hopper 标记为排名 hopper，则允许对该 hopper 中的玩家进行互配，即使他们阻止对方。 这有助于防止玩家通过阻止对方试图避开技能更高的玩家。 |
| 从会话自动更新 | 如果启用此字段，则对会话的成员列表或成员的自定义属性所做的更改，将自动传播到以前提交的票证。 |

> **重要提示：** 在作品通过最终认证后，该作品中现有的匹配 hopper 将再也不能更改或删除。

## <a name="custom-invite-strings"></a>自定义邀请字符串
当你的作品向玩家发送加入多人游戏的邀请时，你可以选择显示自定义邀请文本字符串，而非默认邀请字符串。

创建新的自定义邀请字符串时，你必须定义以下各项：

| 字段 | 说明 |
|---|---|
| ID | 要用于标识字符串的自定义邀请字符串的 ID。 “custominvitestrings_”将自动追加到你的 ID 的开头。 最多包含 100 个字符 |
| 值 | 将显示在自定义邀请 toast 中的自定义邀请字符串文本。 最多包含 100 个字符 |

## <a name="additional-information"></a>其他信息

有关如何配置多人游戏服务的详细信息，请参阅以下文章：

**文章** | **描述**
--- | ---
[为多人游戏配置 AppXManifest](configure-your-appxmanifest-for-multiplayer.md) | 介绍如何配置 UWP AppXManifest 文件，以使用 Xbox Live 多人游戏服务。
[多人游戏会话模板](session-templates.md) | 简要概述了多人游戏会话模板，并提供了可复制和修改的多人游戏会话模板的几个示例。
[会话模板常量](session-template-constants.md) | 介绍多人游戏会话模板的预定义元素。
[大型会话](large-sessions.md) | 介绍使用大型会话的时间和方式。

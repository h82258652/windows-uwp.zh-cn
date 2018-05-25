---
title: 游戏聊天概述
author: KevinAsgari
description: 了解如何使用 Xbox Live 游戏聊天将语音通信添加到游戏中。
ms.assetid: 8ef6a578-e911-4006-ac4e-94d3f2fedb98
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: low
ms.openlocfilehash: aa926ce695a538d5019ff5571cada093ab262c70
ms.sourcegitcommit: 01760b73fa8cdb423a9aa1f63e72e70647d8f6ab
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/24/2018
---
# <a name="game-chat-overview"></a>游戏聊天概述

 游戏聊天是一种技术，你可以使用这种技术在远程主机上启用单个主题作品的用户之间的语音通信。 它不包括主机用户之间更广泛的独立通信。

 > [!Note]
 > 新游戏应使用[游戏聊天 2](game-chat-2-overview.md) 而不是“游戏聊天”。 原来的“游戏聊天”将于 2017 年年底前弃用。

 Xbox One 拥有专门的硬件加速语音聊天编码解码器，可用于编码和解码，并通过 `Microsoft.Xbox.GameChat` 命名空间公开。 此编码解码器支持主题作品网络带宽灵活性的多种质量设置，且由主机传输的所有语音聊天通信都需要使用此编码解码器。 不支持其他编码解码器。

> **注意：** 一个名为 `Windows.Xbox.Chat` 的较低级别命名空间会调用此编码解码器。

  * [游戏聊天频道](#ID4EFB)
  * [游戏聊天类](#ID4EKC)
  * [默认 USB 终结点](#ID4E3D)
  * [音量和音调](#ID4EDE)
  * [在游戏聊天过程中使用户听不到](#ID4EWE)
  * [编译 GameChat DLL](#ID4E5H)

<a id="ID4EFB"></a>

## <a name="game-chat-channels"></a>游戏聊天频道


 聊天系统关联用户（而非设备）中的本地输入。 操作系统关联输入设备与玩家 ID。 主要功能之一是支持多个频道。 游戏可能有大厅，用户可以加入团队，且团队中可能有命令结构。 如果游戏允许，用户可以在同一时间在多个频道中活动。 例如，游戏可能有以下频道：

  * 红色团队 1 小队频道 &ndash; 打开 1 小队上所有红色团队之间的通信。
  * 红色团队 2 小队频道 &ndash; 打开 2 小队上所有红色团队之间的通信。

**图 1.**&nbsp;&nbsp;**标准聊天频道。**

![](../../images/chat/gamechat_standard.png)

  *  红色团队命令频道 &ndash; 针对所有红色团队非命令玩家的仅收听频道。 此频道可以闪避其他所有“红色团队”频道。 针对启用了“按下即可发言”的红色团队指挥官的仅发言频道。

**图 2.**&nbsp;&nbsp;**命令聊天频道。**

![](../../images/chat/gamechat_command.png)

  * 蓝色团队频道如上所述。



> **注意：***闪避*是一个聊天术语，意思是，出现其他信号时，音频信号的级别下降。 例如，在收音机中，当演示者发布公告时，音乐曲目的音量下降（闪避）。



 在用户间的标准聊天中，频道是双向的，以便用户可以在关联的频道上同时进行收听和发言。 使用命令频道可以将通信设置为双向（如标准频道）或单向，用户可以在单向频道中发言或收听，但不能同时进行。 通常，例如，指挥官可能想要发言，那么下属只能收听。 命令频道被视为具有高于其他频道的优先级，并会导致其他频道的语音进行闪避。

 远程用户发言时，游戏主题作品可以闪避非聊天音频。 还能设置每个频道的属性（例如音量），为部分用户设置“仅收听”属性，为其他用户设置“仅发言”属性。


<a id="ID4EKC"></a>


## <a name="game-chat-classes"></a>游戏聊天类


 游戏聊天的命名空间为 `Microsoft.Xbox.GameChat`。 本部分介绍了命名空间的类层次结构。 请参考 `Microsoft.Xbox.GameChat` 命名空间文档，获取每个类、接口和枚举的完整说明。

  * [ChatManager](#ID4EYC)
  * [ChatManagerSettings](#ID4EDD)
  * [ChatUser](#ID4EQD)

<a id="ID4EYC"></a>


### <a name="chatmanager"></a>ChatManager


`ChatManager` 类是顶级的游戏聊天类。


<a id="ID4EDD"></a>


### <a name="chatmanagersettings"></a>ChatManagerSettings


 `ChatManagerSettings` 类表示 `ChatManager` 类的设置。


<a id="ID4EQD"></a>


### <a name="chatuser"></a>ChatUser


`ChatUser` 类表示频道上的用户，具有一组特定于此用户与相同频道上的其他用户进行的交互的属性。



<a id="ID4E3D"></a>


## <a name="default-usb-endpoint"></a>默认 USB 终结点


 默认 USB 捕获终结点是单个频道的单声道。 这能够将回声抵消应用于音频流，它是一个有用的聊天应用程序功能。


<a id="ID4EDE"></a>


## <a name="volume-and-pitch"></a>音量和音调


聊天系统中的音量控制与用于聊天 API 基于的 XAudio2 的音量控制相同。 聊天系统不支持音调控制。

 有关 XAudio2 音量设置的详细信息，请参阅 `SetVolume`。

 XAudio2 会基于用户的扬声器设置自动调整音量级别，使配置中的音量级别维持一致。 如果用户的设置与其物理配置不匹配，那么与具有准确设置的系统相比，音量会太大或太小。 例如，为仅连接了两个扬声器的 5.1 环绕声扬声器进行配置的系统，其声音会太小。 XAudio2 无法检测用户扬声器设置是否与其物理设置正确匹配。


<a id="ID4EWE"></a>


## <a name="making-users-inaudible-during-game-chat"></a>在游戏聊天过程中使用户听不到


Xbox One 支持两种在游戏聊天过程中以无声方式呈现用户的机制：
  * 静音
  * 阻止


<a id="ID4EFF"></a>


### <a name="muting"></a>静音


静音通常是单向的。 也就是说，玩家 A 将玩家 B 静音，但玩家 B 仍可以听到玩家 A 的声音。静音示例：
  * 进行“临近”聊天的主题作品。 在这种情况下，玩家可以听到附近玩家的声音，但与其距离较远的玩家会被静音。
  * 主题作品可为玩家用户提供将任意/所有其他玩家静音的功能。


是否实现静音主要由主题作品自主决定。 但是，玩家还可以使用玩家卡片或系统 UI 将 Xbox 系统级别上的其他玩家静音。


<a id="ID4EVF"></a>


### <a name="blocking"></a>阻止


> **注意：** 主题作品不会阻止玩家。 阻止仅在 Xbox 系统级别上执行。



游戏玩家可以通过使用阻止直接影响聊天的可听度。 玩家可以通过玩家卡片直接阻止其他玩家，也可以将语音通信和文本限制设置为“阻止”或“仅好友”。 通常情况下，阻止是双向的。 也就是说，如果玩家 A 阻止了玩家 B，不仅玩家 A 听不到玩家 B，玩家 B 也听不到玩家 A。阻止示例包括：
  * 玩家有一个子帐户，并阻止其他所有玩家访问此帐户。
  * 玩家将帐户配置设置为仅与好友聊天，并阻止非好友。
  * 玩家 A 通过使用“人脉”应用中的信誉/反馈或系统 UI 标记玩家 B 的不适当的行为来阻止玩家 B。


玩家还可以通过以物理方式移除耳机或禁用 Kinect 传感器来阻止其他玩家。


<a id="ID4EJG"></a>


### <a name="what-titles-should-do-to-provide-for-mutingblocking"></a>主题作品应执行哪些操作才能实现静音/阻止


主题作品可使用 GameChat DLL 通过实现的游戏内选项提供静音，如 InGameChat 示例中所示。 库满足静音/阻止的所有要求，而且主题作品未要求执行任何特定操作来遵守系统策略。 主题作品必须仅确保它在聊天会话期间公开允许玩家静音/阻止其他玩家的玩家卡片 UI。

 要实现静音，主题作品可调用 `ChatManager.MuteUserFromAllChannels`。 如果要将所有用户静音，可调用 `MuteAllUsersFromAllChannels`。 主题作品可根据需要，使用对 `ChatManager.UnmuteUserFromAllChannels` 或 `UnmuteAllUsersFromAllChannels` 的调用还原已静音用户的可听度。 不建议主题作品直接使用 `Chat` 命名空间。 此库的大部分功能通过 `GameChat` 公开。

当玩家清除聊天频道并创建新的聊天会话时，会话不会保留有关上一个已静音玩家的信息。 主题作品必须跟踪此行为并调用 `ChatManager`，以便在游戏期间使玩家的静音体验保持一致。

许多主题作品会减少通过网络发送的数据，以限制阻止/静音情况下的语音流量，从而优化网络操作。 例如，玩家 A 阻止/静音玩家 B 以限制语音流量，但主题作品不会停止从玩家 A 到玩家 B 的语音数据传输。是否实施此行为完全取决于主题作品。 `GameChat`  处理阻止玩家 B 的语音数据的情况（如果传递到了 `ChatManager` 中）。


<a id="ID4ERH"></a>


### <a name="debugging-when-players-cannot-hear-each-other"></a>当玩家互相听不到时进行调试

`ChatManager`  使用 `ChatManagerSettings.DiagnosticsTraceLevel` 属性和 `ChatManager.OnDebugMessage` 事件包括主题作品可以启用的调试跟踪。 此跟踪会提供很多能够帮助缩小问题范围的内容。 此内容来源于添加/更改音频设备、添加本地和远程用户、获取每个远程用户的语音数据等。


<a id="ID4E5H"></a>


## <a name="compiling-the-gamechat-dll"></a>编译 GameChat DLL


 用于构建 Microsoft.Xbox.GameChat.dll 的源可在 Xbox 开发人员门户上找到。 开发人员可编译此源以创建 `GameChat` 的自定义版本。 典型自定义是创建用于传输音频数据的自定义网络频道。

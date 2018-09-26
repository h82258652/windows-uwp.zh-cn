---
title: 多人游戏系统简介
author: KevinAsgari
description: 提供对 Xbox Live 多人游戏 2015 系统的高级介绍。
ms.assetid: d025bd2b-2ca4-4ba9-9394-4950d96ad264
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 多人游戏 2015
ms.localizationpriority: medium
ms.openlocfilehash: e43470a19338f4a523c89bec6dd45e2672dfd283
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2018
ms.locfileid: "4209636"
---
# <a name="introduction-to-the-multiplayer-system"></a>多人游戏系统简介

| 注意                                                                                                                                                                                                          |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 本文面向高级 API 的使用。  作为起点，请看一下[多人游戏管理器 API](../multiplayer-manager.md)，其大大简化了开发。  如果你在多人游戏管理器中发现了不受支持的场景，请告知 DAM。 |

本文档包含以下部分
* 关于多人游戏系统
* 多人游戏组件、接口和体系结构
* 2015 多人游戏支持的派对
* 多人游戏术语
* 2015 多人游戏的新增功能
* Xbox 360 与 Xbox One MPSD 会话函数之间的差异

## <a name="about-the-multiplayer-system"></a>关于多人游戏系统


2015 多人游戏优化 MPSD 和游戏会话的直接使用。 它为单个作品或用户的多个并发会话提供更好的支持，并更新 Xbox 服务 API (XSAPI) 以支持作品：

-   播发用户的当前活动和可加入性。
-   向会话发送邀请，以及用户可见（作品指定）的上下文字符串。
-   通过作品代码发现并加入会话。
-   维护与 MPSD 的 Web 套接字连接，以便它们可以收到有关会话更改的简要通知（即时点击），例如，反映更改事件和连接状态更改订阅的更新。 MPSD 还使用 Web 套接字连接来快速检测客户端断开连接并对其执行操作。
-   使用 SmartMatch 匹配。

## <a name="multiplayer-components-interfaces-and-architectures"></a>多人游戏组件、接口和体系结构

### <a name="components-of-2015-multiplayer"></a>2015 多人游戏的组件

多人游戏是一个系统，由多个组件组成。 它足够灵活，可以容纳其他组件，如专用服务器和外部匹配系统。
#### <a name="multiplayer-session-directory-mpsd"></a>多人游戏会话目录 (MPSD)


多人游戏会话目录 (MPSD) 是一项包含一组会话的服务。 会话被定义为驻留在云中并表示一组玩游戏的人的安全文档。 有关 MPSD 的详细信息，请参阅[多人游戏会话目录 (MPSD)](multiplayer-session-directory.md)。


#### <a name="multiplayer-apis"></a>多人游戏 API

2015 多人游戏提供通过 **Microsoft.Xbox.Services.Multiplayer 命名空间**实现的多人游戏 WinRT API。 其组件包括 **MultiplayerService 类**，其定义包装 MPSD 的 Web 服务。

另外还包含多人游戏 REST API。 它定义 WinRT API 方法调用的 URI 和 JSON 对象。 REST 功能可以用于从你的作品发出的直接调用，但我们鼓励你通过 WinRT API 间接访问 MPSD。 有关详细信息，请参阅**调用 MPSD**。

#### <a name="xbox-party-system"></a>Xbox 派对系统

在 2015 多人游戏中，Xbox 派对系统仅作为外部实体来支持聊天派对。 作品可以与该派对系统交互来发现聊天派对的会员身份。 有关详细信息，请参阅 **2015 多人游戏支持的派对**。

派对系统现在直接通过 MPSD 会话，而不是使用游戏派对来支持游戏。 由游戏决定是否使用会话来支持此类操作，如成员交互、在有空间时将玩家拉入游戏、用户在等待期间参与。


### <a name="2015-multiplayer-interfaces"></a>2015 多人游戏接口

2015 多人游戏使用多个其他 Xbox 组件的接口。
#### <a name="xbox-secure-sockets"></a>Xbox 安全套接字

2015 多人游戏使用 Xbox 安全套接字和 Winsock 来支持设备之间的低级别网络通信。 网络功能使用 Internet 协议安全性 (IPSec) 来允许作品提供安全的设备关联。 请参阅**网络概述**了解 Xbox 安全套接字功能的详细信息。


#### <a name="xbox-live-real-time-activity-service"></a>Xbox Live 实时活动服务

2015 多人游戏使用**实时活动服务**来允许游戏订阅 MPSD 会话的更改，并支持自动检测客户端的断开连接情况。 [实时活动 (RTA) 服务](../../real-time-activity-service/real-time-activity-service.md)中提供了详细信息。


#### <a name="xbox-live-matchmaking-service"></a>Xbox Live 匹配服务

**匹配服务**根据玩家的首选项和数据以及在 SmartMatch 匹配期间提供的信息来建立玩家组。 有关多人游戏使用该服务的详细信息，请参阅 [SmartMatch 匹配](smartmatch-matchmaking.md)。


#### <a name="xbox-live-reputation-service"></a>Xbox Live 信誉服务

*信誉服务*管理有关信誉和信誉筛选的匹配期间的用户统计信息。 2015 多人游戏通过 SmartMatch 匹配来使用此服务。 有关详细信息，请参阅[信誉](../../social-platform/people-system/reputation.md)。


#### <a name="xbox-live-compute"></a>Xbox Live 计算

Xbox Live 计算为使用 2015 多人游戏的游戏提供云处理能力。 有关使用 Xbox Live 计算的详细信息，请参阅[在多人游戏中使用 Xbox Live 计算](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-live-compute/using-xbox-live-compute-in-multiplayer)。


### <a name="typical-2015-multiplayer-architectures"></a>典型的 2015 多人游戏体系结构

此部分提供最典型的 2015 多人游戏体系结构。
#### <a name="peer-to-peer-architecture"></a>对等体系结构

在对等体系结构中，作品使用 MPSD 和 SmartMatch 匹配来发现对等地址。 地址然后用于使用 Xbox 安全套接字来连接对等方。 有关详细信息，请参阅 *Xbox One 上的 Winsock 简介*。

![对等体系结构图示](../../images/multiplayer/Mult2015ArchPeer.png)


#### <a name="client-server-architecture"></a>客户端-服务器体系结构

在客户端-服务器多人游戏体系结构中，游戏使用 MPSD 和 SmartMatch 匹配来发现专用的服务器地址。 这些地址然后用于使用 Xbox 安全套接字来连接到专用服务器。 有关详细信息，请参阅 *Xbox One 上的 Winsock 简介*。

| 注意                                                                         |
|-------------------------------------------------------------------------------------------|
| Xbox Live 计算实例可以用作客户端-服务器体系结构中的服务器。 |

![客户端-服务器体系结构图示](../../images/multiplayer/Mult2015ArchClientServer.png)


## <a name="parties-supported-by-2015-multiplayer"></a>2015 多人游戏支持的派对
Xbox One 的 2015 多人游戏不将“游戏派对”作为系统级构造显示。 但是，它与 2014 多人游戏一样，在系统级别支持“聊天派对”。

| 注意                                                                                                                    |
|--------------------------------------------------------------------------------------------------------------------------------------|
| 你的作品仍可以使用游戏派对实现与已实现的体验类似的用户体验，但改为使用 MPSD 会话。 |


### <a name="chat-party"></a>聊天派对

聊天派对是一组正在彼此聊天的人，它由用户通过 Xbox One 派对应用进行管理。 用户在游戏会话中玩游戏或进行另一项主机活动时，可同时参加聊天派对。 但是，在聊天派对中的用户与这些用户的其他活动之间没有联系。

聊天派对使用 *PartyChat 类*显示，这允许作品检查聊天派对的状态。 例如，作品可以使用 *PartyChat.GetPartyChatViewAsync 方法*枚举聊天派对的成员。


### <a name="implementing-features-similar-to-those-related-to-the-game-party"></a>实现与游戏派对相关功能类似的功能

在 2014 多人游戏中，游戏派对服务于多个目的。 它允许作品：

-   播发用户的当前活动和可加入性
-   向会话发送邀请
-   发现并加入会话
-   接收对注册到该派对的会话进行某些更改的通知
-   使用 SmartMatch 匹配
-   跨多个游戏会话将一组人聚集在一起

2015 多人游戏直接通过 MPSD 会话支持上述所有功能。

## <a name="multiplayer-terminology"></a>多人游戏术语


| 术语                                 | 说明|
| --- | --- |
| 活动玩家                        | 已设置为在会话内处于“活动”状态的玩家。 当玩家参与游戏时，游戏将玩家设置为此状态。 有关详细信息，请参阅[会话用户状态](mpsd-session-details.md)。                                                                                                                                                                                                                                                                                          |
| 仲裁程序                              | 为游戏管理多人游戏会话目录 (MPSD) 状态的以查找更多玩家的游戏会话中的单个主机，例如，在向匹配播发游戏会话的会话中。 仲裁程序由作品设置。 仲裁程序并不总是游戏的主机。 请参阅[会话仲裁程序](mpsd-session-details.md)。                                                                                                                                                                            |
| 排列游戏                        | 仅通过邀请其他玩家加入的一个玩家创建的一类游戏，不涉及任何匹配。                                                                                                                                                                                                                                                                                                                                                                                                    |
| 聊天派对                           | 正在一起聊天的一组人。 这些人可能参与同一个活动，也可能参与不同的活动，如游戏、音乐或应用。 请参阅 **2015 多人游戏支持的派对**。                                                                                                                                                                                                                                                                                 |
| 游戏邀请                          | 加入游戏会话的邀请。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 游戏会话                         | 用户实际上在一起玩游戏的会话。 所有多人游戏场景（例如，匹配或加入大厅）最终会在游戏会话中结束。 会话通常作为用户的当前活动播发以支持加入。 它还用来构建最近的玩家列表。 请参阅 [MPSD 会话详细信息](mpsd-session-details.md)。                                                                                                                                                           |
| 游戏会话主机                    | 为在基于主机的对等网络体系结构上生成的游戏运行游戏模拟的主机。 此主机通常会与仲裁程序相同，但没有必要一定相同。                                                                                                                                                                                                                                                                                                                            |
| 句柄（或会话句柄）           | 具有其他状态以及与之关联的行为的 MPSD 会话的引用。 请参阅[MPSD 会话句柄](multiplayer-session-directory.md)。                                                                                                                                                                                                                                                                                                                                                                    |
| 非活动玩家                      | 已设置为在会话内处于“非活动”状态的玩家。 在游戏被挂起或非活动时（由游戏定义），游戏将玩家设置为此状态。 在某些情况下，MPSD 也可能将玩家设置为非活动，但这主要由游戏负责。 有关详细信息，请参阅[会话用户状态](mpsd-session-details.md)。                                                                                                                           |
| Hopper                               | Hopper 是一组逻辑驱动的匹配票证。 作品可以有多个 Hopper，但仅同一个 Hopper 内的票证可以匹配。 例如，游戏可以创建一个玩家技能是最重要的匹配项目的 Hopper。 它可以使用另一个 Hopper，在该 Hopper 中，只有玩家购买了相同的可下载内容才会匹配。 有关 Hopper 在何种情况下适合 SmartMatch 工作流的详细信息，请参阅 [SmartMatch 运行时操作](smartmatch-matchmaking.md) |
| 进行中加入                     | 在游戏开始后加入另一个玩家游戏的概念。 玩家可以通过好友的玩家卡片加入好友的游戏。 游戏随后可以在适当时将这些玩家移入该游戏会话。                                                                                                                                                                                                                                                                                                      |
| 大厅会话                        | 正在等待加入游戏会话的受邀请玩家的帮助程序会话。 请参阅 [MPSD 会话详细信息](mpsd-session-details.md)。                                                                                                                                                                                                                                                                                                                                                                                       |
| 匹配目标会话                 | SmartMatch 匹配期间设置的以表示匹配的匹配会话。 请参阅 [SmartMatch 匹配](smartmatch-matchmaking.md)。                                                                                                                                                                                                                                                                                                                                                                                              |
| 匹配票证会话                 | SmartMatch 匹配期间设置的初步匹配会话。 请参阅 [SmartMatch 匹配](smartmatch-matchmaking.md)。                                                                                                                                                                                                                                                                                                                                                                                                         |
| MPSD 会话                         | 驻留在 Xbox Live 云内的多人游戏会话目录 (MPSD) 中的安全文档。 它包含可能在 Xbox One 上运行游戏时被连接的一组用户，以及有关这些用户及其游戏的元数据。 请参阅 [MPSD 会话详细信息](mpsd-session-details.md)。                                                                                                                                                                                                                  |
| 多人游戏会话目录 (MPSD) | 在多人游戏系统用来存储和检索会话的云中运行的服务。 请参阅[多人游戏会话目录 (MPSD)](multiplayer-session-directory.md)。                                                                                                                                                                                                                                                                                                                                                                |
| “派对”应用                            | 允许用户查看和管理派对的 Xbox One 系统贴靠应用。                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 服务器会话                       | 由 Xbox Live 计算处理创建的游戏会话。 请参阅 [MPSD 会话详细信息](mpsd-session-details.md)。                                                                                                                                                                                                                                                                                                                                                                                                            |
| 即时点击                         | 从 MPSD 向可能在服务中发生有趣变化的作品发送的通知。 即时点击是一种快速提醒，信息量通常少于常规通知。 请参阅 [MPSD 更改通知处理和断开连接检测](multiplayer-session-directory.md)。                                                                                                                                                                                                                     |
| SmartMatch 匹配               | 可用于 Xbox One 作品的 Xbox Live 匹配功能，由匹配服务实现。 使用 MPSD 和匹配，作品发出要匹配的请求，并在稍后找到匹配的组时接收通知。 请参阅 [SmartMatch 匹配](smartmatch-matchmaking.md)。                                                                                                                                                                                                                                  |

## <a name="whats-new-in-2015-multiplayer"></a>2015 多人游戏的新增功能

| 注意                                                                                                                                                                                                                                               |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 在使用 2015 多人游戏时，请务必注意，不应再使用 2014 多人游戏中与派对相关的类。 将 2015 多人游戏功能与派对相关的类混合将导致不一致的行为，因此永远都不应该尝试。 |


### <a name="new-concepts-in-2015-multiplayer"></a>2015 多人游戏中的新概念


#### <a name="web-socket-connections-to-mpsd"></a>与 MPSD 的 Web 套接字连接

MPSD 现在支持作品维护与它之间的 Web 套接字连接。 这些连接允许客户端在会话更改时接收通知。 有关详细信息，请参阅 [MPSD 更改通知处理和断开连接检测](multiplayer-session-directory.md)。


#### <a name="mpsd-session-handles"></a>MPSD 会话句柄

2015 多人游戏增加了对 MPSD 会话句柄的支持，这是对可以包含类型化数据的会话的引用。 有关详细信息，请参阅[MPSD 会话句柄](multiplayer-session-directory.md)。


### <a name="summary-of-new-2015-multiplayer-winrt-api-functionality"></a>新 2015 多人游戏 WinRT API 功能摘要

新的多人游戏 WinRT API 功能基于现有 XSAPI，目标是帮助已将 2014 多人游戏与游戏派对 API 结合使用的游戏轻松过渡。

2015 多人游戏增加了 *MultiplayerActivityDetails 类*，其表示用户当前活动的详细信息，例如，用户可加入其中游戏的会话。

新功能已添加到 *MultiplayerService 类*。 示例是处理用户和社交组的活动、使用各个筛选器和句柄检索会话、发送游戏邀请以及使用句柄读写会话的方法和属性。

*MultiplayerSession 类*添加了使用会话更改类型、通知订阅、会话比较和将会话设置为关闭的功能。

*MultiplayerSessionReference 类*被更改为从 **IMultiplayerSessionReference** 继承，以支持跨命名空间的调用。 此类还具有新的 URI 路径分析方法。

| 注意                                                                                                                                                                                                                                                                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 为了支持事件和通知订阅，2015 多人游戏向 *Microsoft.Xbox.Services.RealTimeActivity 命名空间*添加了功能。 新功能还包含 *SystemUI.ShowSendGameInvitesAsync 方法*，用于显示 2015 多人游戏的游戏邀请。 |

## <a name="differences-between-xbox-360-and-xbox-one-mpsd-session-functions"></a>Xbox 360 与 Xbox One MPSD 会话函数之间的差异

| 函数 | Xbox 360 | Xbox One |
|---|---|---|
| **获取游戏会话信息** | XSessionGetDetails、XSessionSearchByID 或作品执行跟踪。 | 作品从 MPSD 请求会话信息。 |
|**迁移主机** | 在需要时，作品调用 XSessionMigrateHost。 | 根据迁移的原因，作品可以为会话分配新主机，或者可以创建新的 MPSD 会话。 |
| **多个玩家会话** | 一次处理多个会话比较复杂，例如，XNetReplaceKey 与 XNetUnregisterKey。 | 基于服务的会话让管理一个会话更加简洁，并简化了多个会话的处理。 |
| **注销和断开连接** | 作品必须使用 XCloseHandle 或 XSessionDelete 以不同方式处理断开连接和注销。 | MPSD 简化了注销和断开连接的处理，以及游戏配置中的超时设置。 |
| **匹配** | 基于客户端的匹配查询 | 允许在作品内获得更好的匹配质量和更轻松的背景匹配的基于服务的匹配。 |


### <a name="sessions"></a>会话

在 Xbox 360 中，会话表示游戏的实例。 用户在匹配服务中搜索会话，并在会话结束时报告统计信息。

在 Xbox One 上，会话更为通用，表示一组玩家。 任何两个主机之间的网络连接都需要会话，会话保留应在会话中的所有用户之间共享的信息。 此信息的一些示例包括：会话中允许的玩家数量、会话中每个主机的安全地址、自定义游戏数据。


### <a name="xbox-matchmaking"></a>Xbox 匹配

在 Xbox 360 中，作品通过配置属性架构和一组用于搜索这些属性的查询来执行匹配。 在运行时，作品选择托管会话或搜索会话。

在 Xbox One 上，匹配基于服务器，玩家和游戏不再决定是托管还是搜索。 相反，每个预建立的玩家组创建一个“票证”会话，并将该会话提交到匹配服务。 服务然后查找其他会话，并合并这些组来建立新的“目标”会话。 客户端接收匹配通知，并执行服务质量 (QoS) 来在游戏开始之前验证与其他会话成员的连接。


### <a name="xbox-live-compute-service"></a>Xbox Live 计算服务

Xbox Live 计算服务是 Xbox One 的一项新功能。 它支持开发人员利用云的弹性计算能力，并支持比对等网络中可能的多人游戏场景更大的场景。 有关 Xbox Live 计算服务的详细信息，请参阅 XDK 文档中的 Xbox Live 计算文档。


## <a name="see-also"></a>另请参阅

[多人游戏会话目录 (MPSD)](multiplayer-session-directory.md)

[SmartMatch 匹配](smartmatch-matchmaking.md)

[实时活动 (RTA) 服务](../../real-time-activity-service/real-time-activity-service.md)

[信誉](../../social-platform/people-system/reputation.md)

[在多人游戏中使用 Xbox Live 计算（需要托管合作伙伴的访问权限）](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-live-compute/using-xbox-live-compute-in-multiplayer)

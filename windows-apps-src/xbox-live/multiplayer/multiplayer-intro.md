---
title: Xbox Live 多人游戏平台
author: KevinAsgari
description: 了解 Xbox Live 支持的多人游戏平台。
ms.assetid: 958b94b3-dccd-479a-bf52-54f7ff1656fa
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 多人游戏
ms.localizationpriority: medium
ms.openlocfilehash: 27b91376bfdb58bd474f7d291fa9f650455461c0
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2018
ms.locfileid: "4212240"
---
# <a name="xbox-live-multiplayer-platform"></a>Xbox Live 多人游戏平台

Xbox Live 多人游戏平台授权你的游戏通过 Internet 将 Xbox Live 玩家汇集在一起，还可以极大地延长除典型单人游戏之外的游戏的有效时间和使用。

通过为观众构建出色的多人游戏体验，你可以利用 Xbox Live 玩家的大型社交网络为游戏增加用户群，促进形成一个可持续的供游戏迷们一起游戏的社区。


## <a name="what-is-the-xbox-live-multiplayer-platform"></a>什么是 Xbox Live 多人游戏平台？

Xbox Live 多人游戏平台是一组你可以用来实现实时多人游戏的客户端 API。 API 套件中的主要子系统是：

-   **Xbox Live 多人游戏会话目录 (MPSD)** 服务。 MPSD 服务通过集成的 UI 体验帮助用户在玩游戏时互相进行查找和邀请。 与 Xbox Live 服务集成还可以允许客户使用 Xbox Live 群聊天集合。
-   **简单且高级的匹配工具。** Xbox Live 不仅提供传统的 quickmatch 功能，还提供会话浏览功能并支持高度自定义的匹配场景。 Xbox Live 查找组 (LFG) 还允许玩家相互查找，在群聊天中集合，然后玩游戏。
-   **对等和客户端服务器网络 API** 提供安全的实时通信，该通信采用现代的 Internet 标准并且被 Xbox Live 主动监控。 标准化和集成化 Xbox Live 网络疑难解答体验可以使用户快速更正连接性问题。  
-   **集成的语音和文字聊天解决方案**促进了游戏内的安全通信，该通信利用了 Xbox Live 社交图表、媒体服务和 Xbox One 设备上的专用编码硬件。

有关一些最常见的多人游戏场景的概述和可帮助实现这些场景的 Xbox Live 功能，请参阅[多人游戏级别](multiplayer-scenarios.md)。

## <a name="how-can-i-implement-xbox-live-multiplayer-in-my-game"></a>如何在我的游戏中实现 Xbox Live 多人游戏？
取决于你的场景，Xbox Live 多人游戏平台提供了多种可在你的游戏中实现 Xbox Live 多人游戏的方法。

### <a name="xbox-integrated-multiplayer-xim"></a>Xbox 集成的多人游戏 (XIM)
XIM 是通过 Xbox Live 服务的功能向你的游戏添加实时多人游戏网络和通信的一站式解决方案。 XIM 的目标是使在 Xbox One 和 Windows 10 上生成高质量对等 (P2P) 多人游戏变得比以往更容易。

XIM 提供以下功能：
- 支持游戏邀请和简单匹配。
- 通过 Xbox Live 多人游戏中继服务以透明方式增强简单且安全的实时网络。
- 简单的语音和文字聊天，配备用于转录和解说通信的工具以提供更加方便的终端用户体验。
- 用于检测和管理网络拥挤以及迁移游戏状态的助手。

XIM 不仅是可通过 Xbox Live 多人游戏平台使用的最简单的多人游戏解决方案，还是可自定义程度最低的，仅适用于 P2P 游戏。 有关 XIM 的更多信息，请参阅 [Xbox 集成的多人游戏](xbox-integrated-multiplayer.md)。

### <a name="xbox-multiplayer-manager"></a>Xbox 多人游戏管理器
Xbox 多人游戏管理器 (MPM) 是提供 Xbox Live 多人游戏会话目录、邀请和匹配服务的灵活访问权限的客户端 API。

它以遵循最佳实践的有效方式实现多个常见的多人游戏场景，还处理你的游戏必须实现以通过认证的多个 Xbox 要求 (XR)。

Xbox 多人游戏管理器并不实现网络或聊天层。 MPM 经过专门设计，是一款灵活而又简化的综合多人游戏管理 API，可通过 Windows.Networking.XboxLive 实现游戏与安全网络层的配对。 可使用[游戏聊天 2](chat/game-chat-2-overview.md) API 或通过 XIM 聊天保留添加游戏内通信。 网络和游戏聊天 2 API 会记录在 Xbox One XDK 和 Xbox Live Platform Extensions SDK 中。

如果你要为多人游戏托管专用服务器，MPM 是最佳选择。 MPM 也非常适合高级场景，如与 Xbox Live 锦标赛集成。 有关 MPM 的更多信息，请参阅[多人游戏管理器简介](multiplayer-manager/multiplayer-manager-api-overview.md)。

要使用多人游戏管理器，你必须为多人游戏场景配置 Xbox Live 服务。 有关此配置的更多信息，请参阅[配置多人游戏服务](service-configuration/configure-the-multiplayer-service.md)。

>多人游戏管理器当前不支持会话浏览。 相关信息请参阅[多人游戏会话浏览](session-browse.md)。

### <a name="api-capabilites"></a>API 功能

功能 | Xbox 集成的多人游戏| 多人游戏管理器
--  | -- | --
可见性 |  以没有源的已编译库形式提供 XIM。  | 提供有源的 MPM，因此你可以通过直接与 Xbox Live 服务或 XSAPI 交互来自定义行为。
会话和匹配 | XIM 提供简单的预配置匹配规则，并不要求多人游戏配置。 | MPM [要求配置多人游戏服务](service-configuration/configure-the-multiplayer-service.md)，实现了配对和会话行为的精密规范化。
网络 | XIM 在需要时提供 Xbox Live 中继服务支持的简单且安全的玩家对玩家网络。 | MPM 进行了设计，因此你可以使用 Windows.Networking.XboxLive 插入到你自己的安全网络解决方案。
游戏聊天 | XIM 提供集成的语音和文字聊天。 | 通过游戏聊天 2 API 或使用 XIM 带外保留实现游戏内通信，以为 MPM 管理的名单启用聊天。

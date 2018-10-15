---
title: 实时活动服务
author: KevinAsgari
description: 了解 Xbox Live 实时活动服务。
ms.assetid: 50de262f-fc55-4301-83b5-0a8a30bc7852
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 实时活动服务
ms.localizationpriority: medium
ms.openlocfilehash: 1969a36a363f86ba6eced1c27d2206de1160c7bc
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2018
ms.locfileid: "4613373"
---
# <a name="real-time-activity-service"></a>实时活动服务

使用实时活动 (RTA) 服务，任何设备上的应用程序均可订阅状态数据、用户统计数据和状态。 通过该系统，用户可以根据其隐私设置，订阅任何作品中的自身数据和其他用户的数据。 这就实现了信息的流动，而无需不断轮询以获取最新数据。


## <a name="developer-scenarios"></a>开发人员方案

RTA 支持很多方案。 这里只列出其中几个方案，但是，RTA 的强大之处其实在于，你提供的很多方案都超乎了我们的想象。 你可以帮助定义下一代游戏玩法，即用户通常可使用手头的 Microsoft Surface 或 Apple iPad 与你的主机作品进行交互。 RTA 采用了 WebSocket 技术，因此，各种子主题演练包括如何使用 Windows 提供的 WebSockets API 实现的概述。

下面是一些可以使用 RTA 为你的作品创建的简单方案：

-   成就进度应用
-   游戏帮助应用
-   Squad 查看器应用
-   统计数据查看器
-   状态查看器


## <a name="achievements-progress-app"></a>成就进度应用

用户几乎总想知道他们在取得某些成就方面的进展情况，尤其是需要执行一定次数的操作所取得的成就。 通过实时访问用户的统计数据（聚合在 Xbox Live 玩家统计数据服务中），你可以在 Xbox One 或配套设备上向你的作品玩家及其好友提供关于成就和里程碑的实时进展情况。


## <a name="game-help-app"></a>游戏帮助应用

当用户在你的作品中导航时，你可以通过实时访问数据在用户使用 Xbox One 或任何配套设备的同时提供游戏帮助。 用户可能会碰到新地图、新赛道或具有挑战性的 Boss 战，而你的 Game Help 助手可以显示用户生成的或开发人员生成的视频和文本，以帮助该用户在你的作品中获得完美体验。


## <a name="squad-viewer-app"></a>Squad 查看器应用

在多人合作游戏中，玩家及其队友可以为共同的目标携手合作。 由于玩家如此之多，因此可能很难跟踪更全面的情况。 通过实时访问数据，你可以创建伴侣应用，以显示操作可能所在位置的高级地图和热点图。


## <a name="statistics-viewer"></a>统计数据查看器

虽然考虑 RTA 时通常会想到伴侣应用，但是，你也可以在核心作品中使用 RTA。 例如，你可以使用 RTA 让多人游戏的玩家在游戏中显示每个人的当前统计数据，而玩家可能只需在多人游戏匹配中按主机上的“查看”按钮即可。


## <a name="presence-viewer"></a>状态查看器

如果位于大厅，则实时了解哪些好友在线以及他们是否在玩相同的作品非常有用。 你可以订阅用户的好友状态，并显示哪些好友即将上线（如果他们开始玩你的作品），所有这一切都是实时进行的。


## <a name="subscription-privacy-and-authorization"></a>订阅隐私和授权

最新版的 RTA 包括对隐私和授权/内容隔离的检查。 只要满足隐私和授权检查条件，你的应用就可以订阅标记为启用 RTA 的任何统计数据。 （有关如何将统计数据标记为启用 RTA 的更多信息，请参阅[注册获得统计数据通知](register-for-stat-notifications.md)。 可标记为启用 RTA 的统计数据的类型不受限制，这取决于你，即开发人员。 但是，用户可订阅每应用会话的统计数据的*数量*存在限制。 如果用户达到该限制，则在下次订阅时会收到错误消息。


## <a name="in-this-section"></a>本部分内容

[注册获得玩家统计数据更改通知](register-for-stat-notifications.md)  
介绍如何启用实时活动 (RTA) 以获得统计数据或状态信息。

[使用 winrt API 对实时活动 (RTA) 服务进行编程](programming-the-real-time-activity-service.md)  
介绍如何使用 WinRT API 对实时活动 (RTA) 服务进行编程。

[使用 RESTful 接口对实时活动 (RTA) 服务进行编程](programming-the-real-time-activity-service.md)  
介绍如何使用 RESTful 接口对实时活动 (RTA) 服务进行编程。

[实时活动 (RTA) 最佳实践](rta-best-practices.md)  
使用 Xbox 实时活动 (RTA) 服务从 Xbox 数据平台订阅统计数据和状态数据的最佳实践。

---
title: Xbox Live 的新增功能
author: PhillipLucas
description: Xbox Live SDK 的新增功能
ms.author: aablackm
ms.date: 10/23/2018
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 22c70f0dee1226057a69cb876dac4f4512b378b5
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5876734"
---
# <a name="whats-new-for-xbox-live"></a>Xbox Live 的新增功能
你也可以查看 [Xbox Live API GitHub 提交历史记录](https://github.com/Microsoft/xbox-live-api/commits/master)，了解 Xbox Live API 的所有最新的代码更改。

## <a name="in-this-article"></a>本文内容

* [2018 年 6 月](#june-2018)
* [2017 年 8 月](#august-2017)
* [2017 年 7 月](#july-2017)
* [2017 年 6 月](#june-2017)
* [2017 年 5 月](#may-2017)
* [2017 年 4 月](#april-2017)
* [2017 年 3 月](#march-2017)
* [已存档](#archived)

## <a name="june-2018"></a>2018 年 6 月

### <a name="xbox-live-features"></a>Xbox Live 功能

#### <a name="c-api-layer-for-xsapi"></a>Xsapi C API 图层

C Api 现已推出的 Xbox Live 的一些功能。 新的 API 层为受支持的功能，包括自定义内存管理、 异步任务的手动线程管理和新的 HTTP 库提供了很多优势。

有关详细信息，请参阅[Xbox Live C Api](../xsapi-flat-c.md)。

## <a name="august-2017"></a>2017 年 8 月

### <a name="xbox-live-features"></a>Xbox Live 功能

#### <a name="in-game-clubs"></a>游戏内俱乐部

开发人员现在可以创建“游戏内俱乐部”。 游戏内俱乐部与标准 Xbox 俱乐部不同，它们可由开发人员完全自定义，并在游戏内部和外部均可使用。 作为游戏开发人员，你可以使用它们来在游戏内快速构建任何类型的满足你的唯一要求的永久组场景，如团队、部落、小队、公会等。

Xbox live 成员可以通过在 Xbox 主机、电脑或 iOS/Android 设备上随意使用聊天、供给、LFG 和 Mixer 等俱乐部功能，跨任何 Xbox 体验在你的游戏之外访问游戏内俱乐部，从而保持相互联系或与你的游戏保持联系。

API 可用于直接从你的游戏内创建和管理游戏内俱乐部。 这些 API 位于 xbox::services::clubs 命名空间中。

## <a name="july-2017"></a>2017 年 7 月

### <a name="xbox-live-features"></a>Xbox Live 功能

#### <a name="tournaments"></a>锦标赛

添加了新 API 以支持锦标赛。 你现在可以使用 xbox::services::tournaments::tournament_service 类从你的游戏访问锦标赛服务。
这些新锦标赛 API 支持以下场景：

* 查询服务以查找当前标题的所有现有锦标赛。
* 从服务检索有关锦标赛的详细信息。
* 查询服务以检索锦标赛的团队列表。
* 从服务检索有关锦标赛团队的详细信息。
* 使用实时活动 (RTA) 订阅跟踪锦标赛和团队的变化。

## <a name="june-2017"></a>2017 年 6 月

### <a name="xbox-live-features"></a>Xbox Live 功能

#### <a name="game-chat-2"></a>游戏聊天 2

更新和改进的游戏聊天版本现已推出。 有关详细信息，请参阅[游戏聊天 2 概述](../multiplayer/chat/game-chat-2-overview.md)。

### <a name="xbox-live-tools"></a>Xbox Live 工具

#### <a name="xbox-live-powershell-module"></a>Xbox Live PowerShell 模块

* 添加了 PowerShell 模块，让在开发计算机上切换沙盒更加轻松。 有关详细信息，请参阅[工具](../tools/tools.md)。

#### <a name="bug-fixes"></a>Bug 修复

* 修复了多个 Bug。 请查看 [GitHub 提交历史记录](https://github.com/Microsoft/xbox-live-api/commits/master)获取完整列表。

## <a name="may-2017"></a>2017 年 5 月

### <a name="xbox-services-apis"></a>Xbox 服务 API

#### <a name="multiplayer"></a>多人游戏

* 查询搜索句柄现在在响应中包含的自定义会话属性。

#### <a name="bug-fixes"></a>Bug 修复

* 修复后返回“损坏的 json”，而不是有效的 HTTP 错误代码。

## <a name="april-2017"></a>2017 年 4 月

### <a name="xbox-services-apis"></a>Xbox 服务 API

#### <a name="visual-studio-2017"></a>Visual Studio 2017

对于通用 Windows 平台 (UWP) 和 Xbox One 游戏，Xbox Live API 均已更新为支持 Visual Studio 2017。

#### <a name="tournaments"></a>锦标赛

添加了新 API 以支持锦标赛。 你现在可以使用 `xbox::services::tournaments::tournament_service` 类从你的游戏访问锦标赛服务。

这些新锦标赛 API 支持以下场景：

* 查询服务以查找当前标题的所有现有锦标赛。
* 从服务检索有关锦标赛的详细信息。
* 查询服务以检索锦标赛的团队列表。
* 从服务检索有关锦标赛团队的详细信息。
* 使用实时活动 (RTA) 订阅跟踪锦标赛和团队的变化。

## <a name="march-2017"></a>2017 年 3 月

### <a name="xbox-services-api"></a>Xbox 服务 API

#### <a name="data-platform-2017"></a>数据平台 2017

我们引入了简化的统计 API。  传统上来讲，你必须发送与 XDP 或开发人员中心定义的统计规则对应的事件，并且这会更新云中的统计信息值。  我们将此模型称为 Stats 2013。

使用 Stats 2017，你的标题现在可以控制统计信息值。  你只需调用包含最新统计信息值的 API，它们即可被直接发送到服务，无需发送事件。  这使用新的 `StatsManager` API，你可以在[玩家统计信息](../leaderboards-and-stats-2017/player-stats.md)中了解更多信息

#### <a name="github"></a>GitHub

Xbox Live API (XSAPI) 现已在 GitHub 上提供，位置在 [https://github.com/Microsoft/xbox-live-api](https://github.com/Microsoft/xbox-live-api)。  仍建议使用 XDK 附带的二进制文件或将其用作 NuGet 程序包，不过，欢迎你使用源，我们乐于接收有关源代码的反馈。  

### <a name="xbox-live-creators-program"></a>Xbox Live 创意者计划

Xbox Live 创意者计划是一项开发人员计划，为更广大的开发人员受众提供一部分 Xbox Live 功能。  如果你已经参与了 ID@Xbox 计划，这不会对你产生任何影响。

你可以在[开发人员计划概述](../developer-program-overview.md)中了解有关此计划的详细信息。

### <a name="documentation"></a>文档

推出了以下新文章：

| 文章 | 描述 |
|---------|-------------|
|[Xbox Live 服务配置](../xbox-live-service-configuration.md) | 更新了为 Xbox Live 游戏进行服务配置的相关信息
| [配置 Unity 中的 Xbox Live](../get-started-with-creators/configure-xbox-live-in-unity.md) | 针对 Xbox Live 创意者计划开发人员的 Unity 设置的新信息 |
| [Xbox Live 沙盒](../xbox-live-sandboxes.md) | Xbox Live 沙盒和内容隔离的简化指南 |
| [Xbox Live 测试帐户](../xbox-live-test-accounts.md) | 有关测试帐户如何工作，以及如何在 Windows 开发人员中心创建帐户的信息 |

## <a name="archived"></a>已存档

* [2016 年 12 月](1612-whats-new.md)
* [2016 年 11 月](1611-whats-new.md)
* [2016 年 8 月](1608-whats-new.md)
* [2016 年 6 月](1606-whats-new.md)
* [2016 年 4 月](1603-whats-new.md)
* [2016 年 3 月](1603-whats-new.md)
* [2016 年 2 月](1602-whats-new.md)
* [2015 年 10 月](1510-whats-new.md)
* [2015 年 9 月](1509-whats-new.md)
* [2015 年 8 月](1508-whats-new.md)
* [2015 年 6 月](1506-whats-new.md)

---
title: 多人游戏会话目录
author: KevinAsgari
description: 了解 Xbox Live 多人游戏会话目录 (MPSD)。
ms.assetid: a9b029ea-4cc1-485a-8253-e1c74184f35e
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, mpsd, 多人游戏会话目录。
ms.localizationpriority: medium
ms.openlocfilehash: d69867e2ba5d56eb47007732ae7197c9991be4c4
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2018
ms.locfileid: "4052060"
---
# <a name="multiplayer-session-directory-mpsd"></a>多人游戏会话目录 (MPSD)

本文包含以下部分：

* MPSD 概述
* MPSD 更改通知处理和断开连接检测
* MPSD 会话句柄
* 会话更新的同步
* 调用 MPSD
* MPSD 会话模板
* 多人游戏会话资源管理器

## <a name="session-overview"></a>会话概述

### <a name="what-is-mpsd"></a>什么是 MPSD？

多人游戏会话目录 (MPSD) 是在 Xbox Live 云中运行的一项服务，它集中游戏跨多个客户端的多人游戏系统元数据。 它由 **MultiplayerService 类**包装。

MPSD 允许作品分享连接用户组所需的基本信息。 它确保会话功能同步并保持一致。 它通过玩家卡片在发送/接收邀请并加入方面协调 shell 和主机操作系统。


### <a name="mpsd-sessions"></a>MPSD 会话

MPSD 会话由 **MultiplayerSession 类**表示为有一个或多个联系人使用游戏的场景。 会话由 MPSD 存储为在 Xbox Live 云中驻留的安全 JSON 文档。 具体来说，MPSD 会话具有以下特点：由作品创建和管理。

-   具有唯一的 URI。 有关详细信息，请参阅**会话目录 URI**。
-   支持用户（称为会话成员）之间的连接。
-   存储对支持游戏执行有用的数据，例如，每个成员的属性、游戏设置、引导信息，以及游戏服务器信息。

MPSD 支持在设置多人游戏中使用多个会话差异。 每个会话包含玩家的 Xbox 用户标识符 (XUID) 和安全设备关联地址数据。

-   游戏会话，用作游戏执行的模式。 游戏会话可以是点对点、点对主机、点对服务器或这些类型的混合。
-   票证会话，用于跟踪匹配期间匹配状态的帮助程序会话。 它经常还是大厅会话，有时可能是游戏会话。 请参阅 [SmartMatch 匹配](smartmatch-matchmaking.md)。
-   目标会话，在匹配期间创建的用来代表匹配的游戏的帮助程序会话。 它几乎始终还是一个游戏会话。 请参阅 [SmartMatch 匹配](smartmatch-matchmaking.md)。
-   大厅会话，用来容纳等待加入游戏会话的受邀请玩家的帮助程序会话。 许多游戏既创建大厅会话也创建游戏会话。 有关详细信息，请参阅**在你的作品中管理玩家**。

## <a name="mpsd-change-notification-handling-and-disconnect-detection"></a>MPSD 更改通知处理和断开连接检测

MPSD 支持客户端使用实时活动服务 Web 套接字与其连接。 此连接用于：

-   当会话更改发生时，根据作品启动的事件订阅向下发送简要通知（即时点击）。
-   检测用户断开连接的情况。
-   根据断开连接检测将用户设置为非活动，并随后将其从会话中删除。


### <a name="making-user-connections"></a>建立用户连接

XSAPI 库管理客户端与 MPSD 之间的连接。 作品首先调用 **MultiplayerService.EnableMultiplayerSubscriptions 方法**。 此方法告诉 XSAPI 客户端打算为多人游戏使用实时活动连接。 然后，当游戏进行第一次 **MultiplayerService.WriteSessionAsync 方法**或 **MultiplayerService.WriteSessionByHandleAsync 方法**调用时（将当前用户设置为“活动”状态），将创建连接并将其与 MPSD 挂钩。

| 注意                                                                                                                         |
|-------------------------------------------------------------------------------------------------------------------------------------------|
| 若要启用会话通知并检测断开连接，会话模板必须将 connectionRequiredForActiveMembers 设置为 true。 |

| 注意                                                                                                                                                                                                                                                                                                                            |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 在以前的 XSAPI 版本中，作品调用 **RealTimeActivityService.ConnectAsync 方法**来创建与实时活动服务的用户连接。 对于 2015 多人游戏，不使用此方法，而是根据需要创建连接。 |

### <a name="subscribing-for-session-changes"></a>订阅会话更改

MPSD 使用“即时点击”作为感兴趣事物发生更改时发出的轻型通知。 作品应该检索经过修改的资源来确定更改的真正性质。 订阅启用后，游戏可以使用对 **MultiplayerSession.SetSessionChangeSubscription 方法**的调用订阅会话更改的“即时点击”。 请参阅[操作方法：订阅 MPSD 会话更改通知](multiplayer-how-tos.md)。


### <a name="handling-shoulder-taps"></a>处理“即时点击”

当会话更改与游戏对该会话的订阅匹配时，MPSD 将使用 **RealTimeActivityService.MultiplayerSessionChanged 事件**通知游戏所发生的更改。 作品随后应该检索会话，并将检索到的会话版本与之前缓存的视图进行比较，并相应地执行操作。


### <a name="handling-notifications-of-connection-state-changes"></a>处理连接状态更改通知

作品还可以接收与 MPSD 的连接的运行状况变化相关的通知。 两个事件表示这些更改：**RealTimeActivityService.MultiplayerSubscriptionsLost 事件** - 当游戏使用实时活动服务与 MPSD 的连接丢失时触发。 当发生此事件时，游戏应关闭多人游戏。
-   **RealTimeActivityService.ConnectionStateChanged 事件** - 当作品与实时活动服务的连接的运行状况发生临时变化时触发。 作品在收到此事件时无需执行任何操作，不过使用此事件来进行诊断可能很有用。


### <a name="disconnecting-clients"></a>断开客户端连接

当游戏通过调用 **RealTimeActivityService.DisableMultiplayerSubscriptions 方法**禁用通知时，你的作品的客户端将从 MPSD 断开。 执行此调用之后不久，**MultiplayerSubscriptionsLost** 事件触发，指示客户端已从 MPSD 断开。

| 注意                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 在以前的多人游戏版本中，游戏调用 **RealTimeActivityService.Disconnect 方法**来从实时活动服务中断开。 对于 2015 多人游戏，不使用此方法。 如果没有用户具有 Web 套接字连接（例如，实时活动服务的状态订阅），断开连接将在 **DisableMultiplayerSubscriptions** 被调用后自动发生。 |


### <a name="disconnect-detection"></a>断开连接检测

MPSD 使用断开连接检测功能快速发现用户的非正常断开。 当玩家的网络出现故障或者游戏崩溃时，可能发生非正常断开连接。 MPSD 将断开连接的玩家的状态从“活动”更改为“非活动”，并根据情况、基于成员的会话订阅，通知其他会话成员这一更改。

## <a name="mpsd-handles-to-sessions"></a>MPSD 会话句柄

MPSD 会话句柄是对还可以包含其他类型数据的会话的抽象且不可变的引用。 它类似于文件句柄。 所有句柄均有句柄 ID (GUID)，以及由服务配置 ID (SCID)、会话模板和会话名称组成的完整会话引用。 句柄无法更新，但可以创建、读取和删除。

| 注意                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| 句柄可以指向不存在的会话。 使用不存在的会话名称创建句柄不会导致新会话的创建。 |


### <a name="handle-types"></a>句柄类型

2015 多人游戏支持此部分介绍的句柄类型。

#### <a name="invite-handle"></a>邀请句柄

邀请句柄表示对某个人的邀请。 类型特定数据包括源人员、目标人员和描述邀请的上下文字符串，例如，特定游戏模式。

邀请句柄授予对开放会话的读写访问权限。 如果会话已关闭，句柄则授予会话的只读访问权限。

| 注意                                                |
|------------------------------------------------------------------|
| MPSD 可以创建邀请，即使会话已满或已关闭。 |


#### <a name="activity-handle"></a>活动句柄

活动句柄指示用户当时正在执行的操作。 用户活动由 **MultiplayerActivityDetails 类**表示。

| 注意                                                                                                                                                                                                                      |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 活动手柄还可以明确删除，但只能由特定用户所有的作品执行。 此删除使用 **MultiplayerService.ClearActivityAsync 方法**完成。 |


### <a name="creating-an-invite-handle"></a>创建邀请句柄

若要创建邀请句柄，游戏调用 **MultiplayerService.SendInvitesAsync 方法**。 此方法通过接收人可以对其执行操作来接受邀请的 UI toast 形式向指定用户发送邀请。


### <a name="creating-an-activity-handle"></a>创建活动句柄

若要创建活动句柄，游戏调用 **MultiplayerService.SetActivityAsync 方法**。 MPSD 作为会话成员的绑定活动设置新的句柄 ID。 如果存在之前的绑定活动，MPSD 将删除对应的句柄。 当活动成员进入非活动状态或离开会话时，MPSD 将删除绑定活动句柄。


### <a name="using-handles"></a>使用句柄

当用户接受邀请（邀请句柄）以及用户加入好友的当前活动（活动句柄）时，作品使用句柄。 在这两种情况下，作品均必须：

1.  从作品激活参数获取句柄 ID。
2.  创建本地 MPSD 会话对象，并以活动状态加入。
3.  写入会话，传入相应句柄。

## <a name="synchronization-of-session-updates"></a>会话更新的同步

由于会话是可以由任何成员创建或更新的共享资源，因此可能存在冲突写入。 这可能导致意外结果，例如，一个作品可能无意中覆盖了另一个作品所做的更改。 MPSD 解决这些冲突的方法是支持开放式并发和读取-修改-写入模式。

MPSD 执行的会话更新同步使用两个相关的高级实现模式：

-   仲裁程序更新共享的会话部分。 如果你的实现涉及单个仲裁程序，你可以避免为大多数写入操作使用同步更新。 作品可以避免以下同步：(1) 仲裁程序对共享的会话部分进行的任何更新，除非这些更新与传达仲裁程序的标识有关，以及 (2) 作品对会话内的成员区域进行的任何更新。

| 注意                                                                                                                                                                                                                                                                                                                                              |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 虽然以上更新类型无需同步，但对于将任何更新同步到 **MultiplayerSessionProperties.HostDeviceToken 属性**仍然很重要。 该属性用于传达仲裁程序的标识，例如，作为仲裁程序迁移的一部分。 |

-   所有客户端更新共享的会话部分。 在此情况下，必须同步共享的会话部分的所有更新。 但是，作品仍然可以在自己的成员区域写入，无需同步。


### <a name="session-update-synchronization-using-the-multiplayer-winrt-api"></a>使用多人游戏 WinRT API 的会话更新同步

下面的多人游戏 WinRT API 方法实现开放式并发：

-   **MultiplayerService.WriteSessionAsync 方法**
-   **MultiplayerService.WriteSessionByHandleAsync 方法**
-   **MultiplayerService.TryWriteSessionAsync 方法**
-   **MultiplayerService.TryWriteSessionByHandleAsync 方法**

每个写入方法均接受 **MultiplayerSessionWriteMode 枚举**值。 传递值 SynchronizedUpdate 将利用开放式并发来进行更新。

枚举中的其他值帮助解决初始创建会话后可能出现的冲突。 对 MPSD 会话的某部分（该会话有可能被另一个作品写入）的任何写入均必须使用同步更新。 但是，并非所有写入都必须受到保护。

如果你的作品尝试使用写入会话方法之一将本地会话对象写入 MPSD，并收到了 HTTP/412 状态代码，它应该通过发出 **MultiplayerService.GetCurrentSessionAsync 方法**调用来刷新本地副本，以在再次尝试写入之前获取会话的最新服务器版本。 否则，本地会话文档将继续包含错误数据，写入会话的调用将继续失败。

| 注意                                                                                                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 如果作品使用 **TryWrite\*** 方法之一，更新的会话在收到 HTTP/412 状态代码时返回。 此行为避免了对调用 **GetCurrentSessionAsync** 的需要。 |

当作品调用写入会话方法之一时，可能会返回更新的会话版本。 如果是这样，作品应该以线程安全的方式将本地缓存的副本切换为此新版本。


### <a name="session-update-synchronization-using-the-multiplayer-rest-api"></a>使用多人游戏 REST API 的会话更新同步

MPSD 通过 REST 功能在会话更新中支持开放式并发，方法是使用具有 ETag 设置和读取-修改-写入模式的 HTTP "if-match" 标头。 传入写入请求的 ETag 应该是 MPSD 通过之前的读取请求返回的那一个。

## <a name="calling-mpsd"></a>调用 MPSD

游戏可以通过两种方法 MPSD 以使用多人游戏系统和匹配：
-   推荐。 使用多人游戏 WinRT API，其包含充当 RESTful 功能包装器的类。 请参见 **Microsoft.Xbox.Services.Multiplayer 命名空间**。 对于 SmartMatch 匹配，请使用匹配 WinRT API，其由 **Microsoft.Xbox.Services.Matchmaking 命名空间**表示。
-   使用对多人游戏和匹配 REST API 的直接标准 HTTP 调用，其包含在 **Xbox Live 服务 RESTful 引用**中。 适用的 URI 在**会话目录 URI**（对于多人游戏）和**匹配 URI**（对于匹配）中有述。 相关的 JSON 对象在 **JavaScript 对象表示法 (JSON) 对象引用**中有述。


### <a name="using-the-multiplayer-winrt-api-to-call-mpsd"></a>使用多人游戏 WinRT API 调用 MPSD

建议的调用 MPSD 的方法是使用多人游戏 WinRT API 和匹配 WinRT API。

| 注意                                                                                             |
|---------------------------------------------------------------------------------------------------------------|
| XDK 示例使用多人游戏和匹配 WinRT API 以及其他 XSAPI 元素编写。 |

为基本 REST 功能使用包装器代码可以以更传统的方式来使用客户端 API 方法，而无需处理每个调用的 HTTP 流量。 XSAPI 的二进制文件和源文件均在 XDK 中附带。 你可以直接使用二进制文件，也可以根据需要修改源文件，并将其合并到作品中。


### <a name="using-the-multiplayer-rest-api-to-interact-with-mpsd"></a>使用多人游戏 REST API 与 MPSD 交互

游戏或其服务可以使用对多人游戏 REST API 和匹配 REST API 的标准 HTTP 调用。 在直接使用 REST 功能时，调用方针对大多数操作的会话目录 URI 发出 DELETE、PUT、POST 和 GET 调用。 在 PUT 请求中，请求正文合并到现有会话中。 如果没有现有会话，请求正文用于创建新会话，以及存储在[Xbox 开发人员门户 (XDP)](https://xdp.xboxlive.com)或[Windows 开发人员中心](https://developer.microsoft.com/dashboard/windows/overview)上的会话模板。 所有字段都是可选的，只有增量必须指定。 因此，{}是增量为零的有效 PUT 请求。

若要执行返回合并结果的假设 PUT 请求而不影响服务器的官方会话副本，可以将查询字符串“?nocommit=true”附加到 PUT 请求中。

多人游戏和匹配 REST API 方法的请求和响应是 JSON 文档。 有关多人游戏会话请求的结构，请参见 **MultiplayerSessionRequest (JSON)**。 关联的响应结构显示在 **MultiplayerSession (JSON)** 中。 响应结构将会话成员构建为链接的列表，并填充会话及其成员的其他只读属性。


### <a name="querying-for-sessions-and-session-templates-rest"></a>查询会话和会话模板 (REST)

你的作品可以在服务配置和会话模板级别查询会话信息。 本主题讨论使用多人游戏 REST API 的查询。


#### <a name="query-for-basic-session-information"></a>查询基本会话信息

你可以使用会话目录和匹配 URI 来设置基本会话信息的查询。 查询结果是一个会话引用的 JSON 阵列，其中内联包含了一些会话数据。 默认情况下，查询最多可检索 100 个非专用会话。

| 注意                                                          |
|----------------------------------------------------------------------------|
| 每个查询必须包括关键字筛选器、XUID 筛选器或两者都有。 |


#### <a name="query-for-session-templates"></a>查询会话模板

若要检索 SCID 的会话模板列表，以及特定会话模板的详细信息，请为以下 URI 之一使用 GET 方法：
-   **/serviceconfigs/{scid}/sessiontemplates**
-   **/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}**


#### <a name="query-for-session-state"></a>查询会话状态

若要查询会话状态，请为以下 URI 之一使用 GET 方法：
-   **/serviceconfigs/{scid}/sessions**
-   **/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions**

## <a name="multiplayer-session-explorer"></a>多人游戏会话资源管理器

多人游戏会话资源管理器是一个内置于 MPSD 的工具，用于浏览会话、会话模板和本地化字符串。 该工具单纯是为了用于开发沙盒。


### <a name="accessing-multiplayer-session-explorer"></a>访问多人游戏会话资源管理器

| 注意                                                                                                      |
|------------------------------------------------------------------------------------------------------------------------|
| 若要使用该工具，你必须登录。 你的浏览仅限于将登录用户作为成员的会话。 |

若要访问多人游戏会话资源管理器，在 Xbox One 上打开 Internet Explorer，按**视图**按钮，然后在**地址**字段中输入 <https://sessiondirectory.xboxlive.com/debug>。

| 注意                                                                                                                                                                                |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 如果你尝试在零售沙盒中访问该工具，你将收到 HTTP/404 状态代码。 有关此代码的详细信息，请参阅[多人游戏会话状态代码](multiplayer-session-status-codes.md)。 |


### <a name="using-multiplayer-session-explorer"></a>使用多人游戏会话资源管理器


#### <a name="open-the-main-page"></a>打开主页面

1.  访问工具的主页面。 它在沙盒中显示安全上下文（登录用户和沙盒）和服务配置 ID (SCID) 的列表。
2.  按**菜单**按钮将此页面固定到“主页”，这样你无需每次都键入 URI。


#### <a name="display-available-sessions-and-templates"></a>显示可用会话和模板

1.  在工具中单击 SCID 以在登录用户属于其成员的 SCID 中显示会话列表。
2.  在同一页面上，你可以单击 SCID，显示 SCID 的服务配置中的会话模板和本地化字符串。 这些项目通过[XDP](https://xdp.xboxlive.com)或[Windows 开发人员中心](https://developer.microsoft.com/dashboard/windows/overview)引入。


#### <a name="display-the-full-contents-of-a-session"></a>显示会话的完整内容

在多人游戏会话资源管理器中，单击会话名称可以显示相应会话的完整内容。

MPSD 显示的会话可能由于以下几个原因与会话 URI 的标准 GET 方法的响应不同：

-   GET 调用可能在 X-Xbl-Contract-Version 标头中使用较旧的协定版本。 会话资源管理器始终使用最新的协定版本显示会话。
-   当通过 GET 正常请求会话时，可能触发转换和负面影响，例如，到期超时。 会话资源管理器将在会话被存储时显示会话的快照，无需执行任何逻辑、转换或负面影响。
-   由于 nextTimer JSON 对象字段与负面影响同时计算，因此它不存在于 MPSD 会话中。

## <a name="see-also"></a>另请参阅

[会话概述](mpsd-session-details.md)

[多人游戏会话状态代码](multiplayer-session-status-codes.md)

[操作方法：更新多人游戏会话](multiplayer-how-tos.md)

[操作方法：从作品激活加入 MPSD 会话](multiplayer-how-tos.md)

[操作方法：订阅 MPSD 会话更改通知](multiplayer-how-tos.md)

[SmartMatch 匹配](smartmatch-matchmaking.md)
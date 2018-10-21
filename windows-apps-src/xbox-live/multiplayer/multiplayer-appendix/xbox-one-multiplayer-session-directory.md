---
title: Xbox One 多人游戏会话目录
author: KevinAsgari
description: 了解如何通过使用 Xbox Live 多人游戏会话目录 (MPSD) 服务创建多人游戏会话。
ms.assetid: 70da1be3-5f39-4eed-b62d-9cdd47e413d2
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0de1431aea2695d72682fd70f23695d672c465a6
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2018
ms.locfileid: "5165504"
---
# <a name="xbox-one-multiplayer-session-directory"></a>Xbox One 多人游戏会话目录

本主题使用新的 Xbox One 多人游戏会话目录 (MPSD) 服务提供多人游戏会话创建的概述。 本文主要面向将自己的会话模板直接提交到 Xbox 开发门户 (XDP) 的 Xbox One 游戏开发人员。 MPSD 服务可以使用 Windows 开发人员中心，配置，但不专注于在本文中。 它旨在让开发人员熟悉与 MPSD 配置、使用情况以及多人游戏会话疑难解答关联的术语和概念。

## <a name="revision-summary"></a>修订摘要

与 MPSD 服务通信时，客户端 XSAPI（Xbox 服务 API）库当前使用协定版本 104。 此版本文档更新会话模板架构到协定版本 107，这是 MPSD 服务支持的最新协定版本。 版本 104 和 107 之间的更改都汇总在标题为[协定架构更新](#_Contract_schema_update)的部分中。

请注意，为协定版本 104 编写的模板如果要作为 107 重新发布，将需要进行更新。 但是，这些服务都为后向兼容，因此你可以继续使用将在未来 XDK 发布中更新的当前库。

本文档的上次修订更新了关于服务器会话的信息并添加了有关 Xbox Live 服务 API 和 RESTful 服务调用的新部分。 此外，还更新了会话状态信息查询部分中显示的表，修订了服务质量 (QoS) 模板部分。

## <a name="introduction"></a>简介

在 Xbox One 上，多人游戏会话是位于多人游戏会话目录 (MPSD) 上云端的安全文档，此文档表示一组玩家在一起玩游戏。 具体来说，多人游戏会话具有以下特点：

-   每个会话都具有唯一的 URI。

-   会话文档包含当前成员、游戏设置、启动数据和游戏服务器信息。

-   游戏创建和管理它们自己的会话。

-   会话可在其成员间建立连接。

多人游戏会话目录集中跨所有客户端的游戏会话元数据。 它提供所需的关于会话的基本信息以帮助建立客户端之间的安全设备关联。 它还为客户端提供首次启动的基本元数据以在开始传递更多特定游戏数据前连接到游戏。 通过 Xbox One 上的进程周期管理 (PLM) 和应用程序切换本质，MPSD 可以确保客户端具有可用于联系识别为活动游戏会话一部分的对等和服务器的正确信息，并协调 shell 和控制台操作系统以保留、激活和管理游戏会话的玩家生存期。

## <a name="terminology-used-in-this-document"></a>本文档中使用的术语

| 术语                 | 定义                                                                                                                                                                                                                                                                                  |
|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 多人游戏会话  | 驻留在 Xbox Live 云端并在 Xbox One 上运行游戏时表示连接（或将连接）在一起的一组用户的安全文档。 多人游戏的所有方面（如匹配、参与方、加入进程等）都利用多人游戏会话。 |
| 游戏会话         | 这是显示在 MPSD，其中用户一起玩游戏中的实际游戏会话。 所有的多人游戏场景最终会结束于游戏会话。                                                                                                                               |
| 匹配票证会话 | 这是一个在匹配期间用于跟踪匹配票证提交的会话。                                                                                                                                                                                                                 |
| 非活动玩家      | 在会话内已被设置为“非活动”状态的玩家。 在游戏受限、暂停或处于非活动状态（如标题所定义）时，游戏将用户设置为“非活动”状态。                                                                                     |

## <a name="the-multiplayer-session-directory"></a>多人游戏会话目录

MPSD 促进并帮助游戏协调在线玩家之间的会话信息。 可以创建不同类型的会话，完成多人游戏的不同任务。 下表展示了如何在 Xbox 360 上执行此类任务与如何在 Xbox One 上完成它们之间的区别。

| 功能或任务                     | Xbox 360                                                                                                        | Xbox One                                                                                                   |
|--------------------------------------|-----------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| **获取游戏会话信息**     | **XSessionGetDetails**、**XSessionSearchByID** 或跟踪你自己。                                              | 请求来自服务的会话信息。                                                          |
| **迁移主机**                     | 在需要时，游戏调用 **XSessionMigrateHost**。                                                           | 无需创建新会话，只需为 SessionID 分配一个新主机。                                  |
| **管理多个玩家会话**  | 一次处理多个会话较为棘手。 例如，**XNetReplaceKey** 与 **XNetUnregisterKey**。 | 基于服务的会话让管理一个会话变得更简洁，让处理多个会话变得更容易。    |
| **处理注销和断开连接** | 必须分别使用 **XCloseHandle** 或 **XSessionDelete**，以不同方式处理断开连接和注销。 | 集中化服务简化了注销和断开连接处理，并在游戏配置中设置超时。 |

### <a name="session-patterns"></a>会话模式

-   游戏会话

    -   具有玩家的 XUID、安全设备地址数据和属性状态的会话。 这可以视为实际游戏会话。

    -   可以是对等、端点对主机、端点对服务器或混合。

-   匹配票证会话

    -   提交到匹配以查找要一起游戏的其他玩家的会话。 请注意，如果游戏正在查找更多玩家，则游戏会话也可能是票证会话。

-   服务器会话

    -   通过 Xbox Live 计算处理创建和使用的游戏会话。

图 1 说明了 MPSD 会话的使用情况，其中蓝色方框表示 MPSD 会话，红色方框表示正在使用它们的服务。

图 1. MPSD 会话使用。

## <a name="mpsd-sessions"></a>MPSD 会话

会话维护两个相关实体的列表：

1.  已加入或已被邀请加入会话的成员（用户）。

2.  已加入会话的服务器（云服务器或专用服务器）。

各实体具有：

1.  创建时设置的常量值。

2.  可变属性。

3.  只读元数据。

### <a name="xbox-live-service-apis-and-restful-service-calls"></a>Xbox Live 服务 Api 和 RESTful 服务调用

有两种方法可以连接 Xbox One 会话和匹配服务。 第一种方法是使用对 RESTful Xbox Live 服务 URI 的标准 HTTPS 调用。 这允许游戏根据其服务器和游戏配置灵活调用和连接这些服务。 可在“Xbox Live 服务 RESTful 参考”下的 [Xbox One 开发工具包 (XDK) 文档](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/home.aspx)中找到这些 URI 的列表。[1]

第二种方法是使用充当 RESTful 服务 URI 的包装器的 Xbox Live 服务 API。 这两种方法都允许采用更加传统的方式来使用代码中的 API，而无需处理各个调用的 HTTPS 流量。 Xbox 开发工具包 (XDK) 附带 Xbox Live 服务 API 的源代码，可以根据需要对此源代码进行修改并将其合并到你的游戏中。 示例使用 Xbox Live 服务 API 编写。 可在“Xbox Live 服务参考”下的 Xbox One [XDK 文档](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/home.aspx)中找到有关 Xbox Live 服务 API 的更多信息。[2]

### <a name="mpsd-sessions-and-templates"></a>MPSD 会话和目标

通过 Xbox 开发门户 (XDP) 使用引入的模板创建 MPSD 会话。 模板是为正在创建的会话定义框架的 JSON 文档。 模板为新会话提供常量。

以下来自[玩家集合代码示例](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx)的摘要为模板配置示例。

```json
// Used for creating the session that you then pass into your match ticket request. This *should* not have any QoS requirements.
MatchTicketSession (Contract Version: 107)
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {},
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            }
        },
        "custom": {}
    }
}

// This is the new game session that is returned after you’ve been matched.
// Note: This is used for in-game QoS. For out-of-game QoS, you will need P2P/HTP requirements.
GameSession (Contract Version:107)
{
    "constants": {
        "system": {
            "maxMembersCount": 12,
            "capabilities": {"connectivity": true }
        },
        "memberInitialization": {
         "joinTimeout": 20000,
         "measurementTimeout": 15000,
         "externalEvaluation": false,
         "membersNeededToStart": 4
         },

   "custom": {}
  }
}
```

匹配票证会话应与在其 **memberInitialization** 对象中使用 QoS 超时值设置的游戏会话模板一起使用。

图 2. 示例漏斗。

![](../../images/whitepapers/mpsd_image1.png)

后面的代码摘要是对等游戏会话模板（游戏托管的 QoS）的一个示例。

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 2
            }
        },
        "custom": {}
    }
}

```

下面是端点对服务器会话和 RAW 模板的一个示例。

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {}
        },
        "custom": {}
    }
}
```

下列代码显示了用于智能匹配的匹配票证会话模板的一个示例。

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {},
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            }
        },
        "custom": {}
    }
}

```

### <a name="checking-which-templates-are-configured-for-your-scid"></a>查看为 SCID 配置了哪些模板

#### <a name="session-templates"></a>会话模板

可从 MPSD 检索存在于 SCID 中的会话模板列表，以及特定会话模板的详细信息。

```
GET /serviceconfigs/{scid}/sessiontemplates

GET /serviceconfigs/{scid}/sessiontemplates/{session-template-name}
```

#### <a name="query-for-session-state-information"></a>查询会话状态信息

可在服务配置和会话模板级别查询会话：

```
GET /serviceconfigs/{scid}/sessions

GET /serviceconfigs/{scid}/sessiontemplates/{session-template-name}/sessions
```

默认情况下，此操作最多返回 100 个非专用会话。 可使用这些查询字符串参数修改查询：

| 查询字符串             | 作用                                                                                                         | 描述                                                                                         |
|--------------------------|----------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| keyword=foo              | 仅包括具有关键字“foo”的会话。                                                             |                                                                                                     |
| XUID=123                 | 仅包括用户“123”所在的会话。                                                               | 默认情况下，用户必须在要包括的会话中处于活动状态。                                  |
| *reservations*=**true** | 包括在其中将用户设置为保留玩家，但并未加入成为活动玩家的会话。 | 仅在查询你自己的会话，或对特定用户的会话进行服务器到服务器查询时。 |
| *inactive*=**true**      | 包括用户已接受但没有在其中主动进行游戏的会话。                                     | 用户已“准备好”但“活动”状态未算作“非活动”的会话。                           |
| *private*=**true**       | 包含专用会话。                                                                                      | 仅在查询你自己的会话，或进行服务器到服务器查询时。                            |
| *visibility*=open        | 仅包括“打开”的会话。                                                                         | 如果被设置为“private”，则也必须设置“private=true”指令。                                 |
| *take*=5                 | 最多返回五个会话。                                                                                    | 必须介于 0 和 100 之间。                                                                          |

结果是一组会话参考的 JSON 数组。 以内联方式包括某些会话数据。

**注意** 每个查询必须包括关键字筛选器、XUID 筛选器或两者都有。

将 *private*（返回专用会话）或 *reservations*（返回用户尚未加入的会话）设置为 **true** 要求调用方拥有对会话的服务器级访问权限，或需要调用方的 XUID 声明以匹配 URI 中的 XUID 筛选器。 否则，返回 403 禁止访问（是否确实存在任何此类会话）。

以下代码摘要显示了查询响应的一个示例。

```
{
"results": [ {
"xuid": "9876",  // If the session was found from a xuid, that xuid.
"startTime": "2009-06-15T13:45:30.0900000Z",
"sessionRef": {
  "scid": "foo",
  "templateName": "bar",
  "name": "session-seven"
},
"accepted": 4,        // Approximate number of non-reserved members.
"status": "active",   // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
"visibility": "open", // or "private", "visible", or "full"
"myTurn": true,       // Not present is the same as 'false'. Only present if the session was found using a xuid.
"keywords": [ "one", "two" ]
} ]
}

```

## <a name="session-template-attributes"></a>会话模板属性

<div id="_Contract_schema_update"/>

## <a name="contract-schema-update"></a>协定架构更新

正如本文档在开头所提到的，最新的会话模板协定版本为 107，它介绍了对之前 104 版本架构进行的一些更改。 为协定版本 104 编写的模板如果作为 107 重新发布，将需要进行更新。 以下是更改汇总。

-   将 **/constants/system/managedInitialization** 重命名为 **/constants/system/memberInitialization**。

    -   将 **autoEvaluate** 字段重命名为 **externalEvaluation**，并且其极性更改，除了 **false** 保持默认以外。

    -   将 **membersNeededToStart** 的默认值从 2 更改为 1。

    -   将 **joinTimeout** 的默认值从 5 秒更改为 10 秒。

    -   将 **measurementTimeout** 默认值从 5 秒更改为 30 秒。

-   删除 **/constants/system/timeouts**，重命名超时并重新放在 **/constants/system** 下。

    -   **reserved** 超时变为 **reservedRemovalTimeout**。

    -   **inactive** 超时变为 **inactiveRemovalTimeout**，其新的默认值为 0，而不是 2 小时。

    -   **ready** 超时变为 **readyRemovalTimeout**。

    -   **sessionEmpty** 超时变为 **sessionEmptyTimeout**。

-   必须在表示实际游戏（相对于匹配和大厅会话等帮助程序会话）的会话上将 **/constants/system/capabilities/gameplay** 指定为 **true**，以便启用最近玩家和信誉报告。

### <a name="system-objects"></a>系统对象

会话文档中的各个系统对象具有由 MPSD 强制执行和解释的固定架构。

在 PUT 请求的正文内，像自定义对象一样验证和合并系统对象。 但是与自定义对象不同，在系统对象合并后，将根据这些架构对其进一步的验证和操作。

**/constants/system**

```json
{
"version": 1,  //Document Version (FAL release version 1, service contract 107)
"maxMembersCount": 100,  // Defaults to 100 if not set on creation. Must be between 1 and 100.
"visibility": "private",  // Or "visible" or "open", defaults to "open" if not set on creation.

"initiators": [ "1234" ],  // If specified on a new session, the creators xuid must be in the list (or the creator must be a server).
"inviteProtocol": "party",  // Optional URI scheme of the launch URI for invite toasts.

"reservedRemovalTimeout": 10000, // Default is 30 seconds. Member is removed from the session.
"inactiveRemovalTimeout": 0, // Default is zero. Member is removed from the session.
"readyRemovalTimeout": 60000, // Default is three minutes. Member is removed from the session.
"sessionEmptyTimeout": 0, // Default is zero. Session is deleted.

// Capabilities are boolean values that are optionally set in the session template. If no capabilities are needed, an empty "capabilities" object should be in the template in order to prevent capabilities from being specified on session creation, unless the title desires dynamic session capabilities.
"capabilities": {
"clientMatchmaking": true,
"connectivity": true, // Cannot be set if ‘large’ is specified.
     "suppressPresenceActivityCheck": false,
     "gameplay": false,
     "large": false
},
/* If a "memberInitialization" object is set, the session expects the client system or title to perform initialization following session creation and/or as new members join the session. The timeouts and initialization stages are automatically tracked by the session, including QoS measurements if any metrics are set. These timeouts override the session's reservation and ready timeouts for members that have 'initializationEpisode' set. */
"memberInitialization": {
"joinTimeout": 20000,  // Milliseconds. Unspecified counts as 10 seconds.
"externalEvaluation": false,
"membersNeededToStart": 2  // Unspecified counts as 1. Must be between 0 and maxMembersCount. Only applies to episode 1. If 00 and the session is created with no members to initialize, episode 1 is skipped..
},
```

**/properties/system**

```
{
// Optional array of case-insensitive strings. Cannot be set if the session's visibility is "private".
"keywords": [ "hello" ],

// Array of integer indices of members whose turn it is. Defaults to empty. Can't be set (and doesn't appear) on large sessions.
"turn": [ 0 ],

/* Restricts who can join "open" sessions. (Has no effect on reservations, which means it has no impact on "private" and "visible" sessions.) Defaults to "none". On large sessions, "none" is the only valid value.
If "local", only users whose token's DeviceId matches someone else already in the session and "active": true.
If "followed", only local users (as defined above) and users who are followed by an existing (not reserved) member of the session can join without a reservation. */
"joinRestriction": "none",

// Device token of the host. Must match the "deviceToken" of at least one member, otherwise this field is deleted.
// If "peerToHostRequirements" is set and "host" is set, the measurement stage assumes the given host is the correct host and only measures metrics to that host.
// Can't be set on large sessions.
"host": "99e4c701",

// Can only be set while "initializing/stage" is "evaluating". True indicates success, and false indicates failure. Once set, "initializing/stage" is immediately updated, and this field is removed.
"initializationSucceeded": true,

/* The ordered list of case-insensitive connection strings that the session could use to connect to a game server. Generally titles should use the first on the list, but sophisticated titles could use a custom mechanism (e.g. Thunderhead) for choosing one of the others (e.g. based on load). */
"serverConnectionStringCandidates": [ "datacenter b", "serverfarm a" ],

"matchmaking": {
    "targetSessionConstants": { },
    // Force a specific connection string to be used (useful in preserveSession=always cases).
    "serverConnectionString": "datacenter b",
},

// True if the match that was found didn't work out and needs to be resubmitted. Set to false to signal that the match did work, and the matchmaking service can release the session.
"matchmakingResubmit": true
}

```

### <a name="timeouts"></a>超时

会话可以由计时器和其他外部事件更改。 MPSD 中的 **Timeouts** 对象提供基本机制以管理会话生存期和成员。

会话的 **nextTimer** 字段提供下一个计时器的时间。 客户端可以使用此信息为下次轮询选取合适的间隔。 此值也会在 **Expires** HTTP 标头中返回。

超时均使用毫秒指定。 允许为零，表示立即超时。 如果未指定给定超时，则将其视为无限。 因为超时具有默认值，因此会话模板应为无限超时明确指定“null”。

#### <a name="sessionemptytimeout"></a>SessionEmptyTimeout

在将被删除的会话变为空后，**/constants/system/sessionEmptyTimeout** 值会配置毫秒数。 默认值为零，意味着将立即删除此会话。 如果未指定值，将无限提供空会话。

#### <a name="member-timeouts"></a>成员超时

**/constants/system** 内的三个其他超时控制成员可保持特定状态的时间量：

-   **reservedRemovalTimeout**

    -   当超时过期时，将删除保留。 默认值为 30 秒。

-   **inactiveRemovalTimeout**

    -   默认情况下，在两小时后将非活动成员从会话中删除。

-   **readyRemovalTimeout**

    -   默认情况下，“准备好”在三分钟后还原到非活动状态的成员。

<div id="_Member_initialization_in"/>

## <a name="member-initialization-in-session-documents"></a>会话文档中的成员初始化

### <a name="member-initialization"></a>成员初始化


在创建会话后和/或当新成员加入会话时，**memberInitialization** 对象控制初始化阶段。 超时和初始化阶段由会话自动跟踪，包括 QoS 度量（如果设置了任何指标）。

对于设置了 **initializationEpisode** 属性的成员，这些超时值将替代会话的保留和准备超时值。

示例：

```
"memberInitialization": {
        "joinTimeout": 5000,
        "measurementTimeout": 5000,
        "evaluationTimeout": 5000,  // only specify for external evaluation
        "externalEvaluation": true,
       "membersNeededToStart": 2
    },
```


![](../../images/whitepapers/mpsd_image2.png)

**图 3. 成员初始化流程。**

三个成员初始化阶段的每个阶段都可能超时：

1.  **joiningTimeout**

    -   用户必须加入会话的时间量（以毫秒计）。 将删除无法加入的成员保留。

2.  **measuringTimeout**

    -   用户必须上载其度量的时间量。 无法上传度量的成员标有“超时”的 *failureReason*。

3.  **evaluationTimeout**

    -   成员作出和上传评估决定的时间量。 如果未收到任何决定，则将其计作失败。

**externalEvaluation**

-   MPSD 可以根据会话模板中提供的要求执行自动 QoS 评估。 externalEvaluation 被设置为 false 时执行此评估。 设置 **evaluationTimeout** 时，**externalEvaluation** 需要为 **true**。 如果有两个对等或端点对主机要求，你仍然应该将 **externalEvaluation** 设置为 **false**，以便会话自动完成初始化。

**membersNeededToStart**

-   这是需要与 "initialize":"true" 共同存在并通过 QoS 以使自动评估成功的成员保留的最小数量。

### <a name="initialization-episodes"></a>初始化剧集：


设置 **memberInitialization** 对象时，MPSD 将通过剧集发展初始化阶段。 在创建会话且会话将包括在模板中定义的所有阶段时，第一集开始。

当剧集已经在播放中时，将为下一集标记受邀或加入的新成员。 一般情况下，可从会话的 **initializing** 对象检索剧集或 **memberInitialization** 的状态。

**注意** 请注意，在初始化完成后删除此对象。

示例：

```
"initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
},

```

此阶段经历了从加入到度量再到评估的过程。 一旦某集失败，阶段将被设置为 *failed* 且无法初始化会话。 否则，一旦初始化剧集完成，**initializing** 对象将被删除。

还可以为各成员跟踪初始化失败。 如果此成员未通过，则在从加入或度量阶段过渡到其他阶段时对它们进行设置。

示例：
```
"initializationFailure": "latency",
```
根据优先顺序，可以将该属性的值设置为 *timeout、latency、bandwidthdown、bandwidthup、network、group* 或 *episode*。 网络值意味着阻止度量 QoS 指标测量的网络配置和/或条件（如冲突的网络地址转换 \[NAT\]）。 加入末尾的唯一可能值是 *group*。 （一旦加入超时，将删除保留。）

如果设置了 **memberInitialization**，并添加了其中 "initialize":true 的成员，此操作会设置为成员将参加的初始化剧集。 值 1 用于在创建新会话时向其中添加的成员，并在初始化剧集结束时将其删除。
```
"initializationEpisode": 1,
```

## <a name="match-ticket-session"></a>匹配票证会话

当 MPSD 会话正在作为匹配票证会话使用时，使用某些特殊的会话属性和常量。

**/members/{index}/constants/system**

```json
{

  {
  "xuid": "12345678",

  "initialize": "false", // Run initialization for this user (if “memberInitialization” is set). Defaults to false.

```

当匹配服务将用户添加到会话时，它会在 **matchmakingResult** 字段中提供一些有关将这些属性和常量匹配到会话的方式和原因的上下文。

```
"matchmakingResult": {
```

这是匹配会话中的用户的 **serverMeasurements** 副本。

```json
"serverMeasurements": {
    "east.thunderhead.azure.com": {
        "latency": 233  // Milliseconds
      }
    }
  }
}

```

## <a name="quality-of-service-qos-templates"></a>服务质量 (QoS) 模板

对于游戏会话模板，可以添加通知 MPSD 协调网络层和控制台社交平台的值。 协调的目的在于在创建会话和通知用户可以加入游戏之前，验证连接状态的质量。

以下代码摘要是使用 QoS 的端点对主机游戏会话模板的示例：

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 20,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            },
            "peerToHostRequirements": {
                "latencyMaximum": 350,
                "bandwidthDownMinimum": 1000,
                "bandwidthUpMinimum": 100,
                "hostSelectionMetric": "latency"
            }
        },
        "custom": {}
    }
}
```

此代码摘要是具有 QoS 的对等游戏会话模板的示例：

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 20,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            },
            "peerToPeerRequirements": {
                "latencyMaximum": 250,
                "bandwidthMinimum": 10000
            }
        },
        "custom": {}
    }
}

```

### <a name="qos-session-template-attributes"></a>QoS 会话模板属性

如果设置了 **memberInitialization** 对象，则会话需要客户端系统或游戏在创建会话后和/或新成员加入会话后执行初始化操作。

超时和初始化阶段由会话自动跟踪，包括 QoS 度量（如果设置了任何指标）。

对于设置了 **initializationEpisode** 属性的成员，这些超时值将替代会话的保留和准备超时值。

```json
"memberInitialization": {
"joinTimeout": 5000,  // Milliseconds. Unspecified counts as 10 seconds.
"measurementTimeout": 5000,  // Milliseconds. Unspecified counts as 30 seconds.
"evaluationTimeout": 5000,  // Milliseconds. Can only be set if 'externalEvaluation' is true. Unspecified counts as 5 seconds.
"externalEvaluation": true,
"membersNeededToStart": 2  // Unspecified counts as 1. Must be between 0 and maxMembersCount. Only applies to episode 1. If 0 and the session is created with no members to initialize, episode 1 is skipped.
},

```

具有 QoS 的游戏会话要求连接功能。 如果未指定指标，则默认为为了满足 QoS 要求所需的指标。 如果指定了指标，则其必须满足 QoS 要求。

```json
"metrics": {
    "latency": true,
    "bandwidthDown": true,
    "bandwidthUp": true,
    "custom": true

```

以下阈值适用于会话中所有成员的各个成对连接：

```json
"peerToPeerRequirements": {
    "latencyMaximum": 250,  // Milliseconds
    "bandwidthMinimum": 10000  // Kilobits per second
},

```

以下阙值适用于来自主机候选的各个连接：

```json
"peerToHostRequirements": {
    "latencyMaximum": 250,  // Milliseconds
    "bandwidthDownMinimum": 100000,  // Kilobits per second
    "bandwidthUpMinimum": 1000,  // Kilobits per second
    "hostSelectionMetric": "bandwidthup"  // Or "bandwidthdown" or "latency". Not specified is the same as "latency".
},

```

应评估下列潜在的服务器连接字符串（注意，连接字符串必须为小写）：

```json
"measurementServerAddresses": {
        "east.thunderhead.azure.com": {
            "secureDeviceAddress": "r5Y="  // Base-64 encoded secure-device-address
        },
        "west.thunderhead.azure.com": {
            "secureDeviceAddress": "rwY="
        }
    }
}

```

**members/{index}/properties/system**

这些标记控制成员状态和 **activeTitle**，而且它们还相互排斥（将两者都设置为 **true** 是错误的）。 对于它们每一个，**false** 等同于“不存在”。 默认状态为“非活动”，即，二者皆不存在。

```json
"ready": true,
"active": false,

// Base-64 blob, or not present. Empty-string is the same as not present.
// 'capabilities/connectivity' must be enabled in order for this to be set.
"secureDeviceAddress": "ryY=",

// List of members in my group, by index. Each element must be an integer 0 <= n < 'membersInfo/next'.  
// During member initialization, if any members in the list fail, this member will also fail.
"group": [ 5 ],

// QoS measurements by lower-case device token.
// Like all fields, "measurements" must be updated as a whole. It should be set once when measurement is complete, not incrementally.
// Metrics can me omitted if they weren't successfully measured, i.e. the peer is unreachable.
// If a "measurements" object is set, it can't contain an entry for the member's own address.
"measurements": {
"e69c43a8": {
"latency": 5953,  // Milliseconds
"bandwidthDown": 19342,  // Kilobits per second
"bandwidthUp": 944,  // Kilobits per second
"custom": { }
     }
},

// QoS measurements by game-server connection string. Like all fields, "serverMeasurements" must be updated as a whole, so it should be set once when measurement is complete.
// If empty, it means that none of the measurements completed within the "serverMeasurementTimeout".
    "serverMeasurements": {
        "east.thunderhead.azure.com": {
            "latency": 233  // Milliseconds
        }
    },

// Subscriptions for shoulder taps on session changes. The 'profile' indicates which session changes to tap as well as other properties of the registration like the min time between taps.
// The subscription is named with a client-generated GUID that is also sent back with the tap as a context ID.
// Subscriptions can be added and removed individually, without affecting other subscriptions in the "subscriptions" object.
// To remove a subscription, set its context ID to null.
// (Like the "ready" and "active" flags, the "subscriptions" data is copied out and maintained internally, so the normal replace-all rule on system fields doesn't apply to "subscriptions".)
"subscriptions": {
"961dc162-3a8c-4982-b58b-0347ed086bc9": {
"profile": "party",  // Or "matchmaking", "initialization", "roster", "queueHost", or "queue"
"onBehalfOfTitleId": "3948320593",  // Optional decimal title ID of the registered channel.  If not set the title ID is taken from the token.
},
"709fef70-4638-4b94-905b-24cb02706eb5": null
}
}

```

## <a name="qos-phase-and-session-initialization-details"></a>QoS 阶段和会话初始化详细信息

在模板已完成成员初始化后，MPSD 将跟踪和报告游戏创建的 QoS 结果。 将由会话文档中的 *initializing* 对象表示此操作的进度，如上面的[成员初始化](#_Member_initialization_in)部分所述。 *initializing* 对象具有 *stage* 属性，此属性表示初始化的当前阶段。 此阶段经历了从*加入*到*度量*再到*评估*的过程。

-   一旦初始化剧集 \#1 失败，则阶段会被设置为 *failed* 且无法初始化会话。 否则，一旦初始化剧集完成，“initializing”对象将被删除。 如果 **externalEvaluation** 被设置为 **false**，将跳过评估阶段。 如果 **metrics** 和 **measurementServerAddresses** 都未设置，将跳过度量阶段。

```json
"initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
},

```

-   主机候选是按照优先顺序列表的设备令牌。 如果设置了 **peerToHostRequirements**，但未设置 **/properties/system/host**，则在初始化剧集 \#1 的*度量*阶段之后对其进行设置。 在设置 **/properties/system/host** 对象后清除它们。

```json
"hostCandidates": [ "ab90a362", "99582e67" ],

"constants": { /* Property Bag */ },
"properties": { /* Property Bag */ },
"members": {
    "1": {
        "constants": { /* Property Bag */ },
        "properties": { /* Property Bag */ },

```

-   如果玩家已接受且已找到此玩家的玩家代号声明，将仅设置 *gamertag* 属性。

```json
"gamertag": "stacy",
```

-   当玩家上传安全设备地址时，设置 *deviceToken* 属性。 它是可用于相等比较的不区分大小写的字符串。

```json
"deviceToken": "9f4032ba7",
```

-   在用户执行他或她的第一个对会话文档的 PUT 请求后删除 *reserved* 值。 一旦保留玩家，这意味着已邀请其进入游戏会话，但既未接受邀请也未评估连接。

```json
"reserved": true,
```

-   如果成员处于活动状态，*activeTitleId*是处于活动状态的成员所在的游戏（以十进制表示）。

```json
"activeTitleId": "8397267",
```

-   此属性指用户加入会话的时间。 如果 *reserved* 为 **true**，则 *joinTime* 将为执行保留的时间。

```json
"joinTime": "2009-06-15T13:45:30.0900000Z",
```

-   如果此成员位于 properties/system/turn 数组，则存在，否则不存在。

```json
"turn": true,
```

-   如果成员尚未成功通过此阶段，则在从加入或度量阶段过渡到其他阶段时，对成员对象设置 *initializationFailure* 属性。 根据优先顺序，可以将其设置为 *timeout*、*latency*、*bandwidthdown*、*bandwidthup*、*network*、*group* 或 *episode*。
    *网络*值意味着阻止度量 QoS 指标测量的网络配置和/或条件（如冲突网络地址转换 \[NAT\]）。 加入末尾的唯一可能值是 *group*。 （一旦加入超时，将删除保留。）加入或度量期间，“评估”阶段失败后，对未失败的初始化剧集的所有成员设置此 *episode* 值。

```json
"initializationFailure": "latency",
```

-   如果设置了 **memberInitialization**，并添加了其中 "initialize":true 的成员，此操作会设置为成员将参加的初始化剧集。 值 1 用于在创建时间向新会话添加的成员。 初始化剧集结束时删除。

```json
"initializationEpisode": 1,
```

-   *next* 属性表示会话中的下一个成员的索引值。 如果没有更多成员要添加，它将与下面的 **membersInfo** 对象中的 *next* 属性值相同。

```json
            "next": 4
        },
        "4": { "next": 5 /* etc */ }
    },
    "membersInfo": {
        "first": 1,  // The first member's index.
        "next": 5,  // The index that the next member added will get.
        "count": 2,  // The number of members.
        "accepted": 1  // The number of members that are no longer 'pending'.
    },
    "servers": {
        "name": {
            "constants": { /* Property Bag */ },
            "properties": { /* Property Bag */ }
        }
    }
}

```

## <a name="xbox-cloud-compute-session"></a>Xbox 云计算会话

Xbox 云计算会话包含不区分大小写的连接字符串的已排序列表，会话将其用于连接游戏服务器。 一般来说，游戏应使用列表上的第一个字符串，但复杂的游戏可以使用自定义机制（如 Xbox Live 计算）选择其他字符串中的一个（例如，基于负载）。

```json
    "serverConnectionStringCandidates": [ "west.thunderhead.azure.com", "east.thunderhead.azure.com" ],

    "matchmaking": {
        "clientResult": {  // Requires the clientMatchmaking property.
            "status": "searching",  // Or "expired", "found", "failed", or "canceled".
            "statusDetails": "Description",  // Default is empty string.
            "typicalWait": 30,  // The expected number of seconds waiting as a non-negative integer.
            "targetSessionRef": {
                "scid": "1ECFDB89-36EB-4E59-8901-11F7393689AE",
                "templateName": "capture-the-flag",
                "name": "2D58F65F-0E3C-4F1F-8277-2BC9873FDB23"
            }
        },

        "targetSessionConstants": { },

        // Force a specific connection string to be used (useful in preserveSession=always cases).
        "serverConnectionString": "west.thunderhead.azure.com",

        // True if the match that was found didn't work out and needs to be resubmitted. Set to false
        // to signal that the match did work, and the matchmaking service can release the session.
        "resubmit": true
    }
}

```

**/servers/{server-name}/properties/system **

```json
{
    "lockId": "opaque56789",  // If set, a matchmaking service is servicing this session.
    "status": "searching",  // Or "expired", "found", "failed", or "canceled". Optional.
    "statusDetails": "Description",  // Optional free-form text. Default is empty string.
    "typicalWait": 30,  // Optional. The expected number of seconds waiting as a non-negative integer.
    "targetSessionRef": {  // Optional.
        "scid": "1ECFDB89-36EB-4E59-8901-11F7393689AE",
        "templateName": "capture-the-flag",
        "name": "2D58F65F-0E3C-4F1F-8277-2BC9873FDB23"
    }
}

```

## <a name="raw-session-and-custom-title-properties"></a>原始会话和自定义游戏属性

会话可用于存储关于多人游戏的自定义内务信息（元数据）。 游戏数据或保存的信息应存储在 TMS++ 中。

### <a name="property-bags"></a>属性包

标记为属性包的上述各对象包括两个可选内部对象、系统和自定义项。

自定义对象可包含任何 JSON。

```json
"custom": {
    "myField1": true,
    "myField2": "string",
    "myField3": 5.5,
    "myField4": { "myObject": null },
    "myField5": [ "my", "array" ]
}

```

## <a name="active-member-decay"></a>活动成员衰减

当 MPSD 检测到用户不再参与此游戏时，将活动成员自动标记为非活动。 这种情况可能会发生，例如，如果状态超时用户记录。 此机制只是一种支持；即，当成员离开游戏或从游戏切换出去、注销或不参与时，仍需要游戏主动将成员标记为非活动（或将其从会话中删除）。

## <a name="faq-and-troubleshooting"></a>常见问题和疑难解答

### <a name="how-do-i-call-mpsd-"></a>如何调用 MPSD？

使用证书身份验证：client-sessiondirectory.xboxlive.com

示例：

```
PUT https://client-sessiondirectory-stress.xboxlive.com/serviceconfigs/8cvda84-2606-4bab-8eda-d12313e65143/sessiontemplates/teamDeathmatch/sessions/3baafc35-816d-49cd-9656-5772506c988a
```

使用 XToken 身份验证：sessiondirectory.xboxlive.com

示例：

```
PUT https://sessiondirectory-stress.xboxlive.com/serviceconfigs/8cvda84-2606-4bab-8eda-d12313e65143/sessiontemplates/teamDeathmatch/sessions/3baafc35-816d-49cd-9656-5772506c988a
```

### <a name="how-do-i-find-out-what-scid-session-template-and-sandbox-to-use"></a>如何查找要使用的 SCID、会话模板和沙盒？

可以在适用于游戏的 XDP 站点上找到此信息。 如果你还没有权限访问 XDP，请联系你的开发人员帐户管理员，他可以帮助你获取相关信息。

### <a name="is-there-an-example-of-a-request-body-that-i-can-compare-my-own-to"></a>是否有一个请求正文的示例，我可以将自己的与其相比？


### <a name="i-am-getting-a-404-error-when-calling-mpsd"></a>调用 MPSD 时我收到了 404 错误。

收集 Fiddler 跟踪以帮助获取更多信息，然后执行下列操作：

1.  查看作为常见 404 消息的 HttpResponse 正文的一部分返回的消息：

    -   *请求的服务配置无效，或不是为会话配置的*。 验证使用中的 SCID 是否正确。

    -   *未找到请求的会话*。 在检索会话前，验证会话是否创建且会话模板是否正确。 你可以使用 PUT 调用创建会话。

2.  查看你所使用的 URI。

3.  重新启动控制台和/或使用新用户再试一次。

4.  搜索[娱乐开发人员论坛](https://developer.xboxlive.com/en-us/platform/community/forums/Pages/home.aspx)，了解错误代码或其他可能的解决方案。

### <a name="i-am-getting-a-403-error-when-calling-mpsd"></a>调用 MPSD 时我收到了 403 错误。

这通常是权限或范围问题。 收集 Fiddler 跟踪以帮助获取更多信息，然后执行下列操作：

1.  查看作为常见 403 消息的 HttpResponse 正文的一部分返回的消息：

-   *无法访问请求的服务配置。 *

    -   验证你所使用的帐户是否有权访问沙盒。

    -   验证你是否位于正确的沙盒内。

    -   如果你正在使用证书身份验证并收到此错误，请联系你的 DAM。

-   *无法访问请求的会话。 只有会话成员才可以读取专用会话。*

    -   你正在尝试访问具有“专用”可见性的会话。 只有会话内的成员才可以读取会话文档。

-   *请求正文无法包含现有成员引用，除非身份验证主体包括服务器。*

    -   你无法代表用户加入其他用户进行会话。 你只能邀请。 将索引设置为“reserve\_&lt;number&gt;”来邀请玩家。

### <a name="i-am-getting-a-412-precondition-failed-error"></a>我收到了 412 前置条件失败错误。

如果会话已经存在，此操作将返回 412 前置条件失败：

> PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/foo HTTP/1.1 Content-Type: application/json If-None-Match: \*

如果会话 etag 与 If-Match 标头不匹配，此操作将返回 412 前置条件失败：

> PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/foo HTTP/1.1 Content-Type: application/json If--Match: 9555A7DE-8B91-40E4-8CFB-0629312C9C7D

### <a name="i-am-getting-errors-such-as-405-409-503-and-400when-calling-mpsd"></a>调用 MPSD 时，我收到了 405、409、503 和 400 等错误。

收集 Fiddler 跟踪以帮助获取更多信息，然后查看作为 HttpResponse 正文的一部分返回的消息： 这应该为你提供了足够的信息，用来识别并修复错误或搜索[娱乐开发人员论坛](https://developer.xboxlive.com/en-us/platform/community/forums/Pages/home.aspx)以获取可能的解决方案。

如果你正在使用 Xbox Live 服务 API，还可以通过将 **DiagnosticsTraceLevel** 属性设置为“Error”（这将输出调试输出中的信息）来获取响应正文，或者你可以使用 **XboxLiveContextSettings.ServiceCallRouted** 事件（如多个示例中所示）输出到你的游戏 UI。

### <a name="what-can-or-should-i-change-in-the-templates-for-my-title"></a>我可以或应当在模板中为我的游戏做哪些修改？

会话模板不是默认设置，更大程度上是一个模具。 但是，你无法替代模板中已设置的常量，尽管你可以向其中添加。

### <a name="im-getting-an-error-that-is-saying-my-session-isnt-initialized"></a>我收到一条显示我的会话未初始化的错误。

当成员初始化存在于你的模板（通常是游戏、参与方和匹配票证场景）中时，你需要确保为足够的成员保留（需要成员才能开始）设置“initialize=true”以通过 QoS 来修复此问题。

### <a name="my-session-is-not-being-created-and-im-getting-an-http-204-error"></a>未创建我的会话且收到了 HTTP 204 错误。

这表明当你创建会话时，没有将用户添加到会话。 如果在创建时会话中没有用户，将不会创建此会话。 请确保你在创建时至少将一个玩家添加到会话。 对于专用的服务器场景，获取正在尝试创建匹配或需要创建匹配的玩家，并让此用户成为此次匹配中的初始玩家。 或者，你可以更改或删除 **sessionEmptyTimeout**。

### <a name="when-should-i-poll-the-mpsd"></a>应何时轮询 MPSD？

应避免轮询 MPSD 会话。 在高级别上，为做到这一点，你可以通过使用 MPSD 会话来初始建立各客户端的网络连接并为已失去连接的客户端重新建立网络状态，以此来设计你自己的代码。 利用 MPSD 的基于 etag 的同步基元消除对刷新会话状态以解决争用问题的需求也很重要。

当你拥有全部需要连接在一起并且在对等网格中游戏的一组 N 客户端时，会经常应用到这些原则。 各成员可以加入会话、连接到已经在会话中的会员，而不是定期为新成员查询会话，并假设之后的任何加入者都将执行相同的操作。 有关如何实现此操作的示例，请参阅聊天和玩家集合示例。

在极少数情况下，对于较短时间，轮询可能是必不可少的；如果你认为你需要执行此操作，请联系你的开发人员帐户管理员。

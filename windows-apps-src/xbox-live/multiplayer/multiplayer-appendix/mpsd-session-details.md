---
title: 多人游戏会话详细信息
description: 了解 Xbox Live 多人游戏会话的详细信息。
ms.assetid: 5aeae339-4a97-45f4-b0e7-e669c994f249
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 多人游戏 2015, 会话, mpsd
ms.localizationpriority: medium
ms.openlocfilehash: 175dddb79ed7e9d7cddc970e0b48efed11fbe0b6
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2018
ms.locfileid: "8752631"
---
# <a name="mpsd-session-details"></a>MPSD 会话详细信息

本文包含以下部分：
* 会话概述
* 会话功能
* 会话大小
* 会话用户状态
* 可见性和可加入性
* 会话超时
* 单个主机上多个登录的用户
* 进程周期管理
* 清理非活动会话
* 会话仲裁程序

## <a name="session-overview"></a>会话概述

多人游戏会话目录 (MPSD) 会话具有会话名称并被标识为会话模板，这是为会话提供默认设置的 JSON 文档的实例。 模板是具有服务配置标识符 (SCID) 的服务配置的一部分，这是一个 GUID。 可以在[Xbox 开发人员门户 (XDP)](https://xdp.xboxlive.com)和[合作伙伴中心](https://partner.microsoft.com/dashboard)上找到此模板。 服务配置是面向开发人员的资源用于引入、 管理和安全策略。 当通过 MPSD 访问会话时，主要授权根据设置开发人员通过 XDP 或合作伙伴中心访问策略的服务配置对执行。 当会话在授予了服务配置的访问权限后加载时，辅助访问检查（如会话成员身份验证）在会话级别执行。

本主题假设你的模板使用协定版本 107，即当前的 MPSD 为 Xbox One 使用的版本。 如果你有基于协定版本 105（与 104 相同）定义的模板，你必须更改这些设置以支持版本 107。 有关说明，请参阅[常见的多人游戏 2015 迁移问题](common-issues-when-adapting-multiplayer.md)。

| 重要提示                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 通过模板设置的功能不能通过写入到 MPSD 更改。 若要更改这些值，你必须创建并提交包含必要更改的新模板。 未通过模板设置的任何项目均可以通过写入到 MPSD 更改。 |

### <a name="session-reference"></a>会话引用

每个 MPSD 会话被一个会话引用所引用，在多人游戏 WinRT API 中由 **MultiplayerSessionReference 类**表示。 会话引用包含以下字符串值：

-   服务配置标识符 (SCID)
-   会话模板名称
-   会话名称

会话引用会映射到 URI 中以识别会话，如下所示。 在以下示例映射中，“颁发机构”是 sessiondirectory.xboxlive.com。

```HTTP
https://{authority}/serviceconfigs/{service-config-id}/sessiontemplates/{session-template-name}/sessions/{session-name}
```

### <a name="elements-of-a-session"></a>会话元素

每个会话包含多组强制执行可变性和安全规则的会话元素（规则因会话元素而异），以及只读整理信息（元数据）。 本部分介绍 JSON 文件中包含的用于配置会话以及 JSON 文件中用于你选择的模板的各组会话元素。

| 注意                                                                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 如果你为 HTTP/REST 实现使用 WinRT 包装器，你的会话和模板必须定义能够准确反映 WinRT 功能的 JSON 对象。 |

在每个元素组中，有两个内部对象：

-   系统对象 - 这些对象具有 MPSD 强制要求和解释的固定架构。 它们已经过验证并合并。 由于 MPSD 定义并了解这些对象的含义，所以可以对它们执行操作。 若要了解每个系统对象的完整定义，请参见 **MultiplayerSession 类**和**会话目录 URI** 的引用。
-   自定义对象 - 这些对象是可选的，并且没有架构。 它们用于存储有关多人游戏的元数据。 由于 MPSD 无法解释此数据，因此不对数据执行操作。 游戏数据或保存的信息应存储在游戏管理的存储 (TMS) 中。 有关 TMS 的详细信息，请参阅 **Xbox Live 作品存储**。

以下是自定义 JSON 对象的示例：
```JSON
    "custom": {
      "myField1": true,
      "myField2": "string",
      "myField3": 5.5,
      "myField4": { "myObject": null },
      "myField5": [ "my", "array" ]
    }
```

#### <a name="session-constants"></a>会话常量

会话常量仅在创建时由创建者或会话模板设置。 /constants/system 对象用于为多人游戏系统定义常量，因为 MPSD 让它变为已知。 与此对象关联的 WinRT 包装器由 **MultiplayerSessionConstants 类**表示。

/constants/system 对象可以定义很多项目，包括功能对象、指标对象、managedInitialization（模板协定版本 104/105）或 memberInitialization（协定版本 107）对象、peerToPeerRequirements 对象、peerToHostRequirements 对象和 measurementsServerAddresses 对象。


#### <a name="session-properties"></a>会话属性

使用 /properties/system 对象为 MPSD 定义会话属性。 与此对象关联的 WinRT 包装器是 **MultiplayerSessionProperties 类**。 会话属性可以随时由会话成员写入。 JSON 格式的会话属性示例如：joinRestriction、initializationSucceeded 和匹配对象。 有关此元素组的使用示例，请参阅[目标会话初始化和 QoS](smartmatch-matchmaking.md)。


#### <a name="member-constants"></a>成员常量

在加入时为每个会话成员设置成员常量。 JSON 对象是 /members/{index}/constants/system。 表示会话成员的 WinRT 包装器类是 **MultiplayerSessionMember 类**。


## <a name="member-properties"></a>成员属性

成员属性只能由会话成员写入。 它们在 /members/{index}/properties/system 对象中设置，并反映 **MultiplayerSessionMember 类**的元素。 下面是一个示例：

```
    {
      // These flags control the member status and "activeTitle", and are mutually exclusive (it's an error to set both to true).
      // For each, false is the same as not present. The default status is "inactive", i.e. neither present.
      "ready": true,
      "active": false,

      // Base-64 blob, or not present. Empty string is the same as not present.
      "secureDeviceAddress": "ryY=",

      // During member initialization, if any members in the list fail, this member will also fail.
      // Can't be set on large sessions.
      "initializationGroup": [ 5 ],

      // List of the groups I'm in and the encounters I just had.
      // An encounter is a brief interaction with a group. When an encounter is reported, it counts as retroactively joining the group 30 seconds ago and just now leaving.
      // Group names use the session name validation rules (e.g. case-insensitive).
      // On large sessions, groups are used to report who played with who (rather than just session membership). Members
      // that are active in at least one group together at the same time are counted as playing together.
      // Empty lists are the same as no value specified.
      // The set of encounters is a point-in-time property, so it's immediately consumed and will never appear on a response.
      "groups": [ "team-buzz", "posse.99" ],
      "encounters": [ "CoffeeShop-757093D8-E41F-49D0-BB13-17A49B20C6B9" ],

      // Optional list of role preferences the player has specified for role-based game modes.
      // All role names have to match across all members in the session. Role weights are
      // defined from 0-100.
      "RolePreference": { "medic": 75, "sniper": 25, "assault": 50, "support": 100 },

      // QoS measurements by lower-case device token.
      // Like all fields, "measurements" must be updated as a whole. It should be set once when measurement is complete, not incrementally.
      // Metrics can be omitted if they weren't successfully measured, i.e. the peer is unreachable.
      // If a "measurements" object is set, it can't contain an entry for the member's own address.
      "measurements": {
        "e69c43a8": {
          "bandwidthDown": 19342,  // Kilobits per second
          "bandwidthUp": 944,  // Kilobits per second
          "custom": { }
        }

      // QoS measurements by game-server connection string. Like all fields, "serverMeasurements" must be updated as a whole, so it should be set once when measurement is complete.
      // If empty, it means that none of the measurements completed within the "serverMeasurementTimeout".
      "serverMeasurements": {
        "server farm a": {
          "latency": 233  // Milliseconds
        }
      },

      // Subscriptions for shoulder taps on session changes. The "profile" indicates which session changes to tap as well as other properties of the registration like the min time between taps.
      // The subscription is named with a title-generated GUID that is also sent back with the tap as a context ID.
      // Subscriptions can be added and removed individually, without affecting other subscriptions in the "subscriptions" object.
      // To remove a subscription, set its context ID to null.
      // (Like the "ready" and "active" flags, the "subscriptions" data is copied out and maintained internally, so the normal replace-all rule on system fields doesn't apply to "subscriptions".)
      // Can't be set on large sessions.
      "subscriptions": {
        "961dc162-3a8c-4982-b58b-0347ed086bc9": {
          "profile": "party",  // Or "matchmaking", "initialization", "roster", "queuehost", or "queue"
          "onBehalfOfTitleId": "3948320593",  // Optional decimal title ID of the registered channel.  If not set the title ID is taken from the token.
        },
        "709fef70-4638-4b94-905b-24cb02706eb5": null
      }
    }
```

#### <a name="server-elements"></a>服务器元素

服务器是已加入或已被邀请加入会话的非用户。 关联的 JSON 对象是 /servers/{server-name}/constants/system 和 /servers/{server-name}/properties/system。 这些对象只能由服务器写入。

| 注意                                                         |
|---------------------------------------------------------------------------|
| 当前未使用 /servers/{server-name}/constants/system 对象。 |


### <a name="session-configuration"></a>会话配置

有两种方法来控制会话配置：

-   使用通过 XDP 或合作伙伴中心引入的会话模板。
-   使用多人游戏和匹配 WinRT API 或 REST API 调用。 你必须仍使用模板，但模板不必包含你想要配置的值。 请注意，你的作品无法覆盖模板中已设置的常量。

将提供单独的 JSON 文档来定义会话本身。 此外，开发人员还必须实现特定作品所需的任何 WinRT 包装器功能。 JSON 文档和任何 WinRT 包装器代码的内容必须彼此准确反映，并且必须反映最新的模板协定版本。

会话架构通过会话版本（主要版本）和协议版本（次要版本）版本化。 这些版本将合并到 X-Xbl-Contract-Version 标头中，显示为 "100 \* major + minor"。 例如，v1.7 作品在每个 REST 请求中包括以下标头，假设最新模板协定版本为 107：X-Xbl-Contract-Version: 107。

| 注意                                                                                              |
|----------------------------------------------------------------------------------------------------------------|
| 建议大多数作品（使用 XSAPI）使用协定版本 105 和会话模板版本 107。 |


### <a name="session-templates"></a>会话模板

每个会话模板都是一个 JSON 文档，是服务配置的一部分，定义正在创建的会话的框架并为新会话提供常量。 有关详细信息，请参阅 [MPSD 会话模板](../service-configuration/session-templates.md)。

## <a name="session-capabilities"></a>会话功能

功能是 MPSD 会话中的常量，该会话配置 MPSD 应该对其应用的行为。 你最常使用 XDP 和合作伙伴中心来设置会话模板中的功能。 它们在 /constants/system/capabilities 对象中设置。 如果不需要功能，则使用空的功能对象。

| 注意                                                                                                       |
|-------------------------------------------------------------------------------------------------------------------------|
| 游戏几乎永远不会使用多人游戏 WinRT API 或匹配 WinRT API 更改或访问会话功能。 |

会话功能由 **MultiplayerSessionCapabilities 类**表示。 它们是指示可以支持的会话的布尔值：

-   连接性
-   游戏开始
-   大尺寸
-   活动成员需要的连接

**MultiplayerSessionConstants 类**定义以下有关会话功能的属性：

-   **CapabilitiesConnectivity**
-   **CapabilitiesGameplay**
-   **CapabilitiesLarge**

| 注意                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------|
| 如果作品定义动态会话功能，会话常量的相应属性将被设置为 true。 |

## <a name="session-size"></a>会话大小

MPSD 会话的大小由该会话中的成员数量确定。


### <a name="maximum-session-size"></a>最大会话大小

会话的最大大小是它能够容纳的会话成员的最大数。 它由 **MultiplayerSessionConstants.MaxMembersInSession 属性**表示。 最大成员大小在 /constants/system 对象中设置。

最大会话大小介于 1 到 100 个会话成员之间，如果创建时未设置则默认为 100。 如果所需大小超过 100，会话被称为“大”会话，并以特殊方式设置。

设置会话的最大大小可能会导致空位在某些断开连接场景中完整显示。 例如，如果玩家由于网络或电源故障而断开连接，延迟不会立即反映在会话中。 成员使用 [MPSD 更改通知处理和断开连接检测](multiplayer-session-directory.md)中介绍的断开连接检测功能设置为非活动状态。

相比之下，使用检测信号检测断开连接的对等网格通常能在两到三秒内得知断开连接，因此可以立即打开玩家空位。 但是，仲裁程序无法删除其他成员。

### <a name="large-sessions"></a>大型会话

大型 MPSD 会话最多可以有 1000 个成员，但它会禁用部分会话功能，如获取包含所有成员的列表。 会话的“大”由 **MultiplayerSessionCapabilities.Large 属性**表示。 此属性设置为 true 指示大型会话，而“大”功能在 /constants/system/capabilities 对象中指示。 有关详细信息，请参阅[会话功能]()。

## <a name="session-user-states"></a>会话用户状态



MPSD 将用户状态定义为已添加到会话中的用户的状态。 可能的用户状态由 **MultiplayerSessionStatus 枚举**定义。 用户在被添加到会话之前还被视为具有“有空”状态。

**MultiplayerSession.SetCurrentUserStatus 方法**可以用于更改会话用户的状态。 此更改可以通过在游戏会话 JSON 文档中正确设置 /members/{index}/properties/system 来针对 REST 进行。


### <a name="reserved-user-state"></a>保留的用户状态

当仲裁程序已选择某用户来填充会话内的空位时，该用户将被置于保留的用户状态。 在这种状态下，用户尚未正式接受会话邀请或未加入会话以开始与对等方连接。


### <a name="active-user-state"></a>活动用户状态

当用户处于活动状态时，作品已代表用户加入了会话，而且用户正在积极参与会话。 只要用户玩游戏，他或她将继续处于这一状态。

当作品首次启动时，它应查看用户是否已是某个会话的成员，通常通过检查会话状态来查看。 如果用户是会话成员，游戏则可以直接跳转到游戏中，并将任何加入的本地成员的用户状态设置为活动。

用户在会话中玩游戏时应会保持活动状态。 如果用户通过游戏内 UI 离开会话，用户应被从该会话中删除（使用 **MultiplayerSession.Leave 方法**）。 如果用户只是暂时离开游戏，当游戏受限时，游戏应在合理的时间长度内将用户保留为活动状态。 如果用户未在作品指定的时间段后返回，这时适合将用户更改为非活动状态。


### <a name="inactive-user-state"></a>非活动用户状态

在非活动状态下，用户当前未参与游戏，但在会话中仍有保存的空位。 换言之，用户是“不活动”的。

用户在会话中的非活动用户状态由该用户自己的主机负责设置。 仲裁程序无法执行此操作。 用户进入非活动状态的示例场景包括：

-   作品收到“正在挂起”事件。
-   用户处于非活动状态（无输入或控制器响应）的时间已达到作品定义的时长。 我们建议在多人竞技游戏中定义为两分钟。
-   作品处于受限制模式超过两分钟，或作品定义的时长。 此受限模式的超时时间是用户可能使用相关应用或与作品相关的其他体验离开作品的预期时长。
-   用户已从会话非正常断开。 请参阅 [MPSD 更改通知处理和断开连接检测](multiplayer-session-directory.md)。

如果作品启动且特定会话成员的用户状态设置为非活动，则表明作品已挂起或用户在会话中处于非活动状态的时间太长。 由于游戏正在重新启动，则指示用户想要继续进行他或她所属的游戏会话。 如果用户状态在作品启动后是活动的，此状态很可能是由于网络断开连接或另一个场景：作品无法在中断之前将用户设置为非活动状态。 在这两种情况下，你的作品都应尝试将用户与游戏及其他用户重新连接以继续游戏，或从会话中删除该用户。

### <a name="user-state-when-the-session-is-over"></a>会话结束时的用户状态

当会话结束时，游戏将停止。 游戏必须允许所有用户使用 **MultiplayerSession.Leave 方法**删除自己。 与用户关联的会话活动将在用户离开会话时自动清除。

## <a name="visibility-and-joinability"></a>可见性和可加入性

会话访问在 MPSD 级别通过两个设置控制：会话可见性和会话可加入性。 本主题中的可见性和可加入性建议适用于最常见的作品场景。 如果可能，游戏应遵循这些设置，并且应使用游戏内逻辑来进行有关新用户是否被准许进入会话的最终的权威决定。


### <a name="session-visibility"></a>会话可见性

会话可见性由会话创建时设置的常量表示。 它通常在会话模板中定义，并确定哪类用户具有对会话的读写访问权限。 会话可见性的可能值由 **MultiplayerSessionVisibility 枚举**定义。 JSON 文件中允许的可见性常量设置是“开放”、“可见”和“私人”。


#### <a name="recommended-game-session-visibility-open"></a>建议的游戏会话可见性为：开放

开放游戏会话不需要玩家保留，这简化了邀请流程。 仲裁程序不在邀请发送后将玩家保留在 MPSD 中，而只是在本地跟踪受邀请的玩家。 因此，玩家可以立即连接到仲裁程序，并确定他们是应该加入会话、被拒绝，还是应该等待（如果支持等待玩家）。 仲裁程序是最高权威，它会做出响应，并指示新成员留在会话中或离开。

使用开放游戏会话可见性需要受邀请的玩家启动游戏并在进行最终决定前连接到仲裁程序。 如果会话已满或邀请被拒绝，可以接受向用户显示一条错误消息。

若要建立与仲裁程序的连接，需要安全设备地址。 **MultiplayerSessionProperties.HostDeviceToken 属性**用于找出哪个会话成员是会话当前的仲裁程序，以及受邀请的玩家应该在连接时使用哪个安全设备地址。

### <a name="session-joinability"></a>会话可加入性

会话可加入性确定哪类用户可以加入会话。 它可以在会话期间动态设置。 会话可加入性的可能值为：

-   无（默认）- 对于可以加入会话的人没有限制。
-   本地 - 仅本地用户可以加入会话。
-   已关注 - 仅本地用户和其他会话成员关注的用户可以在不保留的情况下加入会话。

会话仲裁程序可以通过可加入性设置创建私人会话。 将可加入性设置为“本地”或“已关注”将限制对会话的访问，并使其成为私人会话。

此外，仲裁程序应跟踪会话可加入性，以便在需要时可以在主机级别拒绝较早的会话邀请。 例如，如果受邀请的玩家在会话已满前未到达并加入会话，仲裁程序可以指示正在加入的玩家会话已被锁定，他们需要自动离开会话。

## <a name="session-timeouts"></a>会话超时

会话可以由计时器和其他外部事件更改。 会话超时定义会话成员在自动进入非活动状态或被从会话中删除前可以处于特定状态的时长。 MPSD 还支持超时以管理会话生存期。

| 注意                                                                                                                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 超时设置在 /constants/system/timeouts 中或在托管初始化对象内针对模板协定版本 104/105 进行。 对于版本 107 或更高版本，设置在 /constants/system 中或在托管初始化对象内单独进行。 |

当计时器到期后，MPSD 不会自动更新会话，也不会在进行任何更改时通知仲裁程序。 会话和超时状态仅在发送读或写请求的前一刻更新。 立即更新可确保返回的数据是最新的。

| 注意                                                                                                          |
|----------------------------------------------------------------------------------------------------------------------------|
| 会话超时不堆叠，并且在更新时只有一个超时应用于每个会话成员的状态转换。 |


### <a name="currently-defined-timeouts"></a>当前定义的超时

本部分介绍当前由 MPSD 定义的超时。 所有超时均使用毫秒指定。 允许值 0，其表示立即超时。 没有值的超时被视为无限超时。 由于超时具有默认值，因此你应为无限超时明确指定 null。
#### <a name="evaluationtimeout"></a>evaluationTimeout

此超时指示会话成员作出评估决定和上传评估决定的时长。 如果未收到任何决定，则将决定计作失败。 此超时位于托管初始化对象中。


#### <a name="inactiveremovaltimeout"></a>inactiveRemovalTimeout

此超时为已加入会话但当前未参与游戏的会话成员设置。 默认情况下该成员将在 2 小时后被从会话中删除。

| 注意                                                                      |
|----------------------------------------------------------------------------------------|
| 此超时为模板协定版本 104/105 指定为非活动超时。 |

在许多情况下，我们建议将非活动状态超时设置为 0，导致被设置为非活动状态的用户被从会话中立即删除，相应的空位也会被清除。 这是大多数多人竞技游戏所需要的行为，这样，如果用户已进入或已到达非活动状态，新玩家可以快速补位。 对于合作或其他多人游戏设计，如果用户断开连接或一段时间内未参与游戏，你可能需要你的作品留给用户更多重新连接的时间。 请注意，没有哪一种解决方案能够适合所有设计场景。

#### <a name="jointimeout"></a>joinTimeout

此超时指示用户必须加入会话的毫秒数。 将删除保留的无法加入的用户。 此超时位于托管初始化对象中。


#### <a name="measurementtimeout"></a>measurementTimeout

此超时指示会话成员必须上传度量值的时长。 无法上传度量值的成员标记有失败原因“超时”。 此超时位于托管初始化对象中。

| 注意                                                                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 匹配期间，会强制执行针对 QoS 度量的 45 秒超时。 因此，我们建议匹配期间使用的度量值超时少于等于 30 秒。 |


#### <a name="readyremovaltimeout"></a>readyRemovalTimeout

此超时为已加入会话且正在尝试加入游戏的会话成员设置。 这通常意味着 shell 已代表作品加入了用户，且作品正在启动。 默认情况下，成员将被从会话中删除并在 3 分钟后进入非活动状态。

| 注意                                                          |
|----------------------------------------------------------------------------|
| 此超时为协定版本 104/105 指定为准备超时。 |


#### <a name="reservedremovaltimeout"></a>reservedRemovalTimeout

此超时为由其他人添加到会话但尚未加入会话的会话成员设置。 当超时到期时，保留被删除、成员被视为处于非活动状态。 默认值为 30 秒。

| 注意                                                             |
|-------------------------------------------------------------------------------|
| 此超时为协定版本 104/105 指定为保留超时。 |


#### <a name="sessionemptytimeout"></a>sessionEmptyTimeout

此超时指示会话在被删除时是变空后多少毫秒。 默认值为 0。

| 注意                                                                 |
|-----------------------------------------------------------------------------------|
| 此超时为协定版本 104/105 指定为 sessionEmpty 超时。 |


### <a name="session-timeout-example"></a>会话超时示例

1.  会话从四个玩家开始。
2.  两个玩家 A 和 B 由于电源故障断开连接。 他们在游戏中的状态仍保持为活动。
3.  其他两个玩家 C 和 D 使用 **MultiplayerSession.Leave 方法**正常退出。
4.  会话仍为打开状态，玩家 A 和 B 断开连接但仍处于活动状态。
5.  几天之后，玩家 A 返回并启动游戏。
6.  玩家 A 的游戏查找 A 所属的会话（执行读取），并从几天前查找孤立的会话。
7.  会话对仍在会话中的两个玩家（A 和 B）进行状态检查。
    1.  由于玩家 A 正在运行游戏，针对玩家 A 的状态检查成功，而且该玩家在匹配中的活动状态保持不变。
    2.  玩家 B 未运行游戏。 因此，对玩家 B 的状态检查失败，服务将玩家 B 的状态设置为非活动。 此时，玩家 B 的非活动超时开始。

8.  玩家 A 使用**离开**方法正常退出会话。
9.  玩家 B 的非活动超时到期，并在任何人下一次读或写时被从会话中删除。
10. 会话现在有零个成员，并被从服务中删除。

如果将示例会话的非活动超时设置为 0，玩家 B 将在步骤 7a 的状态检查后立即超时，并很可能由会话写入删除。 在此情况下，会话关闭，无需对会话执行额外的读取或写入。


## <a name="multiple-signed-in-users-on-a-single-console"></a>单个主机上多个登录的用户


当多个用户被登录到同一个主机时，有可能有些用户处于游戏会话中，而其他用户未在会话中或未在当前游戏中处于活动状态。 游戏邀请还可以针对多个用户接收和接受，这会影响游戏会话的成员身份。 作品需要将这些点视为能够正确处理所有会话成员身份场景。

在常见场景中，新玩家登录，在游戏中进入活动状态，并需要被添加到现有的游戏会话中。 与创建新游戏会话一样，游戏只应在游戏期间在合适时添加用户。

有多个登录的用户时，一个或多个用户还可以收到另一个游戏会话的邀请。 作品不需要通过任何特定方式处理这些场景。 会话状态和成员事件将游戏会话和用户成员身份的任何更新全部通知给游戏。

为了为联机会话处理多个登录的用户，作品为每个用户使用单独的 **XboxLiveContext 类**对象来为所有用户订阅“即时点击”。 游戏使用 **MultiplayerSession.ChangeNumber 属性**确定会话中的特定更改并忽略重复的“即时点击”。


## <a name="process-lifecycle-management"></a>进程周期管理


就像非多人游戏游戏，多人游戏会话中的游戏可能会遇到进程周期事件的游戏挂起和终止。 会话仲裁程序因此应定期保存会话状态。 如果仲裁程序挂起，游戏应该尝试仲裁程序迁移，并根据需要保存游戏状态，以便新仲裁程序可以还原会话状态。 然后有可能整个多人游戏会话被挂起并在稍后恢复，只要会话在 MPSD 中仍然有效。 只有一个指定的对等项（通常为游戏主机）应更新全球游戏的状态。


### <a name="storage-of-game-metadata"></a>游戏元数据的存储

游戏在 MPSD 会话中存储游戏元数据。 游戏元数据是显示会话数据并使游戏可以查找并加入游戏会话所需的信息。 游戏在自定义属性部分为会话成员存储玩家特定的元数据，例如，会话中的玩家颜色、玩家首选武器等。会话范围的元数据（例如，当前地图）存储在 MPSD 会话的全局自定义属性部分。


### <a name="storage-of-game-state"></a>游戏元状态的存储

游戏状态使用**游戏存储服务**存储在游戏托管存储 (TMS) 中。 使用此位置的存储允许作品迁移仲裁程序，无需担心权限。 请参阅[迁移仲裁程序](migrating-an-arbiter.md)。

| 注意                                                                                                               |
|---------------------------------------------------------------------------------------------------------------------------------|
| 游戏尝试将游戏状态保存到 TMS 的频率不应超过每 5 分钟一次，除非它正被挂起。 |

## <a name="cleanup-of-inactive-sessions"></a>清理非活动会话

如果将 sessionEmptyTimeout 设置为 0，MPSD 会话将在最后一个玩家离开会话时被自动删除。 若要了解如何防止未使用的会话在崩溃或断开连接后包含玩家，请参阅 [MPSD 更改通知处理和断开连接检测。](multiplayer-session-directory.md)。 对崩溃或断开连接后的未使用会话的不当处理可能导致游戏在为玩家查询会话时出现问题。

建议的清理非活动会话的方法是通过调用 **MultiplayerService.GetSessionsAsync 方法**然后评估会话来让游戏为特定用户查询所有会话。 在遇到过期会话时，游戏将为会话中的所有本地用户调用 **MultiplayerSession.Leave 方法**。 此调用最终会将成员计数降至 0，并清理会话。

## <a name="session-arbiter"></a>会话仲裁程序


某些多人游戏方法在游戏会话内应仅由一个客户端调用。 此客户端是参与会话的主机之一，称为仲裁程序或主机。 如果游戏中至少有一个会话成员，会话应使用仲裁程序来监控进行中的加入。


### <a name="setting-the-arbiter"></a>设置仲裁程序

在创建会话时，客户端指定一个主机作为仲裁程序。 请参阅[操作方法：为 MPSD 会话设置仲裁程序](multiplayer-how-tos.md)。


### <a name="saving-session-state"></a>保存会话状态

如**进程周期管理**中所讨论的，仲裁程序应定期保存会话状态。 新仲裁程序必须能够在作品迁移仲裁程序时还原会话状态。 有关详细信息，请参阅[迁移仲裁程序](multiplayer-how-tos.md)。


### <a name="managing-game-session-members-and-joins-in-progress"></a>管理游戏会话成员和进行中的加入

会话仲裁程序最重要的角色是管理正在进入要玩的游戏会话的用户。 这包括处理游戏邀请、通知等待玩家，以及处理退出游戏的玩家。


#### <a name="receiving-notifications"></a>接收通知

仲裁程序必须侦听想要使用 **RealTimeActivityService.MultiplayerSessionChanged 事件**加入游戏会话的新玩家。


#### <a name="finding-players-to-fill-empty-game-session-slots"></a>查找填充游戏会话空位的玩家

仲裁程序通过这些操作之一查找填充游戏会话空位的玩家：如果你的作品使用大厅会话或其他机制允许延迟加入，请使用该机制查找新会话成员。
-   创建另一个匹配票证会话。

另请参阅[操作方法：在匹配期间填充会话空位](multiplayer-how-tos.md)。


#### <a name="handling-invited-session-members"></a>处理受邀请的会话成员

仲裁程序必须监视受邀请的会话成员，并在对单个用户的邀请之间应用最短间隔。 另请参阅[操作方法：发送邀请游戏](multiplayer-how-tos.md)。

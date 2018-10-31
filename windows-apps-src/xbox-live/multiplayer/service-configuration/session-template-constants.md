---
title: 会话模板常量
author: KevinAsgari
description: 介绍 Xbox Live 多人游戏会话模板中定义的系统常量。
ms.assetid: d51b2f12-1c56-4261-8692-8f73459dc462
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 多人游戏, 会话模板
ms.localizationpriority: medium
ms.openlocfilehash: 17c7fa48ee637dbca20b67872113a8d43e04da84
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5834658"
---
# <a name="session-template-constants"></a>会话模板常量

下表以会话模板版本 107 为例，介绍了多人游戏会话模板的预定义元素。

## <a name="system"></a>系统

系统常量  | 描述 | 有效值 | 默认值
--|-- | -- | --
version | 会话模板的版本。 | 1 - n | 无
maxMembersCount | 多人游戏活动支持的会话成员空位总数。 | 对于普通会话，为 1 - 100；对于大型会话，为 101+ | 100
visibility | 会话的可见性状态，用于指示其他用户是否可以看见和/或加入会话。 | 专用、可见、公开 | 公开
inviteProtocol | 如果将此常量设置为“game”，则受邀者可以在受邀加入会话时收到一条 toast 通知。 | 游戏、锦标赛、聊天、群体游戏 | 无
reservedRemovalTimeout  | 成员保留的超时时间（以毫秒为单位）。 值为 0 表示立即超时。 如果值为 null，则视为无限期超时。 | 0 - n、null | 30000
inactiveRemovalTimeout  | 成员被视为处于非活动状态的超时时间（以毫秒为单位）。 值为 0 表示立即超时。 如果值为 null，则视为无限期超时。 | 0 - n、null | 0
readyRemovalTimeout | 成员被视为准备就绪的超时时间（以毫秒为单位）。 值为 0 表示立即超时。 如果值为 null，则视为无限期超时。 | 0 - n、null | 180000
sessionEmptyTimeout | 空会话的超时时间（以毫秒为单位）。 值为 0 表示立即超时。 如果值为 null，则视为无限期超时。 | 0 - n、null | 0
[**capabilities**](#capabilities) | 指定会话的功能。 请参阅下面的功能部分。 | 不适用 | 不适用
[**metrics**](#metrics) | 指定一组会话成员必须满足的由作品定义的服务质量要求，例如延迟和带宽速度。  | 不适用 | 不适用
[**memberInitialization**](#memberInitialization) | 指定在新成员加入会话时强制实施的超时和初始化要求。 请参阅下面的成员初始化部分。 | 不适用 | 不适用
[**peerToPeerRequirements**](#peerToPeerRequirements) | 指定对等网格连接的网络服务质量要求。 请参阅下面的对等要求部分。 |不适用 | 不适用
[**peerToHostRequirements**](#peerToHostRequirements) | 指定对等-主机连接的网络服务质量要求。 请参阅下面的对等-主机要求部分。 | 不适用 | 不适用
[**measurementServerAddresses**](#measurementserveraddresses) | 指定用于确定 QoS 度量值的潜在数据中心的集合。 请参阅下面的 measurementServerAddresses 部分。 | 不适用 | 不适用
[**cloudComputePackage**](#cloudComputePackage) | ? | 不适用 | 不适用
[**arbitration**](#arbitration) | 指定成员在锦标赛中提交仲裁结果的超时时间。 请参阅下面的 cloudComputePackage 部分。 | 不适用 | 不适用
[**broadcastViewerTitleIds**](#broadcastViewerTitleIds) | 指定应始终对会话具有读权限的作品 ID 的列表。 请参阅下面的 broadcastViewerTitleIds 部分。 | 不适用 | 不适用
[**ownershipPolicies**](#ownershipPolicies) | 指定与会话所有权相关的策略。 请参阅下面的 OwnershipPolicies 部分。 | 不适用 | 不适用


## <a name="capabilities"></a>功能
功能是可在会话模板中有选择地设置的布尔值。 如果不需要任何功能，则空的“capabilities”对象应位于模板中，以防止在创建会话时指定功能，除非作品需要动态会话功能。

功能 |  描述 | 有效值 | 默认值
-- | -- | -- | -- |
connectivity | 指示会话是否支持对等连接。 如果此值为 false，则会话无法启用任何指标，并且会话成员也无法设置其 SecureDeviceAddress。 不能在大型会话中设置。 | true、false | false
suppressPresenceActivityCheck | 如果为 true，关闭状态检查。 | true、false | false
gameplay | 指示会话是否表示实际游戏玩法（相对于设置/菜单时间），例如大厅或匹配会话。 如果为 true，则会话处于游戏玩法模式。 | true、false | false
large | 指示会话是否为大型会话（超过 100 名成员）。 不支持将大型会话用于多人游戏管理器。 | true、false | false
connectionRequiredForActiveMembers | 指示是否需要连接才能使成员处于活动状态。 | true、false | false
cloudCompute | 让客户端请求代表会话分配云计算实例。 | true、false | false
autoPopulateServerCandidates | 通过“serverMeasurements”自动计算和设置“serverConnectionStringCandidates”。 不能在大型会话中设置此功能。 | true、false | false
userAuthorizationStyle | 指示在没有强大作品标识的情况下会话是否支持从平台进行调用。 不能在大型会话中设置此功能。</br></br>将 `userAuthorizationStyle` 功能设置为 `true` 会默认将会话的 `readRestriction` 和 `joinRestriction` 设置为 `local` 而不是 `none`。 这意味着游戏必须使用搜索句柄或转移手柄来加入游戏会话。| true、false | false
crossplay | 指示会话支持电脑与 Xbox One 设备之间的跨平台游戏。 | true、false | false
broadcast | 指示会话代表广播。 会话名称必须是广播者的 xuid。 需要“大型”功能。 | true、false | false
team | 指示会话代表锦标赛团队。 不能在“大型”或“游戏玩法”会话中设置此功能。 | true、false | false
arbitration | 指示会话必须由添加“仲裁”服务器条目的服务主体创建。 不能在“大型”会话中设置，但是需要“游戏玩法”。 | true、false | false
hasOwners | 指示会话的安全策略基于某些成员是所有者。 | true、false | false
searchable | 指示会话可能是搜索句柄的目标会话。 设置“userAuthorizationStyle”功能时，如果未设置“hasOwners”功能，则无法设置“searchable”功能。 | true、false | false

例如：

```json
"capabilities": {
    "connectivity": true,  
    "suppressPresenceActivityCheck": true,
    "gameplay": true,
    "large": true,
    "connectionRequiredForActiveMembers": true,
    "cloudCompute": true,
    "autoPopulateServerCandidates": true,
    "userAuthorizationStyle": true,
    "crossPlay": true,  
    "broadcast": true,  
    "team": true,   
    "arbitration": true,   
    "hasOwners": true,   
    "searchable": true  
},
```

## <a name="metrics"></a>指标
如果未指定 `metrics` 属性，则这些属性默认为满足服务质量要求所需的值。  
如果指定了这些属性，则其值必须满足服务质量要求。
仅当会话设置有 `connectivity` 功能时，此元素才有效。

指标 | 描述 | 有效值 | 默认值
-- | -- | -- | --
latency | | true、false | see Description
bandwidthDown | | true、false | see Description
bandwidthUp | | true、false | see Description
custom | | true、false | see Description

例如：
```json
"metrics": {
    "latency": true,
    "bandwidthDown": true,
    "bandwidthUp": true,
    "custom": true
},
```

## <a name="memberinitialization"></a>memberInitialization
如果设置了 `memberInitialization` 对象，则会话需要客户端系统或作品在创建会话后和/或新成员加入会话时执行初始化操作。  
超时和初始化阶段由会话自动跟踪，包括 QoS 度量值（如果设置有）。  
对于设置了“initializationEpisode”的成员，这些超时值将覆盖会话的保留和准备超时值。  
不能在大型会话中指定。

元素  | 描述 | 有效值 | 默认值
-- | -- | -- | --
joinTimeout | 指示成员必须加入会话的毫秒数。 将删除保留的无法加入的用户。</br>**注意：** 默认持续时间足以执行作品的正常操作，但是，如果作品要在 MPSD 流期间进行调试，这可能会导入加入超时。 对于这些方案，替代并增加会话的这一默认值。| 0 - n | 10000
measurementTimeout | 指示会话成员必须上传度量值的毫秒数。 无法上传度量值的成员标记有失败原因“超时”。  | 0 - n | 30000
evaluationTimeout | 指示外部评估必须上传度量值的毫秒数。 | 0 -n | 5000
externalEvaluation | 如果为 true，则指示作品代码根据 QoS 度量值评估加入的用户。 多人游戏服务不执行任何 QoS 逻辑，并且应由作品推进初始化阶段。 作品通常不需要此项。 | true、false | false
membersNeededToStart | 启动会话所需的成员数（仅限初始化阶段为零的情况）。 | 1 - maxMembersCount | 1

例如：
```json
"memberInitialization": {
    "joinTimeout": 10000,  
    "measurementTimeout": 30000,  
    "evaluationTimeout": 5000,
    "externalEvaluation": false,
    "membersNeededToStart": 1
},
```


## <a name="peertopeerrequirements"></a>peerToPeerRequirements

对等网络要求 | 描述 | 默认值
-- | -- |--
latencyMaximum | 任意两个客户端之间的最大延迟（以毫秒为单位）。 | 250
bandwidthMinimum | 任意两个客户端之间的最小带宽（以 KB/秒为单位）。 | 10000

例如：
```json
"peerToPeerRequirements": {
    "latencyMaximum": 250,  
    "bandwidthMinimum": 10000
},
```


## <a name="peertohostrequirements"></a>peerToHostRequirements

对等-主机网络要求 | 描述 | 有效值 | 默认值
-- | -- | -- | --
latencyMaximum | 对等-主机连接的最大延迟（以毫秒为单位）。 | | 250
bandwidthDownMinimum | 信息从主机发送到对等端的最小带宽（以 KB/秒为单位）。 | | 100000
bandwidthUpMinimum | 信息从对等端发送到主机的最小带宽（以 KB/秒为单位）。 | | 1000
hostSelectionMetric | 指示使用哪个指标选择主机。 | 带宽增加、带宽减少、带宽和延迟 | 延迟

例如：
```json
"peerToHostRequirements": {
    "latencyMaximum": 250,
    "bandwidthDownMinimum": 100000,
    "bandwidthUpMinimum": 1000,  
    "hostSelectionMetric": "bandwidthup"
},
```

## <a name="measurementserveraddresses"></a>measurementServerAddresses
应评估的潜在服务器连接字符串集。 连接字符串必须采用小写形式。
不能在大型会话中指定。

连接字符串采用以下格式定义：

`"<server name>" : {deviceAddress}`

其中，设备地址描述如下：

服务器连接字符串 | 描述
-- | --
secureDeviceAddress | 服务器的 base-64 编码安全设备地址

例如：
```json
"measurementServerAddresses": {
    "server farm a": {
        "secureDeviceAddress": "r5Y="
    },
    "datacenter b": {
        "secureDeviceAddress": "rwY="
    }
},
```

## <a name="cloudcomputepackage"></a>cloudComputePackage
指定要分配的云计算程序包的属性。 需要设置 `cloudCompute` 功能。

云计算属性 | 描述
-- | -- | -- | --
titleId | 指示要分配的云计算程序包的作品 ID。
gsiSet | 指示要分配的云计算程序包的 GSI 集。
variant | 指示要分配的云计算程序包的变体。

例如：
```json
"cloudComputePackage": {
    "titleId": "4567",
    "gsiSet": "128ce92a-45d0-4319-8a7e-bd8e940114ec",
    "vaiant": "30ebca60-d96e-4629-930b-6957aa6bfbfa"
},
```

## <a name="arbitration"></a>arbitration
指定仲裁进程的超时时间。 需要设置 `arbitration` 功能。 仲裁开始时间在会话的 */servers/arbitration/constants/system/startTime* 元素中定义。

超时 | 描述 | 有效值 | 默认值
-- | -- | -- | --
forfeitTimeout | 指示从仲裁开始时间算起的时间（以毫秒为单位，待定） | 0 - n | 60000
arbitrationTimeout | 指示自仲裁开始时间算起仲裁结果超时的时间（以毫秒为单位）。该值不能小于 `forfeitTimeout` 值 | 0 - n | 300000

例如：
```json
"arbitration": {
    "forfeitTimeout": 60000,
    "arbitrationTimeout": 300000
},
```

## <a name="broadcastviewertitleids"></a>broadcastViewerTitleIds

指定应始终对广播会话具有读权限的作品的作品 ID 数组。

例如：
```json
"broadcastViewerTitleIds" : ["34567", "8910"],
```

## <a name="ownershippolicies"></a>ownershipPolicies
指定如何在最后一个所有者离开会话时处理该会话。 需要设置 `hasOwners` 功能。

所有权策略 | 描述 | 有效值 | 默认值
-- | -- | -- | --
Migration | 指示在最后一个所有者离开会话时发生的行为。 如果将迁移策略设置为“endsession”，则表示会话过期。 如果将迁移策略设置为“oldest”，则选择最早加入会话的成员，使其成为会话的新所有者。 | “oldest”、“endsession” | “endsession”

例如：
```json
"ownershipPolicies": {
     "migration": "oldest"
}
```

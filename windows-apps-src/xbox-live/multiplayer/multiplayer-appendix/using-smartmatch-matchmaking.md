---
title: 使用 SmartMatch 匹配
author: KevinAsgari
description: 了解如何使用 Xbox Live SmartMatch 在多人游戏中匹配玩家。
ms.assetid: 10b6413e-51d9-4fec-9110-5e258d291040
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 多人游戏, 匹配, smartmatch
ms.localizationpriority: medium
ms.openlocfilehash: 4594bd70c28729f38e0c0eaea7ea8ef7a905bbfe
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2018
ms.locfileid: "4563468"
---
# <a name="using-smartmatch-matchmaking"></a>使用 SmartMatch 匹配

以下流程图展示了 SmartMatch 匹配过程。

![](../../images/multiplayer/Multiplayer_2015_SmartMatch_Matchmaking.png)

## <a name="creating-a-match-ticket-session-and-a-match-ticket"></a>创建匹配票证会话和匹配票证

在开始匹配之前，匹配的“侦察兵”设置匹配票证会话以表示想要一起进入匹配的一组玩家。 该组中的所有用户使用 **MultiplayerSession.Join 方法（字符串、布尔值、布尔值）** 加入会话。

创建票证会话并使用玩家填充后，游戏将使用 **MatchmakingService.CreateMatchTicketAsync Method** 将会话提交到匹配服务。 此方法创建表示票证会话的匹配票证，并将票证会话中的 /servers/matchmaking/properties/system/status 字段更新为“正在搜索”。 有关更多信息，请参阅[如何：创建匹配票证](multiplayer-how-tos.md)。

匹配票证创建方法的响应是 **CreateMatchTicketResponse 类**对象。 响应包含匹配票证 ID，一个可用于通过删除票证取消匹配的 GUID。 响应还包含可用于设定用户期望的漏斗的平均等待时间。


## <a name="setting-matchmaking-attributes-on-the-session-and-players"></a>设置会话和玩家的匹配属性

当将会话提交到匹配时，游戏可以设置匹配服务用于将此会话与其他会话归为一组的属性。 可在票证级别或每个成员级别指定属性。


### <a name="setting-matchmaking-attributes-at-the-match-ticket-level"></a>在匹配票证级别设置匹配属性

游戏在 **MatchmakingService.CreateMatchTicketAsync** 方法的 *ticketAttributesJson* 参数中提交匹配票证级别的属性。 在票证级别上指定的属性替代了在每个成员级别上指定的相同属性。


### <a name="setting-matchmaking-attributes-at-the-per-member-level"></a>在每个成员级别设置匹配属性

游戏指定匹配票证会话内的各成员的每个成员属性。 通过使用“matchAttrs”的属性名称并调用 **MultiplayerSession.SetCurrentUserMemberCustomPropertyJson 方法**来对它们进行设置。 此调用将 /members/{index}/properties/custom/matchAttrs 字段中的属性置于票证会话中的每个玩家上。

匹配过程将"平展"每个成员每个为单个票证级别属性，具体取决于漏斗的 Xbox Live 配置中的属性指定的平展方法。 这可以在[XDP](https://xdp.xboxlive.com)或[Windows 开发人员中心](https://developer.microsoft.com/dashboard/windows/overview)上配置。


## <a name="making-the-match"></a>进行匹配

通过设置的票证会话和匹配票证，匹配服务将表示的票证会话与表示其他组的其他票证会话匹配，并创建或标识匹配目标会话。 此服务也会为目标会话中的匹配玩家创建预留，然后将票证会话标记为已匹配。 MPSD 通知游戏此次对票证会话的更改。

现在游戏必须接着采取步骤来初始化目标会话以确认是否已显示足够的玩家，并执行服务质量 (QoS) 检查以确保他们彼此可以成功连接。 如果初始化和/或 QoS 失败，游戏会标记票证会话用来重新提交到匹配，以便可以查找其他组。 有关此过程的更多信息，请参阅[目标会话初始化和 QoS](smartmatch-matchmaking.md)。

在匹配活动期间，会话中的 JSON 对象进行了如下更改：

-   /servers/matchmaking/properties/system/status 字段设置为“已找到”
-   /servers/matchmaking/properties/system/targetSessionRef 字段设置为目标会话
-   各票证会话的 /members/{index}/properties/custom/matchAttrs 字段已复制到 /members/{index}/constants/custom/matchmakingResult/playerAttrs 字段
-   对于每个玩家，将票证属性从匹配票证中的 ticketAttributes 字段复制到 /members/{index}/constants/custom/matchmakingResult/ticketAttrs 字段。


## <a name="maintaining-the-match-ticket"></a>保留匹配票证

匹配服务使用为会话创建匹配票证时的票证会话快照。 因此，如果任何玩家加入或离开票证会话，则游戏必须使用匹配服务，删除并重新创建匹配票证。


## <a name="reusing-the-game-session-as-a-match-ticket-session"></a>将游戏会话重新用作匹配票证会话

| 重要提示                                                                                                                                                                                                                       |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 请务必意识到将 *preserveSession* 设置为“始终”的两个会话将无法互相匹配，因为无法将它们合并。 游戏使用的匹配流程应该考虑到这种情况。 |

游戏可以将现有游戏会话重新用作匹配票证会话以查找更多玩家来加入已在进行中的游戏。 若要启用此功能，游戏必须通过调用将 *preserveSession* 参数设置为 **PreserveSessionMode.Always** 的 **CreateMatchTicketAsync** 来创建匹配票证。 然后，匹配服务可确保用于票证的现有会话在整个匹配过程中都会得到保留并成为生成的目标会话。


## <a name="deleting-the-match-ticket"></a>删除匹配票证

若要删除匹配票证，则游戏调用 **MatchmakingService.DeleteMatchTicketAsync 方法**。 票证的删除：

1.  停止匹配票证会话中的用户。
2.  将票证会话中的 /servers/matchmaking/properties/system/status 字段更新为“已取消”。


## <a name="performing-matchmaking-for-games-using-xbox-live-compute"></a>为使用 Xbox Live 计算的游戏执行匹配

以下是将用户匹配到基于 Xbox Live 计算的游戏中要执行的高级步骤。 类似的流程应适用于第三方托管的游戏。
1.  侦察兵创建票证会话以表示组。 此会话包含潜在的数据中心列表（位于 /constants/system/measurementServerAddresses 中的会话配置）。 如果数据中心列表为静态，则此会话配置来自会话模板，或者来自先从 Xbox Live 计算获取会话配置后，在创建会话时编写会话配置的客户端。 此会话还包含 targetSessionConstants/custom/gameServerPlatform 对象中的 gsiSetId、gameVariantId 和 maxAllowedPlayers 值。
2.  组中的所有其他客户端加入票证会话。
3.  所有小组成员从票证会话的 /constants/system 对象下载 measurementServerAddresses 值，对这些值执行 ping 操作，并将已排序的首选数据中心列表上传到会话，如 /members/{index}/properties/system/serverMeasurements 中所定义。

| 注意                                                                                                                                                                                                                                                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 游戏可以使用 **MultiplayerSession.SetMeasurementServerAddresses** 方法和 **MultiplayerSessionConstants.MeasurementServerAddressesJson 属性**设置并检索会话中的 measurementServerAddresses 值。 |

4.  侦察兵调用 **CreateMatchTicketAsync**，并将其传入对票证会话的引用。

| 注意                                                                                                                                                                                                         |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 如果票证会话对象具有不匹配的常量，则创建票证方法可能不会成功。 这可以通过将 MUST 规则添加到漏斗，阻止与具有不匹配常量的玩家匹配来避免。 |

如果 **MatchTicketDetailsResponse.PreserveSession 属性**被设置为“从不”，则匹配服务会将各成员的服务器度量复制到票证的内部表示。 它将票证的成员的服务器度量平展为用于票证的单个服务器度量集合，此集合作为“特殊”票证属性存储在票证的内部表示中。

如果将 **MatchTicketDetailsResponse.PreserveSession** 设置为“始终”，则不使用服务器度量。 相反，匹配服务将会话的 /properties/system/matchmaking/serverConnectionString 值复制到票证的内部表示（作为大小为 1 且零延迟的 serverMeasurements 集合）。

5.  匹配服务将票证会话与代表其他组的票证会话进行匹配（将服务器度量集合考虑在内）。 它将尝试匹配组与具有高度首选的相同数据中心的其他组。
6.  一旦发现匹配的组，匹配服务创建或标识目标会话并添加一起匹配的票证会话的所有玩家。 该服务为匹配的组将最终平展的服务器度量写入到 /properties/system/serverConnectionStringCandidates。 它为目标会话中新添加的各成员将最初提交的服务器度量写入到 /members/{index}/constants/system/matchmakingResult/serverMeasurements。
7.  如上所述，所有客户端对目标会话执行初始化。 但是，因为客户端将连接到 Xbox Live 计算，因此它们无法相互执行 QoS 以确认连接。
8.  部分或全部客户端调用 **GameServerPlatformService.AllocateClusterAsync 方法**。 第一个会触发分配，而其他的只会收到确认。 此方法从 MPSD 中获取目标会话，并基于目标会话中的数据中心首选项选择数据中心、负载和其他 Xbox Live 计算特定的知识。
9.  所有客户端都运行。


## <a name="see-also"></a>另请参阅

[SmartMatch 运行时操作](smartmatch-matchmaking.md)

[SmartMatch 匹配](smartmatch-matchmaking.md)

**Microsoft.Xbox.Services.Matchmaking 命名空间**

**Microsoft.Xbox.Services.Multiplayer 命名空间**

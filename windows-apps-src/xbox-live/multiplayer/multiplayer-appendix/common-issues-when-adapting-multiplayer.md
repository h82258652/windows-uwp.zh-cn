---
title: 多人游戏 2015 迁移的常见问题
author: KevinAsgari
description: 了解在调整多人游戏 2014 游戏以适应 2015 多人游戏时你可能遇到的常见问题。
ms.assetid: 206f8fe4-c7aa-44b8-923b-18f679d8439f
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0704fe3919a84df718c9c1fba44a4fff85ee02ca
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2018
ms.locfileid: "7162020"
---
# <a name="common-issues-when-adapting-your-multiplayer-2014-title-to-multiplayer-2015"></a>调整多人游戏 2014 游戏以适应多人游戏 2015 时的常见问题

本主题介绍在针对 2015 多人游戏调整 2014 多人游戏游戏时必须考虑的问题。


## <a name="configuration-changes-to-make-for-2015-multiplayer"></a>为 2015 多人游戏进行的配置更改

此部分介绍在为 2015 多人游戏配置会话和模板时需要知晓的各项更改。 有关所述的特定项目的示例，请参阅 [MPSD 会话模板](multiplayer-session-directory.md)。

### <a name="add-a-capability-for-active-member-connection"></a>添加活动成员连接功能

connectionRequiredForActiveMembers 功能支持 2015 多人游戏的断开连接检测和会话更改订阅功能。 请将此功能添加到所有会话模板的 /constants/system/capabilities 对象。


### <a name="add-a-system-constant-for-invite-protocol"></a>添加邀请协议的系统常量

inviteProtocol 系统常量让邀请接收者在发送者游戏调用 **MultiplayerService.SendInvitesAsync 方法**或 **SystemUI.ShowSendGameInvitesAsync 方法（IUser、IMultiplayerSessionReference、String）时可以接收 toast**。 请将此常量（设置为“游戏”）添加到游戏邀请玩家所加入会话的所有模板的 /constants/system 对象。


## <a name="runtime-considerations-for-2015-multiplayer"></a>2015 多人游戏的运行时注意事项

2015 多人游戏系列游戏必须：在进入游戏代码的多人游戏区域前始终调用 **MultiplayerService.EnableMultiplayerSubscriptions Method**。 此调用同时支持会话更改订阅和断开连接检测订阅。
-   请确保为同一个用户的所有调用使用相同的 **XboxLiveContext 类**对象。 上下文包含与用于多人游戏订阅和断开连接检测的连接的管理相关的状态。
-   如果有多个本地用户，请为每个用户使用一个单独的 **XboxLiveContext** 对象。


## <a name="migrating-a-session-template-from-contract-version-104105-to-107"></a>将会话模板从协定版本 104/105 迁移到 107

最新的会话模板协定版本是 107，其对用于 MPSD 的架构进行了一些更改。 如果你为会话模板协定版本 104/105 编写的模板被重新发布以使用版本 107，那么必须更新这些模板。 本主题总结了对将模板迁移到最新版本的过程进行的更改。 模板本身在 [MPSD 会话模板](multiplayer-session-directory.md)中加以介绍。

| 重要提示                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 通过模板设置的功能不能通过写入到 MPSD 更改。 若要更改这些值，你必须创建并提交包含必要更改的新模板。 未通过模板设置的任何项目均可以通过写入到 MPSD 更改。 |


### <a name="changes-to-the-constantssystemmanagedinitialization-object"></a>对 /constants/system/managedInitialization 对象的更改

/constants/system/managedInitialization 对象已被重命名为 /constants/system/memberInitialization。 以下是对此对象的名称/值对进行的更改：autoEvaluate 被重命名为 externalEvaluation。 它的极性将更改，除非默认值仍为 false。
-   membersNeededToStart 的默认值从 2 更改为 1。
-   joinTimeout 的默认值从 5 秒更改为 10 秒。
-   measurementTimeout 的默认值从 5 秒更改为 30 秒。


### <a name="changes-to-the-constantssystemtimeouts-object"></a>对 /constants/system/timeouts 对象的更改

/constants/system/timeouts 对象已被删除，超时被重命名并被重定位在 /constants/system 下。 以下是对此对象的名称/值对进行的更改：保留超时变为 reservedRemovalTimeout。
-   非活动超时变为 inactiveRemovalTimeout。 其新的默认值为 0（小时）。
-   准备超时变为 readyRemovalTimeout。
-   sessionEmpty 超时变为 sessionEmptyTimeout。

超时的详细信息在[会话超时](mpsd-session-details.md)中提供。


### <a name="change-to-the-game-play-capability"></a>对游戏功能的更改

游戏功能支持最近的玩家和信誉报告。 你现在必须在代表实际游戏执行的会话中将 /constants/system/capabilities/gameplay 字段指定为 true，这与帮助程序会话（例如，匹配会话和大厅会话）相反。


## <a name="see-also"></a>另请参阅

[MPSD 会话模板](mpsd-session-details.md)

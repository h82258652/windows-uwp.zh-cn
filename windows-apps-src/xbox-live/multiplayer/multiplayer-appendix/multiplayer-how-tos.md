---
title: 多人游戏操作指南
author: KevinAsgari
description: 介绍如何在 Xbox Live 多人游戏 2015 中实现常见任务。
ms.assetid: 99c5b7c4-018c-4f7a-b2c9-0deed0e34097
ms.author: kevinasg
ms.date: 08/29/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 多人游戏 2015
ms.localizationpriority: medium
ms.openlocfilehash: c9b28c0f587e28a7f46ad5810e8af62b30e5577c
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5880693"
---
# <a name="multiplayer-how-tos"></a>多人游戏操作指南

本主题包含有关如何实现与使用多人游戏 2015 相关的特定任务的信息。

* 订阅 MPSD 会话更改通知
* 创建 MPSD 会话
* 为 MPSD 会话设置仲裁程序
* 管理作品激活
* 让用户可加入
* 发送游戏邀请
* 从大厅会话加入游戏会话
* 从作品激活加入 MPSD 会话
* 设置用户的当前活动
* 更新 MPSD 会话
* 离开 MPSD 会话
* 在匹配期间填充会话空位
* 创建匹配票证
* 获取匹配票证状态

## <a name="subscribe-for-mpsd-session-change-notifications"></a>订阅 MPSD 会话更改通知

| 注意                                                                                                                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 订阅会话更改需要关联的玩家在会话中处于活动状态。 此外，connectionRequiredForActiveMembers 字段还必须在会话的 /constants/system/capabilities 对象中设置为 true。 此字段通常在会话模板中设置。 请参阅 [MPSD 会话模板](multiplayer-session-directory.md)。 |



若要接收 MPSD 会话更改通知，作品可以按照以下过程进行操作。

1.  为同一个用户的所有调用使用相同的 **XboxLiveContext 类**对象。 将订阅绑定到这个对象的生存期。 如果有多个本地用户，请为每个用户使用一个单独的 **XboxLiveContext** 对象。
2.  为 **RealTimeActivityService.MultiplayerSessionChanged 事件**和 **RealTimeActivityService.MultiplayerSubscriptionsLost 事件**实现事件处理程序。
3.  如果为多个用户订阅更改，请将代码添加到你的 **MultiplayerSessionChanged** 事件处理程序以避免不必要的工作。 使用 **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 属性**和 **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 属性**。 使用这些属性可以跟踪看到的最新更改并忽略较旧的更改。
4.  调用 **MultiplayerService.EnableMultiplayerSubscriptions 方法**以允许订阅。
5.  创建本地会话对象，并以活动状态加入该会话。
6.  为每个用户调用 **MultiplayerSession.SetSessionChangeSubscription 方法**，传递要接收通知的会话更改类型。
7.  现在将会话写入 MPSD，如**操作方法：更新多人游戏会话**中所述。

以下流程图说明如何通过订阅此过程中所述的事件开始多人游戏。

![](../../images/multiplayer/Multiplayer_2015_Start_Multiplayer.png)


### <a name="parsing-duplicate-session-change-notifications"></a>分析重复会话更改通知

当有多个用户订阅了同一个会话的通知时，对该会话的每项更改都将为每个用户触发即时点击。 除一个通知以外的所有通知都将是重复的。 虽然仍建议作品为会话中的每个用户订阅通知，作品仍应该忽略已经发出通知的任何更改；你可以使用 Branch 和 ChangeNumber 属性来实现此目的。

若要检测多个即时点击，作品应该：

-   对于看到的每个 **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 属性**值，请存储最新的 **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 属性**。
-   如果即时点击所有的 **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 属性**比最后一次看到的 **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 属性**的该属性更高，请对其进行处理，并更新最新的 **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 属性**。
-   如果即时点击没有该 **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 属性**的更高的 **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 属性**，请跳过。 此更改已经被处理。

| 注意                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 属性**值需要由 **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 属性**跟踪，而不是由会话。 **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 属性**值有可能在会话生存期内更改（**RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 属性**有可能重置）。 |

## <a name="create-an-mpsd-session"></a>创建 MPSD 会话


| 注意                                                                                                                                                                                                                           |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 默认情况下，MPSD 会话在第一个成员加入时创建。 如果你的作品希望作品在加入时存在或不存在，它可以在更新期间将适当的写入模式值传递到写入方法中。 |



作品必须执行以下步骤来创建新会话：

1.  创建新的 **XboxLiveContext Class** 对象。 你的作品创建此对象一次、存储它，并根据需要在整个源代码中重新使用它。 特别是在处理会话订阅时，有必要使用完全相同的上下文。
2.  创建新的 **MultiplayerSession 类**对象以准备 MPSD 创建新会话所需的所有会话数据。
3.  在将会话写入 MPSD 前进行所需的更改。 例如，当通过调用 **MultiplayerSession.Join 方法**将成员加入会话时，客户端会添加告知 MPSD 在调用后加入的隐藏的本地请求数据，以更新会话。
4.  在完成后本地更改后，将这些更改写入 MPSD，如**操作方法：更新多人游戏会话**中所述。
5.  从 MPSD 接收新的 **MultiplayerSession** 对象，其中的很多字段已经填充。
6.  使用新会话对象继续，并放弃旧副本，其中包含了创建新会话的隐藏请求。

### <a name="example"></a>示例

    void Example_MultiplayerService_CreateSession()
    {
      XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));

      // Values found in Xbox Developer Portal(XDP) or Windows Dev Center configuration
      MultiplayerSessionReference^ multiplayerSessionReference = ref new MultiplayerSessionReference(
        "c83c597b-7377-4886-99e3-2b5818fa5e4f", // serviceConfigurationId
        "team-deathmatch", // sessionTemplateName
        "mySession" // sessionName
        );

      MultiplayerSession^ multiplayerSession = ref new MultiplayerSession(
        xboxLiveContext,
        multiplayerSessionReference
        );

      auto asyncOp = xboxLiveContext->MultiplayerService->WriteSessionAsync(
        multiplayerSession,
        MultiplayerSessionWriteMode::CreateNew
        );

      create_task(asyncOp)
      .then([](MultiplayerSession^ newMultiplayerSession)
      {
        // Throw away stale multiplayerSession, and use newMultiplayerSession from now on
      });
    }


## <a name="set-an-arbiter-for-an-mpsd-session"></a>为 MPSD 会话设置仲裁程序




作品使用以下过程为已创建的会话设置仲裁程序。

| 注意                                                                                                                                       |
|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| 成员（潜在主机）的设备令牌在成员加入会话并提供其安全设备地址前不可用。 |

1.  使用 **MultiplayerSession.Members 属性**从 MPSD 检索候选主机的设备令牌。

| 注意                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | 如果会话是通过 SmartMatch 匹配创建的，你的客户端可以通过 **MultiplayerSession.HostCandidates 属性**使用 MPSD 提供的候选主机。 |

2.  从候选主机列表中选择所需的主机。
3.  调用 **MultiplayerSession.SetHostDeviceToken 方法**在 MPSD 的本地缓存中设置设备令牌。 如果设置主机设备令牌的调用成功，本地设备令牌将替换主机令牌。
4.  如果在尝试设置主机设备令牌时收到 HTTP/412 状态代码，请查询会话数据，并查看主机设备令牌是否用于本地主机。 如果不是用于本地主机，则表明另一台主机已被指定为仲裁程序。

| 注意                                                                                                                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 你的客户端应将 HTTP/412 状态代码与其他 HTTP 代码分开单独处理，因为 HTTP/412 不指示标准故障。 有关此状态代码的详细信息，请参阅[多人游戏会话状态代码](multiplayer-session-status-codes.md)。 |

5.  在 MPSD 中更新会话，如**操作方法：更新多人游戏会话**中所述。

| 注意                                                                                                                                                                                                                                           |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 如果你没有更好的算法，客户端可以实现贪婪算法，在此算法中，如果尚未设置主机，每个候选主机都将尝试将自己设置为主机。 有关详细信息，请参阅[会话仲裁程序](mpsd-session-details.md)。 |

## <a name="manage-title-activation"></a>管理作品激活

Xbox One 在协议激活期间触发 **CoreApplicationView.Activated 事件**。 在多人游戏 API 的上下文中，此事件在用户接受邀请或加入另一个用户时触发。 这些操作通过将正在加入的用户引入包含目标用户的游戏来触发游戏必须作出响应的激活。

| 注意                                                                                       |
|---------------------------------------------------------------------------------------------------------|
| 你的作品在任何时候都应获得新的激活参数，并且永远不应按照长度编码。 |

作品必须执行以下主要步骤来处理作品激活。

1.  为 **CoreApplicationView.Activated 事件**设置事件处理程序。 此处理程序在每次发生协议激活时触发，即使作品已经在运行。
2.  在作品激活时，开始会话并订阅会话更改通知。 请参阅**操作方法：订阅 MPSD 会话更改通知**。
3.  将用户以活动状态加入会话。 请参阅**操作方法：从作品激活加入 MPSD 会话**。
4.  将大厅会话设置为活动会话，通过个人资料 UI 显示。 请参阅**操作方法：设置用户的当前活动**。
5.  将用户以活动状态加入游戏会话。 现在，用户可以连接到对等方，并进入游戏或大厅。

以下流程图说明如何处理作品激活。

![](../../images/multiplayer/Multiplayer_2015_OnActivation.png)

## <a name="make-the-user-joinable"></a>让用户可加入

若要让用户可以加入，作品必须执行以下操作：

1.  创建会话对象，并根据需要修改属性。
2.  将用户以活动状态加入会话。 请参阅**操作方法：从作品激活加入 MPSD 会话**。
3.  确定用户是否已被指定为会话仲裁程序。
4.  如果用户不是仲裁程序，请转到步骤 7。
5.  如果用户是仲裁程序，调用 **MultiplayerSession.SetHostDeviceToken 方法**。
6.  尝试使用 **MultiplayerService.TryWriteSessionAsync 方法**的调用写入会话。
7.  将会话设置为活动会话。 请参阅**操作方法：设置用户的当前活动**。

以下流程图说明为让用户可以在游戏期间由其他玩家加入所采取的步骤。

![](../../images/multiplayer/Multiplayer_2015_Become_Joinable.png)

## <a name="send-game-invites"></a>发送游戏邀请

游戏可以支持玩家通过以下方式发送游戏邀请：

-   为大厅会话发送邀请。
-   使用具有游戏会话引用的通用 Xbox 平台发送邀请。

若要为玩家发送游戏邀请，游戏必须执行以下操作：

1.  让邀请游戏玩家可以加入。 请参阅**操作方法：让用户可加入**。
2.  确定是通过大厅会话还是使用邀请 UI 发送邀请。
3.  如果使用大厅会话，则使用 **MultiplayerService.SendInvitesAsync 方法**的调用发送邀请。 此方法可能需要使用 **SystemUI.ShowPeoplePickerAsync 方法**或 **PartyChat.GetPartyChatViewAsync 方法**构建游戏内 UI 名单。
4.  如果使用邀请 UI，则调用 **SystemUI.ShowSendGameInvitesAsync 方法**来显示邀请 UI。
5.  在远程玩家加入后为本地玩家处理 **RealTimeActivityService.MultiplayerSessionChanged 事件**。
6.  对于远程玩家，应实现游戏激活代码。 请参阅**操作方法：管理作品激活**。

以下流程图说明如何发送邀请。

![](../../images/multiplayer/Multiplayer_2015_Send_Invites.png)

## <a name="join-a-game-session-from-a-lobby-session"></a>从大厅会话加入游戏会话

如果 Windows 10 设备上的游戏会话不是大型会话，则必须将 `userAuthorizationStyle` 功能设置为 **true**。 这意味着 `joinRestriction` 属性不能为“无”，即会话无法直接公开加入。

常用方案是创建大厅会话来召集玩家，然后将这些玩家移至游戏会话或匹配会话。 但如果游戏会话不能公开加入，那么游戏客户端将无法加入游戏会话，除非它们符合要求的 `joinRestriction` 设置，这在大多数情况下都对此方案限制过多。

解决办法是使用转移句柄来链接大厅会话和游戏会话。  作品可以按照以下方法来执行此操作：

1. 在创建游戏会话时，使用 `multiplayer_service::set_transfer_handle(gameSessionRef, lobbySessionRef)` API 来创建链接大厅会话和游戏会话的转移句柄。
2. 在大厅会话而不是游戏会话的会话引用中存储转移句柄 GUID。
3. 当游戏想要将成员从大厅会话移到游戏会话时，每个客户端将使用 `multiplayer_service::write_session_by_handle(multiplayerSession, multiplayerSessionWriteMode, handleId)` API 利用来自大厅会话的转移句柄来加入游戏。
4. MPSD 将查找大厅会话以验证尝试使用转移句柄加入游戏会话的成员也在大厅会话中。
5. 如果成员在大厅会话中，他们将可以访问游戏会话。

## <a name="join-an-mpsd-session-from-a-title-activation"></a>从作品激活加入 MPSD 会话

当用户选择使用 Xbox shell UI 加入好友的活动或接受邀请时，使用指示用户想要加入的会话的参数激活作品。 作品必须处理此激活并将用户添加到相应的会话中。

下面是作品应遵循的步骤：

1.  为 **CoreApplicationView.Activated 事件**实现事件处理程序。 此事件通知作品的激活。
2.  当处理程序触发时，检查 **IActivatedEventArgs.Kind 属性**。 如果它被设置为“协议”，则将此事件参数转换为 **ProtocolActivatedEventArgs 类**。
3.  检查 **ProtocolActivatedEventArgs** 对象。 如果 **ProtocolActivatedEventArgs.Uri 属性**中指示的 URI 匹配 inviteHandleAccept（对应接受的邀请）或 activityHandleJoin（对应通过 shell UI 的加入），则分析 URI 的查询字符串，其格式被设置为包含键/值对的普通 URI 查询字符串，并提取以下字段：
    -   对于接受的邀请：
        1.  句柄
        2.  invitedXuid
        3.  senderXuid
    -   对于加入：
        1.  句柄
        2.  joinerXuid
        3.  joineeXuid

4.  启动作品的多人游戏代码，这应该包括调用 **MultiplayerService.EnableMultiplayerSubscriptions 方法**。
5.  使用 **MultiplayerSession 构造函数 (XboxLiveContext)** 创建本地 **MultiplayerSession 类**对象。
6.  调用 **MultiplayerSession.Join 方法（字符串、布尔值、布尔值）** 来加入会话。 请使用以下参数设置，以便将加入设置为活动：
    -   *memberCustomConstantsJson* = null
    -   *initializeRequested* = false
    -   *joinWithActiveStatus* = true

7.  调用 **MultiplayerSession.SetSessionChangeSubscription 方法**以当会话在加入后更改时接收即时点击。
8.  调用 **MultiplayerService.WriteSessionByHandleAsync 方法**，使用按照步骤 3 中所述所获得的句柄。 现在，用户是会话的成员了，可以使用会话中的数据连接到游戏。

## <a name="set-the-users-current-activity"></a>设置用户的当前活动

用户的当前活动显示在游戏的 Xbox 仪表板用户体验中。 用户的活动可以通过会话或通过作品激活设置。 在后面的情况下，用户通过匹配或通过开始游戏来进入会话。

| 注意                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 通过会话设置的活动可以使用 **MultiplayerService.ClearActivityAsync 方法**的调用删除。 |

若要将会话设置为用户的当前活动，游戏调用 **MultiplayerService.SetActivityAsync 方法**，传递会话的会话引用。

若要通过作品激活设置用户的当前活动，请参阅**操作方法：从作品激活加入 MPSD 会话**。

## <a name="update-an-mpsd-session"></a>更新 MPSD 会话

| 注意                                                                                                                                                 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 当你的作品使用多人游戏 API 更新现有会话时，请记住，在进行写入会话调用前，它处理的一直是本地副本。 |

若要更新现有会话，作品必须：

1.  根据需要对当前会话进行更改，例如，通过调用 **MultiplayerSession.Leave 方法**。
2.  当进行所有更改时，将本地更改写入 MPSD，使用下列任一方法：

    -   **MultiplayerService.WriteSessionAsync 方法**
    -   **MultiplayerService.WriteSessionByHandleAsync 方法**。
    -   **MultiplayerService.TryWriteSessionAsync 方法**
    -   **MultiplayerService.TryWriteSessionByHandleAsync 方法**

    如果写入到其他作品也可以修改的共享部分，请将写入模式设置为 **SynchronizedUpdate**。 请参阅[会话更新的同步](multiplayer-session-directory.md)了解详细信息。

    写入方法将加入写入服务器并获取最新会话，从此最新会话发现其他会话成员及其主机的安全设备地址 (SDA)。 有关在这些主机之间建立网络连接的详细信息，请参阅 **Xbox One 上的 Winsock 简介**。

3.  放弃旧的本地会话对象，使用新检索到的会话对象，以便基于最新的已知会话状态决定未来的操作。

## <a name="leave-an-mpsd-session"></a>离开 MPSD 会话

若要允许用户离开会话，作品必须执行以下操作。

1.  为游戏会话调用 **MultiplayerSession.Leave 方法**。
2.  在 MPSD 中更新游戏会话，如**操作方法：更新多人游戏会话**中所述。
3.  如有必要，为大厅会话调用 **Leave** 方法，并更新该会话。
4.  如果对于大厅会话是必要的，通过取消注册 **RealTimeActivityService.MultiplayerSubscriptionsLost 事件**和 **RealTimeActivityService.MultiplayerSessionChanged 事件**关闭多人游戏 API。

以下流程图说明如何离开会话并关闭。

![](../../images/multiplayer/Multiplayer_2015_Shut_Down.png)

## <a name="fill-open-session-slots-during-matchmaking"></a>在匹配期间填充会话空位

若要在匹配期间填充票证会话的空位，作品必须按照以下类似步骤操作：

1.  访问匹配期间创建的票证会话的最新会话状态。
2.  从大厅会话为游戏添加可用玩家。
3.  确定票证会话是否已满。
4.  如果会话已满，继续游戏。
5.  如果会话未满，创建匹配票证，如**操作方法：创建匹配票证**中所述。 请务必创建将 *preserveSession* 参数设置为“始终”的票证。
6.  继续匹配。 请参阅[使用 SmartMatch 匹配](using-smartmatch-matchmaking.md)。

以下流程图说明如何在匹配期间填充会话空位。

![](../../images/multiplayer/Multiplayer_2015_Fill_Open_Slots.png)

## <a name="create-a-match-ticket"></a>创建匹配票证

若要创建匹配票证，匹配侦察兵必须：

1.  调用 **MatchmakingService.CreateMatchTicketAsync 方法**，传入对票证会话的引用。 此方法从 MPSD 读取票证会话，并在会话中开始用户匹配。 此方法内部调用 **POST (/serviceconfigs/{scid}/hoppers/{hoppername})**。
2.  如果匹配服务将会话成员匹配到新会话或另一个现有会话中，请将 *preserveSession* 参数设置为“从不”。 将 *preserveSession* 参数设置为“始终”以允许游戏重新使用现有游戏会话作为票证会话来继续游戏。 匹配服务然后便可以确保提交的会话将保留，且任何匹配的玩家将添加到该会话。

3.  使用 **CreateMatchTicketResponse 类**对象中返回的 **CreateMatchTicketResponse.EstimatedWaitTime 属性**设置匹配时间的用户期望。
4.  如果需要，通过删除票证，使用响应对象中返回的 **CreateMatchTicketResponse.MatchTicketId 属性**取消会话的匹配。 票证删除使用 **MatchmakingService.DeleteMatchTicketAsync 方法**。

## <a name="get-match-ticket-status"></a>获取匹配票证状态

你的作品应该执行以下操作来检索匹配票证状态：

1.  获取票证会话的 **MultiplayerSession 类**对象。
2.  使用 **MultiplayerSession.MatchmakingServer 属性**访问匹配中使用的 **MatchmakingServer 类**对象。
3.  如果已找到匹配，检查 **MatchmakingServer** 对象以确定匹配过程的状态、会话的一般等待时间，以及目标会话引用。

---
title: Multiplayer how-tos
author: KevinAsgari
description: Describes how to implement common tasks in Xbox Live Multiplayer 2015.
ms.assetid: 99c5b7c4-018c-4f7a-b2c9-0deed0e34097
ms.author: kevinasg
ms.date: 08-29-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, games, uwp, windows 10, xbox one, multiplayer 2015
ms.openlocfilehash: 932a61bbdf3dc6e1bd3584f1487fd8341b98df20
ms.sourcegitcommit: bf5cbc3c1fda6ba2dab2a198ede7b9b1b54583e9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/11/2017
---
# <a name="multiplayer-how-tos"></a>Multiplayer how-to's

本主题包含有关如何实现与使用多人游戏 2015 相关的特定任务的信息。

* 订阅 MPSD 会话更改通知
* 创建 MPSD 会话
* 为 MPSD 会话设置仲裁程序
* Manage Title Activation
* 让用户可加入
* 发送游戏邀请
* 从大厅会话加入游戏会话
* 从游戏激活加入 MPSD 会话
* 设置用户的当前活动
* 更新 MPSD 会话
* 离开 MPSD 会话
* 在匹配期间填充会话空位
* 创建匹配票证
* 获取匹配票证状态

## <a name="subscribe-for-mpsd-session-change-notifications"></a>订阅 MPSD 会话更改通知

| 注意                                                                                                                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Subscribing for changes to a session requires the associated player to be active in the session. The connectionRequiredForActiveMembers field must also be set to true in the /constants/system/capabilities object for the session. This field is usually set in the session template. See [MPSD Session Templates](multiplayer-session-directory.md). |



To receive MPSD session change notifications, the title can follow the procedure below.

1.  Use the same **XboxLiveContext Class** object for all calls by the same user. Subscriptions are tied to the lifetime of this object. If there are multiple local users, use a separate **XboxLiveContext** object for each user.
2.  Implement event handlers for the **RealTimeActivityService.MultiplayerSessionChanged Event** and the **RealTimeActivityService.MultiplayerSubscriptionsLost Event**.
3.  If subscribing to changes for more than one user, add code to your **MultiplayerSessionChanged** event handler to avoid unnecessary work. Use the **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch Property** and the **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber Property**. Use of these properties allows tracking of the last change seen and ignoring of older changes.
4.  Call the **RealTimeActivityService.EnableMultiplayerSubscriptions Method** to allow subscriptions.
5.  Create a local session object and join that session as active.
6.  Make calls for each user to the **MultiplayerSession.SetSessionChangeSubscription Method**, passing the session change type for which to be notified.
7.  Now write the session to MPSD as described in **How to: Update a Multiplayer Session**.

以下流程图说明如何通过订阅此过程中所述的事件开始多人游戏。

![](../../images/multiplayer/Multiplayer_2015_Start_Multiplayer.png)


### <a name="parsing-duplicate-session-change-notifications"></a>分析重复会话更改通知

When there are multiple users subscribed to notifications for the same session, every change to that session will trigger a shoulder tap for each user. All but one of these will be duplicates. While it's still recommended that a title subscribe every user in a session to notifications, a title should ignore any changes that it's already been notified of; you can do this using the Branch and ChangeNumber properties.

To detect multiple shoulder taps, a title should:

-   For each **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch Property** value seen, store the latest **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber Property**.
-   If a shoulder tap has a higher **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber Property** than the last seen for that **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch Property**, process it and update the latest **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber Property**.
-   If a shoulder tap does not have a higher **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber Property** for that **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch Property**, skip it. That change has already been handled.

| Note                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber Property** values need to be tracked by **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch Property**, not by session. It's possible for the **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch Property** value to change (and the **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber Property** to reset) within the lifetime of a session. |

## <a name="create-an-mpsd-session"></a>创建 MPSD 会话


| 注意                                                                                                                                                                                                                           |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| By default, an MPSD session is created when the first member joins it. If your title logic expects the title to exist or not exist at join time, it can pass an appropriate write mode value to the write method during the session update. |



The title must do the following to create a new session:

1.  Create a new **XboxLiveContext Class** object. Your title creates this object once, stores it, and reuses it as required throughout the source code. Especially when working with session subscriptions, it is necessary to use exactly the same context.
2.  Create a new **MultiplayerSession Class** object to prepare all the session data that the MPSD needs to create a new session.
3.  Make required changes before writing the session to MPSD. For example, when joining a member to the session with a call to **MultiplayerSession.Join Method**, the client adds hidden local request data that tells MPSD to join upon the call to update the session.
4.  When finished making local changes, write them to MPSD as described in **How to: Update a Multiplayer Session**.
5.  Receive the new **MultiplayerSession** object from MPSD, with many fields filled in.
6.  Use the new session object going forward, and discard the old copy, which contains a hidden request to create a new session.

### <a name="example"></a>示例

    void Example_MultiplayerService_CreateSession()
    {
      XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));

      // These from XDP web portal
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




游戏使用以下过程为已创建的会话设置仲裁程序。

| Note                                                                                                                                       |
|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| Device tokens for the members (potential hosts) are not available until the members have joined the session and included their secure device addresses. |

1.  Retrieve the device tokens for host candidates from the MPSD by using the **MultiplayerSession.Members Property**.

| Note                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | If the session was created by SmartMatch matchmaking, your clients can use the host candidates available from MPSD through the **MultiplayerSession.HostCandidates Property**. |

2.  Select the required host from the list of host candidates.
3.  Call the **MultiplayerSession.SetHostDeviceToken Method** to set the device token in the local cache of the MPSD. If the call to set the host device token succeeds, the local device token replaces the host's token.
4.  If an HTTP/412 status code is received when trying to set the host device token, query the session data and see if the host device token is for the local console. If it is not for the local console, another console has been designated as the arbiter.

| Note                                                                                                                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Your client should handle the HTTP/412 status code separately from other HTTP codes, since HTTP/412 does not indicate a standard failure. For more about the status code, see [Multiplayer Session Status Codes](multiplayer-session-status-codes.md). |

5.  Update the session in MPSD, as described in **How to: Update a Multiplayer Session**.

| Note                                                                                                                                                                                                                                           |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| If you have no better algorithm, the client can implement a greedy algorithm in which each host candidate attempts to set itself as the host if nobody else has done it yet. 有关详细信息，请参阅[会话仲裁程序](mpsd-session-details.md)。 |

## <a name="manage-title-activation"></a>管理游戏激活

Xbox One 在协议激活期间触发 **CoreApplicationView.Activated 事件**。 In the context of the multiplayer API, this event is fired when a user accepts an invite or joins another user. These actions trigger an activation that the title must react to by bringing the joining user into game play with the target user.

| Note                                                                                       |
|---------------------------------------------------------------------------------------------------------|
| Your title should expect new activation arguments at any time and should never be coded against length. |

The title must perform the following main steps to handle title activation.

1.  Set up an event handler for the **CoreApplicationView.Activated Event**. This handler triggers every time protocol activation occurs, even if the title is already running.
2.  At title activation, begin a session and subscribe for session change notifications. See **How to: Subscribe for MPSD Session Change Notifications**.
3.  Join the user to the session as active. See **How to: Join an MPSD Session from a Title Activation**.
4.  Set the lobby session as the activity session, exposed through the profile UI. See **How to: Set the User's Current Activity**.
5.  Join the user to the game session as active. Now the user can connect to peers and enter game play or the lobby.

以下流程图说明如何处理游戏激活。

![](../../images/multiplayer/Multiplayer_2015_OnActivation.png)

## <a name="make-the-user-joinable"></a>让用户可加入

若要让用户可以加入，游戏必须执行以下操作：

1.  Create a session object, and modify the attributes as required.
2.  Join the user to the session as active. See **How to: Join an MPSD Session from a Title Activation**.
3.  Determine if the user has been designated as the session arbiter.
4.  If the user is not the arbiter, go to step 7.
5.  If the user is the arbiter, call the **MultiplayerSession.SetHostDeviceToken Method**.
6.  Attempt to write the session using a call to the **MultiplayerService.TryWriteSessionAsync Method**.
7.  Set the session as the active session. See **How to: Set the User's Current Activity**.

以下流程图说明为让用户可以在游戏期间由其他玩家加入所采取的步骤。

![](../../images/multiplayer/Multiplayer_2015_Become_Joinable.png)

## <a name="send-game-invites"></a>发送游戏邀请

游戏可以支持玩家通过以下方式发送游戏邀请：

-   Send the invites for the lobby session.
-   Send the invites using the generic Xbox platform invite UI with the game session reference.

To send game invites for a player, the title must do the following:

1.  Make the inviting game player joinable. See **How to: Make the User Joinable**.
2.  Determine if the invites are to be sent via the lobby session or using the invite UI.
3.  If using the lobby session, send the invites using a call to **MultiplayerService.SendInvitesAsync Method**. This method might require the building of an in-game UI roster using the **SystemUI.ShowPeoplePickerAsync Method** or the **PartyChat.GetPartyChatViewAsync Method**.
4.  If using the invite UI, call the **SystemUI.ShowSendGameInvitesAsync Method** to show the invite UI.
5.  Handle the **RealTimeActivityService.MultiplayerSessionChanged Event** for the local player after the remote player joins.
6.  For the remote player, implement title activation code. See **How to: Manage Title Activation**.

以下流程图说明如何发送邀请。

![](../../images/multiplayer/Multiplayer_2015_Send_Invites.png)

## <a name="join-a-game-session-from-a-lobby-session"></a>从大厅会话加入游戏会话

如果 Windows 10 设备上的游戏会话不是大型会话，则必须将 `userAuthorizationStyle` 功能设置为 **true**。 这意味着 `joinRestriction` 属性不能为“无”，即会话无法直接公开加入。

常用方案是创建大厅会话来召集玩家，然后将这些玩家移至游戏会话或匹配会话。 但如果游戏会话不能公开加入，那么游戏客户端将无法加入游戏会话，除非它们符合要求的 `joinRestriction` 设置，这在大多数情况下都对此方案限制过多。

解决办法是使用转移句柄来链接大厅会话和游戏会话。  游戏可以按照以下方法来执行此操作：

1. 在创建游戏会话时，使用 `multiplayer_service::set_transfer_handle(gameSessionRef, lobbySessionRef)` API 来创建链接大厅会话和游戏会话的转移句柄。
2. 在大厅会话而不是游戏会话的会话引用中存储转移句柄 GUID。
3. 当游戏想要将成员从大厅会话移到游戏会话时，每个客户端将使用 `multiplayer_service::write_session_by_handle(multiplayerSession, multiplayerSessionWriteMode, handleId)` API 利用来自大厅会话的转移句柄来加入游戏。
4. MPSD 将查找大厅会话以验证尝试使用转移句柄加入游戏会话的成员也在大厅会话中。
5. 如果成员在大厅会话中，他们将可以访问游戏会话。

## <a name="join-an-mpsd-session-from-a-title-activation"></a>从游戏激活加入 MPSD 会话

When a user chooses to join a friend's activity or accept an invite using Xbox shell UI, the title is activated with parameters that indicate what session the user would like to join. The title must handle this activation and add the user to the corresponding session.

Here are the steps the title should follow:

1.  Implement an event handler for the **CoreApplicationView.Activated Event**. This event notifies of activations for the title.
2.  When the handler fires, examine the **IActivatedEventArgs.Kind Property**. If it is set to Protocol, cast the event arguments to **ProtocolActivatedEventArgs Class**.
3.  Examine the **ProtocolActivatedEventArgs** object. If the URI indicated in the **ProtocolActivatedEventArgs.Uri Property** matches either inviteHandleAccept (corresponding to an accepted invite) or activityHandleJoin (corresponding to a join via shell UI), parse the query string of the URI, which is formatted as a normal URI query string with key/value pairs, extracting the following fields:
    -   For an accepted invite:
        1.  handle
        2.  invitedXuid
        3.  senderXuid
    -   For a join:
        1.  handle
        2.  joinerXuid
        3.  joineeXuid

4.  Start the title's multiplayer code, which should include calling the **RealTimeActivityService.EnableMultiplayerSubscriptions Method**.
5.  Create a local **MultiplayerSession Class** object, using the **MultiplayerSession Constructor (XboxLiveContext)**.
6.  Call the **MultiplayerSession.Join Method (String, Boolean, Boolean)** to join the session. Use the following parameter settings so that the join is set as active:
    -   *memberCustomConstantsJson* = null
    -   *initializeRequested* = false
    -   *joinWithActiveStatus* = true

7.  Call the **MultiplayerSession.SetSessionChangeSubscription Method** to be shoulder-tapped when the session changes after joining.
8.  Call the **MultiplayerService.WriteSessionByHandleAsync Method**, using the handle acquired as described in step 3. 现在，用户是会话的成员了，可以使用会话中的数据连接到游戏。

## <a name="set-the-users-current-activity"></a>设置用户的当前活动

用户的当前活动显示在游戏的 Xbox 仪表板用户体验中。 Activity for a user can be set through a session or through title activation. In the latter case, the user enters a session through matchmaking or by starting a game.

| Note                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Activity set through a session can be deleted with a call to the **MultiplayerService.ClearActivityAsync Method**. |

To set a session as the user's current activity, the title calls the **MultiplayerService.SetActivityAsync Method**, passing the session reference for the session.

若要通过游戏激活设置用户的当前活动，请参阅**操作方法：从游戏激活加入 MPSD 会话**。

## <a name="update-an-mpsd-session"></a>更新 MPSD 会话

| 注意                                                                                                                                                 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| When your title updates an existing session using the multiplayer API, remember that it is working with a local copy, until it makes a call to write the session. |

To update an existing session, the title must:

1.  Make changes to the current session as required, for example, by calling the **MultiplayerSession.Leave Method**.
2.  When all changes are made, write the local changes to MPSD, using any of these methods:

    -   **MultiplayerService.WriteSessionAsync Method**
    -   **MultiplayerService.WriteSessionByHandleAsync Method**.
    -   **MultiplayerService.TryWriteSessionAsync Method**
    -   **MultiplayerService.TryWriteSessionByHandleAsync Method**

    Set the write mode to **SynchronizedUpdate** if writing to a shared portion that other titles can also modify. See [Synchronization of Session Updates](multiplayer-session-directory.md) for more information.

    The write method writes the join to the server and gets the latest session, from which to discover other session members and the secure device addresses (SDAs) of their consoles. For more information about establishing a network connection among these consoles, see **Introduction to Winsock on Xbox One**.

3.  Discard the old local session object, and use the newly retrieved session object so that future actions are based on the latest known session state.

## <a name="leave-an-mpsd-session"></a>离开 MPSD 会话

若要允许用户离开会话，游戏必须执行以下操作。

1.  Call the **MultiplayerSession.Leave Method** for the game session.
2.  Update the game session in MPSD, as described in **How to: Update a Multiplayer Session**.
3.  If necessary, call **Leave** method for the lobby session, and update that session.
4.  If necessary for the lobby session, shut down the multiplayer API by unregistering the **RealTimeActivityService.MultiplayerSubscriptionsLost Event** and the **RealTimeActivityService.MultiplayerSessionChanged Event**.

以下流程图说明如何离开会话并关闭。

![](../../images/multiplayer/Multiplayer_2015_Shut_Down.png)

## <a name="fill-open-session-slots-during-matchmaking"></a>在匹配期间填充会话空位

若要在匹配期间填充票证会话的空位，游戏必须按照以下类似步骤操作：

1.  Access the latest session state for the ticket session created during matchmaking.
2.  Add available players for game play from the lobby session.
3.  Determine if the ticket session is full.
4.  If the session is full, continue game play.
5.  If the session is not yet full, create the match ticket as described in **How to: Create a Match Ticket**. Be sure to create the ticket with the *preserveSession* parameter set to Always.
6.  Continue with matchmaking. See [Using SmartMatch Matchmaking](using-smartmatch-matchmaking.md).

以下流程图说明如何在匹配期间填充会话空位。

![](../../images/multiplayer/Multiplayer_2015_Fill_Open_Slots.png)

## <a name="create-a-match-ticket"></a>创建匹配票证

若要创建匹配票证，匹配侦察兵必须：

1.  Call the **MatchmakingService.CreateMatchTicketAsync Method**, passing in a reference to the ticket session. The method reads the ticket session from MPSD, and starts matchmaking for the users in the session. Internally the method calls the **POST (/serviceconfigs/{scid}/hoppers/{hoppername})**.
2.  Set the *preserveSession* parameter to Never if the matchmaking service is to match the members of the session into a new session or another existing session. Set the *preserveSession* parameter to Always to allow the title to reuse an existing game session as a ticket session to continue game play. The matchmaking service can then ensure that the submitted session is preserved and any matched players are added to that session.

3.  Use the **CreateMatchTicketResponse.EstimatedWaitTime Property** returned in the **CreateMatchTicketResponse Class** object to set user expectations of matchmaking time.
4.  Use the **CreateMatchTicketResponse.MatchTicketId Property** returned in the response object to cancel matchmaking for the session if needed, by deleting the ticket. 票证删除使用 **MatchmakingService.DeleteMatchTicketAsync 方法**。

## <a name="get-match-ticket-status"></a>获取匹配票证状态

你的游戏应该执行以下操作来检索匹配票证状态：

1.  Obtain the **MultiplayerSession Class** object for the ticket session.
2.  Use the **MultiplayerSession.MatchmakingServer Property** to access the **MatchmakingServer Class** object used in matchmaking.
3.  Check the **MatchmakingServer** object to determine the status of the matchmaking process, the typical wait time for the session, and the target session reference, if a match has been found.

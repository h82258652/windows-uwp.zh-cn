---
title: 通过远程会话连接设备
description: 通过在远程会话中加入多个设备来创建跨多个设备的共享体验。
ms.assetid: 1c8dba9f-c933-4e85-829e-13ad784dd3e2
ms.date: 06/28/2017
ms.topic: article
keywords: windows 10，uwp，已连接设备、 远程系统、 罗马、 项目罗马
ms.localizationpriority: medium
ms.openlocfilehash: 3dd23603df1f1c3fac151da2aea2f8435b3ee423
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633412"
---
# <a name="connect-devices-through-remote-sessions"></a>通过远程会话连接设备

“远程会话”功能允许应用通过会话连接到其他设备，以进行显式应用消息传送或系统管理数据的中转交换，如用于在 Windows Holographic 设备之间进行全息共享的 **[SpatialEntityStore](https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialentitystore)**。

远程会话可以由任何 Windows 设备进行创建，并且任何 Windows 设备（包括其他用户登录的设备）都可以请求加入远程会话（尽管会话可能具有“仅邀请”可见性）。 本指南为使用远程会话的所有主要方案提供了基本示例代码。 此代码可以合并到现有应用项目中，并且可以根据需要进行修改。 有关端到端实现，请参阅[问答游戏示例应用](https://github.com/microsoft/Windows-appsample-remote-system-sessions)。

## <a name="preliminary-setup"></a>初步设置

### <a name="add-the-remotesystem-capability"></a>添加 RemoteSystem 功能

为了让你的应用启动远程设备上的应用，必须将 `remoteSystem` 功能添加到应用包清单。 可以通过选择**功能**选项卡上的**远程系统**来使用程序包清单设计器添加它，也可以手动将以下行添加到项目的 _Package.appxmanifest_ 文件。

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-user-discovering-on-the-device"></a>在设备上启用跨用户发现
远程会话旨在连接多个不同的用户，因此涉及的设备将需要启用跨用户共享。 这是一种系统设置，可用 **RemoteSystem** 类中的静态方法进行查询：

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Everyone nearby".
}
```

若要更改此设置，用户必须打开**设置**应用。 在**系统** > **共享体验** > **跨设备共享**菜单中有一个下拉框，用户可在此指定系统可与哪些设备共享体验。

![共享体验设置页面](images/shared-experiences-settings.png)

### <a name="include-the-necessary-namespaces"></a>包含必需的命名空间
为了使用本指南中的所有代码段，你将需要在类文件中使用以下 `using` 语句。

```csharp
using System.Runtime.Serialization.Json;
using Windows.Foundation.Collections;
using Windows.System.RemoteSystems;
```

## <a name="create-a-remote-session"></a>创建远程会话

若要创建远程会话实例，你必须从 **[RemoteSystemSessionController](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)** 对象开始进行操作。 使用以下框架创建新的会话并处理其他设备的加入请求。

```csharp
public async void CreateSession() {
    
    // create a session controller
    RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob’s Minecraft game");
    
    // register the following code to handle the JoinRequested event
    manager.JoinRequested += async (sender, args) => {
        // Get the deferral
        var deferral = args.GetDeferral();
        
        // display the participant (args.JoinRequest.Participant) on UI, giving the 
        // user an opportunity to respond
        // ...
        
        // If the user chooses "accept", accept this remote system as a participant
        args.JoinRequest.Accept();
    };
    
    // create and start the session
    RemoteSystemSessionCreationResult createResult = await manager.CreateSessionAsync();
    
    // handle the creation result
    if (createResult.Status == RemoteSystemSessionCreationStatus.Success) {
        // creation was successful, get a reference to the session
        RemoteSystemSession currentSession = createResult.Session;
        
        // optionally subscribe to the disconnection event
        currentSession.Disconnected += async (sender, args) => {
            // update the UI, using args.Reason
            //...
        };
    
        // Use session (see later section)
        //...
    
    } else if (createResult.Status == RemoteSystemSessionCreationStatus.SessionLimitsExceeded) {
        // creation failed. Optionally update UI to indicate that there are too many sessions in progress
    } else {
        // creation failed for an unknown reason. Optionally update UI
    }
}
```

### <a name="make-a-remote-session-invite-only"></a>将远程会话设为仅邀请会话

如果希望远程会话无法被公众发现，可以将其设为“仅邀请”会话。 只有收到邀请的设备才能发送加入请求。 

此过程基本上与上面相同，但在构建 **[RemoteSystemSessionController](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)** 实例时，你将传入配置的 **[RemoteSystemSessionOptions](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.RemoteSystemSessionOptions)** 对象。

```csharp
// define the session options with the invite-only designation
RemoteSystemSessionOptions sessionOptions = new RemoteSystemSessionOptions();
sessionOptions.IsInviteOnly = true;

// create the session controller
RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob's Minecraft game", sessionOptions);

//...
```

若要发送邀请，你必须拥有对接收远程系统的引用（通过正常的远程系统发现来获取）。 只需将此引用传入会话对象的 **[SendInvitationAsync](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsession.sendinvitationasync)** 方法即可。 会话中的所有参与者都拥有对远程会话的引用（请参阅下一部分），所以任何参与者都可以发送邀请。

```csharp
// "currentSession" is a reference to a RemoteSystemSession.
// "guestSystem" is a previously discovered RemoteSystem instance
currentSession.SendInvitationAsync(guestSystem); 
```

## <a name="discover-and-join-a-remote-session"></a>发现并加入远程会话

发现远程会话的过程由 **[RemoteSystemSessionWatcher](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionwatcher)** 类进行处理，类似于发现单个远程系统。

```csharp
public void DiscoverSessions() {
    
    // create a watcher for remote system sessions
    RemoteSystemSessionWatcher sessionWatcher = RemoteSystemSession.CreateWatcher();
    
    // register a handler for the "added" event
    sessionWatcher.Added += async (sender, args) => {
        
        // get a reference to the info about the discovered session
        RemoteSystemSessionInfo sessionInfo = args.SessionInfo;
        
        // Optionally update the UI with the sessionInfo.DisplayName and 
        // sessionInfo.ControllerDisplayName strings. 
        // Save a reference to this RemoteSystemSessionInfo to use when the
        // user selects this session from the UI
        //...
    };
    
    // Begin watching
    sessionWatcher.Start();
}
```

获取 **[RemoteSystemSessionInfo](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioninfo)** 实例后，它可以用于向控制相应会话的设备发出加入请求。 接受的加入请求将异步返回 **[RemoteSystemSessionJoinResult](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionjoinresult)** 对象，该对象包含对已加入会话的引用。

```csharp
public async void JoinSession(RemoteSystemSessionInfo sessionInfo) {

    // issue a join request and wait for result.
    RemoteSystemSessionJoinResult joinResult = await sessionInfo.JoinAsync();
    if (joinResult.Status == RemoteSystemSessionJoinStatus.Success) {
        // Join request was approved

        // RemoteSystemSession instance "currentSession" was declared at class level.
        // Assign the value obtained from the join result.
        currentSession = joinResult.Session;
        
        // note connection and register to handle disconnection event
        bool isConnected = true;
        currentSession.Disconnected += async (sender, args) => {
            isConnected = false;

            // update the UI with args.Reason value
        };
        
        if (isConnected) {
            // optionally use the session here (see next section)
            //...
        }
    } else {
        // Join was unsuccessful.
        // Update the UI, using joinResult.Status value to show cause of failure.
    }
}
```

设备可以同时加入到多个会话中。 因此，可能需要将加入功能与每个会话的实际交互分开。 只要在应用中保留了对 **[RemoteSystemSession](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsession)** 实例的引用，就可以通过该会话尝试通信。

## <a name="share-messages-and-data-through-a-remote-session"></a>通过远程会话共享消息和数据

### <a name="receive-messages"></a>接收消息

你可以使用一个表示单一会话范围的信道的 **[RemoteSystemSessionMessageChannel](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionmessagechannel)** 实例与参与会话的其他设备交换消息和数据。 对其进行初始化后，它会立即开始侦听传入的消息。

>[!NOTE]
>在发送和接收时，必须从字节数组对消息进行序列化和反序列化。 此功能包含在以下示例中，但可以单独实现以获得更好的代码模块性。 有关此功能的示例，请参阅[示例应用](https://github.com/microsoft/Windows-appsample-remote-system-sessions)。

```csharp
public async void StartReceivingMessages() {
    
    // Initialize. The channel name must be known by all participant devices 
    // that will communicate over it.
    RemoteSystemSessionMessageChannel messageChannel = new RemoteSystemSessionMessageChannel(currentSession, 
        "Everyone in Bob's Minecraft game", 
        RemoteSystemSessionMessageChannelReliability.Reliable);
    
    // write the handler for incoming messages on this channel
    messageChannel.ValueSetReceived += async (sender, args) => {
        
        // Update UI: a message was received from the participant args.Sender
        
        // Deserialize the message 
        // (this app must know what key to use and what object type the value is expected to be)
        ValueSet receivedMessage = args.Message;
        object rawData = receivedMessage["appKey"]);
        object value = new ExpectedType(); // this must be whatever type is expected

        using (var stream = new MemoryStream((byte[])rawData)) {
            value = new DataContractJsonSerializer(value.GetType()).ReadObject(stream);
        }
        
        // do something with the "value" object
        //...
    };
}
```

### <a name="send-messages"></a>发送消息

建立通道后，向所有会话参与者发送消息很简单。

```csharp
public async void SendMessageToAllParticipantsAsync(RemoteSystemSessionMessageChannel messageChannel, object value){

    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }
    
    // Send message to all participants. Ordering is not guaranteed.
    await messageChannel.BroadcastValueSetAsync(message);
}
```

为了仅向特定参与者发送消息，你必须首先启动发现过程以获取对参与会话的远程系统的引用。 这类似于在会话之外发现远程系统的过程。 使用 **[RemoteSystemSessionParticipantWatcher](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionparticipantwatcher)** 实例查找会话的参与设备。

```csharp
public void WatchForParticipants() {
    // "currentSession" is a reference to a RemoteSystemSession.
    RemoteSystemSessionParticipantWatcher watcher = currentSession.CreateParticipantWatcher();

    watcher.Added += (sender, participant) => {
        // save a reference to "participant"
        // optionally update UI
    };   

    watcher.Removed += (sender, participant) => {
        // remove reference to "participant"
        // optionally update UI
    };

    watcher.EnumerationCompleted += (sender, args) => {
        // Apps can delay data model render up until this point if they wish.
    };

    // Begin watching for session participants
    watcher.Start();
}
```

当获得对会话参与者的引用列表后，你可以向其中的任何一组发送消息。

若要将消息发送给单个参与者（理想情况下由用户在屏幕上选择），只需将引用传入如下所示的方法中即可。

```csharp
public async void SendMessageToParticipantAsync(RemoteSystemSessionMessageChannel messageChannel, RemoteSystemSessionParticipant participant, object value) {
    
    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }

    // Send message to the participant
    await messageChannel.SendValueSetAsync(message,participant);
}
```

若要将消息发送给多个参与者（理想情况下由用户在屏幕上选择），请将其添加到列表对象，并将列表传入如下所示的方法中即可。

```csharp
public async void SendMessageToListAsync(RemoteSystemSessionMessageChannel messageChannel, IReadOnlyList<RemoteSystemSessionParticipant> myTeam, object value){

    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }

    // Send message to specific participants. Ordering is not guaranteed.
    await messageChannel.SendValueSetToParticipantsAsync(message, myTeam);   
}
```

## <a name="related-topics"></a>相关主题
* [连接的应用和设备 （项目罗马）](connected-apps-and-devices.md)
* [远程系统 API 参考](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)

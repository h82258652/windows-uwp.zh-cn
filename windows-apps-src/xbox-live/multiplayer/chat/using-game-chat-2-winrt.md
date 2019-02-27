---
title: 使用游戏聊天 2 WinRT 投影
description: 了解如何使用 Xbox Live 游戏聊天 2 WinRT 投影将语音通信添加到游戏中。
ms.date: 04/11/2018
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 游戏聊天 2, 游戏聊天, 语音通信
ms.localizationpriority: medium
ms.openlocfilehash: c06ee8610273273f234dbf3c7cb9fbd3a17eaa76
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2019
ms.locfileid: "9115577"
---
# <a name="using-game-chat-2-winrt-projections"></a>使用游戏聊天 2（WinRT 投影）

这是关于使用游戏聊天 2 的 C# API 的简短演练。 需要通过 C++ 访问游戏聊天 2 的游戏开发人员应查看[使用游戏聊天 2](using-game-chat-2.md)。

1. [先决条件](#prereq)
2. [初始化](#init)
3. [配置用户](#config)
4. [处理数据帧](#data)
5. [处理状态更改](#state)
6. [文字聊天](#text)
7. [辅助功能](#access)
8. [UI](#UI)
9. [静音](#mute)
10. [信誉差者自动静音](#automute)
11. [特权和隐私](#priv)
12. [清除](#cleanup)
13. [如何配置热门场景](#how-to-configure-popular-scenarios)

## <a name="prerequisites-a-nameprereq"></a>系统必备 <a name="prereq">

若要使用游戏聊天 2，必须添加 [Microsoft.Xbox.Services.GameChat2 nuget 包](https://www.nuget.org/packages/Microsoft.Xbox.Game.Chat.2.WinRT.UWP/)。

在使用游戏聊天 2 开始编码前，必须先配置应用的 AppXManifest，以便声明“麦克风”设备功能。 平台文档的相应部分更详细地介绍了 AppXManifest 功能；下面的代码段演示了“麦克风”设备功能节点应该位于“包/功能”节点下，否则将阻止聊天：

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <Package ...>
   <Identity ... />
   ...
   <Capabilities>
     <DeviceCapability Name="microphone" />
   </Capabilities>
 </Package>
```

## <a name="initialization-a-nameinit"></a>初始化<a name="init">

你可以通过使用应添加到实例的最大数量的并发聊天用户实例化 `GameChat2ChatManager` 对象，开始与库进行交互。 若要更改此值，必须释放 `GameChat2ChatManager` 对象，并且使用所需值重新创建该对象。 一次只能实例化一个 `GameChat2ChatManager`。

```cs
GameChat2ChatManager myGameChat2ChatManager = new GameChat2ChatManager(<maxChatUserCount>);
```

`GameChat2ChatManager` 对象具有可随时配置的其他可选属性。 下面的示例代码假定变量在用于设置 `myGameChat2ChatManager` 对象的属性前会获得值。

```cs
GameChat2SpeechToTextConversionMode mySpeechToTextConversionMode;
GameChat2SharedDeviceCommunicationRelationshipResolutionMode mySharedDeviceResolutionMode;
GameChat2CommunicationRelationship myDefaultLocalToRemoteCommunicationRelationship;
float myDefaultAudioRenderVolume;
GameChat2AudioEncodingTypeAndBitrate myAudioEncodingTypeAndBitRate;

...

myGameChat2ChatManager.SpeechToTextConversionMode = mySpeechToTextConversionMode;
myGameChat2ChatManager.SharedDeviceResolutionMode = mySharedDeviceResolutionMode;
myGameChat2ChatManager.DefaultLocalToRemoteCommunicationRelationship = myDefaultLocalToRemoteCommunicationRelationship;
myGameChat2ChatManager.DefaultAudioRenderVolume = myDefaultAudioRenderVolume;
myGameChat2ChatManager.AudioEncodingTypeAndBitRate = myAudioEncodingTypeAndBitRate;
```

## <a name="configuring-users-a-nameconfig"></a>配置用户 <a name="config">

初始化该实例之后，必须将本地用户添加到 GC2 实例中。 在本示例中，用户 A 表示本地用户。

```cs
string userAXuid;

...

IGameChat2ChatUser userA = myGameChat2ChatManager.AddLocalUser(userAXuid);
```

然后，必须添加远程用户和用于表示每个远程用户所在的远程“终结点”的标识符。 “终结点”由实现 `IGameChat2Endpoint` 界面的应用所拥有的对象表示。 以下示例演示了实现 `IGameChat2Endpoint` 的 `MyEndpoint` 类的示例实现。

```cs
class MyEndpoint : IGameChat2Endpoint
{
    private uint endpointIdentifier;
    public MyEndpoint(uint identifier)
    {
        endpointIdentifier = identifier;
    }

    // Implementing IsSameEndpointAs, the only method on the IGameChat2Endpoint interface
    public bool IsSameEndpointAs(IGameChat2Endpoint gameChat2Endpoint)
    {
        return endpointIdentifier == ((MyEndpoint)gameChat2Endpoint).endpointIdentifier;
    }
}
```

在以下代码示例中，用户 B、C 和 D 是所添加的远程用户。 用户 B 位于一台远程设备，而用户 C 和 D 位于另一台远程设备。 此示例假设将设置变量，`myGameChat2ChatManager` 是 `GameChat2ChatManager` 的实例以及“MyEndpoint”是实现 `IGameChat2Endpoint` 的类。

```cs
string userBXuid;
string userCXuid;
string userDXuid;
MyEndpoint myRemoteEndpointOne;
MyEndpoint myRemoteEndpointTwo;

...

IGameChat2ChatUser remoteUserB = myGameChat2ChatManager.AddRemoteUser(userBXuid, myRemoteEndpointOne);
IGameChat2ChatUser remoteUserC = myGameChat2ChatManager.AddRemoteUser(userCXuid, myRemoteEndpointTwo);
IGameChat2ChatUser remoteUserD = myGameChat2ChatManager.AddRemoteUser(userDXuid, myRemoteEndpointTwo);
```

现在，你可以配置每个远程用户和本地用户之间的通信关系。 在本示例中，假设用户 A 和用户 B 处于同一团队中，并允许进行双向通信。 `GameChat2CommunicationRelationship.SendAndReceiveAll`  定义为表示双向通信。 使用以下示例定义用户 A 与用户 B 的关系：

```cs
GameChat2ChatUserLocal localUserA = userA as GameChat2ChatUserLocal;
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

现在假设用户 C 和 D 是“观众”，他们能倾听用户 A，但是无法与之交谈。 `GameChat2CommunicationRelationship.ReceiveAll`  定义为单向通信。 使用以下示例设置这种通信关系：

```cs
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.ReceiveAll);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.ReceiveAll);
```

远程用户已添加到 `GameChat2ChatManager` 实例但未配置为与任何本地用户进行通信，这在任何时候都属于正常情况！ 如果用户可以决定团队或随意更改说话频道，则会出现这种情况。 游戏聊天 2 只会为已添加到实例的用户缓存信息（例如隐私关系和信誉），因此，即使所有可能的用户在特定时刻无法与任何本地用户说话，也有必要通知他们的游戏聊天 2。

最后，假设用户 D 已离开游戏，应将其从本地游戏聊天 2 实例中删除。 通过下面的调用可实现此操作：

```cs
remoteUserD.Remove();
```

调用 `Remove()` 后，用户对象立即失效。 如果删除用户，将缓存该用户的最后状态。 用户对象失效后调用的信息方法会反映用户在删除前的状态。 调用其他方法将引发错误。

## <a name="processing-data-frames-a-namedata"></a>处理数据帧 <a name="data">

游戏聊天 2 没有自己的传输层；这必须由应用提供。 通过应用定期频繁地调用 `GameChat2ChatManager.GetDataFrames()` 来管理此插件。 此方法是游戏聊天 2 向应用提供传出数据的方法。 它旨在加快运行速度，以便在专用网络线程上可频繁进行轮询。 这提供了可检索所有已排队数据的方便位置，而不必担心网络计时的不可预测性或多线程回调的复杂性。

当调用 `GameChat2ChatManager.GetDataFrames()` 时，在 `IGameChat2DataFrame` 对象列表中报告所有已排队数据。 应用应迭代该列表，并检查 `TargetEndpointIdentifiers`，然后使用应用的网络层将数据传送到相应的远程应用实例。 在此示例中，`HandleOutgoingDataFrame` 是函数，可根据 `TransportRequirement`，将 `Buffer` 中的数据发送到 `TargetEndpointIdentifiers` 中指定的每个“终结点”上的所有用户。

```cs
IReadOnlyList<IGameChat2DataFrame> frames = myGameChat2ChatManager.GetDataFrames();
foreach (IGameChat2DataFrame dataFrame in frames)
{
    HandleOutgoingDataFrame(
        dataFrame.Buffer,
        dataFrame.TargetEndpointIdentifiers,
        dataFrame.TransportRequirement
        );
}
```

数据帧的处理频率越高，最终用户感受到的音频延迟越低。 音频被合并到 40 ms 数据帧中；这是建议的轮询周期。

## <a name="processing-state-changes-a-namestate"></a>处理状态更改 <a name="state">

通过应用定期频繁调用 `GameChat2ChatManager.GetStateChanges()` 方法，游戏聊天 2 为应用提供更新，例如收到的文本消息。 它旨在加快运行速度，以便在 UI 呈现循环中对每个图形帧执行调用。 这提供了可检索所有已排队更改的方便位置，而不必担心网络计时的不可预测性或多线程回调的复杂性。

当调用 `GameChat2ChatManager.GetStateChanges()` 时，在 `IGameChat2StateChange` 对象列表中报告所有已排队更新。 应用应该：
1. 迭代该列表
2. 检查基本结构的更具体类型
3. 将基本结构转换为相应更详细的类型
4. 根据需要处理更新。 

```cs
IReadOnlyList<IGameChat2StateChange> stateChanges = myGameChat2ChatManager.GetStateChanges();
foreach (IGameChat2StateChange stateChange in stateChanges)
{
    switch (stateChange.Type)
    {
        case GameChat2StateChangeType.CommunicationRelationshipAdjusterChanged:
        {
            MyHandleCommunicationRelationshipAdjusterChanged(stateChange as GameChat2CommunicationRelationshipAdjusterChangedStateChange);
            break;
        }
        case GameChat2StateChangeType.TextChatReceived:
        {
            MyHandleTextChatReceived(stateChange as GameChat2TextChatReceivedStateChange);
            break;
        }

        ...
    }
}
```

## <a name="text-chat-a-nametext"></a>文字聊天<a name="text">

若要发送文字聊天，请使用`GameChat2ChatUserLocal.SendChatText()`。 例如：

```cs
localUserA.SendChatText("Hello");
```

游戏聊天 2 将生成包含此消息的数据帧；该数据帧的目标终结点将是与已配置为从本地用户接收文本消息的用户相关联的终结点。 当远程终结点处理数据时，将通过 `GameChat2TextChatReceivedStateChange` 公开消息。 与语音聊天一样，文字聊天也拥有特权和隐私限制。 如果一对用户已配置为允许文字聊天，但是特权或隐私限制禁用这种通信，则文本消息将被删除。

支持文字聊天输入和显示是辅助功能所必需的（有关更多详细信息，请参阅[辅助功能](#access)）。

## <a name="accessibility-a-nameaccess"></a>辅助功能<a name="access">

支持文字聊天输入和显示是必需的。 文本输入是必需的，因为即使在以往未普遍使用物理键盘的平台或游戏类型上，用户也可以将系统配置为使用文本到语音转换辅助技术。 同样地，文本显示是必需的，因为用户可以将系统配置为使用语音到文本转换功能。 通过分别检查 `GameChat2ChatUserLocal.TextToSpeechConversionPreferenceEnabled` 和 `GameChat2ChatUserLocal.SpeechToTextConversionPreferenceEnabled` 属性，可检测到本地用户的这些首选项，你可能希望按条件启用文本机制。

### <a name="text-to-speech"></a>文本到语音转换

当用户启用文本到语音转换时，`GameChat2ChatUserLocal.TextToSpeechConversionPreferenceEnabled` 将为“true”。 当检测到此状态时，应用必须提供文本输入方法。 当通过实体或虚拟键盘输入文本时，将此字符串传递给 `GameChat2ChatUserLocal.SynthesizeTextToSpeech()` 方法。 游戏聊天 2 将根据该字符串和用户可访问的语音偏好来检测和合成音频数据。 例如：

```cs
localUserA.SynthesizeTextToSpeech("Hello");
```

作为此操作的一部分，合成的音频将被传送到已配置为从本地用户接收音频的所有用户。 如果没有启用文本到语音转换的用户调用了 `GameChat2ChatUserLocal.SynthesizeTextToSpeech()`，则游戏聊天 2 将不执行任何操作。

### <a name="speech-to-text"></a>语音到文本转换

当用户启用语音到文本转换时，`GameChat2ChatUserLocal.SpeechToTextConversionPreferenceEnabled` 将为“true”。 当检测到此状态时，应用必须准备提供与转录聊天消息相关联的 UI。 GC 将自动转录每个远程用户的音频，并通过 `GameChat2TranscribedChatReceivedStateChange` 公开它。

### <a name="speech-to-text-performance-considerations"></a>语音到文本转换性能注意事项

当启用语音到文本转换时，每台远程设备上的游戏聊天 2 实例会启动与语音服务终结点的 Web 套接字连接。 每个远程游戏聊天 2 客户端通过此 WebSocket 将音频上传到语音服务终结点；语音服务终结点偶尔将转录消息返回给远程设备。 然后，远程设备将转录消息（即文本消息）发送到本地设备，游戏聊天 2 将转录消息提供给应用进行呈现。

因此，语音到文本转换功能的主要性能成本是网络使用情况。 大多数网络流量是由编码音频的上传产生的。 WebSocket 会上传在“常规”语音聊天路径中已由游戏聊天 2 编码的音频；应用可通过 `GameChat2ChatManager.AudioEncodingTypeAndBitrate` 控制比特率。

## <a name="ui-a-nameui"></a>UI<a name="UI">

建议在显示玩家的任意位置中，尤其是在玩家代号列表（例如记分牌）中，还应将静音/说话图标显示为用户的反馈。 `IGameChat2ChatUser.ChatIndicator` 属性表示该玩家当前的即时聊天状态。 以下示例展示了如何检索由变量“userA”指向的 `IGameChat2ChatUser` 对象的指标值，以确定要分配给“iconToShow”变量的特定图标常量值：

```cs
switch (userA.ChatIndicator)
{
    case GameChat2UserChatIndicator.Silent:
    {
        iconToShow = Icon_InactiveSpeaker;
        break;
    }
    case GameChat2UserChatIndicator.Talking:
    {
        iconToShow = Icon_ActiveSpeaker;
        break;
    }
    case GameChat2UserChatIndicator.LocalMicrophoneMuted:
    {
        iconToShow = Icon_MutedSpeaker;
        break;
    }
    
    ...
}
```

例如，`IGameChat2ChatUser.ChatIndicator` 的值预计会在玩家开始和停止说话时频繁改变。 它旨在支持应用在每个 UI 框架将其作为结果进行轮询。

## <a name="muting-a-namemute"></a>静音 <a name="mute">

`GameChat2ChatUserLocal.MicrophoneMuted` 属性可用于切换本地用户麦克风的静音状态。 麦克风静音时，不会捕获该麦克风的音频。 如果用户使用的是共享设备（如 Kinect），则静音状态适用于所有用户。 此方法不会反映硬件静音控制（例如用户耳机上的按钮）。 没有任何方法可用来通过游戏聊天 2 检索用户音频设备的硬件静音状态。

`GameChat2ChatUserLocal.SetRemoteUserMuted()` 方法可用来切换与特定本地用户相关的远程用户的静音状态。 当该远程用户被静音时，本地用户将不会从该用户处听到任何音频或收到任何文本消息。

## <a name="bad-reputation-auto-mute-a-nameautomute"></a>信誉差者自动静音<a name="automute">

在刚开始时，远程用户通常处于取消静音状态。 当出现以下两种情况时，游戏聊天 2 会以静音状态启动用户：(1) 远程用户不是本地用户的好友，(2) 远程用户有信誉较差的标志。 当用户由于此操作而处于静音状态时，`IGameChat2ChatUser.ChatIndicator` 将返回 `GameChat2UserChatIndicator.ReputationRestricted`。 对 `GameChat2ChatUserLocal.SetRemoteUserMuted()` 的首次调用将会替代此状态，其将远程用户作为目标用户包含在内。

## <a name="privilege-and-privacy-a-namepriv"></a>特权和隐私<a name="priv">

除了由游戏配置的通信关系之外，游戏聊天 2 还会强制执行特权和隐私限制。 游戏聊天 2 在首次添加用户时执行特权和隐私限制查找；在这些操作完成之前，用户的 `IGameChat2ChatUser.ChatIndicator` 将始终返回 `GameChat2UserChatIndicator.Silent`。 如果与用户的通信受到了特权或隐私限制的影响，则用户的 `IGameChat2ChatUser.ChatIndicator` 将返回 `GameChat2UserChatIndicator.PlatformRestricted`。 平台通信限制将同时应用于语音和文字聊天；绝不会出现平台限制阻止文字聊天但不阻止语音聊天的情况，反之亦然。

`GameChat2ChatUserLocal.GetEffectiveCommunicationRelationship()`  可用于帮助区分用户由于不完备的特权和隐私操作而无法通信的情况。 它将返回由游戏聊天 2 强制执行的通信关系（其形式为 `GameChat2CommunicationRelationship`），以及该通信关系不等于配置关系的原因（其形式为 `GameChat2CommunicationRelationshipAdjuster`）。 例如，如果查找操作仍在进行中，则 `GameChat2CommunicationRelationshipAdjuster` 将是 `GameChat2CommunicationRelationshipAdjuster.Initializing`。 此方法有望用于开发和调试场景中；不应用于影响 UI（请参阅 [UI](#UI)）。

## <a name="cleanup-a-namecleanup"></a>清除 <a name="cleanup">

当应用不再需要通过游戏聊天 2 进行通信时，应该调用 `GameChat2ChatManager.Dispose()`。 这使游戏聊天 2 能够回收已分配的资源，以管理通信。

## <a name="how-to-configure-popular-scenarios"></a>如何配置热门场景

### <a name="push-to-talk"></a>按下即可发言

应使用 `GameChat2ChatUserLocal.MicrophoneMuted` 实现按下即可发言。 将 `MicrophoneMuted` 设置为 false 以允许语音，将 `MicrophoneMuted` 设置为 true 以限制语音。 此属性更改将提供游戏聊天 2 最低延迟响应。

### <a name="teams"></a>团队

假设用户 A 和用户 B 都在蓝队中，并且用户 C 和用户 D 都在红队中。 每个用户都是应用的唯一实例。

在用户 A 的设备上：

```cs
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAndReceiveAll);
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

在用户 B 的设备上：

```cs
localUserB.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.SendAndReceiveAll);
localUserB.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserB.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

在用户 C 的设备上：

```cs
localUserC.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

在用户 D 的设备上：

```cs
localUserD.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

### <a name="broadcast"></a>广播

假设用户 A 是发布命令的领导者，并且用户 B、C 和 D 只能听。 每位玩家使用的是唯一设备。

在用户 A 的设备上：

```cs
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAll);
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.SendAll);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.SendAll);
```

在用户 B 的设备上：

```cs
localUserB.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserB.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserB.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

在用户 C 的设备上：

```cs
localUserC.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserC.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

在用户 D 的设备上：

```cs
localUserD.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserD.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
```

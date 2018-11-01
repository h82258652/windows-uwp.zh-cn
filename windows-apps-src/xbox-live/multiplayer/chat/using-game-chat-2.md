---
title: 使用游戏聊天 2
author: KevinAsgari
description: 了解如何通过使用 Xbox Live 游戏聊天 2 将语音通信添加到游戏中。
ms.author: kevinasg
ms.date: 3/20/2018
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 游戏聊天 2, 游戏聊天, 语音通信
ms.localizationpriority: medium
ms.openlocfilehash: e3042d7dac5fd9ef3e99c874dc4ca1937db94fff
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5879337"
---
# <a name="using-game-chat-2-c"></a>使用游戏聊天 2 (C++)

这是关于使用游戏聊天 2 的 C++ API 的简短演练。 需要通过 C# 访问游戏聊天 2 的游戏开发人员应查看[使用游戏聊天 2 WinRT 投影](using-game-chat-2-winrt.md)。

1. [先决条件](#prerequisites)
2. [初始化](#initialization)
3. [配置用户](#configuring-users)
4. [处理数据帧](#processing-data-frames)
5. [处理状态更改](#processing-state-changes)
6. [文字聊天](#text-chat)
7. [辅助功能](#accessibility)
8. [UI](#ui)
9. [静音](#muting)
10. [信誉差者自动静音](#bad-reputation-auto-mute)
11. [特权和隐私](#privilege-and-privacy)
12. [清除](#cleanup)
13. [失败模型](#failure-model)
14. [如何配置热门场景](#how-to-configure-popular-scenarios)

## <a name="prerequisites"></a>必备软件

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

编译游戏聊天 2 需要包括 GameChat2.h 主标头。 为了进行正确地链接，你的项目还必须在至少一个编译单元中包含 GameChat2Impl.h（建议使用通用的预编译标头，因为这些存根功能实现很小，编译器很容易将其生成为“内联”）。

在使用游戏聊天 2 接口时，项目无需在使用 C++/CX 或传统 C++ 进行编译之间作出选择；该接口可与以上两者一起使用。 此外，该实现也不会将引发异常作为报告非致命错误的手段，因此你可以根据需要在无异常项目中轻松使用它。 然而，该实现确实将引发异常作为报告致命错误的手段（请参阅[失败模型](#failure)了解详细信息）。

## <a name="initialization"></a>初始化

你可以通过初始化游戏聊天 2 单一实例（使用应用于单一实例初始化的生命周期的参数）来开始与库进行交互。

```cpp
chat_manager::singleton_instance().initialize(...);
```

## <a name="configuring-users"></a>配置用户

初始化该实例之后，必须将本地用户添加到游戏聊天 2 实例中。 在本示例中，用户 A 表示本地用户。

```cpp
chat_user* chatUserA = chat_manager::singleton_instance().add_local_user(<user_a_xuid>);
```

此外，还必须添加远程用户和用于表示用户所在的远程“终结点”的标识符。 “终结点”是在远程设备上运行的应用实例。 在本示例中，用户 B 位于终结点 X，用户 C 和 D 均位于终结点 Y。终结点 X 是任意分配的标识符“1”，终结点 Y 是任意分配的标识符“2”。 可以使用以下调用通知远程用户的游戏聊天 2：

```cpp
chat_user* chatUserB = chat_manager::singleton_instance().add_remote_user(<user_b_xuid>, 1);
chat_user* chatUserC = chat_manager::singleton_instance().add_remote_user(<user_c_xuid>, 2);
chat_user* chatUserD = chat_manager::singleton_instance().add_remote_user(<user_d_xuid>, 2);
```

现在，你可以配置每个远程用户和本地用户之间的通信关系。 在本示例中，假设用户 A 和用户 B 处于同一团队中，并允许进行双向通信。 `c_communicationRelationshipSendAndReceiveAll`  是 GameChat2.h 中定义的常量，用于表示双向通信。 使用以下示例定义用户 A 与用户 B 的关系：

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
```

现在假设用户 C 和 D 是“观众”，他们能倾听用户 A，但是无法与之交谈。 `c_communicationRelationshipSendAll`  是 GameChat2.h 中定义的常量，用于表示这种单向通信。 使用以下示例设置这种通信关系：

```cpp
chatUserA->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAll);
```

只要远程用户已添加到单一实例但未配置为与任何本地用户进行通信，这在任何时候都属于正常情况！ 如果用户可以决定团队或随意更改说话频道，则会出现这种情况。 游戏聊天 2 只会为已添加到实例的用户缓存信息（例如隐私关系和信誉），因此，即使所有可能的用户在特定时刻无法与任何本地用户说话，也有必要通知他们的游戏聊天 2。

最后，假设用户 D 已离开游戏，应将其从本地游戏聊天 2 实例中删除。 通过下面的调用可实现此操作：

```cpp
chat_manager::singleton_instance().remove_user(chatUserD);
```

调用 `chat_manager::remove_user()` 可能会使用户对象失效。 如果使用[实时音频操作](real-time-audio-manipulation.md)，请参考文档[聊天用户生存期](real-time-audio-manipulation.md#chat-user-lifetimes)，了解详细信息。 否则，调用 `chat_manager::remove_user()` 后，用户对象会立即失效。 有关何时可删除用户的细微限制的详细信息，请参阅[处理状态更改](#state)。

## <a name="processing-data-frames"></a>处理数据帧

游戏聊天 2 没有自己的传输层；这必须由应用提供。 此插件通过应用对方法的 `chat_manager::start_processing_data_frames()` 和 `chat_manager::finish_processing_data_frames()` 对的常规且频繁的调用进行管理。 这些方法是游戏聊天 2 向应用提供传出数据的方法。 它们旨在加快运行速度，以便在专用网络线程上可频繁进行轮询。 这提供了一个可检索所有已排队数据的方便位置，而不必担心网络计时的不可预测性或多线程回调的复杂性。

当调用 `chat_manager::start_processing_data_frames()` 时，在 `game_chat_data_frame` 结构指针数组中报告所有已排队数据。 应用应迭代该数组，并检查目标“终结点”，然后使用应用的网络层将数据传送到相应的远程应用实例。 完成了所有 `game_chat_data_frame` 结构的操作后，应通过调用 `chat_manager:finish_processing_data_frames()` 来将该数组应传递回游戏聊天 2 以释放资源。 例如：

```cpp
uint32_t dataFrameCount;
game_chat_data_frame_array dataFrames;
chat_manager::singleton_instance().start_processing_data_frames(&dataFrameCount, &dataFrames);
for (uint32_t dataFrameIndex = 0; dataFrameIndex < dataFrameCount; ++dataFrameIndex)
{
    game_chat_data_frame const* dataFrame = dataFrames[dataFrameIndex];
    HandleOutgoingDataFrame(
        dataFrame->packet_byte_count,
        dataFrame->packet_buffer,
        dataFrame->target_endpoint_identifier_count,
        dataFrame->target_endpoint_identifiers,
        dataFrame->transport_requirement
        );
}
chat_manager::singleton_instance().finish_processing_data_frames(dataFrames);
```

数据帧的处理频率越高，最终用户感受到的音频延迟越低。 音频被合并到 40 ms 数据帧中；这是建议的轮询周期。

## <a name="processing-state-changes"></a>处理状态更改

游戏聊天 2 通过应用对方法的 `chat_manager::start_processing_state_changes()` 和 `chat_manager::finish_processing_state_changes()` 对的常规且频繁的调用来为应用（例如收到的文本消息）提供更新。 它们旨在加快运行速度，以便在 UI 呈现循环中对每个图形帧执行调用。 这提供了一个可检索所有已排队更改的方便位置，而不必担心网络计时的不可预测性或多线程回调的复杂性。

当调用 `chat_manager::start_processing_state_changes()` 时，在 `game_chat_state_change` 结构指针数组中报告所有已排队更新。 应用应迭代该数组，并检查更具体类型的基本结构，将基本结构转换为相应更详细的类型，然后根据需要处理更新。 完成所有当前可用的 `game_chat_state_change` 对象的操作后，应通过调用 `chat_manager::finish_processing_state_changes()` 来将该数组应传递回游戏聊天 2 以释放资源。 例如：

```cpp
uint32_t stateChangeCount;
game_chat_state_change_array gameChatStateChanges;
chat_manager::singleton_instance().start_processing_state_changes(&stateChangeCount, &gameChatStateChanges);

for (uint32_t stateChangeIndex = 0; stateChangeIndex < stateChangeCount; ++stateChangeIndex)
{
    switch (gameChatStateChanges[stateChangeIndex]->state_change_type)
    {
        case game_chat_state_change_type::text_chat_received:
        {
            HandleTextChatReceived(static_cast<const game_chat_text_chat_received_state_change*>(gameChatStateChanges[stateChangeIndex]));
            break;
        }

        case Xs::game_chat_2::game_chat_state_change_type::transcribed_chat_received:
        {
            HandleTranscribedChatReceived(static_cast<const Xs::game_chat_2::game_chat_transcribed_chat_received_state_change*>(gameChatStateChanges[stateChangeIndex]));
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_state_changes(gameChatStateChanges);
```

因为`chat_manager::remove_user()` 会使与用户对象关联的内存立即失效，且状态更改可能包含用户对象指针，因此处理状态更改时不得调用 `chat_manager::remove_user()`。

## <a name="text-chat"></a>文字聊天

若要发送文字聊天，请使用`chat_user::chat_user_local::send_chat_text()`。 例如：

```cpp
chatUserA->local()->send_chat_text(L"Hello");
```

游戏聊天 2 将生成包含此消息的数据帧；该数据帧的目标终结点将是与已配置为从本地用户接收文本消息的用户相关联的终结点。 当远程终结点处理数据时，将通过 `game_chat_text_chat_received_state_change` 公开消息。 与语音聊天一样，文字聊天也拥有特权和隐私限制。 如果一对用户已配置为允许文字聊天，但是特权或隐私限制禁用这种通信，则文本消息将被删除。

支持文字聊天输入和显示是辅助功能所必需的（有关更多详细信息，请参阅[辅助功能](#access)）。

## <a name="accessibility"></a>辅助功能

支持文字聊天输入和显示是必需的。 文本输入是必需的，因为即使在以往未普遍使用物理键盘的平台或游戏类型上，用户也可以将系统配置为使用文本到语音转换辅助技术。 同样地，文本显示是必需的，因为用户可以将系统配置为使用语音到文本转换功能。 通过分别调用 `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` 和 `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` 方法，可检测到本地用户的这些首选项，你可能希望按条件启用文本机制。

### <a name="text-to-speech"></a>文本到语音转换

当用户启用文本到语音转换时，`chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` 将返回“true”。 当检测到此状态时，应用必须提供文本输入方法。 当通过实体或虚拟键盘输入文本时，将此字符串传递给 `chat_user::chat_user_local::synthesize_text_to_speech()` 方法。 游戏聊天 2 将根据该字符串和用户可访问的语音偏好来检测和合成音频数据。 例如：

```cpp
chat_userA->local()->synthesize_text_to_speech(L"Hello");
```

作为此操作的一部分，合成的音频将被传送到已配置为从本地用户接收音频的所有用户。 如果没有启用文本到语音转换的用户调用了 `chat_user::chat_user_local::synthesize_text_to_speech()`，则游戏聊天 2 将不执行任何操作。

### <a name="speech-to-text"></a>语音到文本转换

当用户启用语音到文本转换时，`chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` 将返回 true。 当检测到此状态时，应用必须准备提供与转录聊天消息相关联的 UI。 游戏聊天 2 将自动转录每个远程用户的音频，并通过 `game_chat_transcribed_chat_received_state_change` 公开它。

> `Windows::Xbox::UI::Accessibility`  是一种专门用于使用语言到文本转换辅助技术来简单呈现游戏内文字聊天的 Xbox One 类。

### <a name="speech-to-text-performance-considerations"></a>语音到文本转换性能注意事项

当启用语音到文本转换时，每台远程设备上的游戏聊天 2 实例会启动与语音服务终结点的 Web 套接字连接。 每个远程游戏聊天 2 客户端通过此 WebSocket 将音频上传到语音服务终结点；语音服务终结点偶尔将转录消息返回给远程设备。 然后，远程设备将转录消息（即文本消息）发送到本地设备，游戏聊天 2 将转录消息提供给应用进行呈现。

因此，语音到文本转换功能的主要性能成本是网络使用情况。 大多数网络流量是由编码音频的上传产生的。 WebSocket 会上传在“常规”语音聊天路径中已由游戏聊天 2 编码的音频；应用可通过 `chat_manager::set_audio_encoding_type_and_bitrate` 控制比特率。

## <a name="ui"></a>UI

无论玩家在什么地方显示（尤其是显示在记分牌等玩家代号列表中），都建议你也为用户显示静音/说话图标作为反馈。 通过调用 `chat_user::chat_indicator()` 以检索表示玩家当前的即时聊天状态的 `game_chat_user_chat_indicator` 来完成此操作。 以下示例展示了如何检索由变量“chatUserA”指向的 `chat_user` 对象的指标值，以确定要分配给“iconToShow”变量的特定图标常量值：

```cpp
switch (chatUserA->chat_indicator())
{
   case game_chat_user_chat_indicator::silent:
   {
       iconToShow = Icon_InactiveSpeaker;
       break;
   }

   case game_chat_user_chat_indicator::talking:
   {
       iconToShow = Icon_ActiveSpeaker;
       break;
   }

   case game_chat_user_chat_indicator::local_microphone_muted:
   {
       iconToShow = Icon_MutedSpeaker;
       break;
   }
   ...
}
```

例如，当玩家开始和停止说话时，`xim_player::chat_indicator()` 报告的值预计会频繁更改。 它旨在支持应用在每个 UI 框架将其作为结果进行轮询。

## <a name="muting"></a>静音

`chat_user::chat_user_local::set_microphone_muted()` 方法可用于切换本地用户麦克风的静音状态。 麦克风静音时，不会捕获该麦克风的音频。 如果用户使用的是共享设备（如 Kinect），则静音状态适用于所有用户。

`chat_user::chat_user_local::microphone_muted()` 方法可用于检索本地用户麦克风的静音状态。 此方法仅反映本地用户的麦克风在软件中是否已通过调用 `chat_user::chat_user_local::set_microphone_muted()` 设为静音；此方法不会反映通过用户耳机上的按钮等控制的硬件静音。 没有任何方法可用来通过游戏聊天 2 检索用户音频设备的硬件静音状态。

`chat_user::chat_user_local::set_remote_user_muted()` 方法可用来切换与特定本地用户相关的远程用户的静音状态。 当该远程用户被静音时，本地用户将不会从该用户处听到任何音频或收到任何文本消息。

## <a name="bad-reputation-auto-mute"></a>信誉差者自动静音

在刚开始时，远程用户通常处于取消静音状态。 当出现以下两种情况时，游戏聊天 2 会以静音状态启动用户：(1) 远程用户不是本地用户的好友，(2) 远程用户有信誉较差的标志。 当用户由于此操作而处于静音状态时，`chat_user::chat_indicator()` 将返回 `game_chat_user_chat_indicator::reputation_restricted`。 对 `chat_user::chat_user_local::set_remote_user_muted()` 的首次调用将会替代此状态，其将远程用户作为目标用户包含在内。

## <a name="privilege-and-privacy"></a>特权和隐私

除了由游戏配置的通信关系之外，游戏聊天 2 还会强制执行特权和隐私限制。 游戏聊天 2 在首次添加用户时执行特权和隐私限制查找；在这些操作完成之前，用户的 `chat_user::chat_indicator()` 将始终返回 `game_chat_user_chat_indicator::silent`。 如果与用户的通信受到了特权或隐私限制的影响，则用户的 `chat_user::chat_indicator()` 将返回 `game_chat_user_chat_indicator::platform_restricted`。 平台通信限制将同时应用于语音和文字聊天；绝不会出现平台限制阻止文字聊天但不阻止语音聊天的情况，反之亦然。

`chat_user::chat_user_local::get_effective_communication_relationship()`  可用于帮助区分用户由于不完备的特权和隐私操作而无法通信的情况。 它将返回由游戏聊天 2 强制执行的通信关系（其形式为 `game_chat_communication_relationship_flags`），以及该通信关系不等于配置关系的原因（其形式为 `game_chat_communication_relationship_adjuster`）。 例如，如果查找操作仍在进行中，则 `game_chat_communication_relationship_adjuster` 将是 `game_chat_communication_relationship_adjuster::intializing`。 此方法有望用于开发和调试场景中；不应用于影响 UI（请参阅 [UI](#UI)）。

## <a name="cleanup"></a>清除

当应用不再需要通过游戏聊天 2 进行通信时，应该调用 `chat_manager::cleanup()`。 这使游戏聊天 2 能够回收已分配的资源，以管理通信。

## <a name="failure-model"></a>失败模型

游戏聊天 2 实现也不会将引发异常作为报告非致命错误的手段，因此，可以根据需要在无异常项目中轻松使用它。 然而，游戏聊天 2 确实会引发异常，以报告致命错误。 这些错误是误用 API 的结果，例如在初始化实例前将用户添加到游戏聊天实例，或者从游戏聊天 2 实例中删除用户对象后访问用户对象。 应该在开发阶段早期阶段发现这些错误，通过修改与游戏聊天 2 交互所用的模式可以更正错误。 发生此类错误时，在引发异常前，有关错误原因的提示会打印到调试程序中。

## <a name="how-to-configure-popular-scenarios"></a>如何配置热门场景

### <a name="push-to-talk"></a>按下即可发言

应使用 `chat_user::chat_user_local::set_microphone_muted()` 实现按下即可发言。 调用 `set_microphone_muted(false)` 可允许语音，调用 `set_microphone_muted(true)` 可限制语音。 此方法将提供游戏聊天 2 的最低延迟响应。

### <a name="teams"></a>团队

假设用户 A 和用户 B 都在蓝队中，并且用户 C 和用户 D 都在红队中。 每个用户都是应用的唯一实例。

在用户 A 的设备上：

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
chatUserA->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserA->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

在用户 B 的设备上：

```cpp
chatUserB->local()->set_communication_relationship(chatUserA, c_communicationRelationshipSendAndReceiveAll);
chatUserB->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserB->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

在用户 C 的设备上：

```cpp
chatUserC->local()->set_communication_relationship(chatUserA, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAndReceiveAll);
```

在用户 D 的设备上：

```cpp
chatUserD->local()->set_communication_relationship(chatUserA, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAndReceiveAll);
```

### <a name="broadcast"></a>广播

假设用户 A 是发布命令的领导者，并且用户 B、C 和 D 只能听。 每位玩家使用的是唯一设备。

在用户 A 的设备上：

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAll);
```

在用户 B 的设备上：

```cpp
chatUserB->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserB->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserB->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

在用户 C 的设备上：

```cpp
chatUserC->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserC->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

在用户 D 的设备上：

```cpp
chatUserD->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserD->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
```

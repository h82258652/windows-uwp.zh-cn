---
title: 游戏聊天 2 迁移
author: KevinAsgari
description: 了解如何迁移现有游戏聊天代码以使用游戏聊天 2。
ms.author: kevinasg
ms.date: 5/2/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 游戏聊天 2, 游戏聊天, 语音通信
ms.localizationpriority: medium
ms.openlocfilehash: 7695fda4502f8359491e5fb822e59bc91a20e0c7
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2018
ms.locfileid: "4177139"
---
# <a name="migration-from-game-chat-to-game-chat-2"></a>从游戏聊天迁移到游戏聊天 2

本文档详细介绍游戏聊天与游戏聊天 2 之间的相似之处，以及如何从游戏聊天迁移到游戏聊天 2。 因此，本文面向的是已有游戏聊天实现但希望迁移到游戏聊天 2 的作品。 如果你还没有游戏聊天实现，建议先[使用游戏聊天 2](using-game-chat-2.md)。 本文档包含以下主题：

1. [前言](#preface)
2. [先决条件](#prerequisites)
3. [初始化](#initialization)
4. [配置用户](#configuring-users)
5. [处理数据](#processing-data)
6. [处理事件](#processing-events)
7. [文字聊天](#text-chat)
8. [辅助功能](#accessibility)
9. [UI](#UI)
10. [静音](#muting)
11. [信誉差者自动静音](#bad-reputation-auto-mute)
12. [特权和隐私](#privilege-and-privacy)
13. [清理](#cleanup)
14. [失败模型和调试](#failure-model-and-debugging)
15. [如何配置热门场景](#how-to-configure-popular-scenarios)
16. [编码前与解码后音频操作。](#pre-encode-and-post-decode-audio-manipulation)

## <a name="preface"></a>前言

原来的游戏聊天 API 是一个 WinRT API，它公开了聊天用户与语音通道的概念，以帮助实现 Xbox Live 游戏内语音聊天场景。 该游戏聊天 API 以聊天 API 为基础构建，后者自身便是一个 WinRT API，公开了聊天用户与语音通道的概念，同时需要对音频设备进行底层管理。 游戏聊天 2 是原游戏聊天和聊天 API 的后继 API - 它设计作为适用于基本聊天场景（如团队通信）的更简单的 API，同时为高级聊天场景（如广播式通信和实时音频操作）提供了更大的灵活性。 游戏聊天与游戏聊天 2 具有共同点：这两种 API 都提供了用于集成启用了 Xbox Live 的游戏内语音聊天的简便方法，同时作品中提供了一个传输层，用于从/向游戏聊天或游戏聊天 2 的远程实例传输数据包。

游戏聊天 2 API 与原游戏聊天和聊天 API 相比具有很多优势。 一些主要优势包括：
* 面向用户的灵活 API，不局限于“通道”模型。
* 通过数据源和数据接收者进行数据包筛选实现带宽改进。
* 通过应用可配置相关性实现 2 个长生存期线程的内部限制。
* C++ 和 WinRT 两种形式的 API。
* 符合“做工”模式的简化消费模型。
* 通过自定义分配器重定向游戏聊天 2 分配的内存挂钩。
* 相同的 UWP + 独占资源应用程序 (ERA) 标头，提供更方便的跨平台开发体验。

本文档详细介绍游戏聊天与游戏聊天 2 之间的相似之处，以及如何从游戏聊天迁移到游戏聊天 2 C++ API。 如果你有意从游戏聊天迁移到游戏聊天 2 WinRT API，建议阅读本文了解游戏聊天与游戏聊天 2 的对应概念，然后参阅[使用游戏聊天 2 WinRT 投影](using-game-chat-2-winrt.md)了解特定于 WinRT 的模式。 本文中，原游戏聊天的示例代码将使用 C++/CX。

## <a name="prerequisites"></a>先决条件

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

### <a name="game-chat"></a>游戏聊天

与原游戏聊天的交互通过 `ChatManager` 类进行。 下面的示例显示了如何使用默认参数构造 `ChatManager` 实例：

```cpp
auto chatManager = ref new ChatManager();
```

### <a name="game-chat-2"></a>游戏聊天 2

与游戏聊天 2 的所有交互均通过游戏聊天 2 的 `chat_manager` 单一实例进行。 必须初始化单一实例，才能与库进行任何有意义的交互。 该单一实例要求在初始化时指定并发本地和远程聊天用户的最大数目，这是因为游戏聊天 2 会按预期用户数预先分配一定比例的内存。 下面的示例显示了如何在并发本地和远程聊天用户最大数目为 4 时初始化该单一实例：

```cpp
chat_manager::singleton_instance().initialize(4);
```

在初始化过程中还可以指定几个可选参数，例如远程聊天用户的默认绘制体，以及游戏聊天 2 是否应该执行语音到文本自动转换。 有关这些参数的详细信息，请参阅有关 `chat_manager::initialize()` 的文档。

## <a name="configuring-users"></a>配置用户

### <a name="game-chat"></a>游戏聊天

向原游戏聊天 API 添加本地用户的操作通过 `ChatManager::AddLocalUserToChatChannelAsync()` 进行。 将该用户添加到多个聊天通道需要执行多个调用，每个调用均指定一个不同的通道。 下面的示例显示了如何将 "myXuid" 用户添加到通道 0：

```cpp
auto asyncOperation = chatManager->AddLocalUserToChatChannelAsync(0, L"myXuid");
```

远程用户不是直接添加到实例。 当某个远程设备出现在作品的网络中时，该作品将创建一个对象来表示该远程设备，并通过 `ChatManager::HandleNewRemoteConsole()` 通知游戏聊天该新设备的存在。 该作品还必须提供一个方法，通过实现 `CompareUniqueConsoleIdentifiersHandler` 来比较表示远程设备的对象与游戏聊天。 下面的示例显示了如何将此委托提供给游戏聊天，其中假设 `Platform::String` 对象用于表示远程设备，并且一个由字符串 `L"1"` 表示的新设备已加入作品的网络：

```cpp
auto token = chatManager->OnCompareUniqueConsoleIdentifiers +=
    ref new CompareUniqueConsoleIdentifiersHandler(
        [this](Platform::Object^ obj1, Platform::Object^ obj2)
    {
        return (static_cast<Platform::String^>(obj1)->Equals(static_cast<Platform::String^>(obj2)));
    });

...

Platform::String^ newDeviceIdentifier = ref new Platform::String(L"1");
chatManager->HandleNewRemoteConsole(newDeviceIdentifier);
```

一旦游戏聊天获得有关此设备的通知，就会生成包含有关任何本地用户的信息的数据包，并将这些数据包提供给该作品以传输到远程设备上的游戏聊天实例。 同样，远程设备上的游戏聊天实例也会生成包含该远程设备上用户相关信息的数据包，将由作品传输给游戏聊天的本地实例。 当本地实例收到包含新远程用户相关信息的数据包时，新的远程用户将添加到本地实例的用户列表中，可供通过 `ChatManager::GetUsers()` 使用。

从游戏聊天实例删除用户通过与 `ChatManager::RemoveLocalUserFromChatChannelAsync()` 类似的调用执行。 `ChatManager::RemoveRemoteConsoleAsync()`

### <a name="game-chat-2"></a>游戏聊天 2

向游戏聊天 2 添加本地用户的操作通过 `chat_manager::add_local_user()` 同步进行。 在此示例中，用户 A 表示 Xbox 用户 ID 为 `L"myLocalXboxUserId"` 的本地用户：

```cpp
chat_user* chatUserA = chat_manager::singleton_instance().add_local_user(L"myLocalXboxUserId");
```

请注意，该用户不会添加到特定的通道 - 游戏聊天 2 使用“通信关系”的概念而不是通道来管理用户是否可以相互交谈，并听到彼此。 本节将在后面介绍配置通信关系的方法。

与本地用户类似，远程用户同步添加到本地游戏聊天 2 实例。 远程用户必须同时与用于表示远程设备的标识符相关联。 游戏聊天 2 引用在远程设备上作为“终结点”运行的应用的一个实例。 在此示例中，用户 B 将是由整数 `1` 所表示的终结点上 Xbox 用户 ID 为 `L"remoteXboxUserId"` 的用户。

```cpp
chat_user* chatUserB = chat_manager::singleton_instance().add_remote_user(L"remoteXboxUserId", 1);
```

将用户添加到该实例之后，你应在每个远程用户和每个本地用户之间配置“通信关系”。 在本示例中，假设用户 A 和用户 B 处于同一团队中，并允许进行双向通信。 `c_communicationRelationshiSendAndReceiveAll`  是 GameChat2.h 中定义的常量，用于表示双向通信。 关系可以通过以下方式配置：

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
```

最后，假设用户 B 已离开游戏，应将其从本地游戏聊天实例中删除。 此操作通过以下调用同步执行：

```cpp
chat_manager::singleton_instance().remove_user(chatUserD);
```

请参阅[使用游戏聊天 2 - 配置用户](using-game-chat-2.md#configuring-users)查看各个 API 方法的更详细示例和参考，以了解更多信息。

## <a name="processing-data"></a>处理数据

### <a name="game-chat"></a>游戏聊天

游戏聊天没有自己的传输层；这必须由应用提供。 传出数据包的处理方式是：订阅 `OnOutgoingChatPacketReady` 事件，并检查参数以确定数据包目的地和传输要求。 下面的示例显示了如何订阅该事件并将参数转发给作品实现的 `HandleOutgoingPacket()` 方法：

```cpp
auto token = chatManager->OnOutgoingChatPacketReady +=
    ref new Windows::Foundation::EventHandler<Microsoft::Xbox::GameChat::ChatPacketEventArgs^>(
        [this](Platform::Object^, Microsoft::Xbox::GameChat::ChatPacketEventArgs^ args)
    {
        this->HandleOutgoingPacket(args);
    });
```

传入数据包通过`ChatManager::ProcessingIncomingChatMessage()` 提交到游戏聊天。 必须提供原始数据包缓冲区和远程设备标识符。 下面的示例显示了如何提交存储在本地 `packetBuffer` 中的数据包以及存储在本地变量 `remoteIdentifier` 中的远程设备标识符。

```cpp
Platform::String^ remoteIdentifier = /* The identifier associated with the device that generated this packet */
Windows::Storage::Streams::IBuffer^ packetBuffer = /* The incoming chat packet */

chatManager->ProcessIncomingChatMessage(packetBuffer, remoteIdentifier);
```

### <a name="game-chat-2"></a>游戏聊天 2

同样，游戏聊天 2 也没有自己的传输层；这必须由应用提供。 传出数据包通过由应用定期、频繁地调用 `chat_manager::start_processing_data_frames()` 和 `chat_manager::finish_processing_data_frames()` 方法对进行处理。 这些方法是游戏聊天 2 向应用提供传出数据的方法。 它们旨在加快运行速度，以便在专用网络线程上可频繁进行轮询。 这提供了一个可检索所有已排队数据的方便位置，而不必担心网络计时的不可预测性或多线程回调的复杂性。

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

传入数据通过`chat_manager::processing_incoming_data()` 提交到游戏聊天 2。 必须提供数据缓冲区和远程终结点标识符。 下面的示例显示了如何提交存储在本地变量 `dataFrame` 中的数据包以及存储在本地变量 `remoteEndpointIdentifier` 中的远程终结点标识符。

```cpp
uin64_t remoteEndpointIdentier = /* The identifier associated with the endpoint that generated this packet */
uint32_t dataFrameSize = /* The size of the incoming data frame, in bytes */
uint8_t* dataFrame = /* A pointer to the buffer containing the incoming data */

chatManager::singleton_instance().process_incoming_data(remoteEndpointIdentifier, dataFrameSize, dataFrame);
```

## <a name="processing-events"></a>处理事件

### <a name="game-chat"></a>游戏聊天

游戏聊天使用事件模型通知应用发生了值得关注的事件 - 例如收到了文本消息或更改了用户的辅助功能首选项。 应用必须为每个值得关注的事件订阅并实现一个处理程序。 此示例显示了如何订阅 `OnTextMessageReceived` 事件并将参数转发给应用实现的 `OnTextChatReceived()` 或 `OnTranscribedChatReceived()` 方法：

```cpp
auto token = chatManager->OnTextMessageReceived +=
    ref new Windows::Foundation::EventHandler<Microsoft::Xbox::GameChat::TextMessageReceivedEventArgs^>(
        [this](Platform::Object^, Microsoft::Xbox::GameChat::TextMessageReceivedEventArgs^ args)
    {
        if (args->ChatTextMessageType != ChatTextMessageType::TranscribedSpeechMessage)
        {
            this->OnTextChatReceived(args);
        }
        else
        {
            this->OnTranscribedChatReceived(args);
        }
    });
```

### <a name="game-chat-2"></a>游戏聊天 2

游戏聊天 2 通过由应用定期、频繁地调用 `chat_manager::start_processing_state_changes()` 和 `chat_manager::finish_processing_state_changes()` 方法对向用应程序提供更新（例如收到的文本消息）。 它们旨在加快运行速度，以便在 UI 呈现循环中对每个图形帧执行调用。 这提供了一个可检索所有已排队更改的方便位置，而不必担心网络计时的不可预测性或多线程回调的复杂性。

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

### <a name="game-chat"></a>游戏聊天

若要通过游戏聊天发送文字聊天，可以使用 `GameChatUser::GenerateTextMessage()`。 下面的示例显示如何通过由 `chatUser` 变量表示的本地聊天用户发送一条聊天文本消息：

```cpp
chatUser->GenerateTextMessage(L"Hello", false);
```

第二个布尔参数控制文本到语音转换。 有关详细信息，请参阅 [辅助功能](#accessibility)。然后，游戏聊天生成包含此消息的聊天数据包。 游戏聊天的远程实例将通过 `OnTextMessageReceived` 事件获得该文本消息的通知。

### <a name="game-chat-2"></a>游戏聊天 2

若要通过游戏聊天 2 发送文字聊天，请使用 `chat_user::chat_user_local::send_chat_text()`。 下面的示例显示如何通过由 `chatUser` 变量表示的本地聊天用户发送一条聊天文本消息：

```cpp
chatUser->local()->send_chat_text(L"Hello");
```

游戏聊天 2 将生成包含此消息的数据帧；该数据帧的目标终结点将是与已配置为从本地用户接收文本消息的用户相关联的终结点。 当远程终结点处理数据时，将通过 `game_chat_text_chat_received_state_change` 公开消息。 与语音聊天一样，文字聊天也拥有特权和隐私限制。 如果一对用户已配置为允许文字聊天，但是特权或隐私限制禁用这种通信，则文本消息将被删除而无任何提示。

支持文字聊天输入和显示是辅助功能所必需的（有关更多详细信息，请参阅[辅助功能](#accessibility)）。

## <a name="accessibility"></a>辅助功能

支持文字聊天输入和显示是游戏聊天和游戏聊天 2 所必需的。 文本输入是必需的，因为即使在以往未普遍使用物理键盘的平台或游戏类型上，用户也可以将系统配置为使用文本到语音转换辅助技术。 同样地，文本显示是必需的，因为用户可以将系统配置为使用语音到文本转换功能。 游戏聊天和游戏聊天 2 都提供了检测和尊从用户辅助功能首选项的方法；你可能希望根据这些设置有条件地启用文本机制。

### <a name="text-to-speech---game-chat"></a>文本到语音转换 - 游戏聊天

当用户启用文本到语音转换时，`GameChatUser::HasRequestedSynthesizedAudio()` 将返回 true。 检测到此状态时，`GameChatUser::GenerateTextMessage()` 将另外生成文本到语音音频，该音频插入到与本地用户关联的音频流中。 下面的示例显示如何通过由 `chatUser` 变量表示的本地用户发送一条聊天文本消息：

```cpp
chatUser->GenerateTextMessage(L"Hello", true);
```

第二个布尔参数用于允许应用覆盖语音到文本首选项 - "true" 指示当用户的文本到语音转换首选项启用时，游戏聊天应生成文本到语音转换音频；"false" 指示游戏聊天从不根据消息生成文本到语音转换音频。

### <a name="text-to-speech---game-chat-2"></a>文本到语音转换 - 游戏聊天 2

当用户启用文本到语音转换时，`chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` 将返回 "true"。 当检测到此状态时，应用必须提供文本输入方法。 当通过实体或虚拟键盘输入文本时，将此字符串传递给 `chat_user::chat_user_local::synthesize_text_to_speech()` 方法。 游戏聊天 2 将根据该字符串和用户可访问的语音偏好来检测和合成音频数据。 例如：

```cpp
chat_userA->local()->synthesize_text_to_speech(L"Hello");
```

作为此操作的一部分，合成的音频将被传送到已配置为从本地用户接收音频的所有用户。 如果没有启用文本到语音转换的用户调用了 `chat_user::chat_user_local::synthesize_text_to_speech()`，则游戏聊天 2 将不执行任何操作。

### <a name="speech-to-text---game-chat"></a>语音到文本转换 - 游戏聊天

当用户启用语音到文本转换时，`GameChatUser::HasRequestSynthesizedAudio()` 将返回 "true"。 检测到此状态时，游戏聊天将自动转录每个远程用户的音频，并通过 `OnTextMessageReceived` 事件公开它。 当因为收到转录消息而触发 `OnTextMessageReceived` 事件时，`TextMessageReceivedEventArgs` 将指示消息类型为 `ChatTextMessageType::TranscribedSpeechMessage`。

### <a name="speech-to-text---game-chat-2"></a>语音到文本转换 - 游戏聊天 2

当用户启用语音到文本转换时，`chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` 将返回 true。 当检测到此状态时，应用必须准备提供与转录聊天消息相关联的 UI。 游戏聊天 2 将自动转录每个远程用户的音频，并通过 `game_chat_transcribed_chat_received_state_change` 公开它。

> `Windows::Xbox::UI::Accessibility`  是专门为提供游戏内文字聊天的简单呈现而设计的 Xbox One 类，专注于语音到文本转换辅助技术。

请参阅[使用游戏聊天 2 - 语音到文本性能注意事项](using-game-chat-2.md#speech-to-text-performance-considerations)了解有关语音到文本转换性能注意事项的详细信息。

## <a name="ui"></a>UI

无论玩家在什么地方显示（尤其是显示在记分牌等玩家代号列表中），都建议你也为用户显示静音/说话图标作为反馈。 游戏聊天 2 引入了一个合并指示器，它提供更简单的机制来确定要显示的相应 UI 元素。

### <a name="game-chat"></a>游戏聊天

`GameChatUser` 提供了在确定相应 UI 元素时通常要检查的三个属性，分别是 `GameChatUser::TalkingMode`、`GameChatUser::IsMuted` 和 `GameChatUser::RestrictionMode`。 下面的示例演示了一种可能的启发机制，用于确定从 'chatUser' 变量所指向的 `GameChatUser` 对象分配到 `iconToShow` 变量的特定图标常量值。

```cpp
if (chatUser->RestrictionMode != None)
{
    if (!chatUser->IsMuted)
    {
        if (chatUser->TalkingMode != NotTalking)
        {
            iconToShow = Icon_ActiveSpeaker;
        }
        else
        {
            iconToShow = Icon_InactiveSpeaker;
        }
    }
    else
    {
        iconToShow = Icon_MutedSpeaker;
    }
}
else
{
    iconToShow = Icon_RestrictedSpeaker;
}
```

### <a name="game-chat-2"></a>游戏聊天 2

游戏聊天 2 有一个合并的 `game_chat_user_chat_indicator`，用于表示玩家的当前、即时聊天状态。 此值通过调用 `chat_user::chat_indicator()` 进行检索。 以下示例展示了如何检索由变量 'chatUser' 指向的 `chat_user` 对象的指标值，以确定要分配给 'iconToShow' 变量的特定图标常量值：

```cpp
switch (chatUser->chat_indicator())
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

## <a name="muting"></a>静音

## <a name="game-chat"></a>游戏聊天

在游戏聊天中，将某用户静音与取消静音分别通过调用 `ChatManager::MuteUserFromAllChannels()` 和 `ChatManager::UnMuteUIserFromAllChannels()` 执行。 可以通过检查 `GameChatUser::IsMuted` 或 `GameChatUser::IsLocalUserMuted` 检索用户的静音状态。

## <a name="game-chat-2"></a>游戏聊天 2

`chat_user::chat_user_local::set_microphone_muted()` 方法可用于切换本地用户麦克风的静音状态。 麦克风静音时，不会捕获该麦克风的音频。 如果用户使用的是共享设备（如 Kinect），则静音状态适用于所有用户。

`chat_user::chat_user_local::microphone_muted()` 方法可用于检索本地用户麦克风的静音状态。 此方法仅反映本地用户的麦克风在软件中是否已通过调用 `chat_user::chat_user_local::set_microphone_muted()` 设为静音；此方法不会反映通过用户耳机上的按钮等控制的硬件静音。 没有任何方法可用来通过游戏聊天 2 检索用户音频设备的硬件静音状态。

`chat_user::chat_user_local::set_remote_user_muted()` 方法可用来切换与特定本地用户相关的远程用户的静音状态。 当该远程用户被静音时，本地用户将不会从该用户处听到任何音频或收到任何文本消息。

## <a name="bad-reputation-auto-mute"></a>信誉差者自动静音

在刚开始时，远程用户通常处于取消静音状态。 游戏聊天和游戏聊天 2 都具有“信誉差者自动静音”功能。 这意味着当出现以下两种情况时，会以静音状态启动该用户：(1) 远程用户不是本地用户的好友，(2) 远程用户有信誉较差的标志。 当用户由于此功能静音时，游戏聊天 2 将提供反馈。 请参阅[使用游戏聊天 2 - 信誉差者自动静音](using-game-chat-2.md#bad-reputation-auto-mute)了解更多信息。

## <a name="privilege-and-privacy"></a>特权和隐私

在应用所管理的所有通道或通信关系上方，游戏聊天和游戏聊天 2 都会强制执行 Xbox Live 特权和隐私限制。 游戏聊天 2 还另外提供诊断信息，用于确定限制对音频方向的确切影响（例如音频及音频限制是单向还是双向的）。

### <a name="game-chat"></a>游戏聊天

游戏聊天通过 `RestrictionMode` 属性公开特权和隐私信息。 该属性可通过检查 `GameChatUser::RestrictionMode` 进行检索。

### <a name="game-chat-2"></a>游戏聊天 2

游戏聊天 2 在首次添加用户时执行特权和隐私限制查找；在这些操作完成之前，用户的 `chat_user::chat_indicator()` 将始终返回 `game_chat_user_chat_indicator::silent`。 如果与用户的通信受到了特权或隐私限制的影响，则用户的 `chat_user::chat_indicator()` 将返回 `game_chat_user_chat_indicator::platform_restricted`。 平台通信限制将同时应用于语音和文字聊天；绝不会出现平台限制阻止文字聊天但不阻止语音聊天的情况，反之亦然。

`chat_user::chat_user_local::get_effective_communication_relationship()`  可用于帮助区分用户由于不完备的特权和隐私操作而无法通信的情况。 它将返回由游戏聊天 2 强制执行的通信关系（其形式为 `game_chat_communication_relationship_flags`），以及该通信关系不等于配置关系的原因（其形式为 `game_chat_communication_relationship_adjuster`）。 例如，如果查找操作仍在进行中，则 `game_chat_communication_relationship_adjuster` 将是 `game_chat_communication_relationship_adjuster::intializing`。 此方法有望用于开发和调试场景中；不应用于影响 UI（请参阅 [UI](#UI)）。

## <a name="cleanup"></a>清理

原游戏聊天的 `ChatManager` 是 WinRT 运行时类 - 一个引用计数接口。 因此，当最后一个引用计数降为零时，将回收其内存资源，并进行清理。 与游戏聊天 2 的 C++ API 的交互通过单一实例进行；当应用不再需要通过游戏聊天 2 通信时，你应调用 `chat_manager::cleanup()`。 这使游戏聊天 2 能够立即释放已分配用于管理通信的所有资源。 有关游戏聊天 2 的 WinRT API 和资源管理的详细信息，请参阅[使用游戏聊天 2 WinRT 投影](using-game-chat-2-winrt.md#cleanup)。

## <a name="failure-model-and-debugging"></a>失败模型和调试

原来的游戏聊天是一个 WinRT API，因此通过异常来报告错误。 非致命错误或警告通过可选的调试回调进行报告。 游戏聊天 2 的 C++ API 不会将引发异常作为报告非致命错误的手段，因此，可以根据需要在无异常项目中轻松使用它。 然而，游戏聊天 2 确实会引发异常，以报告致命错误。 这些错误是误用 API 的结果，例如在初始化实例前将用户添加到游戏聊天实例，或者从游戏聊天实例中删除用户对象后访问该用户对象。 应该在开发阶段早期阶段发现这些错误，通过修改与游戏聊天 2 交互所用的模式可以更正错误。 发生此类错误时，在引发异常前，有关错误原因的提示会输出到调试程序中。 游戏聊天 2 的 WinRT API 遵循通过异常报告错误的 WinRT 模式。

## <a name="how-to-configure-popular-scenarios"></a>如何配置热门场景

请参阅[使用游戏聊天 2 - 如何配置热门场景](using-game-chat-2.md#how-to-configure-popular-scenarios)查看有关如何配置热门场景（如按下即可发言、团队和广播式通信场景）的示例。

## <a name="pre-encode-and-post-decode-audio-manipulation"></a>编码前与解码后音频操作。

原来的游戏聊天允许通过 `OnPreEncodeAudioBuffer` 和 `OnPostDecodeAudioBuffers` 事件访问原始麦克风音频，从而实现音频操作。 游戏聊天 2 通过轮询模型公开此功能。 有关详细信息，请参阅[实时音频操作](real-time-audio-manipulation.md)。

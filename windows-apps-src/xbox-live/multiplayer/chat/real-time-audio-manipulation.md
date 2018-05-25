---
title: 实时音频操作
author: KevinAsgari
description: 了解如何操作和处理由游戏聊天 2 捕获的聊天音频。
ms.author: kevinasg
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 游戏聊天 2, 游戏聊天, 语音通信, 缓冲区操作, 音频操作
ms.localizationpriority: low
ms.openlocfilehash: 43be2692b53ac7036d4bf67659fafde913d52a95
ms.sourcegitcommit: 1eabcf511c7c7803a19eb31f600c6ac4a0067786
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="real-time-audio-manipulation"></a>实时音频操作

游戏聊天 2 (GC2) 使开发人员可以选择在聊天音频管道中插入它，以检查和操作玩家的聊天音频数据。 这可用于将有趣的音频效果应用于游戏中玩家的语音。 将通过为获得音频数据而轮询的音频流对象与 GC2 的音频操作管道交互。 相较于使用回调，此模型允许开发人员在任何最方便的处理线程中检查或操作音频。

特别推荐以下关于使用实时音频操作的简短演练，其中包含以下主题：

1. [初始化音频操作管道](#initializing-the-audio-manipulation-pipeline)
2. [处理音频流状态更改](#processing-audio-stream-state-changes)
3. [操作预编码的聊天音频](#manipulating-pre-encode-chat-audio-data)
4. [操作解码后的聊天音频](#manipulating-post-decode-chat-audio-data)
5. [聊天用户生命周期](#chat-user-lifetimes)

## <a name="initializing-the-audio-manipulation-pipeline"></a>初始化音频操作管道

默认情况下，GC2 不会启用实时音频操作。 要启用实时音频操作，该应用必须在 `chat_manager::initialize()` 中指定想要启用的音频操作的形式。

目前，支持以下音频操作形式：

* `game_chat_audio_manipulation_mode_flags::none` - 禁用音频操作。 这是默认配置。 在此模式中，聊天音频将不中断地传输。
* `game_chat_audio_manipulation_mode_flags::pre_encode_stream_manipulation` - 启用预编码音频操作。 在此模式中，本地用户产生的所有聊天音频都将在编码之前通过音频操作管道馈送。 即使该应用仅检查聊天音频数据而并不进行操作，它仍然负责将未更改的音频缓冲区提交回 GC2，以便进行编码和传输。
* `game_chat_audio_manipulation_mode_flags::post_decode_stream_manipulation` - 启用解码后的音频操作。 此模式当前处于开发阶段，不能使用。

## <a name="processing-audio-stream-state-changes"></a>处理音频流状态更改

GC2 通过 `game_chat_stream_state_change` 结构为音频流的状态提供更新。 这些更新存储关于哪些流已更新以及更新方式的信息。 这些更新可以通过对 `chat_manager::start_processing_stream_state_changes` 和`chat_manager::finish_processing_stream_state_changes` 方法对的调用获得轮询。 此方法对以 `game_chat_stream_state_change` 结构指针数组的形式提供所有最新的排队音频流状态更新。 应用应迭代该数组，并正确处理每次的更新。 处理完所有可用`game_chat_stream_state_change`更新后，该数组应通过 `chat_manager::finish_processing_stream_state_changes()` 传递回 GC2。 例如：

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::pre_encode_audio_stream_created:
        {
            HandlePreEncodeAudioStreamCreated(streamStateChanges[streamStateChangeIndex].preEncodeAudioStream);
            break;
        }

        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            HandlePreEncodeAudioStreamClosed(streamStateChanges[streamStateChangeIndex].preEncodeAudioStream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);
```

## <a name="manipulating-pre-encode-chat-audio-data"></a>操作预编码的聊天音频数据

GC2 通过 `pre_encode_audio_stream` 类为本地用户提供对预编码聊天音频数据的访问。

### <a name="stream-lifetime"></a>流生命周期
当新 `pre_encode_audio_stream` 实例可供该应用使用时，它将通过 `game_chat_stream_state_change` 结构传递，其类型字段设置为 `game_chat_stream_state_change_type::pre_encode_audio_stream_created`。 当流状态更改返回 GC2 后，音频流将可用于预编码音频操作。

当现有 `pre_encode_audio_stream` 已不能用于音频操作时，该应用将通过 `game_chat_stream_state_change` 结构收到通知，其类型字段设置为 `game_chat_stream_state_change_type::pre_encode_audio_stream_closed`。 现在，应用可以开始清理与此音频流关联的资源。 当此流状态更改返回 GC2 后，音频流将不可用于预编码音频操作。

当关闭的 `pre_encode_audio_stream` 返回所有资源后，该流将被销毁，应用将通过 `game_chat_stream_state_change` 结构收到通知，其类型字段设置为 `game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed`。 应该清除对此流的任何引用或指向它的指针。 当此流状态更改返回 GC2 后，音频流内存将失效。

### <a name="stream-users"></a>流用户
可以使用 `pre_encode_audio_stream::get_users()` 检查与流关联的用户列表。

### <a name="audio-formats"></a>音频格式
可以使用 `pre_encode_audio_stream::get_pre_processed_format()` 检查该应用从 GC2 检索的缓冲区的音频格式。 预处理的音频格式始终为单声道。 该应用应该会处理以 32 位浮点数、16 位整数和 32 位整数表示的数据。

该应用必须使用 `pre_encode_audio_stream::set_processed_format()` 通知 GC2 提交给它进行编码和传输的操作缓冲区的音频格式。 处理过的预编码音频流的格式必须满足以下先决条件：

* 格式为单声道。
* 格式必须为 32 位浮点 PCM、32 位整数 PCM 或 16 位整数 PCM 格式。
* 根据其平台，格式的采样率必须满足相应的先决条件。 Xbox One ERA 支持 8 kHz、12 kHz、16 kHz 和 24 kHz 采样率。 适用于 Xbox One 和电脑的 UWP 支持 8 kHz、12 kHz、16 kHz、24 kHz、32 kHz、44.1 kHz 和 48 kHz 采样率。

### <a name="retrieving-and-submitting-audio"></a>检索和提交音频
应用可以使用 `pre_encode_audio_stream::get_available_buffer_count()` 查询预编码音频流，以获得可用于处理的缓冲区数目。 如果该应用想要延迟音频处理，直到可用缓冲区最少时，则可以使用此信息。 每个预编码的音频流中仅将 10 个缓冲区加入队列，音频延迟会在音频管道中引进延迟，因此建议应用在排队缓冲区达到 4 个之前消耗预编码的音频流。

应用可以使用 `pre_encode_audio_stream::get_next_buffer()` 从预编码的音频流中检索音频缓冲区。 新音频缓冲区平均 40 毫秒可使用一次。 使用完成后，此方法返回的缓冲区必须发布到 `pre_encode_audio_stream::return_buffer()`。 在任何给定时间，预编码的音频流最多只能存在 10 个已排队或未返回的缓冲区。 一旦达到此限制，从玩家的音频源中捕获的新缓冲区将被丢弃，直到返回一些未完成的缓冲区。

应用可以使用 `pre_encode_audio_stream::submit_buffer()` 将已检查和操作过的缓冲区提交回 GC2 以进行编码和传输。 GC2 支持就地和异地音频操作，因此提交给 `pre_encode_audio_stream::submit_buffer()` 的缓冲区不一定是通过 `pre_encode_audio_stream::get_next_buffer()` 检索的相同缓冲区。 根据与此流关联的用户，这些提交的缓冲区的隐私/特权同样适用。 每 40 毫秒，将对此流中接下来 40 毫秒的音频进行编码和传输。 要防止音频暂时中断，应以恒定的速度向此流提交应持续听到的音频的缓冲区。

### <a name="stream-contexts"></a>流上下文
应用可以使用 `pre_encode_audio_stream::set_custom_stream_context()` 和 `pre_encode_audio_stream::custom_stream_context()` 在预编码音频流上管理自定义指针大小上下文值。 这些自定义流上下文对于在 GC2 的音频流和辅助数据（例如流元数据、游戏状态等）之间创建映射非常有用。

### <a name="example"></a>示例
下面是一个简化的端到端示例，展示了在音频处理帧中如何使用预编码的音频流：

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::pre_encode_audio_stream_created:
        {
            stream->set_processed_audio_format(...);
            stream->set_custom_stream_context(...);
            HandlePreEncodeAudioStreamCreated(streamStateChanges[streamStateChangeIndex].preEncodeAudioStream);
            break;
        }

        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            HandlePreEncodeAudioStreamClosed(streamStateChanges[streamStateChangeIndex].preEncodeAudioStream);
            break;
        }

        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed:
        {
            HandlePreEncodeAudioStreamDestroyed(streamStateChanges[streamStateChangeIndex].preEncodeAudioStream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

uint32_t preEncodeAudioStreamCount;
pre_encode_audio_stream_array preEncodeAudioStreams;
chat_manager::singleton_instance().get_pre_encode_audio_streams(&preEncodeAudioStreamCount, &preEncodeAudioStreams);
for (uint32_t preEncodeAudioStreamIndex = 0; preEncodeAudioStreamIndex < preEncodeAudioStreamCount; ++preEncodeAudioStreamIndex)
{
    pre_encode_audio_stream* stream = preEncodeAudioStreams[preEncodeAudioStreamIndex];
    StreamContext* context = reinterpret_cast<StreamContext*>(stream->custom_stream_context());

    game_chat_audio_format audio_format = stream->get_pre_processed_format();

    uint32_t preProcessedBufferByteCount;
    void* preProcessedBuffer;
    stream->get_next_buffer(&preProcessedBufferByteCount, &preProcessedBuffer);

    while (preProcessedBuffer != nullptr)
    {
        void* processedBuffer = nullptr;
        switch (audio_format.bits_per_sample)
        {
            case 16:
            {
                assert (audio_format.sample_type == game_chat_sample_type::integer);
                processedBuffer = ManipulateChatBuffer<int16_t>(preProcessedBufferByteCount, preProcessedBuffer, context);
                break;
            }
            case 32:
            {
                switch (audio_format.sample_type)
                {
                    case game_chat_sample_type::integer:
                    {
                        processedBuffer = ManipulateChatBuffer<int32_t>(preProcessedBufferByteCount, preProcessedBuffer, context);
                        break;
                    }
                    case game_chat_sample_type::ieee_float:
                    {
                        processedBuffer = ManipulateChatBuffer<float>(preProcessedBufferByteCount, preProcessedBuffer, context);
                        break;
                    }
                    default:
                    {
                        assert(false);
                        break;
                    }
                }
                break;
            }
            default:
            {
                assert(false);
                break;
            }
        }
        // processedBuffer can be the same as preProcessedBuffer (in-place manipulation) or it can be a buffer of
        // memory not managed by GC2 (out-of-place manipulation).
        stream->submit_buffer(processedBuffer);
        // Only return buffers retrieved from GC2. Do not return foreign memory to return_buffer.
        stream->return_buffer(preProcessedBuffer);
        stream->get_next_buffer(&preProcessedBufferByteCount, &preProcessedBuffer);
    }
}

Sleep(audioProcessingPeriodInMilliseconds);
```

## <a name="manipulating-post-decode-chat-audio-data"></a>操作解码后的聊天音频数据

此功能当前正在开发阶段。 调用解码后的音频操作接口将导致失败。

## <a name="chat-user-lifetimes"></a>聊天用户生命周期

启用实时音频操作将会影响聊天用户的生命周期。 如果调用 `chat_manager::remove_user(chatUserX)`，chatUserX 指向的 chat_user 将保持有效，直到引用 chatUserX 的所有音频流都已销毁。 请考虑以下方案：

```cpp
// At somepoint a chat user, chatUserX, leaves the game session.
chat_manager::singleton_instance().remove_user(chatUserX);

// chatUserX is still valid, but to avoid further synchronization, prevent non-audio-stream use of chatUserX
chatUserX = nullptr;

// On the audio processing thread...
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);
for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        ...

        // All of the streams associated with chatUserX will close.
        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            CleanupPreEncodeAudioStreamResources(streamStateChanges[streamStateChangeIndex].preEncodeAudioStream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

// The next time the app processes stream state changes...
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);
for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        ...

        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed:
        {
            uint32_t chatUserCount;
            Xs::game_chat_2::chat_user_array chatUsers;
            streamStateChanges[streamStateChangeIndex].preEncodeAudioStream->get_users(&chatUserCount, &chatUsers);
            assert(chatUserCount != 0);
            for (uint32_t chatUserIndex = 0; chatUserIndex < chatUserCount; ++chatUserIndex)
            {
                // chat_user objects such as chatUserX will still be valid while the destroyed state change is being processed.
                Log(chatUsers[chatUserIndex]->xbox_user_id());
            }
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);
// Once the all destroyed state changes have been processed for all streams associated with chatUserX, it's memory will be invalidated.
// Do not call methods on chatUserX, e.g. chatUserX->xbox_user_id()
```

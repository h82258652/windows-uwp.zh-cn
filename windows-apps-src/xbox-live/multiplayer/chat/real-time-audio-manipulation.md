---
title: 实时音频操作
author: KevinAsgari
description: 了解如何操作和处理由游戏聊天 2 捕获的聊天音频。
ms.author: kevinasg
ms.date: 05/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 游戏聊天 2, 游戏聊天, 语音通信, 缓冲区操作, 音频操作
ms.localizationpriority: medium
ms.openlocfilehash: 4d5f9863bf4a023520486567de1f5feb1907b177
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "4152285"
---
# <a name="real-time-audio-manipulation"></a>实时音频操作

游戏聊天 2 为开发人员提供了自行插入聊天音频管道检查和处理玩家的聊天音频数据的选项。 这可用于将有趣的音频效果应用于游戏中玩家的语音。 游戏聊天 2 的音频操作管道是通过可以轮询的音频数据的音频流对象与之交互。 相较于使用回调，此模型允许开发人员在任何最方便的处理线程中检查或操作音频。

特别推荐以下关于使用实时音频操作的简短演练，其中包含以下主题：

1. [初始化音频操作管道](#initializing-the-audio-manipulation-pipeline)
2. [处理音频流状态更改](#processing-audio-stream-state-changes)
3. [操作预编码的聊天音频](#manipulating-pre-encode-chat-audio-data)
4. [操作解码后的聊天音频](#manipulating-post-decode-chat-audio-data)
5. [聊天用户生命周期](#chat-user-lifetimes)

## <a name="initializing-the-audio-manipulation-pipeline"></a>初始化音频操作管道

游戏聊天 2 默认情况下不会启用实时音频操作。 若要启用实时音频操作，应用必须指定想要的音频操作的形式中已启用`chat_manager::initialize()`通过设置 audioManipulationMode 参数。

目前，支持以下音频操作形式：

* `game_chat_audio_manipulation_mode_flags::none` - 禁用音频操作。 这是默认配置。 在此模式中，聊天音频将不中断地传输。
* `game_chat_audio_manipulation_mode_flags::pre_encode_stream_manipulation` - 启用预编码音频操作。 在此模式中，本地用户产生的所有聊天音频都将在编码之前通过音频操作管道馈送。 即使应用仅检查聊天音频数据而并不进行操作，它仍然是应用的责任连同音频缓冲区提交回游戏聊天 2，以便他们可以进行编码和传输。
* `game_chat_audio_manipulation_mode_flags::post_decode_stream_manipulation` - 启用解码后的音频操作。 此模式当前处于开发阶段，不能使用。

## <a name="processing-audio-stream-state-changes"></a>处理音频流状态更改

游戏聊天 2 为通过音频流的状态提供更新`game_chat_stream_state_change`结构。 这些更新存储关于哪些流已更新以及更新方式的信息。 这些更新可以通过对 `chat_manager::start_processing_stream_state_changes()` 和`chat_manager::finish_processing_stream_state_changes()` 方法对的调用获得轮询。 此方法对以 `game_chat_stream_state_change` 结构指针数组的形式提供所有最新的排队音频流状态更新。 应用应迭代该数组，并正确处理每次的更新。 一次所有可用`game_chat_stream_state_change`更新已处理，该数组应传递回游戏聊天 2 通过`chat_manager::finish_processing_stream_state_changes()`。 例如：

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
            HandlePreEncodeAudioStreamCreated(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            HandlePreEncodeAudioStreamClosed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);
```

## <a name="manipulating-pre-encode-chat-audio-data"></a>操作预编码的聊天音频数据

游戏聊天 2 提供预编码的聊天音频数据的本地用户完成的访问权限`pre_encode_audio_stream`类。

### <a name="stream-lifetime"></a>流生命周期
当新`pre_encode_audio_stream`实例已准备好要使用的应用，它将通过传递`game_chat_stream_state_change`结构的`state_change_type`字段设置为`game_chat_stream_state_change_type::pre_encode_audio_stream_created`。 当此流状态更改返回到游戏聊天 2 后, 的音频流将成为可用于预编码音频操作。

当现有`pre_encode_audio_stream`变得不可用于音频操作，应用将通过通知`game_chat_stream_state_change`结构的`state_change_type`字段设置为`game_chat_stream_state_change_type::pre_encode_audio_stream_closed`。 现在，应用可以开始清理与此音频流关联的资源。 当此流状态更改返回到游戏聊天 2 后, 的音频流将不可用于预编码音频操作。

当关闭`pre_encode_audio_stream`已返回的所有其资源，该流将被销毁，并且应用将通过通知`game_chat_stream_state_change`结构和它的`state_change_type`字段设置为`game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed`。 应该清除对此流的任何引用或指向它的指针。 当此流状态更改返回到游戏聊天 2 后, 的音频流内存将失效。

### <a name="stream-users"></a>流用户
可以使用 `pre_encode_audio_stream::get_users()` 检查与流关联的用户列表。

### <a name="audio-formats"></a>音频格式
可以使用检查该应用从游戏聊天 2 检索的缓冲区的音频格式`pre_encode_audio_stream::get_pre_processed_format()`。 预处理的音频格式始终为单声道。 该应用应该会处理以 32 位浮点数、16 位整数和 32 位整数表示的数据。

应用必须通知进行编码和传输使用提交给它的操作缓冲区的音频格式的游戏聊天 2 `pre_encode_audio_stream::set_processed_format()`。 处理过的预编码音频流的格式必须满足以下先决条件：

* 格式为单声道。
* 格式必须为 32 位浮点 PCM、32 位整数 PCM 或 16 位整数 PCM 格式。
* 根据其平台，格式的采样率必须满足相应的先决条件。 Xbox One ERA 支持 8 kHz、12 kHz、16 kHz 和 24 kHz 采样率。 适用于 Xbox One 和电脑的 UWP 支持 8 kHz、12 kHz、16 kHz、24 kHz、32 kHz、44.1 kHz 和 48 kHz 采样率。

### <a name="retrieving-and-submitting-audio"></a>检索和提交音频
应用可以使用 `pre_encode_audio_stream::get_available_buffer_count()` 查询预编码音频流，以获得可用于处理的缓冲区数目。 如果该应用想要延迟音频处理，直到可用缓冲区最少时，则可以使用此信息。 每个预编码的音频流中仅将 10 个缓冲区加入队列，音频延迟会在音频管道中引进延迟，因此建议应用在排队缓冲区达到 4 个之前消耗预编码的音频流。

应用可以使用 `pre_encode_audio_stream::get_next_buffer()` 从预编码的音频流中检索音频缓冲区。 新音频缓冲区平均 40 毫秒可使用一次。 使用完成后，此方法返回的缓冲区必须发布到 `pre_encode_audio_stream::return_buffer()`。 在任何给定时间，预编码的音频流最多只能存在 10 个已排队或未返回的缓冲区。 一旦达到此限制，从玩家的音频源中捕获的新缓冲区将被丢弃，直到返回一些未完成的缓冲区。

应用可以其已检查和操作缓冲区提交回游戏聊天 2 进行编码和传输使用`pre_encode_audio_stream::submit_buffer()`。 游戏聊天 2 支持就地和外的位置的音频操作，因此缓冲区提交到`pre_encode_audio_stream::submit_buffer()`执行操作不一定必须从检索的相同缓冲区`pre_encode_audio_stream::get_next_buffer()`。 根据与此流关联的用户，这些提交的缓冲区的隐私/特权同样适用。 每 40 毫秒，将对此流中接下来 40 毫秒的音频进行编码和传输。 要防止音频暂时中断，应以恒定的速度向此流提交应持续听到的音频的缓冲区。

### <a name="stream-contexts"></a>流上下文
应用可以使用 `pre_encode_audio_stream::set_custom_stream_context()` 和 `pre_encode_audio_stream::custom_stream_context()` 在预编码音频流上管理自定义指针大小上下文值。 这些自定义流上下文都有助于创建游戏聊天 2 的音频流和辅助数据之间的映射： 流元数据、 游戏状态等。

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
            pre_encode_audio_stream* stream = streamStateChanges[streamStateChangeIndex]->pre_encode_audio_stream;
            stream->set_processed_audio_format(...);
            stream->set_custom_stream_context(...);
            HandlePreEncodeAudioStreamCreated(stream);
            break;
        }

        case game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            HandlePreEncodeAudioStreamClosed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        case game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed:
        {
            HandlePreEncodeAudioStreamDestroyed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
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
        // memory not managed by Game Chat 2 (out-of-place manipulation).
        stream->submit_buffer(processedBuffer);
        // Only return buffers retrieved from Game Chat 2. Do not return foreign memory to return_buffer.
        stream->return_buffer(preProcessedBuffer);
        stream->get_next_buffer(&preProcessedBufferByteCount, &preProcessedBuffer);
    }
}

Sleep(audioProcessingPeriodInMilliseconds);
```

## <a name="manipulating-post-decode-chat-audio-data"></a>操作解码后的聊天音频数据

游戏聊天 2 提供解码后的聊天音频数据的访问权限`post_decode_audio_source_stream`和`post_decode_audio_sink_stream`类，以便用户可能会为每个本地接收方的聊天音频唯一操作来自远程用户的音频。

### <a name="sources-and-sinks"></a>源和接收器
与 pre-encode 管道，该模型的处理解码后音频数据拆分跨两个类：`post_decode_audio_source_stream`和`post_decode_audio_sink_stream`。 可从检索远程用户的已解码的音频`post_decode_audio_source_stream`对象，操作，并发送到`post_decode_audio_sink_stream`对象进行呈现。 这允许游戏聊天 2 的之间的集成解码后的音频处理管道和很有帮助的音频中间件。

### <a name="stream-lifetime"></a>流生命周期
当新`post_decode_audio_source_stream`或`post_decode_audio_sink_stream`实例已准备好要使用的应用，它将通过传递`game_chat_stream_state_change`使用它的结构`state_change_type`字段设置为`game_chat_stream_state_change_type::post_decode_audio_source_stream_created`或`game_chat_stream_state_change_type::post_decode_audio_sink_stream_created`分别。 当此流状态更改返回到游戏聊天 2 后, 的音频流将成为可用于解码后音频操作。

当现有`post_decode_audio_source_stream`或`post_decode_audio_sink_stream`变为不可用于音频操作，应用将通过通知`game_chat_stream_state_change`使用它的结构`state_change_type`字段设置为`game_chat_stream_state_change_type::post_decode_audio_source_stream_closed`或`game_chat_stream_state_change_type::post_decode_audio_sink_stream`分别。 现在，应用可以开始清理与此音频流关联的资源。 当此流状态更改返回到游戏聊天 2 后, 的音频流将不可用于解码后音频操作。 对于源流，这意味着没有多个缓冲区，将排队等待操作。 对于接收器流，这意味着，提交将无法再呈现的缓冲区。

当关闭`post_decode_audio_source_stream`或`post_decode_audio_sink_stream`已返回的所有其资源，该流将被销毁，并且将通过通知应用`game_chat_stream_state_change`使用它的结构`state_change_type`字段设置为`game_chat_stream_state_change_type::post_decode_audio_source_stream_destroyed`或`game_chat_stream_state_change_type::post_decode_audio_sink_stream_destroyed`分别。 应该清除对此流的任何引用或指向它的指针。 当此流状态更改返回到游戏聊天 2 后, 的音频流内存将失效。

### <a name="stream-users"></a>流用户
可以检查与 post-decode 源流关联的远程用户的列表，使用`post_decode_audio_source_stream::get_users()`。 可以检查与 post-decode 接收器流关联的本地用户的列表，使用`post_decode_audio_sink_stream::get_users()`。

### <a name="audio-formats"></a>音频格式
可以使用检查该应用从游戏聊天 2 检索的缓冲区的音频格式`post_decode_audio_source_stream::get_pre_processed_format()`。 预处理的音频格式将始终为单声道、 16 位整数 PCM。

应用必须通知游戏聊天 2 的呈现使用提交给它的操作缓冲区的音频格式`post_decode_audio_sink_stream::set_processed_format()`。 处理的格式解码后音频接收器流必须满足以下先决条件：

* 格式必须小于 64 通道。
* 格式必须为 16 位整数 PCM （最佳）、 20 位整数 PCM （24 位的容器） 中、 24 位整数 PCM、 32 位整数 PCM 或 32 位浮点 PCM （首选后 16 位整数 PCM 格式）。 
* 该格式的采样率必须之间每秒的速度 1000年和 200000 示例。

### <a name="retrieving-and-submitting-audio"></a>检索和提交音频
应用可以查询解码后的音频源流，以提供用于处理的缓冲区使用数`post_decode_audio_source_stream::get_available_buffer_count()`。 如果该应用想要延迟音频处理，直到可用缓冲区最少时，则可以使用此信息。 只有 10 个缓冲区将在每个排队解码后的音频源流和音频延迟将管道中引进延迟音频，因此建议应用清空其解码后的音频流之前超过 4 个缓冲区加入队列。

应用可以检索从音频缓冲区解码后的音频源流使用`post_decode_audio_source_stream::get_next_buffer()`。 新音频缓冲区平均 40 毫秒可使用一次。 使用完成后，此方法返回的缓冲区必须发布到 `post_decode_audio_source_stream::return_buffer()`。 最多 10 个已排队或未返回的缓冲区可以在任何给定时间存在解码后的音频源流。 一旦达到此限制，从远程玩家的新解码的缓冲区将被丢弃，直到返回一些未完成的缓冲区。

应用可以提交回游戏聊天 2 通过其已检查和操作缓冲区解码后音频接收器流，以呈现使用`post_decode_audio_sink_stream::submit_mixed_buffer()`。 游戏聊天 2 支持就地和外的位置的音频操作，因此缓冲区提交到`post_decode_audio_sink_stream::submit_mixed_buffer()`执行操作不一定必须从检索的相同缓冲区`post_decode_audio_source_stream::get_next_buffer()`。 每 40 毫秒，将会呈现音频此流中接下来 40 毫秒。 要防止音频暂时中断，应以恒定的速度向此流提交应持续听到的音频的缓冲区。

### <a name="privacy-and-mixing"></a>隐私和混合
由于 post-decode 管道的源接收器模型，它是应用的责任混合从检索的缓冲区`post_decode_audio_source_stream`对象，并提交到混合的缓冲区`post_decode_audio_sink_stream`对象进行呈现。 这也意味着它是应用的责任具有正确的隐私和强制执行特权执行组合。 游戏聊天 2 提供`post_decode_audio_sink_stream::can_receive_audio_from_source_stream()`若要更简单和高效此信息查询。

### <a name="chat-indicators"></a>聊天指示器

解码后音频操作不会影响聊天指示器状态为每个用户。 例如，当远程用户被静音时，音频将提供给该应用，但该远程用户的聊天指示器仍将指示静音。 当远程用户正在与其交互时，将提供其音频，但聊天指示器将指示无论应用是否提供包含从该用户的音频混音交谈。 有关 UI 和聊天指示器的详细信息，请参阅[使用游戏聊天 2](using-game-chat-2.md#ui)。 如果额外特定于应用的限制将用来确定哪些用户存在音频组合中，它是应用的责任它正在阅读由游戏聊天 2 的聊天指示器时考虑这些相同的限制。

### <a name="stream-contexts"></a>流上下文
应用可以管理自定义指针大小上下文值在解码后的音频流，使用`set_custom_stream_context()`和`custom_stream_context()`方法。 这些自定义流上下文都有助于创建游戏聊天 2 的音频流和辅助数据之间的映射： 流元数据、 游戏状态等。

### <a name="example"></a>示例
下面是有关如何使用简化的端到端示例解码后的音频处理帧中的音频流：

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::post_decode_audio_source_stream_created:
        {
            post_decode_audio_source_stream* stream = streamStateChanges[streamStateChangeIndex]->post_decode_audio_source_stream;
            stream->set_custom_stream_context(...);
            HandlePostDecodeAudioSourceStreamCreated(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_source_stream_closed:
        {
            HandlePostDecodeAudioSourceStreamClosed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_source_stream_destroyed:
        {
            HandlePostDecodeAudioSourceStreamDestroyed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_created:
        {
            post_decode_audio_sink_stream* stream = streamStateChanges[streamStateChangeIndex]->post_decode_audio_sink_stream;
            stream->set_custom_stream_context(...);
            stream->set_processed_format(...);
            HandlePostDecodeAudioSinkStreamCreated(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_closed:
        {
            HandlePostDecodeAudioSinkStreamClosed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_destroyed:
        {
            HandlePostDecodeAudioSinkStreamDestroyed(stream);
            break;
        }

        ...
    }
}

chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

uint32_t sourceStreamCount;
post_decode_audio_source_stream_array sourceStreams;
chatManager::singleton_instance().get_post_decode_audio_source_streams(&sourceStreamCount, &sourceStreams);

uint32_t sinkStreamCount;
post_decode_audio_sink_stream_array sinkStreams;
chatManager::singleton_instance().get_post_decode_audio_sink_streams(&sinkStreamCount, &sinkStreams);

//
// MixBuffer is a custom type defined as:
// struct MixBuffer
// {
//     uint32_t bufferByteCount;
//     void* buffer;
// };
//
std::vector<std::pair<post_decode_audio_source_stream*, MixBuffer>> cachedSourceBuffers;

for (uint32_t sourceStreamIndex = 0; sourceStreamIndex < sourceStreamCount; ++sourceStreamIndex)
{
    post_decode_audio_source_stream* sourceStream = sourceStreams[sourceStreamIndex];

    MixBuffer mixBuffer;
    sourceStream->get_next_buffer(&mixBuffer.bufferByteCount, &mixBuffer.buffer);
    if (buffer != nullptr)
    {
        // Stash the buffer to return after we are done with mixing. If this program was using audio middleware, now
        // would be an appropriate time to plumb the buffer through the middleware
        cachedSourceBuffer.push_back(std::pair<post_decode_audio_source_stream*, MixBuffer>{sourceStream, mixBuffer});
    }
}

// Loop over each sink stream, perform mixing, and submit
for (uint32_t sinkStreamIndex = 0; sinkStreamIndex < sinkStreamCount; ++sinkStreamIndex)
{
    post_decode_audio_sink_stream* sinkStream = sinkStreams[sinkStreamIndex];

    if (sinkStream->is_open())
    {
        std::vector<std::pair<MixBuffer, float>> buffersToMixForThisStream;

        for (const std::pair<post_decode_audio_source_stream, MixBuffer>& sourceBufferPair : cachedSourceBuffers)
        {
            float volume;
            if (sinkStream->can_receive_audio_from_source_stream(sourceBufferPair.first, &volume))
            {
                buffersToMixForThisStream.push_back(std::pair<MixBuffer, float>{sourceBufferPair.second, volume});
            }
        }

        if (buffersToMixForThisStream.size() > 0)
        {
            uint32_t mixedBufferByteCount;
            uint8_t* mixedBuffer;
            MixPostDecodeBuffers(buffersToMixForThisStream, &mixedBufferByteCount, &mixedBuffer);
            sinkStream->submit_mixed_buffer(mixedBufferByteCount, mixedBuffer);
        }
    }
}

// Return buffers after mix and submission
for (const std::pair<post_decode_audio_source_stream*, MixBuffer>& cachedSourceBuffer : cachedSourceBuffers)
{
    post_decode_audio_source_stream* sourceStream = cachedSourceBuffer.first;
    void* bufferToReturn = cachedSourceBuffer.second.buffer;
    sourceStream->return_buffer(bufferToReturn);
}

Sleep(audioProcessingPeriodInMilliseconds);
```

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
            CleanupPreEncodeAudioStreamResources(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
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
            streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream->get_users(&chatUserCount, &chatUsers);
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

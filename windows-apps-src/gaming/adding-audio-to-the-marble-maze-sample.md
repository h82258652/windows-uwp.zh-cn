---
title: 向 Marble Maze 示例添加音频
description: 本文档介绍了在使用音频时要考虑的重要实践，并展示了 Marble Maze 如何应用这些实践。
ms.assetid: 77c23d0a-af6d-17b5-d69e-51d9885b0d44
ms.date: 10/18/2017
ms.topic: article
keywords: Windows 10, uwp, 音频, 游戏, 示例
ms.localizationpriority: medium
ms.openlocfilehash: 666ea75f1d4f18121b7ae9fa3def3b455ae3e7a3
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "7835452"
---
# <a name="adding-audio-to-the-marble-maze-sample"></a>向 Marble Maze 添加音频示例

本文档介绍了在使用音频时要考虑的重要实践，并展示了 Marble Maze 如何应用这些实践。 Marble Maze 使用 [Microsoft 媒体基础](https://msdn.microsoft.com/library/windows/desktop/ms694197)从文件加载音频资源，使用 [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049) 混合并播放音频，以及向音频应用效果。

Marble Maze 在后台播放音乐，还使用游戏声音来指示游戏事件，例如弹珠撞到墙壁时。 该实现的一个重要部分是，Marble Maze 使用一个混响或回声效果来模拟弹珠弹跳时的声音。 混响效果实现可导致回声在小空间中更快且更响亮地传到你耳中；但在大空间中，回声会更安静、更慢地传到你耳中。

> [!NOTE]
> 与本文档对应的示例代码位于 [DirectX Marble Maze 游戏示例](http://go.microsoft.com/fwlink/?LinkId=624011)中。

本文档讨论了在游戏中使用音频时的一些重要事项：

- 考虑使用媒体基础来解码音频资源，使用 XAudio2 播放音频。 但是，如果已有 适用于通用 Windows 平台 (UWP) 应用的音频资源加载机制，你可以使用它。

- 音频图包含每种有效声音的一个源语音，零个或多个子混合语音，以及一个主语音。 源语音可置于子混合语音和/或主语音中。 子混合语音可置于其他子混合语音和/或主语音中。

- 如果背景音乐文件很大，可考虑将音乐流式传输到较小的缓冲区中，以减少内存的使用。

- 如果这么做有意义，可在应用失去焦点、不可见或暂停时暂停音频播放。 在应用重新获得焦点、变为可见或被恢复时恢复音频回放。

- 设置音频类别，以反映每种声音的作用。 例如，通常使用 **AudioCategory_GameMedia** 作为游戏背景音乐，使用 **AudioCategory\_GameEffects** 作为声音效果。

- 通过释放并重新创建所有音频资源和接口，处理设备更改（包括耳机）。

- 需要最少的磁盘空间和流成本时，考虑是否压缩音频文件。 否则，可保留音频未压缩状态，以便可更快地加载它。

## <a name="introducing-xaudio2-and-microsoft-media-foundation"></a>XAudio2 和 Microsoft 媒体基础简介

XAudio2 是一个专门用于支持游戏音频的 Windows 低级别音频库。 它为游戏提供了数字信号处理 (DSP) 和音频图引擎。 XAudio2 扩展了它的前任 DirectSound 和 XAudio，支持 SIMD 浮点体系结构和高清音频等计算趋势。 它还支持当今游戏更复杂的声音处理需求。

[XAudio2 重要概念](https://msdn.microsoft.com/library/windows/desktop/ee415764)文档解释了使用 XAudio2 的重要概念。 简单来讲，这些概念包括：

- [IXAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415908) 接口是 XAudio2 引擎的核心。 Marble Maze 使用此接口创建语音，并在输出设备更改或失败时接收通知。

- **语音**处理、调整和播放音频数据。

- **源语音**是一个音频通道集合（单声道、5.1 等），表示一个音频数据流。 在 XAudio2 中，源语音是音频处理开始的地方。 通常，都是从外部来源（如文件或网络）加载声音数据，并发送到源语音。 Marble Maze 使用[媒体基础](https://msdn.microsoft.com/library/windows/desktop/ms694197)从文件加载声音数据。 媒体基础将在本文后面介绍。

- **子混合语音**处理音频数据。 这种处理可能包括更改音频流或将多个流组合为一个。 Marble Maze 使用子混合来创建混响效果。

- **主语音**组合来自来源和子混合语音的数据，并将该数据发送给音频硬件。

- **音频图**包含每种有效声音的一个源语音，零或多个子混合语音，以及单个主语音。

- **回调**可通知客户端代码，一个语音或引擎对象中发生了某个事件。 使用回调，可在 XAudio2 完成时通过缓冲区重用内存，在音频设备更改时（例如连接耳机或连接断开时）做出反应等。 本文后面的[处理耳机和设备更改](#handling-headphones-and-device-changes)将介绍 Marble Maze 如何使用此机制处理设备更改。

Marble Maze 使用两个音频引擎（换言之，两个 [IXAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415908) 对象）处理音频。 一个引擎处理背景音乐，另一个引擎处理游戏声音。

Marble Maze 还必须为每个引擎创建一个主语音。 回想一下，主语音将音频流组合到一个流中，然后将这个流发送到音频硬件。 背景音乐流（一个源语音）将数据输出到一个主语音和两个子混合语音。 子混合语音执行混响效果。

媒体基础是一个支持多种音频和视频格式的多媒体库。 XAudio2 和媒体基础互为补充。 Marble Maze 使用媒体基础从文件加载音频资源，并使用 XAudio2 播放音频。 你无需使用媒体基础加载音频资源。 但是，如果已有适用于通用 Windows 平台 (UWP) 应用的音频资源加载机制，请使用它。 [音频、视频和相机](../audio-video-camera/index.md)讨论在 UWP 应用中实现音频的几种方法。

有关 XAudio2 的详细信息，请参阅[编程指南](https://msdn.microsoft.com/library/windows/desktop/ee415737)。 有关媒体基础的详细信息，请参阅 [Microsoft 媒体基础](https://msdn.microsoft.com/library/windows/desktop/ms694197)。

## <a name="initializing-audio-resources"></a>初始化音频资源

Marble Maze 使用 Windows Media 音频 (.wma) 文件作为背景音乐，使用 WAV (.wav) 文件作为游戏声音。 媒体基础支持这些格式。 尽管 .wav 文件格式受 XAudio2 本机支持，但游戏需要手动解析文件格式，以填充合适的 XAudio2 数据结构。 Marble Maze 使用媒体基础可更轻松地处理 .wav 文件。 有关媒体基础支持的媒体格式的完整列表，请参阅[媒体基础中支持的媒体格式](https://msdn.microsoft.com/library/windows/desktop/dd757927)。 Marble Maze 不使用独立的设计时和运行时音频格式，也不使用 XAudio2 ADPCM 压缩支持。 有关 XAudio2 中的 ADPCM 压缩的详细信息，请参阅 [ADPCM 概述](https://msdn.microsoft.com/library/windows/desktop/ee415711)。

**Audio::CreateResources** 方法（从 **MarbleMazeMain::LoadDeferredResources** 调用）从文件加载音频流、初始化 XAudio2 引擎对象，并创建源、子混合和主语音。

### <a name="creating-the-xaudio2-engines"></a>创建 XAudio2 引擎

回想一下，Marble Maze 创建一个 [IXAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415908) 对象来表示它使用的每个音频引擎。 若要创建音频引擎，请调用 [XAudio2Create](https://msdn.microsoft.com/library/windows/desktop/ee419212) 方法。 以下示例展示了 Marble Maze 如何创建可处理背景音乐的音频引擎。

```cpp
// In Audio.h
class Audio
{
private:
    IXAudio2*                   m_musicEngine;
// ...
}

// In Audio.cpp
void Audio::CreateResources()
{
    try
    {
        // ...
        DX::ThrowIfFailed(
            XAudio2Create(&m_musicEngine)
            );
        // ...
    }
    // ...
}
```

Marble Maze 执行一个类似的步骤来创建可播放游戏声音的音频引擎。

在 UWP 应用中与在桌面应用中使用 [IXAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415908) 接口的方式有两个方面的区别。 首先，无需在调用 [XAudio2Create](https://msdn.microsoft.com/library/windows/desktop/ms695279) 之前调用 [CoInitializeEx](https://msdn.microsoft.com/library/windows/desktop/ee419212)。 此外，**IXAudio2** 不再支持设备枚举。 有关如何枚举音频设备的信息，请参阅[枚举设备](https://msdn.microsoft.com/library/windows/apps/hh464977)。

### <a name="creating-the-mastering-voices"></a>创建主语音

以下示例展示了 **Audio::CreateResources** 方法如何使用 [IXAudio2::CreateMasteringVoice](https://msdn.microsoft.com/library/windows/desktop/hh405048) 方法为背景音乐创建主语音。 在此示例中，**m\_musicMasteringVoice** 是一个 [IXAudio2MasteringVoice](https://msdn.microsoft.com/library/windows/desktop/ee415912) 对象。 我们指定两个输入通道；这可以简化混响效果的逻辑。 

我们指定输入采样率为 48000。 我们选择这个采样率是因为它代表着音频质量和所需 CPU 处理量之间的一种平衡。 更高的采样率需要更多的 CPU 处理量，但不会出现明显的质量提升。 

最后，我们指定 **AudioCategory_GameMedia** 作为音频流类别，以便用户在玩游戏时可通过不同的应用程序听音乐。 当音乐应用运行时，Windows 会将 **AudioCategory\_GameMedia** 选项创建的所有语音设置为静音。 用户仍会听到游戏开始声音，因为它们是通过 **AudioCategory\_GameEffects** 选项创建的。 有关音频类别的详细信息，请参阅 [AUDIO\_STREAM\_CATEGORY](https://msdn.microsoft.com/library/windows/desktop/hh404178)。

```cpp
// This sample plays the equivalent of background music, which we tag on the  
// mastering voice as AudioCategory_GameMedia. In ordinary usage, if we were  
// playing the music track with no effects, we could route it entirely through 
// Media Foundation. Here, we are using XAudio2 to apply a reverb effect to the 
// music, so we use Media Foundation to decode the data then we feed it through 
// the XAudio2 pipeline as a separate Mastering Voice, so that we can tag it 
// as Game Media. We default the mastering voice to 2 channels to simplify  
// the reverb logic.
DX::ThrowIfFailed(
    m_musicEngine->CreateMasteringVoice(
        &m_musicMasteringVoice,
        2,
        48000,
        0,
        nullptr,
        nullptr,
        AudioCategory_GameMedia
        )
);
```

**Audio::CreateResources** 方法执行类似的步骤为游戏开始声音创建主语音，但它为 *StreamCategory* 参数指定了 **AudioCategory\_GameEffects**，这是默认设置。

### <a name="creating-the-reverb-effect"></a>创建混响效果

对于每种语音，可使用 XAudio2 创建处理音频的效果序列。 这种序列称为效果链。 希望向一个语音应用一种或多种效果时，可使用效果链。 效果链可能是破坏性的；也就是说，链中的每个效果可能覆盖音频缓冲区。 此属性很重要，因为 XAudio2 无法保证输出缓冲区最初是静音的。 效果对象在 XAudio2 中由跨平台音频处理对象 (XAPO) 表示。 有关 XAPO 的详细信息，请参阅 [XAPO 概述](https://msdn.microsoft.com/library/windows/desktop/ee415735)。

创建效果链时，执行以下步骤：

1. 创建效果对象。

2. 使用效果数据填充 [XAUDIO2\_EFFECT\_DESCRIPTOR](https://msdn.microsoft.com/library/windows/desktop/ee419236) 结构。

3. 使用数据填充 [XAUDIO2\_EFFECT\_CHAIN](https://msdn.microsoft.com/library/windows/desktop/ee419235) 结构。

4. 向一个语音应用效果链。

5. 填充一个效果参数结构并将它应用到效果上。

6. 在适当时禁用或启用效果。

**Audio** 类定义 **CreateReverb** 方法来创建可实现混响的效果链。 此方法调用 [XAudio2CreateReverb](https://msdn.microsoft.com/library/windows/desktop/ee419213) 方法创建 **ComPtr&lt;IUnknown&gt;** 对象 **soundEffectXAPO**，该对象可用作混响效果的子混合语音。

```cpp
Microsoft::WRL::ComPtr<IUnknown> soundEffectXAPO;

DX::ThrowIfFailed(
    XAudio2CreateReverb(&soundEffectXAPO)
    );
```

[XAUDIO2\_EFFECT\_DESCRIPTOR](https://msdn.microsoft.com/library/windows/desktop/ee419236) 结构包含要在效果链中使用的 XAPO 的信息，例如，输出通道的目标编号。 **Audio::CreateReverb** 方法创建一个设置为禁用状态的 **XAUDIO2\_EFFECT\_DESCRIPTOR** 对象 **soundEffectdescriptor**、使用两个输出通道，并引用 **soundEffectXAPO** 实现混响效果。 **soundEffectdescriptor** 对象最初为禁用状态，因为游戏必须在该效果开始修改游戏声音之前设置参数。 Marble Maze 使用两个输出通道来简化混响效果的逻辑。

```cpp
soundEffectdescriptor.InitialState = false;
soundEffectdescriptor.OutputChannels = 2;
soundEffectdescriptor.pEffect = soundEffectXAPO.Get();
```

如果效果链有多个效果，则每个效果需要一个对象。 [XAUDIO2\_EFFECT\_CHAIN](https://msdn.microsoft.com/library/windows/desktop/ee419235) 结构包含参与该效果的 [XAUDIO2\_EFFECT\_DESCRIPTOR](https://msdn.microsoft.com/library/windows/desktop/ee419236) 对象的数组。 以下示例展示了 **Audio::CreateReverb** 方法如何指定一种实现混响的效果。

```cpp
XAUDIO2_EFFECT_CHAIN soundEffectChain;

// ...

soundEffectChain.EffectCount = 1;
soundEffectChain.pEffectDescriptors = &soundEffectdescriptor;
```

**Audio::CreateReverb** 方法调用 [IXAudio2::CreateSubmixVoice](https://msdn.microsoft.com/library/windows/desktop/ee418608) 方法来为该效果创建子混合语音。 它为 *pEffectChain* 参数指定了 [XAUDIO2\_EFFECT\_CHAIN](https://msdn.microsoft.com/library/windows/desktop/ee419235) 对象 **soundEffectChain**，以将效果链与该语音关联。 Marble Maze 还指定两个输出通道和 48 千赫的采样率。

```cpp
DX::ThrowIfFailed(
    engine->CreateSubmixVoice(newSubmix, 2, 48000, 0, 0, nullptr, &soundEffectChain)
    );
```

> [!TIP]
> 如果希望将一个现有的效果链附加到一个现有的子混合语音，或者希望替换当前效果链，可使用 [IXAudio2Voice::SetEffectChain](https://msdn.microsoft.com/library/windows/desktop/ee418594) 方法。

**Audio::CreateReverb** 方法调用 [IXAudio2Voice::SetEffectParameters](https://msdn.microsoft.com/library/windows/desktop/ee418595) 来设置其他与该效果关联的参数。 此方法接受一种特定于该效果的参数结构。 在 **Audio::Initialize** 方法中初始化一个 [XAUDIO2FX\_REVERB\_PARAMETERS](https://msdn.microsoft.com/library/windows/desktop/ee419224) 对象 **m_reverbParametersSmall**（它包含混响的效果参数），因为每种混响效果共享相同的参数。 下面的示例展示了 **Audio::Initialize** 方法如何初始化近场混响的混响参数。

```cpp
m_reverbParametersSmall.ReflectionsDelay = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_DELAY;
m_reverbParametersSmall.ReverbDelay = XAUDIO2FX_REVERB_DEFAULT_REVERB_DELAY;
m_reverbParametersSmall.RearDelay = XAUDIO2FX_REVERB_DEFAULT_REAR_DELAY;
m_reverbParametersSmall.PositionLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionRight = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionMatrixLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.PositionMatrixRight = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.EarlyDiffusion = 4;
m_reverbParametersSmall.LateDiffusion = 15;
m_reverbParametersSmall.LowEQGain = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_GAIN;
m_reverbParametersSmall.LowEQCutoff = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_CUTOFF;
m_reverbParametersSmall.HighEQGain = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_GAIN;
m_reverbParametersSmall.HighEQCutoff = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_CUTOFF;
m_reverbParametersSmall.RoomFilterFreq = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_FREQ;
m_reverbParametersSmall.RoomFilterMain = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_MAIN;
m_reverbParametersSmall.RoomFilterHF = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_HF;
m_reverbParametersSmall.ReflectionsGain = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_GAIN;
m_reverbParametersSmall.ReverbGain = XAUDIO2FX_REVERB_DEFAULT_REVERB_GAIN;
m_reverbParametersSmall.DecayTime = XAUDIO2FX_REVERB_DEFAULT_DECAY_TIME;
m_reverbParametersSmall.Density = XAUDIO2FX_REVERB_DEFAULT_DENSITY;
m_reverbParametersSmall.RoomSize = XAUDIO2FX_REVERB_DEFAULT_ROOM_SIZE;
m_reverbParametersSmall.WetDryMix = XAUDIO2FX_REVERB_DEFAULT_WET_DRY_MIX;
m_reverbParametersSmall.DisableLateField = TRUE;
```

此示例为大多数混响参数都使用了默认值，但它将 **DisableLateField** 设置为 TRUE 来指定近场混响，将 **EarlyDiffusion** 设置为 4 来模拟附近的平坦表面，将 **LateDiffusion** 设置为 15 来模拟远距离的漫反射表面。 附近的平坦表面导致回声更快且更响亮地传到你耳中，远距离的漫反射表面导致回声更安静、更慢地传到你耳中。 你可试验不同的混响值来在游戏中获得想要的效果，或者使用 **ReverbConvertI3DL2ToNative** 方法来使用行业标准的 I3DL2 (Interactive 3D Audio Rendering Guidelines Level 2.0) 参数。

下面的示例展示了 **Audio::CreateReverb** 如何设置混响参数。 **newSubmix** 是一个 [IXAudio2SubmixVoice](https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.ixaudio2submixvoice.ixaudio2submixvoice)** 对象。 **parameters** 是一个 [XAUDIO2FX\_REVERB\_PARAMETERS](https://msdn.microsoft.com/library/windows/desktop/ee419224)* 对象。

```cpp
DX::ThrowIfFailed(
    (*newSubmix)->SetEffectParameters(0, parameters, sizeof(m_reverbParametersSmall))
    );
```

**Audio::CreateReverb** 方法最后会使用 [IXAudio2Voice::EnableEffect](https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.ixaudio2voice.ixaudio2voice.enableeffect) 启用该效果（如果设置了 **enableEffect** 标志）。 它还将使用 [IXAudio2Voice::SetVolume](https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.ixaudio2voice.ixaudio2voice.setvolume) 来设置卷，并使用 [IXAudio2Voice::SetOutputMatrix](https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.ixaudio2voice.ixaudio2voice.setoutputmatrix) 设置输出矩阵。 这部分将音量设置为最大 (1.0)，然后将左侧和右侧输入及左侧和右侧输出扬声器的音量矩阵设置为静音。 我们这么做是因为，其他代码以后会在两种混响之间交叉淡入淡出（模拟从靠近一面墙壁到处于大空间的过渡），或者在需要时静音两种混响。 混响路径在以后被取消静音时，游戏设置一个矩阵 {1.0f, 0.0f, 0.0f, 1.0f} 来将左侧混响输出路由到主语音的左侧输入，将右侧混响输出路由到主语音的右侧输入。

```cpp
if (enableEffect)
{
    DX::ThrowIfFailed(
        (*newSubmix)->EnableEffect(0)
        );    
}

DX::ThrowIfFailed(
    (*newSubmix)->SetVolume (1.0f)
    );

float outputMatrix[4] = {0, 0, 0, 0};
DX::ThrowIfFailed(
    (*newSubmix)->SetOutputMatrix(masteringVoice, 2, 2, outputMatrix)
    );
```

Marble Maze 调用 **Audio::CreateReverb** 方法四次：两次用于背景音乐，两次用于游戏声音。 以下示例展示了 Marble Maze 如何调用 **CreateReverb** 方法来用于背景音乐。

```cpp
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersSmall, 
    &m_musicReverbVoiceSmallRoom, 
    true
    );
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersLarge, 
    &m_musicReverbVoiceLargeRoom, 
    true
    );
```

有关用于 XAudio2 的效果的可能来源列表，请参阅 [XAudio2 音频效果](https://msdn.microsoft.com/library/windows/desktop/ee415756)。

### <a name="loading-audio-data-from-file"></a>从文件加载音频数据

Marble Maze 定义 **MediaStreamer** 类，它使用媒体基础从文件加载音频资源。 Marble Maze 使用一个 **MediaStreamer** 对象加载每个音频文件。

Marble Maze 调用 **MediaStreamer::Initialize** 方法来初始化每个音频流。 下面展示了 **Audio::CreateResources** 方法如何调用 **MediaStreamer::Initialize** 来初始化背景音乐的音频流：

```cpp
// Media Foundation is a convenient way to get both file I/O and format decode for 
// audio assets. You can replace the streamer in this sample with your own file I/O 
// and decode routines.
m_musicStreamer.Initialize(L"Media\\Audio\\background.wma");
```

**MediaStreamer::Initialize** 方法首先调用 [MFStartup](https://msdn.microsoft.com/library/windows/desktop/ms702238) 方法来初始化媒体基础。 **MF_VERSION** 是在 **mfapi.h** 中定义的宏，是我们指定的要使用的媒体基础版本。

```cpp
DX::ThrowIfFailed(
    MFStartup(MF_VERSION)
    );
```

然后，**MediaStreamer::Initialize** 调用 [MFCreateSourceReaderFromURL](https://msdn.microsoft.com/library/windows/desktop/dd388110) 来创建 [IMFSourceReader](https://msdn.microsoft.com/library/windows/desktop/dd374655) 对象。 **IMFSourceReader** 对象 **m_reader** 从 **url** 指定的文件读取媒体数据。

```cpp
DX::ThrowIfFailed(
    MFCreateSourceReaderFromURL(url, nullptr, &m_reader)
    );
```

然后，**MediaStreamer::Initialize** 方法使用 [MFCreateMediaType](https://msdn.microsoft.com/library/windows/desktop/ms693861) 创建 [IMFMediaType](https://msdn.microsoft.com/library/windows/desktop/ms704850) 对象来描述音频流的格式。 音频格式有两种类型：主要类型和子类型。 主要类型定义媒体的总体格式，例如视频、音频、脚本等。 子类型定义格式，例如 PCM、ADPCM 或 WMA。

**MediaStreamer::Initialize** 方法使用 [IMFAttributes::SetGUID](https://msdn.microsoft.com/library/windows/desktop/bb970530) 方法将主要类型 ([MF_MT_MAJOR_TYPE](https://msdn.microsoft.com/library/windows/desktop/ms702272)) 指定为音频 (**MFMediaType\_Audio**)，将次要类型 ([MF_MT_SUBTYPE](https://msdn.microsoft.com/library/windows/desktop/ms700208)) 指定为未压缩的 PCM 音频 (**MFAudioFormat\_PCM**)。 **MF_MT_MAJOR_TYPE** 和 **MF_MT_SUBTYPE** 是[媒体基础属性](https://msdn.microsoft.com/library/windows/desktop/ms696989)。 **MFMediaType_Audio** 和 **MFAudioFormat_PCM** 是类型和子类型 GUID；请参阅[音频媒体类型](https://msdn.microsoft.com/library/windows/desktop/bb530108)了解详细信息。 [IMFSourceReader::SetCurrentMediaType](https://msdn.microsoft.com/library/windows/desktop/dd374667) 方法将媒体类型与流读取器关联。

```cpp
// Set the decoded output format as PCM. 
// XAudio2 on Windows can process PCM and ADPCM-encoded buffers. 
// When this sample uses Media Foundation, it always decodes into PCM.

DX::ThrowIfFailed(
    MFCreateMediaType(&mediaType)
    );

DX::ThrowIfFailed(
    mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
    );

DX::ThrowIfFailed(
    mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
    );

DX::ThrowIfFailed(
    m_reader->SetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, 0, mediaType.Get())
    );
```

然后，**MediaStreamer::Initialize** 方法使用 [IMFSourceReader::GetCurrentMediaType](https://msdn.microsoft.com/library/windows/desktop/dd374660) 从媒体基础获取完整的输出媒体格式，并调用 [MFCreateWaveFormatExFromMFMediaType](https://msdn.microsoft.com/library/windows/desktop/ms702177) 方法来将媒体基础音频媒体类型转换为一种 [WAVEFORMATEX](https://msdn.microsoft.com/library/windows/hardware/ff538799) 结构。 **WAVEFORMATEX** 结构定义波形音频数据的格式。 Marble Maze 使用此结构创建源语音并向弹珠滚动声音应用低通筛选器。

```cpp
// Get the complete WAVEFORMAT from the Media Type.
DX::ThrowIfFailed(
    m_reader->GetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, &outputMediaType)
    );

uint32 formatSize = 0;
WAVEFORMATEX* waveFormat;
DX::ThrowIfFailed(
    MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &formatSize)
    );
CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
CoTaskMemFree(waveFormat);
```

> [!IMPORTANT]
> [MFCreateWaveFormatExFromMFMediaType](https://msdn.microsoft.com/library/windows/desktop/ms702177) 方法使用 **CoTaskMemAlloc** 分配 [WAVEFORMATEX](https://msdn.microsoft.com/library/windows/hardware/ff538799) 对象。 因此，请确保在使用完此对象时调用 **CoTaskMemFree**。

 

**MediaStreamer::Initialize** 方法最后计算流的长度 **m\_maxStreamLengthInBytes**（以字节为单位）。 为此，它调用 [IMFSourceReader::GetPresentationAttribute](https://msdn.microsoft.com/library/windows/desktop/dd374662) 方法来获取以 100 纳秒为单位的音频流持续时间、将持续时间转换为节，然后乘以平均数据传输速率（以字节每秒为单位）。 Marble Maze 稍后使用此值分配可存储每种游戏声音的缓冲区。

```cpp
// Get the total length of the stream, in bytes.
PROPVARIANT var;
DX::ThrowIfFailed(
    m_reader->
        GetPresentationAttribute(MF_SOURCE_READER_MEDIASOURCE, MF_PD_DURATION, &var)
    );

// duration is in 100ns units; convert to seconds, and round up
// to the nearest whole byte.
ULONGLONG duration = var.uhVal.QuadPart;
m_maxStreamLengthInBytes =
    static_cast<unsigned int>(
        ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000)
        / 10000000
        );
```

### <a name="creating-the-source-voices"></a>创建源语音

Marble Maze 创建 XAudio2 源语音来播放源语音中的每种游戏声音和音乐。 **Audio** 类为背景音乐定义 [IXAudio2SourceVoice](https://msdn.microsoft.com/library/windows/desktop/ee415914) 对象，并定义 **SoundEffectData** 对象数组来保留游戏声音。 **SoundEffectData** 结构保留效果的 **IXAudio2SourceVoice** 对象，还定义了其他与效果相关的数据，例如音频缓冲区。 **Audio.h** 定义 **SoundEvent** 枚举。 Marble Maze 使用此枚举识别每种游戏声音。 **Audio** 类也使用此枚举来为 **SoundEffectData** 对象数组编制索引。

```cpp
enum SoundEvent
{
    RollingEvent        = 0,
    FallingEvent        = 1,
    CollisionEvent      = 2,
    CheckpointEvent     = 3,
    MenuChangeEvent     = 4,
    MenuSelectedEvent   = 5,
    LastSoundEvent,
};
```

下表给出了其中每个值之间的关系、包含关联的声音数据的文件，以及每种声音有何含义的简短描述。 音频文件位于 **\\Media\\Audio** 文件夹中。

| SoundEvent 值  | 文件名      | 描述                                              |
|-------------------|----------------|----------------------------------------------------------|
| RollingEvent      | MarbleRoll.wav | 在弹珠滚动时播放。                              |
| FallingEvent      | MarbleFall.wav | 在弹珠从迷宫掉落时播放。               |
| CollisionEvent    | MarbleHit.wav  | 在弹珠与迷宫碰撞时播放。           |
| CheckpointEvent   | Checkpoint.wav | 在弹珠通过一个检查点时播放。         |
| MenuChangeEvent   | MenuChange.wav | 在用户更改当前菜单项时播放。 |
| MenuSelectedEvent | MenuSelect.wav | 在用户选择一个菜单项时播放。           |

 

下面的示例展示了 **Audio::CreateResources** 方法如何为背景音乐创建源语音。 [XAUDIO2\_SEND\_DESCRIPTOR](https://msdn.microsoft.com/library/windows/desktop/ee419244) 结构定义来自另一个语音的目标语音，并指定是否应使用筛选器。 Marble Maze 调用 **Audio::SetSoundEffectFilter** 方法，以使用筛选器在球滚动时更改其声音。 [XAUDIO2\_VOICE\_SENDS](https://msdn.microsoft.com/library/windows/desktop/ee419246) 结构定义要从单个输出语音接收数据的一组语音。 Marble Maze 将数据从源语音发送到主语音（对于枯燥或保持不变的游戏声音部分）和两个子混合语音（用于实现有趣或混响的游戏声音部分）。

[IXAudio2::CreateSourceVoice](https://msdn.microsoft.com/library/windows/desktop/ee418607) 方法可创建和配置源语音。 它利用 [WAVEFORMATEX](https://msdn.microsoft.com/library/windows/hardware/ff538799) 结构来定义将发送给语音的音频缓冲区的格式。 如前面所述，Marble Maze 使用 PCM 格式。

```cpp
XAUDIO2_SEND_DESCRIPTOR descriptors[3];
descriptors[0].pOutputVoice = m_musicMasteringVoice;
descriptors[0].Flags = 0;
descriptors[1].pOutputVoice = m_musicReverbVoiceSmallRoom;
descriptors[1].Flags = 0;
descriptors[2].pOutputVoice = m_musicReverbVoiceLargeRoom;
descriptors[2].Flags = 0;
XAUDIO2_VOICE_SENDS sends = {0};
sends.SendCount = 3;
sends.pSends = descriptors;
WAVEFORMATEX& waveFormat = m_musicStreamer.GetOutputWaveFormatEx();

DX::ThrowIfFailed(
    m_musicEngine->CreateSourceVoice(&m_musicSourceVoice, &waveFormat, 0, 1.0f, &m_voiceContext, &sends, nullptr)
    );

DX::ThrowIfFailed(
    m_musicMasteringVoice->SetVolume(0.4f)
    );
```

## <a name="playing-background-music"></a>播放背景音乐


创建一个停止状态的源语音。 Marble Maze 启动游戏循环中的背景音乐。 首次调用 **MarbleMazeMain::Update** 会调用 **Audio::Start** 来启动背景音乐。

```cpp
if (!m_audio.m_isAudioStarted)
{
    m_audio.Start();
}
```

**Audio::Start** 方法调用 [IXAudio2SourceVoice::Start](https://msdn.microsoft.com/library/windows/desktop/ee418471) 开始处理背景音乐的源语音。

```cpp
void Audio::Start()
{     
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicSourceVoice->Start(0);

    if SUCCEEDED(hr) {
        m_isAudioStarted = true;
    }
    else
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

源语音将该音频数据传递到音频图的下一个阶段。 对于 Marble Maze，下一个阶段包含两个将混响效果应用到音频的子混合语音。 一个子混合语音应用一种近迟场混响；第二个应用一种远迟场混响。

每个子混合语音对最终混合体的贡献量取决于空间的大小和形状。 在球靠近一面墙壁或处于小空间中时近场混响贡献得更多；在球处于大空间中时迟场混响贡献得更多。 弹珠在迷宫中移动时，此技术会生成一种更加逼真的回声效果。 若要了解有关 Marble Maze 如何实现此效果的详细信息，请参阅 Marble Maze 源代码中的 **Audio::SetRoomSize** 和 **Physics::CalculateCurrentRoomSize**。

> [!NOTE]
> 在大部分空间大小基本都相同的游戏中，可使用一种更加基本的混响模型。 例如，可为所有空间使用一种混响设置，或者可为每个空间创建一种预定义的混响设置。

**Audio::CreateResources** 方法使用媒体基础加载背景音乐。 但是，此时源语音还没有可使用的音频数据。 此外，因为背景音乐会循环播放，所以源语音必须定期更新数据，以便音乐继续播放。

为了不断向源语音中填充数据，游戏循环会在每个帧中更新音频缓冲区。 **MarbleMazeMain::Render** 方法调用 **Audio::Render** 来处理背景音乐音频缓冲区。 **Audio** 类定义了一个包含三个音频缓冲区的数组 **m\_audioBuffers**。 每个缓冲区保留 64 KB（65536 字节）数据。 该循环从媒体基础对象读取数据并将其写入源语音，直到源语音有 3 个排队的缓冲区。

> [!CAUTION]
> 尽管 Marble Maze 使用一个 64 KB 缓冲区来存储音乐数据，但你可能需要更大或更小的缓冲区。 具体容量取决于游戏的需求。

```cpp
// This sample processes audio buffers during the render cycle of the application.
// As long as the sample maintains a high-enough frame rate, this approach should
// not glitch audio. In game code, it is best for audio buffers to be processed
// on a separate thread that is not synced to the main render loop of the game.
void Audio::Render()
{
    if (m_engineExperiencedCriticalError)
    {
        m_engineExperiencedCriticalError = false;
        ReleaseResources();
        Initialize();
        CreateResources();
        Start();
        if (m_engineExperiencedCriticalError)
        {
            return;
        }
    }

    try
    {
        bool streamComplete;
        XAUDIO2_VOICE_STATE state;
        uint32 bufferLength;
        XAUDIO2_BUFFER buf = {0};

        // Use MediaStreamer to stream the buffers.
        m_musicSourceVoice->GetState(&state);
        while (state.BuffersQueued <= MAX_BUFFER_COUNT - 1)
        {
            streamComplete = m_musicStreamer.GetNextBuffer(
                m_audioBuffers[m_currentBuffer],
                STREAMING_BUFFER_SIZE,
                &bufferLength
                );

            if (bufferLength > 0)
            {
                buf.AudioBytes = bufferLength;
                buf.pAudioData = m_audioBuffers[m_currentBuffer];
                buf.Flags = (streamComplete) ? XAUDIO2_END_OF_STREAM : 0;
                buf.pContext = 0;
                DX::ThrowIfFailed(
                    m_musicSourceVoice->SubmitSourceBuffer(&buf)
                    );

                m_currentBuffer++;
                m_currentBuffer %= MAX_BUFFER_COUNT;
            }

            if (streamComplete)
            {
                // Loop the stream.
                m_musicStreamer.Restart();
                break;
            }

            m_musicSourceVoice->GetState(&state);
        }
    }
    catch (...)
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

游戏循环还处理媒体基础对象何时到达流末端。 在本例中，它调用 [IMFSourceReader::SetCurrentPosition](https://msdn.microsoft.com/library/windows/desktop/dd374668) 方法重置音频源的位置。

```cpp
void MediaStreamer::Restart()
{
    if (m_reader == nullptr)
    {
        return;
    }

    PROPVARIANT var = {0};
    var.vt = VT_I8;

    DX::ThrowIfFailed(
        m_reader->SetCurrentPosition(GUID_NULL, var)
        );
}
```

若要为单个缓冲区（或一个完全加载到内存中的完整声音）实现音频循环，可在初始化声音时将 [XAUDIO2_BUFFER](https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.xaudio2.xaudio2_buffer)::LoopCount 字段设置为 **XAUDIO2\_LOOP\_INFINITE**。 Marble Maze 使用此技术播放弹珠的滚动声音。

```cpp
if (sound == RollingEvent)
{
    m_soundEffects[sound].m_audioBuffer.LoopCount = XAUDIO2_LOOP_INFINITE;
}
```

但是，对于背景音乐，Marble Maze 直接管理缓冲区，以便它能更好地控制所用的内存量。 音乐文件很大时，你可将音乐数据传入更小的缓冲区。 执行此操作可帮助平衡内存大小与游戏处理和流式传输音频数据的频率。

> [!TIP]
> 如果游戏具有较低或不断变化的帧速率，在主线程上处理音频可能在音频中产生意外的暂停或爆裂声，因为音频引擎没有足够的缓冲音频数据可供处理。 如果游戏对此问题很敏感，可考虑在一个不执行呈现的单独线程上处理音频。 此方法在拥有多个处理器的计算机上尤其有用，因为你的游戏可使用空闲的处理器。

## <a name="reacting-to-game-events"></a>对游戏事件做出反应

**Audio** 类可提供多种方法（例如 **PlaySoundEffect**、**IsSoundEffectStarted**、**StopSoundEffect**、**SetSoundEffectVolume**、**SetSoundEffectPitch** 和 **SetSoundEffectFilter**）来使游戏能够控制声音何时播放和停止，控制音量和音高等声音属性。 例如，如果弹珠从迷宫落下，**MarbleMazeMain::Update** 会调用 **Audio::PlaySoundEffect** 方法来播放 **FallingEvent** 声音。

```cpp
m_audio.PlaySoundEffect(FallingEvent);
```

**Audio::PlaySoundEffect** 方法会调用 [IXAudio2SourceVoice::Start](https://msdn.microsoft.com/library/windows/desktop/ee418471) 方法来开始回放声音。 如果已调用 **IXAudio2SourceVoice::Start** 方法，它不会再次启动。 然后，**Audio::PlaySoundEffect** 对某些声音执行自定义逻辑。

```cpp
void Audio::PlaySoundEffect(SoundEvent sound)
{
    XAUDIO2_BUFFER buf = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (m_engineExperiencedCriticalError)
    {
        // If there's an error, then we'll recreate the engine on the next
        // render pass.
        return;
    }

    SoundEffectData* soundEffect = &m_soundEffects[sound];
    HRESULT hr = soundEffect->m_soundEffectSourceVoice->Start();

    if FAILED(hr)
    {
        m_engineExperiencedCriticalError = true;
        return;
    }

    // For one-off voices, submit a new buffer if there's none queued up,
    // and allow up to two collisions to be queued up. 
    if (sound != RollingEvent)
    {
        XAUDIO2_VOICE_STATE state = {0};

        soundEffect->m_soundEffectSourceVoice->
            GetState(&state, XAUDIO2_VOICE_NOSAMPLESPLAYED);

        if (state.BuffersQueued == 0)
        {
            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }
        else if (state.BuffersQueued < 2 && sound == CollisionEvent)
        {
            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }

        // For the menu clicks, we want to stop the voice and replay the click
        // right away.
        // Note that stopping and then flushing could cause a glitch due to the
        // waveform not being at a zero-crossing, but due to the nature of the 
        // sound (fast and 'clicky'), we don't mind.
        if (state.BuffersQueued > 0 && sound == MenuChangeEvent)
        {
            soundEffect->m_soundEffectSourceVoice->Stop();
            soundEffect->m_soundEffectSourceVoice->FlushSourceBuffers();

            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);

            soundEffect->m_soundEffectSourceVoice->Start();
        }
    }

    m_soundEffects[sound].m_soundEffectStarted = true;
}
```

对于滚动以外的声音，**Audio::PlaySoundEffect** 方法调用 [IXAudio2SourceVoice::GetState](https://msdn.microsoft.com/library/windows/desktop/hh405047) 来确定播放源语音的缓冲区数量。 它调用 [IXAudio2SourceVoice::SubmitSourceBuffer](https://msdn.microsoft.com/library/windows/desktop/ee418473) 将声音的音频数据添加到语音的输入队列中，前提是没有活动的缓冲区。 **Audio::PlaySoundEffect** 方法还支持碰撞声音按顺序播放两次。 例如，在弹珠与迷宫的一角碰撞时就会发生此情况。

如前面所述，Audio 类使用在初始化滚动事件的声音时会使用 **XAUDIO2\_LOOP\_INFINITE** 标志。 该声音在第一次为此事件调用 **Audio::PlaySoundEffect** 时开始循环回放。 为了简化滚动声音的回放逻辑，Marble Maze 将声音设置为静音而不是停止它。 在弹珠改变速度时，Marble Maze 更改声音的音高和音量，以提供一种更加逼真的效果。 下面展示了 **MarbleMazeMain::Update** 方法如何在弹珠的速度改变时更新它的音高和音量，以及它如何在弹珠停止时通过将音量设置为零来将声音设置为静音。

```cpp
// Play the roll sound only if the marble is actually rolling.
if (ci.isRollingOnFloor && volume > 0)
{
    if (!m_audio.IsSoundEffectStarted(RollingEvent))
    {
        m_audio.PlaySoundEffect(RollingEvent);
    }

    // Update the volume and pitch by the velocity.
    m_audio.SetSoundEffectVolume(RollingEvent, volume);
    m_audio.SetSoundEffectPitch(RollingEvent, pitch);

    // The rolling sound has at most 8000Hz sounds, so we linearly
    // ramp up the low-pass filter the faster we go.
    // We also reduce the Q-value of the filter, starting with a
    // relatively broad cutoff and get progressively tighter.
    m_audio.SetSoundEffectFilter(
        RollingEvent,
        600.0f + 8000.0f * volume,
        XAUDIO2_MAX_FILTER_ONEOVERQ - volume*volume
        );
}
else
{
    m_audio.SetSoundEffectVolume(RollingEvent, 0);
}
```

## <a name="reacting-to-suspend-and-resume-events"></a>对暂停和恢复事件做出反应

[Marble Maze 应用程序结构](marble-maze-application-structure.md)描述了 Marble Maze 如何支持暂停和恢复。 游戏暂停时，游戏会暂停音频。 游戏恢复时，游戏会从暂停的地方恢复音频。 我们在执行此操作时遵循以下最佳做法：在知道不需要资源时就不使用它们。

**Audio::SuspendAudio** 方法在游戏暂停时调用。 此方法调用 [IXAudio2::StopEngine](https://msdn.microsoft.com/library/windows/desktop/ee418628) 方法来停止所有音频。 尽管 **IXAudio2::StopEngine** 立即停止所有音频输出，但它保留了音频图及其效果参数（例如在弹珠弹跳时应用的混响效果）。

```cpp
// Uses the IXAudio2::StopEngine method to stop all audio immediately.  
// It leaves the audio graph untouched, which preserves all effect parameters   
// and effect histories (like reverb effects) voice states, pending buffers,  
// cursor positions and so on. 
// When the engines are restarted, the resulting audio will sound as if it had  
// never been stopped except for the period of silence. 
void Audio::SuspendAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    if (m_isAudioStarted)
    {
        m_musicEngine->StopEngine();
        m_soundEffectEngine->StopEngine();
    }

    m_isAudioStarted = false;
}
```

**Audio::ResumeAudio** 方法在游戏恢复时调用。 此方法使用 [IXAudio2::StartEngine](https://msdn.microsoft.com/library/windows/desktop/ee418626) 方法来重启音频。 因为对 [IXAudio2::StopEngine](https://msdn.microsoft.com/library/windows/desktop/ee418628) 的调用保留音频图及其效果参数，所以音频输出会从上次停止的地方恢复。

```cpp
// Restarts the audio streams. A call to this method must match a previous call
// to SuspendAudio. This method causes audio to continue where it left off.
// If there is a problem with the restart, the m_engineExperiencedCriticalError
// flag is set. The next call to Render will recreate all the resources and
// reset the audio pipeline.
void Audio::ResumeAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicEngine->StartEngine();
    HRESULT hr2 = m_soundEffectEngine->StartEngine();

    if (FAILED(hr) || FAILED(hr2))
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

## <a name="handling-headphones-and-device-changes"></a>处理耳机和设备更改

Marble Maze 使用引擎回调来处理 XAudio2 引擎失败，例如在音频设备更改时。 设备更改的一种可能原因是，游戏用户连接耳机或断开耳机连接。 我们建议你实现可处理设备更改的引擎回调。 否则，在用户插入或移除耳机时，游戏将停止播放声音，直到重新启动游戏。

**Audio.h** 定义 **AudioEngineCallbacks** 类。 该类实现 [IXAudio2EngineCallback](https://msdn.microsoft.com/library/windows/desktop/ee415910) 接口。

```cpp
class AudioEngineCallbacks: public IXAudio2EngineCallback
{
private:
    Audio* m_audio;

public :
    AudioEngineCallbacks(){};
    void Initialize(Audio* audio);

    // Called by XAudio2 just before an audio processing pass begins.
    void _stdcall OnProcessingPassStart(){};

    // Called just after an audio processing pass ends.
    void  _stdcall OnProcessingPassEnd(){};

    // Called when a critical system error causes XAudio2
    // to be closed and restarted. The error code is given in Error.
    void  _stdcall OnCriticalError(HRESULT Error);
};
```

[IXAudio2EngineCallback](https://msdn.microsoft.com/library/windows/desktop/ee415910) 接口使你的代码在发生音频处理事件时和引擎遇到致命错误时获得通知。 为注册回调，Marble Maze 在为音乐引擎创建 [IXAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415908) 对象后调用 **Audio::CreateResources** 中的 [IXAudio2::RegisterForCallbacks](https://msdn.microsoft.com/library/windows/desktop/ee418620) 方法。

```cpp
m_musicEngineCallback.Initialize(this);
m_musicEngine->RegisterForCallbacks(&m_musicEngineCallback);
```

Marble Maze 不需要在开始或结束音频处理时获得通知。 因此，它实现 [IXAudio2EngineCallback::OnProcessingPassStart](https://msdn.microsoft.com/library/windows/desktop/ee418463) 和 [IXAudio2EngineCallback::OnProcessingPassEnd](https://msdn.microsoft.com/library/windows/desktop/ee418462) 方法，而不执行任何操作。 对于 [IXAudio2EngineCallback::OnCriticalError](https://msdn.microsoft.com/library/windows/desktop/ee418461) 方法，Marble Maze 调用 **SetEngineExperiencedCriticalError** 方法，该方法可设置 **m\_engineExperiencedCriticalError** 标志。

```cpp
// Audio.cpp

// Called when a critical system error causes XAudio2 
// to be closed and restarted. The error code is given in Error. 
void  _stdcall AudioEngineCallbacks::OnCriticalError(HRESULT Error)
{
    m_audio->SetEngineExperiencedCriticalError();
}
```

```cpp
// Audio.h (Audio class)

// This flag can be used to tell when the audio system 
// is experiencing critial errors.
// XAudio2 gives a critical error when the user unplugs
// the headphones and a new speaker configuration is generated.
void SetEngineExperiencedCriticalError()
{
    m_engineExperiencedCriticalError = true;
}
```

发生致命错误时，音频处理将停止，所有对 XAudio2 其他调用将失败。 若要从此情形恢复，必须释放 XAudio2 实例并创建一个新实例。 **Audio::Render** 方法（在每个帧中通过游戏循环调用）首先检查 **m\_engineExperiencedCriticalError** 标志。 如果此标志已设置，它清除此标志、释放当前的 XAudio2 实例、初始化资源，然后启动背景音乐。

```cpp
if (m_engineExperiencedCriticalError)
{
    m_engineExperiencedCriticalError = false;
    ReleaseResources();
    Initialize();
    CreateResources();
    Start();
    if (m_engineExperiencedCriticalError)
    {
        return;
    }
}
```

Marble Maze 还使用 **m\_engineExperiencedCriticalError** 标志以防止在没有可用的音频设备时调用 XAudio2。 例如，**MarbleMazeMain::Update** 方法在设置此标志时不会处理滚动或碰撞事件的音频。 该应用尝试在每帧中修复音频引擎（如果需要）；但是，如果计算机没有音频设备或耳机已拔出且没有其他可用的音频设备，可以始终设置 **m\_engineExperiencedCriticalError** 标志。

> [!CAUTION]
> 作为一项规则，不要在引擎回调主体中执行阻止操作。 执行此操作可能会导致性能问题。 Marble Maze 在 **OnCriticalError** 回调中设置一个标志，随后在常规音频处理阶段处理该错误。 有关 XAudio2 回调的详细信息，请参阅 [XAudio2 回调](https://msdn.microsoft.com/library/windows/desktop/ee415745)。

## <a name="conclusion"></a>结束语

这就是 Marble Maze 游戏示例的基础知识！ 虽然这是一个相对简单的游戏，但它包含了许多可以转到任何 UWP DirectX 游戏的重要部分，是可以在制作你自己的游戏时使用的一个好示例。

现在，你已完成了规定内容的学习，请尝试使用源代码试试看，看看会发生什么。 或者查看[使用 DirectX 创建一款简单的 UWP 游戏](tutorial--create-your-first-uwp-directx-game.md)，另一个 UWP DirectX 游戏示例。

准备使用 DirectX 向前迈进一步？ 然后在 [DirectX 编程](directx-programming.md)查看我们的指南。

如果你对在 UWP 上进行一般性地游戏开发感兴趣，请参阅文档[游戏编程](index.md)。

## <a name="related-topics"></a>相关主题

* [向 Marble Maze 添加输入和交互性示例](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [开发 Marble Maze，一款使用 C++ 和 DirectX 的 UWP 游戏](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)
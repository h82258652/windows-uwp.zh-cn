---
title: 添加声音
description: 使用 XAudio2 Api 开发一个简单的声音引擎，以播放游戏音乐和声音效果。
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 声音
ms.localizationpriority: medium
ms.openlocfilehash: 0e624c750bfce0633bc91d440fd883341b831836
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409646"
---
# <a name="add-sound"></a>添加声音

> [!NOTE]
> 本主题是[使用 DirectX 教程系列创建简单通用 Windows 平台（UWP）游戏](tutorial--create-your-first-uwp-directx-game.md)的一部分。 该链接上的主题设置了序列的上下文。

在本主题中，我们将使用[XAudio2](/windows/desktop/xaudio2/xaudio2-introduction) api 创建一个简单的声音引擎。 如果你不熟悉__XAudio2__，我们将在[音频概念](#audio-concepts)下提供简短的简介。

>[!Note]
>如果尚未下载此示例的最新游戏代码，请参阅[Direct3D 示例游戏](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此示例是大型 UWP 功能示例集合的一部分。 有关如何下载示例的说明，请参阅[从 GitHub 获取 UWP 示例](/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目标

使用[XAudio2](/windows/desktop/xaudio2/xaudio2-introduction)向示例游戏添加声音。

## <a name="define-the-audio-engine"></a>定义音频引擎

在示例游戏中，音频对象和行为在三个文件中定义：

* __/.Cpp [Audio.h](#audioh)__：定义__音频__对象，其中包含用于播放声音的__XAudio2__资源。 它还定义了在游戏暂停或停用的情况下暂停音频播放和恢复音频播放的方法。
* __ [MediaReader](#mediareaderh)/.cpp__：定义从本地存储读取音频 .wav 文件的方法。
* __ [SoundEffect](#soundeffecth)/.cpp__：定义用于游戏中音频播放的对象。

## <a name="overview"></a>概述

设置音频播放到游戏中有三个主要部分。

1. [创建和初始化音频资源](#create-and-initialize-the-audio-resources)
2. [加载音频文件](#load-audio-file)
3. [将声音关联到对象](#associate-sound-to-object)

它们都是在[Simple3DGame：： Initialize](#simple3dgameinitialize-method)方法中定义的。 接下来，让我们先查看此方法，然后深入了解每个部分中的更多详细信息。

设置完成后，我们将了解如何触发声音效果播放。 有关详细信息，请参阅[播放声音](#play-the-sound)。

### <a name="simple3dgameinitialize-method"></a>Simple3DGame：： Initialize 方法

在__Simple3DGame：： Initialize__中，其中__m \_ 控制器__和__m \_ 呈现__器也被初始化，我们设置了音频引擎并使其可以播放声音。

 * 创建__m \_ audioController__，它是[音频](#audioh)类的实例。
 * 使用[audio：： CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method)方法创建所需的音频资源。 此处，两个__XAudio2__对象 &mdash; 是一个音乐引擎对象和一个声音引擎对象，并为每个对象创建一个控制。 音乐引擎对象可用于播放游戏的背景音乐。 声音引擎可用于在游戏中播放声音效果。 有关详细信息，请参阅[创建和初始化音频资源](#create-and-initialize-the-audio-resources)。
 * 创建__mediaReader__，它是[mediaReader](#mediareaderh)类的实例。 [MediaReader](#mediareaderh)是[SoundEffect](#soundeffecth)类的帮助器类，它从文件位置同步读取小的音频文件，并将声音数据作为字节数组返回。
 * 使用[MediaReader：： LoadMedia](#mediareaderloadmedia-method)从其位置加载声音文件，并创建__targetHitSound__变量来保存加载的 .wav 声音数据。 有关详细信息，请参阅[Load audio file](#load-audio-file)。 

声音效果与游戏对象相关联。 因此，当游戏对象发生冲突时，它会触发要播放的声音效果。 在此示例游戏中，我们将对 ammo （用于对目标进行拍摄的操作）和目标的声音产生影响。 
    
* 在__GameObject__类中，有一个__HitSound__属性，该属性用于将声音效果与对象相关联。
* 创建[SoundEffect](#soundeffecth)类的新实例，并对其进行初始化。 在初始化期间，会创建声音效果的源语音。 
* 此类使用从[音频](#audioh)类提供的主控语音播放声音。 使用[MediaReader](#mediareaderh)类从文件位置读取声音数据。 有关详细信息，请参阅[将声音与对象关联](#associate-sound-to-object)。

>[!Note]
>播放声音的实际触发器由这些游戏对象的移动和冲突决定。 因此，实际播放这些声音的调用是在[Simple3DGame：： UpdateDynamics](#simple3dgameupdatedynamics-method)方法中定义的。 有关详细信息，请参阅[播放声音](#play-the-sound)。

```cppwinrt
void Simple3DGame::Initialize(
    _In_ std::shared_ptr<MoveLookController> const& controller,
    _In_ std::shared_ptr<GameRenderer> const& renderer
    )
{
    // The following member is defined in the header file:
    // Audio m_audioController;

    ...

    // Create the audio resources needed.
    // Two XAudio2 objects are created - one for music engine,
    // the other for sound engine. A mastering voice is also
    // created for each of the objects.
    m_audioController.CreateDeviceIndependentResources();

    m_ammo.resize(GameConstants::MaxAmmo);

    ...

    // Create a media reader which is used to read audio files from its file location.
    MediaReader mediaReader;
    auto targetHitSoundX = mediaReader.LoadMedia(L"Assets\\hit.wav");

    // Instantiate the targets for use in the game.
    // Each target has a different initial position, size, and orientation.
    // But share a common set of material properties.
    for (int a = 1; a < GameConstants::MaxTargets; a++)
    {
        ...
        // Create a new sound effect object and associate it
        // with the game object's (target) HitSound property.
        target->HitSound(std::make_shared<SoundEffect>());

        // Initialize the sound effect object with
        // the sound effect engine, format of the audio wave, and audio data
        // During initialization, source voice of this sound effect is also created.
        target->HitSound()->Initialize(
            m_audioController.SoundEffectEngine(),
            mediaReader.GetOutputWaveFormatEx(),
            targetHitSoundX
            );
        ...
    }

    // Instantiate a set of spheres to be used as ammunition for the game
    // and set the material properties of the spheres.
    auto ammoHitSound = mediaReader.LoadMedia(L"Assets\\bounce.wav");

    for (int a = 0; a < GameConstants::MaxAmmo; a++)
    {
        m_ammo[a] = std::make_shared<Sphere>();
        m_ammo[a]->Radius(GameConstants::AmmoRadius);
        m_ammo[a]->HitSound(std::make_shared<SoundEffect>());
        m_ammo[a]->HitSound()->Initialize(
            m_audioController.SoundEffectEngine(),
            mediaReader.GetOutputWaveFormatEx(),
            ammoHitSound
            );
        m_ammo[a]->Active(false);
        m_renderObjects.push_back(m_ammo[a]);
    }
    ...
}
```

## <a name="create-and-initialize-the-audio-resources"></a>创建和初始化音频资源

* 使用[XAudio2Create](/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create)（XAudio2 API）来创建两个新的 XAudio2 对象，这些对象定义音乐和声音效果引擎。 此方法返回指向对象的[IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2)接口的指针，该接口管理所有音频引擎状态、音频处理线程、语音图形等。
* 在实例化引擎后，使用[IXAudio2：： CreateMasteringVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice)为每个声音引擎对象创建一个控制声音。

有关详细信息，请参阅[如何：初始化 XAudio2](/windows/desktop/xaudio2/how-to--initialize-xaudio2)。

### <a name="audiocreatedeviceindependentresources-method"></a>Audio：： CreateDeviceIndependentResources 方法

```cppwinrt
void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    winrt::check_hresult(
        XAudio2Create(m_musicEngine.put(), flags)
        );

    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    winrt::check_hresult(
        XAudio2Create(m_soundEffectEngine.put(), flags)
        );

    winrt::check_hresult(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}
```

## <a name="load-audio-file"></a>加载音频文件

在示例游戏中，用于读取音频格式文件的代码是在[MediaReader](#mediareaderh)/cpp__ 中定义的。  若要读取编码的 .wav 音频文件，请调用[MediaReader：： LoadMedia](#mediareaderloadmedia-method)，并将 .wav 的文件名作为输入参数传入。

### <a name="mediareaderloadmedia-method"></a>MediaReader：： LoadMedia 方法

此方法使用[媒体基础](/windows/desktop/medfound/microsoft-media-foundation-sdk) API 作为脉冲编码调制 (PCM) 缓冲区读入 .wav 音频文件。

#### <a name="set-up-the-source-reader"></a>设置源读取器

1. 使用[MFCreateSourceReaderFromURL](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl)创建媒体源读取器（[IMFSourceReader](/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader)）。
2. 使用[MFCreateMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype)创建媒体类型（[IMFMediaType](/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype)_）对象（媒体_类型）。 它表示媒体格式的说明。 
3. 指定_媒体_类型的解码输出为 PCM 音频，这是__XAudio2__可以使用的音频类型。
4. 通过调用[IMFSourceReader：： SetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype)为源读取器设置已解码的输出媒体类型。

有关为何使用源读取器的详细信息，请参阅[源读取器](/windows/desktop/medfound/source-reader)。

#### <a name="describe-the-data-format-of-the-audio-stream"></a>描述音频流的数据格式

1. 使用[IMFSourceReader：： GetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype)获取流的当前媒体类型。
2. 使用[IMFMediaType：： MFCreateWaveFormatExFromMFMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype)将之前操作的结果作为输入，以将当前音频媒体类型转换为[WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex)缓冲区。 此结构指定加载音频后使用的波形音频流的数据格式。 

__WAVEFORMATEX__格式可用于描述 PCM 缓冲区。 与[WAVEFORMATEXTENSIBLE](/windows-hardware/drivers/ddi/content/ksmedia/ns-ksmedia-waveformatextensible)结构相比，它仅可用于描述一小部分音频波形格式。 有关__WAVEFORMATEX__与__WAVEFORMATEXTENSIBLE__之间的差异的详细信息，请参阅[可扩展波形格式说明符](/windows-hardware/drivers/audio/extensible-wave-format-descriptors)。

#### <a name="read-the-audio-stream"></a>读取音频流

1.  通过调用[IMFSourceReader：： GetPresentationAttribute](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute)获取音频流的持续时间（以秒为单位），然后将持续时间转换为字节。
2.  通过调用[IMFSourceReader：： ReadSample](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-readsample)，将中的音频文件作为流进行读取。 __ReadSample__读取媒体源中的下一个示例。
3.  使用[IMFSample：： ConvertToContiguousBuffer](/windows/desktop/api/mfobjects/nf-mfobjects-imfsample-converttocontiguousbuffer)将音频示例缓冲区（_示例_）的内容复制到数组（_mediaBuffer_）中。

```cppwinrt
std::vector<byte> MediaReader::LoadMedia(_In_ winrt::hstring const& filename)
{
    winrt::check_hresult(
        MFStartup(MF_VERSION)
        );

    // Creates a media source reader.
    winrt::com_ptr<IMFSourceReader> reader;
    winrt::check_hresult(
        MFCreateSourceReaderFromURL(
        (m_installedLocationPath + filename).c_str(),
            nullptr,
            reader.put()
            )
        );

    // Set the decoded output format as PCM.
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    winrt::com_ptr<IMFMediaType> mediaType;
    winrt::check_hresult(
        MFCreateMediaType(mediaType.put())
        );

    // Define the major category of the media as audio. For more info about major media types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa367377.aspx
    winrt::check_hresult(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    // Define the sub-type of the media as uncompressed PCM audio. For more info about audio sub-types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa372553.aspx
    winrt::check_hresult(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );

    // Sets the media type for a stream. This media type defines that format that the Source Reader 
    // produces as output. It can differ from the native format provided by the media source.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx
    winrt::check_hresult(
        reader->SetCurrentMediaType(static_cast<uint32_t>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), 0, mediaType.get())
        );

    // Get the current media type for the stream.
    // For more info, go to:
    // https://msdn.microsoft.com/library/windows/desktop/dd374660.aspx
    winrt::com_ptr<IMFMediaType> outputMediaType;
    winrt::check_hresult(
        reader->GetCurrentMediaType(static_cast<uint32_t>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), outputMediaType.put())
        );

    // Converts the current media type into the WaveFormatEx buffer structure.
    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    winrt::check_hresult(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.get(), &waveFormat, &size)
        );

    // Copies the waveFormat's block of memory to the starting address of the m_waveFormat variable in MediaReader.
    // Then free the waveFormat memory block.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/aa366535.aspx and
    // https://msdn.microsoft.com/library/windows/desktop/ms680722.aspx
    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    PROPVARIANT propVariant;
    winrt::check_hresult(
        reader->GetPresentationAttribute(static_cast<uint32_t>(MF_SOURCE_READER_MEDIASOURCE), MF_PD_DURATION, &propVariant)
        );

    // 'duration' is in 100ns units; convert to seconds, and round up
    // to the nearest whole byte.
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes =
        static_cast<unsigned int>(
            ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000) /
            10000000
            );

    std::vector<byte> fileData(maxStreamLengthInBytes);

    winrt::com_ptr<IMFSample> sample;
    winrt::com_ptr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        // Read audio data.
        ...
    }

    return fileData;
}
```

## <a name="associate-sound-to-object"></a>将声音关联到对象

在[Simple3DGame：： Initialize](#simple3dgameinitialize-method)方法中初始化游戏时，将声音关联到对象。

Recap:
* 在__GameObject__类中，有一个__HitSound__属性，该属性用于将声音效果与对象相关联。
* 创建[SoundEffect](#soundeffecth)类对象的新实例，并将其与游戏对象相关联。 此类使用__XAudio2__ api 播放声音。  它使用由[音频](#audioh)类提供的 "主控语音"。 可以使用[MediaReader](#mediareaderh)类从文件位置读取声音数据。

[SoundEffect：： Initialize](#soundeffectinitialize-method)用于使用以下输入参数初始化__SoundEffect__实例：指向声音引擎对象的指针（在[音频：： CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method)方法中创建的 IXAudio2 对象）、使用__MediaReader：： GetOutputWaveFormatEx__的 .Wav 文件格式的指针，以及使用[MediaReader：： LoadMedia](#mediareaderloadmedia-method)方法加载的声音数据。 在初始化期间，还会创建声音效果的源声音。

### <a name="soundeffectinitialize-method"></a>SoundEffect：： Initialize 方法

```cppwinrt
void SoundEffect::Initialize(
    _In_ IXAudio2* masteringEngine,
    _In_ WAVEFORMATEX* sourceFormat,
    _In_ std::vector<byte> const& soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available so just return.
        m_audioAvailable = false;
        return;
    }

    // Create a source voice for this sound effect.
    winrt::check_hresult(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

## <a name="play-the-sound"></a>播放声音

用[Simple3DGame：： UpdateDynamics](#simple3dgameupdatedynamics-method)方法定义用于播放声音效果的触发器，因为在这种情况下，将更新对象的移动，并确定对象之间的冲突。

由于对象之间的交互差异很大，根据游戏的不同，我们不会在此处讨论游戏对象的动态。 如果你有兴趣了解其实现，请参阅[Simple3DGame：： UpdateDynamics](#simple3dgameupdatedynamics-method)方法。

原则上，当发生冲突时，它会触发声音效果，方法是调用**SoundEffect：:P laysound**。 此方法停止当前正在播放的任何声音效果，并将内存中缓冲区与所需的声音数据进行排队。 它使用源语音设置卷、提交声音数据并开始播放。

### <a name="soundeffectplaysound-method"></a>SoundEffect：:P laySound 方法

* 使用源 voice 对象**m \_ sourceVoice**开始播放声音数据缓冲区**m \_ soundData**
* 创建一个[XAUDIO2 \_ 缓冲区](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer)，该缓冲区向其提供对声音数据缓冲区的引用，然后通过调用[IXAudio2SourceVoice：： SubmitSourceBuffer](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer)提交该缓冲区。 
* 在声音数据排好队列之后，**SoundEffect::PlaySound** 将通过调用 [IXAudio2SourceVoice::Start](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start) 开始播放。

```cppwinrt
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = { 0 };

    if (!m_audioAvailable)
    {
        // Audio is not available so just return.
        return;
    }

    // Interrupt sound effect if it is currently playing.
    winrt::check_hresult(
        m_sourceVoice->Stop()
        );
    winrt::check_hresult(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue the memory buffer for playback and start the voice.
    buffer.AudioBytes = (UINT32)m_soundData.size();
    buffer.pAudioData = m_soundData.data();
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    winrt::check_hresult(
        m_sourceVoice->SetVolume(volume)
        );
    winrt::check_hresult(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    winrt::check_hresult(
        m_sourceVoice->Start()
        );
}
```

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame：： UpdateDynamics 方法

__Simple3DGame：： UpdateDynamics__方法负责处理游戏对象之间的交互和冲突。 当对象发生冲突（或相交）时，它会触发关联的声音效果播放。

```cppwinrt
void Simple3DGame::UpdateDynamics()
{
    ...
    // Check for collisions between ammo.
#pragma region inter-ammo collision detection
if (m_ammoCount > 1)
{
    ...
    // Check collision between instances One and Two.
    ...
    if (distanceSquared < (GameConstants::AmmoSize * GameConstants::AmmoSize))
    {
        // The two ammo are intersecting.
        ...
        // Start playing the sounds for the impact between the two balls.
        m_ammo[one]->PlaySound(impact, m_player->Position());
        m_ammo[two]->PlaySound(impact, m_player->Position());
    }
}
#pragma endregion

#pragma region Ammo-Object intersections
    // Check for intersections between the ammo and the other objects in the scene.
    // ...
    // Ball is in contact with Object.
    // ...

    // Make sure that the ball is actually headed towards the object. At grazing angles there
    // could appear to be an impact when the ball is actually already hit and moving away.

    if (impact > 0.0f)
    {
        ...
        // Play the sound associated with the Ammo hitting something.
        m_objects[i]->PlaySound(impact, m_player->Position());

        if (m_objects[i]->Target() && !m_objects[i]->Hit())
        {
            // The object is a target and isn't currently hit, so mark
            // it as hit and play the sound associated with the impact.
            m_objects[i]->Hit(true);
            m_objects[i]->HitTime(timeTotal);
            m_totalHits++;

            m_objects[i]->PlaySound(impact, m_player->Position());
        }
        ...
    }
#pragma endregion

#pragma region Apply Gravity and world intersection
            // Apply gravity and check for collision against enclosing volume.
            ...
                if (position.z < limit)
                {
                    // The ammo instance hit the a wall in the min Z direction.
                    // Align the ammo instance to the wall, invert the Z component of the velocity and
                    // play the impact sound.
                    position.z = limit;
                    m_ammo[i]->PlaySound(-velocity.z, m_player->Position());
                    velocity.z = -velocity.z * GameConstants::Physics::GroundRestitution;
                }
                ...
#pragma endregion
}
```

## <a name="next-steps"></a>后续步骤

我们介绍了 Windows 10 游戏的 UWP 框架、图形、控件、用户界面和音频。 本教程的下一部分是[扩展示例游戏](tutorial-resources.md)，说明了在开发游戏时可以使用的其他选项。

## <a name="audio-concepts"></a>音频概念

对于 Windows 10 游戏开发，请使用 XAudio2 版本2.9。 此版本随 Windows 10 一起提供。 有关详细信息，请参阅[XAudio2 版本](/windows/desktop/xaudio2/xaudio2-versions)。

__AudioX2__是一种低级别 API，提供信号处理和混合基础。 有关详细信息，请参阅[XAudio2 关键概念](/windows/desktop/xaudio2/xaudio2-key-concepts)。

### <a name="xaudio2-voices"></a>XAudio2 语音

有三种类型的 XAudio2 voice 对象：源、submix 和控制声。 声音是 XAudio2 用于处理、操作和播放音频数据的对象。 
* 源语音对客户提供的音频数据操作。 
* 源语音和子混合语音将其输出发送到一个或多个子混合语音或主语音。 
* 子混合语音和主语音将传入的所有音频混合在一起，并对结果进行操作。 
* "主控语音" 通过源语音和 submix 声音接收数据，并将该数据发送到音频硬件。

有关详细信息，请参阅[XAudio2 声音](/windows/desktop/xaudio2/xaudio2-voices)。

### <a name="audio-graph"></a>音频图形

音频图形是[XAudio2 语音](/windows/desktop/xaudio2/xaudio2-voices)的集合。 音频在源语音的音频图形的一侧开始，可以选择通过一个或多个 submix 声音，并结束控制声。 音频图形会为当前播放的每个声音、零个或多个 submix 声音以及一个控制声音提供源语音。 最简单的音频图形和在 XAudio2 中产生干扰所需的最小值是直接输出到主控语音的单个源语音。 有关详细信息，请参阅[音频图](/windows/desktop/xaudio2/audio-graphs)。

### <a name="additional-reading"></a>其他阅读材料

* [如何：初始化 XAudio2](/windows/desktop/xaudio2/how-to--initialize-xaudio2)
* [如何：在 XAudio2 中加载音频数据文件](/windows/desktop/xaudio2/how-to--load-audio-data-files-in-xaudio2)
* [如何：使用 XAudio2 播放声音](/windows/desktop/xaudio2/how-to--play-a-sound-with-xaudio2)

## <a name="key-audio-h-files"></a>关键的音频 .h 文件

### <a name="audioh"></a>Audio.h

```cppwinrt
// Audio:
// This class uses XAudio2 to provide sound output. It creates two
// engines - one for music and the other for sound effects - each as
// a separate mastering voice.
// The SuspendAudio and ResumeAudio methods can be used to stop
// and start all audio playback.

class Audio
{
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

private:
    ...
};
```

### <a name="mediareaderh"></a>MediaReader

```cppwinrt
// MediaReader:
// This is a helper class for the SoundEffect class. It reads small audio files
// synchronously from the package installed folder and returns sound data as a
// vector of bytes.

class MediaReader
{
public:
    MediaReader();

    std::vector<byte> LoadMedia(_In_ winrt::hstring const& filename);
    WAVEFORMATEX* GetOutputWaveFormatEx();

private:
    winrt::Windows::Storage::StorageFolder  m_installedLocation{ nullptr };
    winrt::hstring                          m_installedLocationPath;
    WAVEFORMATEX                            m_waveFormat;
};
```

### <a name="soundeffecth"></a>SoundEffect.h

```cppwinrt
// SoundEffect:
// This class plays a sound using XAudio2. It uses a mastering voice provided
// from the Audio class. The sound data can be read from disk using the MediaReader
// class.

class SoundEffect
{
public:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2* masteringEngine,
        _In_ WAVEFORMATEX* sourceFormat,
        _In_ std::vector<byte> const& soundData
        );

    void PlaySound(_In_ float volume);

private:
    ...
};
```

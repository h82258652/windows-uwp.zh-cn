---
title: 添加声音
description: 开发一个简单的声音引擎使用 XAudio2 Api 到游戏播放音乐和声音效果。
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 声音
ms.localizationpriority: medium
ms.openlocfilehash: 945270247b8a288554e1910ac1c6f8e5c1ec1619
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367845"
---
# <a name="add-sound"></a>添加声音

在本主题中，我们创建一个简单的声音引擎 using [XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-introduction) Api。 如果您不熟悉__XAudio2__，现已下的简短介绍[音频概念](#audio-concepts)。

>[!Note]
>如果你尚未下载适用于此示例的最新游戏代码，请转到 [Direct3D 游戏示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此示例是大型 UWP 功能示例集合的一部分。 有关如何下载示例的说明，请参阅[从 GitHub 获取 UWP 示例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目标

将声音添加到示例游戏使用[XAudio2](/windows/desktop/xaudio2/xaudio2-introduction)。

## <a name="define-the-audio-engine"></a>定义音频引擎

在游戏示例中，音频对象和行为在三个文件中定义：

* __[Audio.h](#audioh)/.cpp__:定义__音频__对象，它包含__XAudio2__声音播放的资源。 它还定义了在游戏暂停或停用的情况下暂停音频播放和恢复音频播放的方法。
* __[MediaReader.h](#mediareaderh)/.cpp__:定义用于从本地存储中读取音频.wav 文件的方法。
* __[SoundEffect.h](#soundeffecth)/.cpp__:定义一个对象，用于在游戏声音播放。

## <a name="overview"></a>概述

对于音频播放到您的游戏设置中有三个主要部分。

1. [创建和初始化的音频资源](#create-and-initialize-the-audio-resources)
2. [加载音频文件](#load-audio-file)
3. [将关联到对象的声音](#associate-sound-to-object)

它们都被定义为在[Simple3DGame::Initialize](#simple3dgameinitialize-method)方法。 因此，让我们首先检查此方法，然后深入了解每个部分中的更多详细信息。

设置后，我们将了解如何在触发要播放的声音效果。 有关详细信息，请转到[播放声音](#play-the-sound)。

### <a name="simple3dgameinitialize-method"></a>Simple3DGame::Initialize 方法

在中__Simple3DGame::Initialize__，其中__m\_控制器__并__m\_呈现器__是还初始化，我们设置音频引擎，并获取它已准备好播放声音。

 * 创建__m\_audioController__，这是实例[音频](#audioh)类。
 * 创建使用所需的音频资源[Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method)方法。 此处，两个__XAudio2__对象&mdash;创建音乐引擎对象和一个声音引擎对象和为每个掌握语音。 音乐引擎对象可以用于播放背景音乐，为您的游戏。 声音引擎可用于在您的游戏中播放声音效果。 有关详细信息，请参阅[创建并初始化的音频资源](#create-and-initialize-the-audio-resources)。
 * 创建__mediaReader__，即实例[MediaReader](#mediareaderh)类。 [MediaReader](#mediareaderh)，这是一个帮助器类[SoundEffect](#soundeffecth)类的读取较小的音频文件以同步方式从文件位置和声音数据作为字节数组返回。
 * 使用[MediaReader::LoadMedia](#mediareaderloadmedia-method)若要从其位置加载声音文件，并创建__targetHitSound__变量以保存已加载的.wav 声音数据。 有关详细信息，请参阅[加载音频文件](#load-audio-file)。 

声音效果与相关联的游戏对象。 因此时与该游戏对象发生冲突时，它将触发要播放的声音效果。 在此游戏的示例中，我们有声音效果 （什么我们使用投篮机会控制在具有目标） ammo 和目标。 
    
* 在中__GameObject__类，因此存在__HitSound__用于将声音效果与对象相关联的属性。
* 创建的新实例[SoundEffect](#soundeffecth)类，并对其进行初始化。 在初始化期间，创建声音效果的源语音。 
* 此类播放声音使用从提供的掌握语音[音频](#audioh)类。 声音数据从文件位置使用读取[MediaReader](#mediareaderh)类。 有关详细信息，请参阅[将关联到对象的声音](#associate-sound-to-object)。

>[!Note]
>要播放声音的实际触发器取决于移动和这些游戏对象的冲突。 因此，在定义调用实际上播放这些声音[Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method)方法。 有关详细信息，请转到[播放声音](#play-the-sound)。

```cpp
void Simple3DGame::Initialize(
    _In_ MoveLookController^ controller,
    _In_ GameRenderer^ renderer
    )
{
    // ...
    // Create a new Audio class object.
    m_audioController = ref new Audio;

    // Create the audio resources needed.
    // Two XAudio2 objects are created - one for music engine,
    // the other for sound engine. A mastering voice is also
    // created for each of the objects.
    m_audioController->CreateDeviceIndependentResources();

    m_ammo = std::vector<Sphere^>(GameConstants::MaxAmmo);

    // ..
    // Create a media reader which is used to read audio files from its file location.
    MediaReader^ mediaReader = ref new MediaReader;
    auto targetHitSound = mediaReader->LoadMedia("Assets\\hit.wav");

    // Instantiate the targets for use in the game.
    // Each target has a different initial position, size, and orientation.
    // But share a common set of material properties.
    for (int a = 1; a < GameConstants::MaxTargets; a++)
    {
        // ...
        // Create a new sound effect object and associate it
        // with the game object's (target) HitSound property.
        target->HitSound(ref new SoundEffect());

        // Initialize the sound effect object with
        // the sound effect engine, format of the audio wave, and audio data
        // During initialization, source voice of this sound effect is also created.
        target->HitSound()->Initialize(
            m_audioController->SoundEffectEngine(),
            mediaReader->GetOutputWaveFormatEx(),
            targetHitSound
            );
        // ...
    }

    // Instantiate a set of spheres to be used as ammunition for the game
    // and set the material properties of the spheres.
    auto ammoHitSound = mediaReader->LoadMedia("Assets\\bounce.wav");

    for (int a = 0; a < GameConstants::MaxAmmo; a++)
    {
        m_ammo[a] = ref new Sphere;
        m_ammo[a]->Radius(GameConstants::AmmoRadius);
        m_ammo[a]->HitSound(ref new SoundEffect());
        m_ammo[a]->HitSound()->Initialize(
            m_audioController->SoundEffectEngine(),
            mediaReader->GetOutputWaveFormatEx(),
            ammoHitSound
            );
        m_ammo[a]->Active(false);
        m_renderObjects.push_back(m_ammo[a]);
    }
    // ...
}
```

## <a name="create-and-initialize-the-audio-resources"></a>创建和初始化的音频资源

* 使用[XAudio2Create](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create)，XAudio2 API，以创建两个新 XAudio2 对象定义的音乐和声音效果引擎。 此方法返回一个指向对象的[IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2)界面，用于管理所有音频引擎指出，音频处理线程、 语音图形和的详细信息。
* 引擎已实例化后，使用[IXAudio2::CreateMasteringVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice)掌握语音创建的每个声音引擎对象。

有关详细信息，请转到[如何：初始化 XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--initialize-xaudio2)。

### <a name="audiocreatedeviceindependentresources-method"></a>Audio::CreateDeviceIndependentResources 方法

```cpp

void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    DX::ThrowIfFailed(
        XAudio2Create(&m_musicEngine, flags)
        );

    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    DX::ThrowIfFailed(
        XAudio2Create(&m_soundEffectEngine, flags)
        );

    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}
```

## <a name="load-audio-file"></a>加载音频文件

在游戏的示例中，用于读取音频格式文件的代码中定义[MediaReader.h](#mediareaderh)/cpp__。  若要读取编码的.wav 音频文件，调用[MediaReader::LoadMedia](#mediareaderloadmedia-method)、 传入作为输入参数.wav 文件的文件名。

### <a name="mediareaderloadmedia-method"></a>MediaReader::LoadMedia 方法

此方法使用[媒体基础](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk) API 作为脉冲编码调制 (PCM) 缓冲区读入 .wav 音频文件。

#### <a name="set-up-the-source-reader"></a>设置源读取器

1. 使用[MFCreateSourceReaderFromURL](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl)若要创建媒体源读取器 ([IMFSourceReader](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader))。
2. 使用[MFCreateMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype)若要创建的媒体类型 ([IMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype)) 对象 (_mediaType_)。 它表示媒体格式的说明。 
3. 指定的_mediaType_解码的输出是 PCM 音频，这是一个音频类型__XAudio2__可以使用。
4. 集的已解码的输出媒体类型的源读取器通过调用[IMFSourceReader::SetCurrentMediaType](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype)。

我们为什么使用源读取器的详细信息，请转到[源读取器](https://docs.microsoft.com/windows/desktop/medfound/source-reader)。

#### <a name="describe-the-data-format-of-the-audio-stream"></a>描述数据格式的音频流

1. 使用[IMFSourceReader::GetCurrentMediaType](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype)的流中获取当前的媒体类型。
2. 使用[IMFMediaType::MFCreateWaveFormatExFromMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype)要转换到当前的音频媒体类型[WAVEFORMATEX](https://docs.microsoft.com/windows/desktop/api/mmreg/ns-mmreg-twaveformatex)缓冲区，使用早期的操作的结果作为输入。 此结构指定 wave 音频流后加载音频时使用的数据格式。 

__WAVEFORMATEX__格式可用于描述 PCM 缓冲区。 相比[WAVEFORMATEXTENSIBLE](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/ksmedia/ns-ksmedia-waveformatextensible)结构，它仅可用于描述波形音频格式的子集。 有关详细信息之间的差异__WAVEFORMATEX__并__WAVEFORMATEXTENSIBLE__，请参阅[可扩展波形格式说明符](https://docs.microsoft.com/windows-hardware/drivers/audio/extensible-wave-format-descriptors)。

#### <a name="read-the-audio-stream"></a>读取的音频流

1.  获取的持续时间，以秒为单位，通过调用的音频流[IMFSourceReader::GetPresentationAttribute](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute) ，然后将持续时间转换为字节。
2.  读取流的形式中的音频文件，通过调用[IMFSourceReader::ReadSample](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-readsample)。 __ReadSample__从媒体源中读取下一个示例。
3.  使用[IMFSample::ConvertToContiguousBuffer](https://docs.microsoft.com/windows/desktop/api/mfobjects/nf-mfobjects-imfsample-converttocontiguousbuffer)音频采样缓冲区的内容复制 (_示例_) 到一个数组 (_mediaBuffer_)。

```cpp
Platform::Array<byte>^ MediaReader::LoadMedia(_In_ Platform::String^ filename)
{
    DX::ThrowIfFailed(
        MFStartup(MF_VERSION)
        );

    // Creates a media source reader.
    ComPtr<IMFSourceReader> reader;
    DX::ThrowIfFailed(
        MFCreateSourceReaderFromURL(
            Platform::String::Concat(m_installedLocationPath, filename)->Data(),
            nullptr,
            &reader
            )
        );

    // Set the decoded output format as PCM.
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    Microsoft::WRL::ComPtr<IMFMediaType> mediaType;
    DX::ThrowIfFailed(
        MFCreateMediaType(&mediaType)
        );

    // Define the major category of the media as audio. For more info about major media types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa367377.aspx
    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    // Define the sub-type of the media as uncompressed PCM audio. For more info about audio sub-types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa372553.aspx
    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );
    
    // Sets the media type for a stream. This media type defines that format that the Source Reader 
    // produces as output. It can differ from the native format provided by the media source.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx
    DX::ThrowIfFailed(
        reader->SetCurrentMediaType(static_cast<uint32>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), 0, mediaType.Get())
        );

    // Get the current media type for the stream.
    // For more info, go to:
    // https://msdn.microsoft.com/library/windows/desktop/dd374660.aspx
        Microsoft::WRL::ComPtr<IMFMediaType> outputMediaType;
    DX::ThrowIfFailed(
        reader->GetCurrentMediaType(static_cast<uint32>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), &outputMediaType)
        );

    // Converts the current media type into the WaveFormatEx buffer structure.
    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    DX::ThrowIfFailed(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &size)
        );

    // Copies the waveFormat's block of memory to the starting address of the m_waveFormat variable in MediaReader.
    // Then free the waveFormat memory block.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/aa366535.aspx and
    // https://msdn.microsoft.com/library/windows/desktop/ms680722.aspx
    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    PROPVARIANT propVariant;
    DX::ThrowIfFailed(
        reader->GetPresentationAttribute(static_cast<uint32>(MF_SOURCE_READER_MEDIASOURCE), MF_PD_DURATION, &propVariant)
        );

    // 'duration' is in 100ns units; convert to seconds, and round up
    // to the nearest whole byte.
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes =
        static_cast<unsigned int>(
            ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000) /
            10000000
            );

    Platform::Array<byte>^ fileData = ref new Platform::Array<byte>(maxStreamLengthInBytes);

    ComPtr<IMFSample> sample;
    ComPtr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        //...
        // Read audio data.
    }

    return fileData;
}
```
## <a name="associate-sound-to-object"></a>将关联到对象的声音

游戏初始化时，将相关联的对象的声音将进行[Simple3DGame::Initialize](#simple3dgameinitialize-method)方法。

回顾一下：
* 在中__GameObject__类，因此存在__HitSound__用于将声音效果与对象相关联的属性。
* 创建的新实例[SoundEffect](#soundeffecth)类对象，并将其与游戏对象相关联。 此类播放声音 using __XAudio2__ Api。  它使用提供的掌握语音[音频](#audioh)类。 可以从文件位置使用读取声音数据[MediaReader](#mediareaderh)类。

[SoundEffect::Initialize](#soundeffectinitialize-method)来初始化__SoundEffect__实例使用以下输入参数： 指向声音引擎对象的指针 (中创建的 IXAudio2 对象[音频::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method)方法)，用于设置格式的指针的.wav 文件使用__MediaReader::GetOutputWaveFormatEx__，并使用声音数据加载[MediaReader::LoadMedia](#mediareaderloadmedia-method)方法。 在初始化期间，还会创建源语音声音效果。

### <a name="soundeffectinitialize-method"></a>SoundEffect::Initialize 方法

```cpp
void SoundEffect::Initialize(
    _In_ IXAudio2 *masteringEngine,
    _In_ WAVEFORMATEX *sourceFormat,
    _In_ Platform::Array<byte>^ soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available so just return.
        m_audioAvailable = false;
        return;
    }

    // Create a source voice for this sound effect.
    DX::ThrowIfFailed(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

## <a name="play-the-sound"></a>播放声音

在定义触发器以播放声音效果[Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method)确定方法由于这是更新的对象移动的位置和对象之间的冲突。

因为对象之间的交互不同很大，具体取决于游戏时，我们不打算讨论游戏对象的动态。 如果您感兴趣，以了解其实现，请转到[Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method)方法。

从原理上讲，当发生冲突时，将触发要通过调用播放的声音效果**SoundEffect::PlaySound**。 此方法停止当前播放和队列具有所需的声音数据的内存中缓冲区的任何声音效果。 它使用源语音的卷设置、 提交良好的数据，并开始播放。

### <a name="soundeffectplaysound-method"></a>SoundEffect::PlaySound 方法

* 使用源语音对象**m\_sourceVoice**若要开始播放的声音数据缓冲区**m\_soundData**
* 创建[XAUDIO2\_缓冲区](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer)到它提供对声音数据缓冲区的引用的其中，然后将其提交到调用[IXAudio2SourceVoice::SubmitSourceBuffer](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer)。 
* 使用声音数据排队等候， **SoundEffect::PlaySound**开始播放通过调用[IXAudio2SourceVoice::Start](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start)。

```cpp
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (!m_audioAvailable)
    {
        // Audio is not available, so just return.
        return;
    }

    // Interrupt sound effect if currently playing.
    DX::ThrowIfFailed(
        m_sourceVoice->Stop()
        );
    DX::ThrowIfFailed(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue in-memory buffer for playback and start the voice.
    buffer.AudioBytes = m_soundData->Length;
    buffer.pAudioData = m_soundData->Data;
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    m_sourceVoice->SetVolume(volume);
    DX::ThrowIfFailed(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    DX::ThrowIfFailed(
        m_sourceVoice->Start()
        );
}
```

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame::UpdateDynamics 方法

__Simple3DGame::UpdateDynamics__方法负责的交互和游戏对象之间的冲突。 当对象发生冲突 （或 intersect） 时，将触发关联要播放的声音效果。

```cpp
void Simple3DGame::UpdateDynamics()
{
    // ...
    // Check for collisions between ammo.
#pragma region inter-ammo collision detection
    if (m_ammoCount > 1)
    {
       // ...
       // Check collision between instances One and Two.
       // ...
       if (distanceSquared < (GameConstants::AmmoSize * GameConstants::AmmoSize))
       {
           // The two ammo are intersecting.
           // ...
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
        // ...
        // Play the sound associated with the Ammo hitting something.
           m_ammo[one]->PlaySound(impact, m_player->Position());

        if (m_objects[i]->Target() && !m_objects[i]->Hit())
        {
            // The object is a target and isn't currently hit, so mark it as hit and
            // play the sound associated with the impact.
             m_objects[i]->Hit(true);
             m_objects[i]->HitTime(timeTotal);
             m_totalHits++;

             m_objects[i]->PlaySound(impact, m_player->Position());
        }
        // ...
    }
#pragma endregion

#pragma region Apply Gravity and world intersection
    // Apply gravity and check for collision against enclosing volume.
    // ...
    if (position.z < limit)
    {
        // The ammo instance hit the a wall in the min Z direction.
        // Align the ammo instance to the wall, invert the Z component of the velocity and
        // play the impact sound.
           position.z = limit;
           m_ammo[i]->PlaySound(-velocity.z, m_player->Position());
           velocity.z = -velocity.z * GameConstants::Physics::GroundRestitution;
    }
    // ...
#pragma endregion
}
```
## <a name="next-steps"></a>后续步骤

我们已介绍的 UWP 框架、 图形、 控件、 用户界面和 Windows 10 游戏的音频。 本教程的下一部分[扩展游戏示例](tutorial-resources.md)，介绍了开发游戏时可以使用其他选项。

## <a name="audio-concepts"></a>音频概念

对于 Windows 10 游戏开发，使用 XAudio2 版本为 2.9。 此版本随附于 Windows 10。 有关详细信息，请转到[XAudio2 版本](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-versions)。

__AudioX2__是信号处理和混合 foundation 提供了一个低级别 API。 有关详细信息，请参阅[XAudio2 的基础概念](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-key-concepts)。

### <a name="xaudio2-voices"></a>XAudio2 语音

有三种类型的 XAudio2 语音对象： 源、 submix，和掌握语音。 语音是 XAudio2 中的对象使用来处理、 操作和播放的音频数据。 
* 源语音对客户提供的音频数据操作。 
* 源语音和子混合语音将其输出发送到一个或多个子混合语音或主语音。 
* 子混合语音和主语音将传入的所有音频混合在一起，并对结果进行操作。 
* 掌握语音接收来自源语音和 submix 语音数据并将该数据发送到的音频硬件。

有关详细信息，请转到[XAudio2 语音](/windows/desktop/xaudio2/xaudio2-voices)。

### <a name="audio-graph"></a>音频图形

音频图形是一系列[XAudio2 语音](/windows/desktop/xaudio2/xaudio2-voices)。 音频开始在源语音在音频图形的一侧，根据需要通过一个或多个 submix 语音，并结束于掌握语音。 音频图形将包含当前正在播放，零个或多个 submix 语音，每个声音的源语音和掌握语音。 最简单的音频图形和 XAudio2，在发出任何声音所需的最低要求是单个源语音输出直接到掌握语音。 有关详细信息，请转到[音频图形](https://docs.microsoft.com/windows/desktop/xaudio2/audio-graphs)。

### <a name="additional-reading"></a>其他阅读材料

* [如何：Initialize XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--initialize-xaudio2)
* [如何：加载中 XAudio2 音频数据文件](https://msdn.microsoft.com/library/windows/desktop/ee415781(v=vs.85).aspx)
* [如何：播放声音，XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--play-a-sound-with-xaudio2)

## <a name="key-audio-h-files"></a>密钥音频.h 文件

### <a name="audioh"></a>Audio.h

```cpp
// Audio:
// This class uses XAudio2 to provide sound output.  It creates two
// engines - one for music and the other for sound effects - each as
// a separate mastering voice.
// The SuspendAudio and ResumeAudio methods can be used to stop
// and start all audio playback.
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

protected:
    // ...
};
```
### <a name="mediareaderh"></a>MediaReader.h

```cpp
// MediaReader:
// This is a helper class for the SoundEffect class.  It reads small audio files
// synchronously from the package installed folder and returns sound data as a
// byte array.

ref class MediaReader
{
internal:
    MediaReader();

    Platform::Array<byte>^          LoadMedia(_In_ Platform::String^ filename);
    WAVEFORMATEX*                   GetOutputWaveFormatEx();

protected private:
    Windows::Storage::StorageFolder^ m_installedLocation;
    Platform::String^               m_installedLocationPath;
    WAVEFORMATEX                    m_waveFormat;
};
```
### <a name="soundeffecth"></a>SoundEffect.h

```cpp
// SoundEffect:
// This class plays a sound using XAudio2.  It uses a mastering voice provided
// from the Audio class.  The sound data can be read from disk using the MediaReader
// class.

ref class SoundEffect
{
internal:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2*              masteringEngine,
        _In_ WAVEFORMATEX*          sourceFormat,
        _In_ Platform::Array<byte>^ soundData
        );

    void PlaySound(_In_ float volume);

protected private:
    //...

};
```



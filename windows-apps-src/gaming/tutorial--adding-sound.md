---
title: 添加声音
description: 开发一种简单的声音引擎，使用 XAudio2 Api 将播放游戏音乐和声音效果。
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 声音
ms.localizationpriority: medium
ms.openlocfilehash: 94044e3d10df15cb1cb256d86ced798395e6af6f
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8485579"
---
# <a name="add-sound"></a>添加声音

在本主题中，我们将创建一个简单的声音引擎使用[XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813) Api。 如果你不熟悉__XAudio2__，我们已包括简短下[音频概念](#audio-concepts)简介。

>[!Note]
>如果你尚未下载适用于此示例的最新游戏代码，请转到 [Direct3D 游戏示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此示例是大型 UWP 功能示例集合的一部分。 有关如何下载示例的说明，请参阅[从 GitHub 获取 UWP 示例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目标

添加到示例游戏中使用[XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813)的声音。

## <a name="define-the-audio-engine"></a>定义音频引擎

在游戏示例中，音频对象和行为在三个文件中定义：

* __[Audio.h](#audioh)/.cpp__： 定义__音频__对象，其中包含用于声音播放的__XAudio2__资源。 它还定义了在游戏暂停或停用的情况下暂停音频播放和恢复音频播放的方法。
* __ [MediaReader.h](#mediareaderh)/.cpp__： 定义从本地存储中读取.wav 音频文件的方法。
* __ [SoundEffect.h](#soundeffecth)/.cpp__： 定义游戏内声音播放对象。

## <a name="overview"></a>概述

获取到你的游戏的音频播放设置中有三个主要部分。

1. [创建和初始化音频资源](#create-and-initialize-the-audio-resources)
2. [加载音频文件](#load-audio-file)
3. [关联到对象的声音](#associate-sound-to-object)

所有被[simple3dgame:: initialize](#simple3dgameinitialize-method)方法中定义。 因此，让我们先检查此方法，然后再深入研究中每个部分的更多详细信息。

设置后，我们将了解如何触发要播放的声音效果。 有关详细信息，请转到[播放声音](#play-the-sound)。

### <a name="simple3dgameinitialize-method"></a>Simple3dgame:: initialize 方法

在__simple3dgame:: initialize__，其中还初始化__m\_controller__和__m\_renderer__ ，我们设置音频引擎，并准备好以播放声音。

 * 创建__m\_audioController__，这是[音频](#audioh)类的实例。
 * 创建使用[Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method)方法所需的音频资源。 此处，两个__XAudio2__对象&mdash;创建音乐引擎对象和声音引擎对象，并为每个主语音。 音乐引擎对象可用于玩游戏的背景音乐。 声音引擎可以用于在游戏中播放声音效果。 有关详细信息，请参阅[创建并初始化音频资源](#create-and-initialize-the-audio-resources)。
 * 创建__mediaReader__，即[MediaReader](#mediareaderh)类的实例。 [MediaReader](#mediareaderh)，这是一个帮助程序类，用于[SoundEffect](#soundeffecth)类，从文件位置同步读取较小的音频文件，并返回字节数组的形式的声音数据。
 * 使用[mediareader:: Loadmedia](#mediareaderloadmedia-method)从其位置加载声音文件并创建__targetHitSound__变量以保留加载的.wav 声音数据。 有关详细信息，请参阅[加载音频文件](#load-audio)。 

声音效果都与游戏对象关联。 因此发生冲突时与该游戏的对象，它会触发要播放的声音效果。 在此游戏示例中，我们有声音效果 （我们使用全与目标） 弹药和目标。 
    
* 在__GameObject__类中，没有用于关联到对象的声音效果__HitSound__属性。
* 创建[SoundEffect](#soundeffecth)类的新实例并对其进行初始化。 在初始化期间，将创建源语音声音效果。 
* 此类播放声音使用主语音的[音频](#audioh)类中提供。 声音数据读取文件的位置使用[MediaReader](#mediareaderh)类。 有关详细信息，请参阅[将声音对象相关联](#associate-sound-to-object)。

>[!Note]
>由移动和这些游戏对象的碰撞确定实际触发器来播放声音。 因此，调用实际上声音这些[Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method)方法中定义。 有关详细信息，请转到[播放声音](#play-the-sound)。

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

## <a name="create-and-initialize-the-audio-resources"></a>创建和初始化音频资源

* 使用[XAudio2Create](https://msdn.microsoft.com/library/windows/desktop/ee419212)，XAudio2 API 时，可以创建两个新的 XAudio2 对象定义音乐和声音效果引擎。 此方法返回到管理所有音频引擎状态，音频处理线程、 语音图和的详细信息的对象的[IXAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415908)接口指针。
* 后引擎已实例化，使用[ixaudio2:: Createmasteringvoice](https://msdn.microsoft.com/library/windows/desktop/hh405048)为每个声音引擎对象创建主语音。

有关详细信息，请转到[如何： 初始化 XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415779.aspx)。

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

在游戏示例中，用于读取音频格式文件的代码是[MediaReader.h](#mediareaderh)/cpp__ 中定义的。  若要读取编码的.wav 音频文件，请调用[mediareader:: Loadmedia](#mediareaderloadmedia-method)，传入.wav 文件作为输入参数的文件名。

### <a name="mediareaderloadmedia-method"></a>Mediareader:: Loadmedia 方法

此方法使用[媒体基础](https://msdn.microsoft.com/library/windows/desktop/ms694197) API 作为脉冲编码调制 (PCM) 缓冲区读入 .wav 音频文件。

#### <a name="set-up-the-source-reader"></a>设置源阅读器

1. 使用[MFCreateSourceReaderFromURL](https://msdn.microsoft.com/library/windows/desktop/dd388110)创建媒体源阅读器 ([IMFSourceReader](https://msdn.microsoft.com/library/windows/desktop/dd374655))。
2. 使用[MFCreateMediaType](https://msdn.microsoft.com/library/windows/desktop/ms693861)创建的媒体类型 ([IMFMediaType](https://msdn.microsoft.com/library/windows/desktop/ms704850)) 对象 (_mediaType_)。 此外，它表示媒体格式的说明。 
3. 指定_媒体类型_的已解码的输出是 PCM 音频，它是__XAudio2__可以使用的音频类型。
4. 设置解码的输出媒体类型为通过调用[imfsourcereader:: Setcurrentmediatype](https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx)源阅读器。

有关我们为什么要使用源阅读器的详细信息，请转到[源阅读器](https://msdn.microsoft.com/library/windows/desktop/dd940436.aspx)。

#### <a name="describe-the-data-format-of-the-audio-stream"></a>描述音频流的数据格式

1. [Imfsourcereader:: Getcurrentmediatype](https://msdn.microsoft.com/library/windows/desktop/dd374660)用于获取流的当前的媒体类型。
2. 使用[IMFMediaType::MFCreateWaveFormatExFromMFMediaType](https://msdn.microsoft.com/library/windows/desktop/ms702177)将当前的音频媒体类型转换为[WAVEFORMATEX](https://msdn.microsoft.com/library/windows/hardware/ff538799)缓冲区，作为输入使用更早版本的操作的结果。 此结构指定音频加载后，使用波形音频流的数据格式。 

__WAVEFORMATEX__格式可以用于描述 PCM 缓冲区。 相比[WAVEFORMATEXTENSIBLE](https://msdn.microsoft.com/library/windows/hardware/ff538802)结构，它仅可用于来描述音频波形格式的一个子集。 有关__WAVEFORMATEX__和__WAVEFORMATEXTENSIBLE__之间的区别的详细信息，请参阅[可扩展波形格式描述符](https://docs.microsoft.com/windows-hardware/drivers/audio/extensible-wave-format-descriptors)。

#### <a name="read-the-audio-stream"></a>读取音频流

1.  获取的持续时间，以秒为单位，通过调用[imfsourcereader:: Getpresentationattribute](https://msdn.microsoft.com/library/windows/desktop/dd374662) ，然后转换为字节的持续时间的音频流。
2.  通过调用[IMFSourceReader::ReadSample](https://msdn.microsoft.com/library/windows/desktop/dd374665)流的形式读取音频文件中。 __ReadSample__读取将媒体源中的下一个示例。
3.  使用[IMFSample::ConvertToContiguousBuffer](https://msdn.microsoft.com/library/windows/desktop/ms698917.aspx)将音频示例缓冲区 （_示例_） 的内容复制到数组 (_mediaBuffer_)。

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
## <a name="associate-sound-to-object"></a>关联到对象的声音

关联到对象的声音便会在游戏初始化时， [simple3dgame:: initialize](#simple3dgameinitialize-method)方法中。

概述：
* 在__GameObject__类中，没有用于关联到对象的声音效果__HitSound__属性。
* 创建[SoundEffect](#soundeffecth)类对象的新实例并将其与游戏对象关联。 此类播放声音使用__XAudio2__ Api。  它使用由[Audio](#audioh)类提供一个主语音。 可以使用[MediaReader](#mediareaderh)类的文件位置中读取的声音数据。

[Soundeffect:: Initialize](#soundeffectinitialize-method)用于的初始化__SoundEffect__实例使用以下输入参数： 指向声音引擎对象 （ [Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method)方法中创建 IXAudio2 对象）若要设置格式的指针的.wav 文件，将加载使用[mediareader:: Loadmedia](#mediareaderloadmedia-method)方法使用__mediareader:: Getoutputwaveformatex__和声音数据的文件。 在初始化期间，还会创建声音效果的源语音。

### <a name="soundeffectinitialize-method"></a>Soundeffect:: Initialize 方法

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

由于这是更新对象的运动，并确定对象之间的碰撞[Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method)方法中定义触发器来播放声音效果。

由于对象之间的交互的不同很大，具体取决于游戏中，我们不打算讨论了动态的游戏的对象。 如果你有兴趣了解其实现，请转到[Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method)方法。

原则上，在发生冲突时，它会触发声音效果以播放通过调用 [SoundEffect::PlaySound]((soundeffectplaysound-method)。 此方法停止当前正在播放，并且队列与所需的声音数据的内存缓冲区的声音效果。 它使用源语音来设置卷、 提交声音数据，并开始播放。

### <a name="soundeffectplaysound-method"></a>Soundeffect:: Playsound 方法

* 使用源语音对象**m\_sourceVoice**以开始播放声音数据缓冲区**m\_soundData**
* 创建[XAUDIO2\_BUFFER](https://msdn.microsoft.com/library/windows/desktop/ee419228)，到它提供对声音数据缓冲区的引用，然后通过调用[ixaudio2sourcevoice:: Submitsourcebuffer](https://msdn.microsoft.com/library/windows/desktop/ee418473)中提交它。 
* 在声音数据排好队列之后，**SoundEffect::PlaySound** 将通过调用 [IXAudio2SourceVoice::Start](https://msdn.microsoft.com/library/windows/desktop/ee418471) 开始播放。

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

__Simple3DGame::UpdateDynamics__方法负责交互和游戏对象之间的冲突。 当对象碰撞 （或相交也是如此） 时，它会触发关联的声音效果以播放。

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

我们已介绍的 UWP 框架、 图形、 控件、 用户界面和 Windows 10 游戏的音频。 本教程中，[扩展游戏示例](tutorial-resources.md)，下一个部分介绍了开发游戏时可以使用其他选项。

## <a name="audio-concepts"></a>音频概念

对于 Windows 10 游戏开发，使用 XAudio2 版本 2.9。 此版本被随 Windows 10。 有关详细信息，请转到[XAudio2 版本](https://msdn.microsoft.com/library/windows/desktop/ee415802.aspx)。

__AudioX2__是一个低级别的 API，可提供信号处理和混合的基础。 有关详细信息，请参阅[XAudio2 重要概念](https://msdn.microsoft.com/library/windows/desktop/ee415764.aspx)。

### <a name="xaudio2-voices"></a>XAudio2 语音

有三种类型的 XAudio2 语音对象： 源、 子混合和主语音。 语音是 XAudio2 的对象使用处理、 操作，并播放音频数据。 
* 源语音对客户提供的音频数据操作。 
* 源语音和子混合语音将其输出发送到一个或多个子混合语音或主语音。 
* 子混合语音和主语音将传入的所有音频混合在一起，并对结果进行操作。 
* 主语音接收来自源语音和子混合语音的数据，并将该数据发送给音频硬件。

有关详细信息，请转到[XAudio2 语音](https://msdn.microsoft.com/library/windows/desktop/ee415824.aspx)。

### <a name="audio-graph"></a>音频图

音频图是[XAudio2 语音](#xaudio2-voice-objects)的集合。 音频在一侧的源语音中的音频图开始、 （可选） 通过一个或多个子混合语音，并在主语音结束。 音频图将包含有关当前正在播放，零个或多个子混合语音，每种声音源语音和一个主语音。 最简单的音频图中，并在 XAudio2 中，发出所需的最小值是单个源语音输出直接到主语音。 有关详细信息，请转到[音频图](https://msdn.microsoft.com/library/windows/desktop/ee415739.aspx)。

### <a name="additional-reading"></a>其他阅读

* [如何：初始化 XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415779.aspx)
* [如何：在 XAudio2 中加载音频数据文件](https://msdn.microsoft.com/library/windows/desktop/ee415781(v=vs.85).aspx)
* [如何：使用 XAudio2 播放声音](https://msdn.microsoft.com/library/windows/desktop/ee415787.aspx)

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



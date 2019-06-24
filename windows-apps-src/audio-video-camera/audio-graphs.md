---
ms.assetid: CB924E17-C726-48E7-A445-364781F4CCA1
description: 本文介绍如何使用 Windows.Media.Audio 命名空间中的 API 来创建音频路由、混合和处理方案的音频图。
title: 音频图
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ff067729e71ed4d4a49a082adf9fc754804836a6
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317602"
---
# <a name="audio-graphs"></a>音频图



本文介绍如何使用 [**Windows.Media.Audio**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio) 命名空间中的 API 来创建音频路由、混合和处理方案的音频图。

*音频图*是音频数据流经的一组相互连接的音频节点。 

- *音频输入节点*为音频图提供来自音频输入设备、音频文件或自定义代码的音频数据。 

- *音频输出节点*是音频图处理的音频的目标。 可以绕过音频图将音频路由到音频输出设备、音频文件或自定义代码。 

- *子混合节点*从一个或多个节点获取音频，并将其合并为可以路由到音频图中其他节点的单个输出。 

创建完所有节点并在它们之间建立连接后，你只需启动音频图，音频数据便会从输入节点开始，流经所有子混合节点，最后流至输出节点。 此模型可以快速轻松地实现以下方案：将设备麦克风的音频录制到音频文件、通过设备扬声器播放文件中的音频，或混合来自多个源的音频。

其他方案通过向音频图添加音频效果实现。 音频图中的每个节点都可以通过零或更多种音频效果来填充， 音频效果会对通过该节点的音频执行音频处理。 提供回音、均衡器、限制和混响等几种内置效果， 只需几行代码即可将其附加到音频节点。 你还可以创建其效果与内置效果完全相同、你自己的自定义音频效果。

> [!NOTE]
> [AudioGraph UWP 示例](https://go.microsoft.com/fwlink/?LinkId=619481)可实现本概述中所讨论的代码。 你可以下载该示例以查看上下文中的代码，或将该示例用作你自己的应用的起点。

## <a name="choosing-windows-runtime-audiograph-or-xaudio2"></a>选择 Windows 运行时 AudioGraph 或 XAudio2

Windows 运行时音频图 API 提供也可通过使用基于 COM 的 [XAudio2 API](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal) 实现的功能。 以下是不同于 XAudio2 的 Windows 运行时音频图框架的功能。

Windows 运行时音频图 API：

-   比使用 XAudio2 简单得多。
-   除了受 C++ 支持，还可以通过 C# 使用。
-   可以直接使用音频文件，包括压缩的文件格式。 XAudio2 仅在音频缓冲区上运行，不提供任何文件 I/O 功能。
-   可以使用在 Windows 10 中的低延迟音频管道。
-   支持在默认终结点参数处于使用状态时，自动切换终结点。 例如，如果用户从设备扬声器切换到耳机，则音频会自动重定向到新输入。

## <a name="audiograph-class"></a>AudioGraph 类

[  **AudioGraph**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraph) 类是构成音频图的所有节点的父类。 使用此对象创建所有音频节点类型的实例。 可通过以下方式创建 **AudioGraph** 类的实例：初始化包含音频图的配置设置的 [**AudioGraphSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraphSettings) 对象，然后调用 [**AudioGraph.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createasync)。 返回的 [**CreateAudioGraphResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.CreateAudioGraphResult) 将提供对创建的音频图的访问权限，或提供一个错误值（如果音频图创建失败）。

[!code-cs[DeclareAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareAudioGraph)]

[!code-cs[InitAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetInitAudioGraph)]

-   通过使用创建创建所有音频的节点类型\*的方法**AudioGraph**类。
-   [  **AudioGraph.Start**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.start) 方法可使音频图开始处理音频数据。 [  **AudioGraph.Stop**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.stop) 方法终止音频处理。 在音频图运行时，音频图中的每一个节点都可以单独启动和停止，但在音频图停止运行时，没有任何节点会处于活动状态。 [**ResetAllNodes** ](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.resetallnodes)图中要丢弃其音频缓冲区中当前所有数据将导致所有节点。
-   当音频图开始处理新的音频数据量子时，将发生 [**QuantumStarted**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.quantumstarted) 事件。 当处理完某个量子时，将发生 [**QuantumProcessed**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.quantumprocessed) 事件。

-   仅需的 [**AudioGraphSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraphSettings) 属性是 [**AudioRenderCategory**](https://docs.microsoft.com/uwp/api/Windows.Media.Render.AudioRenderCategory)。 指定此值将允许系统优化指定类别的音频管道。
-   音频图的量子大小确定一次处理的样本数。 默认情况下，基于默认采样率，量子大小为 10 毫秒。 如果你通过设置 [**DesiredSamplesPerQuantum**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.desiredsamplesperquantum) 属性来指定自定义量子大小，则还必须将 [**QuantumSizeSelectionMode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.quantumsizeselectionmode) 属性设置为 **ClosestToDesired**，或忽略提供的值。 如果使用此值，则系统将选择尽可能接近你所指定大小的量子大小。 若要确定实际量子大小，请在创建 **AudioGraph** 之后检查它的 [**SamplesPerQuantum**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.samplesperquantum)。
-   如果你仅计划将音频图和文件结合使用，而并不打算输出到音频设备，建议你不设置 [**DesiredSamplesPerQuantum**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.desiredsamplesperquantum) 属性而使用默认量子大小。
-   [  **DesiredRenderDeviceAudioProcessing**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.desiredrenderdeviceaudioprocessing) 属性确定主呈现设备量对音频图输出执行的处理量。 **Default** 设置允许系统针对指定的音频呈现类别使用默认音频处理。 此处理可以明显地改善音频在某些设备上的声音，尤其是配备小型扬声器的移动设备。 **Raw** 设置可以通过尽量减少执行的信号处理量来提高性能，但会导致某些设备上的声音质量变差。
-   如果 [**QuantumSizeSelectionMode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.quantumsizeselectionmode) 设置为 **LowestLatency**，则音频图会自动将 **Raw** 用于 [**DesiredRenderDeviceAudioProcessing**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.desiredrenderdeviceaudioprocessing)。
- 自 Windows 10 版本 1803 起，可设置 [**AudioGraphSettings.MaxPlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.maxplaybackspeedfactor) 属性，进而设置用于 [**AudioFileInputNode.PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.playbackspeedfactor)、[**AudioFrameInputNode.PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.playbackspeedfactor) 和 [**MediaSourceInputNode.PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.mediasourceinputnode.playbackspeedfactor) 属性的最大值。 音频图支持的播放速度系数大于 1 时，系统必须分配额外的内存，以确保拥有足够大的音频数据缓存区。 为此，如果将 **MaxPlaybackSpeedFactor** 设置为应用所需的最低值，则会减少应用的内存消耗。 如果应用仅以正常速度播放内容，建议将 MaxPlaybackSpeedFactor 设置为 1。
-   [  **EncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.encodingproperties) 确定音频图所使用的音频格式。 仅支持 32 位浮点格式。
-   [  **PrimaryRenderDevice**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice) 设置音频图的主呈现设备。 如果不设置，则使用默认系统设备。 主呈现设备用于计算音频图中其他节点的量子大小。 如果系统上不存在音频呈现设备，音频图创建将失败。

你可以让音频图使用默认的音频呈现设备，或者通过调用 [**FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 并传入由 [**Windows.Media.Devices.MediaDevice.GetAudioRenderSelector**](https://docs.microsoft.com/uwp/api/windows.media.devices.mediadevice.getaudiorenderselector) 返回的音频呈现设备选择器，使用 [**Windows.Devices.Enumeration.DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 类来获取系统的可用音频呈现设备列表。 你可以以编程方式选择返回的 **DeviceInformation** 对象之一，或显示 UI 以允许用户选择某台设备，然后使用它来设置 [**PrimaryRenderDevice**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice) 属性。

[!code-cs[EnumerateAudioRenderDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioRenderDevices)]

##  <a name="device-input-node"></a>设备输入节点

设备输入节点通过连接到系统的音频捕获设备（例如麦克风）将音频送入音频图中。 通过调用 [**CreateDeviceInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createdeviceinputnodeasync) 创建使用系统的默认音频捕获设备的 [**DeviceInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceInputNode) 对象。 提供 [**AudioRenderCategory**](https://docs.microsoft.com/uwp/api/Windows.Media.Render.AudioRenderCategory)，以允许系统优化指定类别的音频管道。

[!code-cs[DeclareDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceInputNode)]


[!code-cs[CreateDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceInputNode)]

如果你想要指定设备输入节点的特定音频捕获设备，则可以使用[ **Windows.Devices.Enumeration.DeviceInformation** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)类，以获取系统的可用音频的列表通过调用捕获设备[ **FindAllAsync** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)并在返回的音频呈现设备选择器将传递[ **Windows.Media.Devices.MediaDevice.GetAudioCaptureSelector**](https://docs.microsoft.com/uwp/api/windows.media.devices.mediadevice.getaudiocaptureselector)。 你可以以编程方式选择返回的 **DeviceInformation** 对象之一，或显示 UI 以允许用户选择某台设备，然后将其传递到 [**CreateDeviceInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createdeviceinputnodeasync) 中。

[!code-cs[EnumerateAudioCaptureDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioCaptureDevices)]

##  <a name="device-output-node"></a>设备输出节点

设备输出节点可将音频从音频图推送到音频呈现设备，例如扬声器或耳机。 通过调用 [**CreateDeviceOutputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createdeviceoutputnodeasync) 创建 [**DeviceOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode)。 输出节点使用音频图的 [**PrimaryRenderDevice**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice)。

[!code-cs[DeclareDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceOutputNode)]

[!code-cs[CreateDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceOutputNode)]

##  <a name="file-input-node"></a>文件输入节点

文件输入节点允许你将音频文件中的数据送入音频图。 通过调用 [**CreateFileInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createfileinputnodeasync) 创建 [**AudioFileInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileInputNode)。

[!code-cs[DeclareFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileInputNode)]


[!code-cs[CreateFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileInputNode)]

-   文件输入节点支持以下文件格式：mp3、wav、wma、m4a。
-   设置 [**StartTime**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.starttime) 属性，指定文件应开始播放时的时间偏移。 如果此属性为 null，则使用文件的开头。 设置 [**EndTime**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.endtime) 属性，指定文件应结束播放时的时间偏移。 如果此属性为 null，则使用文件的末尾。 开始时间值必须小于结束时间值，并且结束时间值必须小于或等于音频文件的持续时间，后者可以通过检查 [**Duration**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.duration) 属性值来确定。
-   通过调用 [**Seek**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.seek) 并在文件中指定播放位置应移动到的时间偏移，在音频文件中寻找某个位置。 指定的值必须介于 [**StartTime**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.starttime) 和 [**EndTime**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.endtime) 范围内。 通过只读的 [**Position**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.position) 属性获取节点的当前播放位置。
-   通过设置 [**LoopCount**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.loopcount) 属性启用音频文件的循环播放。 如果此值为非 null，则指示该文件在初始播放后继续播放的次数。 例如，将 **LoopCount** 设置为 1 将导致该文件总共播放 2 次，而将其设置为 5 将导致该文件总共播放 6 次。 将 **LoopCount** 设置为 null 将导致该文件无限循环播放。 若要停止循环播放，请将该值设置为 0。
-   通过设置 [**PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.playbackspeedfactor) 调整音频文件的播放速度。 值 1 表示该文件的原始速度、0.5 表示半速，2 表示双倍速度。

##  <a name="mediasource-input-node"></a>MediaSource 输入节点

[  **MediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource) 类提供从不同的源引用媒体的常用方法，并公开用于访问媒体数据的常用模型，而不考虑基础媒体格式（可能是磁盘上的文件或自适应流式处理网络源）。 可使用 [**MediaSourceAudioInputNode](https://docs.microsoft.com/uwp/api/windows.media.audio.mediasourceaudioinputnode) 节点将 **MediaSource** 中的音频数据定向到音频图中。 通过调用 [**CreateMediaSourceAudioInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createmediasourceaudioinputnodeasync#Windows_Media_Audio_AudioGraph_CreateMediaSourceAudioInputNodeAsync_Windows_Media_Core_MediaSource_) 创建 **MediaSourceAudioInputNode**，进而传入代表想要播放的内容的 **MediaSource** 对象。 返回了 [**CreateMediaSourceAudioInputNodeResult](https://docs.microsoft.com/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult)，你可以使用它检查 [**Status**](https://docs.microsoft.com/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult.status) 属性，从而确定操作的状态。 如果状态是**成功**，可通过访问 [**Node**](https://docs.microsoft.com/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult.node) 属性获取所创建的 **MediaSourceAudioInputNode**。 以下示例介绍如何使用代表通过网络进行内容流式处理的 AdaptiveMediaSource 对象创建节点。 若要详细了解如何使用 **MediaSource**，请参阅[媒体项、播放列表和曲目](media-playback-with-mediasource.md)。 若要详细了解如何通过 Internet 流式处理媒体内容，请参阅[自适应流式处理](adaptive-streaming.md)。

[!code-cs[DeclareMediaSourceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareMediaSourceInputNode)]

[!code-cs[CreateMediaSourceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateMediaSourceInputNode)]

若要在播放到 **MediaSource** 内容的末尾时收到通知，请注册 [**MediaSourceCompleted**](https://docs.microsoft.com/uwp/api/windows.media.audio.mediasourceaudioinputnode.mediasourcecompleted) 事件的处理程序。 

[!code-cs[RegisterMediaSourceCompleted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetRegisterMediaSourceCompleted)]

[!code-cs[MediaSourceCompleted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetMediaSourceCompleted)]

虽然从磁盘播放文件的成功率很大，但由于网络连接的更改或其他音频图无法控制的问题，从网络源中流式处理的媒体仍有可能在播放时出现问题。 如果播放期间无法播放 **MediaSource**，音频图将发起 [**UnrecoverableErrorOccurred**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.unrecoverableerroroccurred) 事件。 可使用该事件的处理程序停止并处理音频图，然后重新初始化音频图。 

[!code-cs[RegisterUnrecoverableError](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetRegisterUnrecoverableError)]

[!code-cs[UnrecoverableError](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUnrecoverableError)]

##  <a name="file-output-node"></a>文件输出节点

可以使用文件输出节点将音频图中的音频数据定向到音频文件中。 通过调用 [**CreateFileOutputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createfileoutputnodeasync) 创建 [**AudioFileOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileOutputNode)。

[!code-cs[DeclareFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileOutputNode)]


[!code-cs[CreateFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileOutputNode)]

-   文件输出节点支持以下文件格式：mp3、wav、wma、m4a。
-   在调用 [**AudioFileOutputNode.FinalizeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileoutputnode.finalizeasync) 前，必须先调用 [**AudioFileOutputNode.Stop**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileoutputnode.stop) 以停止节点处理，否则将引发异常。

##  <a name="audio-frame-input-node"></a>音频帧输入节点

音频帧输入节点允许你将使用自己的代码生成的音频数据推送到音频图中。 这可实现创建自定义软件合成器等方案。 通过调用 [**CreateFrameInputNode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createframeinputnode) 创建 [**AudioFrameInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFrameInputNode)。

[!code-cs[DeclareFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameInputNode)]


[!code-cs[CreateFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameInputNode)]

当音频图准备开始处理下一音频数据量子时，将引发 [**FrameInputNode.QuantumStarted**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.quantumstarted) 事件。 你将提供从此事件的处理程序中生成的自定义音频数据。

[!code-cs[QuantumStarted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumStarted)]

-   传递到 **QuantumStarted** 事件处理程序中的 [**FrameInputNodeQuantumStartedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.FrameInputNodeQuantumStartedEventArgs) 对象将公开 [**RequiredSamples**](https://docs.microsoft.com/uwp/api/windows.media.audio.frameinputnodequantumstartedeventargs.requiredsamples) 属性，该属性指示音频图需要多少样本才能填满待处理的量子。
-   调用 [**AudioFrameInputNode.AddFrame**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.addframe)，将已填充音频数据的 [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame) 对象传递到音频图中。
- Windows 10 版本 1803 中采用了一组新的 API，借助其可将 **MediaFrameReader** 与音频数据结合使用。 借助这些 API，可从媒体帧源中获取 **AudioFrame** 对象，后者可通过 **AddFrame** 方法传递到 **FrameInputNode** 中。 有关详细信息，请参阅[使用 MediaFrameReader 处理音频帧](process-audio-frames-with-mediaframereader.md)。
-   以下显示了 **GenerateAudioData** 帮助程序方法的一个示例实现。

若要使用音频数据填充 [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame)，则必须访问音频帧的基础内存缓冲区。 为此，必须通过在你的命名空间内添加以下代码来初始化 **IMemoryBufferByteAccess** COM 接口。

[!code-cs[ComImportIMemoryBufferByteAccess](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetComImportIMemoryBufferByteAccess)]

以下代码显示 **GenerateAudioData** 帮助程序方法的示例实现，该方法将创建 [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame) 并使用音频数据填充它。

[!code-cs[GenerateAudioData](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetGenerateAudioData)]

-   因为此方法可访问含有基础 Windows 运行时类型的原始缓冲区，所以必须使用 **unsafe** 关键字进行声明。 你还必须使用 Microsoft Visual Studio 配置你的项目，以允许通过以下操作编译不安全的代码：打开项目的 **“属性”** 页面、单击 **“生成”** 属性页，然后选中 **“允许不安全代码”** 复选框。
-   通过将所需的缓冲区大小传入构造函数，在 **Windows.Media** 命名空间中初始化 [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame) 的新实例。 缓冲区大小等于样本数乘以每个样本的大小。
-   通过调用 [**LockBuffer**](https://docs.microsoft.com/uwp/api/windows.media.audioframe.lockbuffer) 获取音频帧的 [**AudioBuffer**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioBuffer)。
-   通过调用 [**CreateReference**](https://docs.microsoft.com/uwp/api/windows.media.audiobuffer.createreference) 从音频缓冲区获取 [**IMemoryBufferByteAccess**](https://docs.microsoft.com/previous-versions/mt297505(v=vs.85)) COM 接口的实例。
-   通过调用 [**IMemoryBufferByteAccess.GetBuffer**](https://docs.microsoft.com/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer) 获取指向原始音频缓冲区数据的指针，并将其转换为音频数据的样本数据类型。
-   使用数据填充缓冲区并返回 [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame) 以提交到音频图中。

##  <a name="audio-frame-output-node"></a>音频帧输出节点

音频帧输出节点允许你使用你创建的自定义代码来接收和处理来自音频图的音频数据输出。 其示例方案是：对音频输出执行信号分析。 通过调用 [**CreateFrameOutputNode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createframeoutputnode) 创建 [**AudioFrameOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFrameOutputNode)。

[!code-cs[DeclareFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameOutputNode)]

[!code-cs[CreateFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameOutputNode)]

当音频图开始处理音频数据量子时，将引发 [**AudioGraph.QuantumStarted**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraph.QuantumStarted) 事件。 你可以访问来自此事件的处理程序中的音频数据。 

> [!NOTE]
> 如果你想按照规律的节奏检索与音频图同步的音频帧，可从同步的 **QuantumStarted** 事件处理程序中调用 [AudioFrameOutputNode.GetFrame](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeoutputnode.GetFrame)。 音频引擎在完成音频处理后，将异步引发 **QuantumProcessed** 事件，这表示其节奏可能没有规律。 因此，不应使用 **QuantumProcessed** 事件来同步处理音频帧数据。

[!code-cs[SnippetQuantumStartedFrameOutput](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumStartedFrameOutput)]

-   调用 [**GetFrame**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeoutputnode.getframe) 以获取已填充音频图中音频数据的 [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame) 对象。
-   以下显示了 **ProcessFrameOutput** 帮助程序方法的一个示例实现。

[!code-cs[ProcessFrameOutput](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetProcessFrameOutput)]

-   与上述音频帧输入节点示例类似，你将需要声明 **IMemoryBufferByteAccess** COM 接口并将你的项目配置为允许不安全代码，才能访问基础音频缓冲区。
-   通过调用 [**LockBuffer**](https://docs.microsoft.com/uwp/api/windows.media.audioframe.lockbuffer) 获取音频帧的 [**AudioBuffer**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioBuffer)。
-   通过调用 [**CreateReference**](https://docs.microsoft.com/uwp/api/windows.media.audiobuffer.createreference) 从音频缓冲区获取 **IMemoryBufferByteAccess** COM 接口的实例。
-   通过调用 **IMemoryBufferByteAccess.GetBuffer** 获取指向原始音频缓冲区数据的指针，并将其转换为音频数据的样本数据类型。

## <a name="node-connections-and-submix-nodes"></a>节点连接和子混合节点

所有输入节点类型都将公开 **AddOutgoingConnection** 方法，该方法可将节点产生的音频路由到传入该方法的节点。 以下示例将 [**AudioFileInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileInputNode) 连接到 [**AudioDeviceOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode)，这是一种用于在设备扬声器上播放音频文件的简单设置。

[!code-cs[AddOutgoingConnection1](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection1)]

你可以创建多个从某个输入节点到其他节点的连接。 下面的示例添加了从 [**AudioFileInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileInputNode) 到 [**AudioFileOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileOutputNode) 的另一种连接方法。 现在，音频文件中的音频将在设备的扬声器中播放，还将写出到音频文件。

[!code-cs[AddOutgoingConnection2](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection2)]

输出节点也可以接收来自其他节点的多个连接。 下面的示例将建立从 [**AudioDeviceInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceInputNode) 到 [**AudioDeviceOutput**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode) 节点的连接。 由于输出节点具有来自文件输入节点和设备输入节点的连接，输出将包含来自这两个源的混合音频。 **AddOutgoingConnection** 将提供一个重载，可使你为通过连接传递的信号指定增益值。

[!code-cs[AddOutgoingConnection3](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection3)]

虽然输出节点可以接受来自多个节点的连接，但是你可能希望在将混合音频传递到输出之前，先从一个或多个节点创建中级混合信号。 例如，你可能想要设置级别或将效果应用到音频图中音频信号的子集。 若要执行此操作，请使用 [**AudioSubmixNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioSubmixNode)。 你可以从一个或多个输入节点或其他子混合节点连接到某个子混合节点。 下面的示例将使用 [**AudioGraph.CreateSubmixNode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createsubmixnode) 创建一个新的子混合节点。 然后，将添加从文件输入节点和帧输出节点到子混合节点的连接。 最后，子混合节点将连接到文件输出节点。

[!code-cs[CreateSubmixNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateSubmixNode)]

## <a name="starting-and-stopping-audio-graph-nodes"></a>启动和停止音频图节点

调用 [**AudioGraph.Start**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.start) 时，音频图将开始处理音频数据。 每个节点类型都提供可使单个节点开始或停止处理数据的 **Start** 和 **Stop** 方法。 调用 [**AudioGraph.Stop**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.stop) 时，无论个别节点的状态如何，所有节点中的所有音频处理都将停止，但可在音频图停止时设置各个节点的状态。 例如，你可以在音频图停止时对单个节点调用 **Stop**，然后调用 **AudioGraph.Start**，这样该单个节点将保持在停止状态。

所有节点类型都将公开 **ConsumeInput** 属性，当将该属性设置为 false 时，节点可以继续处理音频，但会阻止它使用要从其他节点输入的任何音频数据。

所有节点类型都将公开 **Reset** 方法，该方法将使节点丢弃当前处于其缓冲区中的任何音频数据。

## <a name="adding-audio-effects"></a>添加音频效果

可以使用音频图 API 将音频效果添加到音频图中各种类型的节点。 输出节点、输入节点和子混合节点分别可以具有无数音频效果，仅受硬件功能限制。下面的示例将演示如何将内置回音效果添加到子混合节点。

[!code-cs[AddEffect](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddEffect)]

-   所有音频效果都可实现 [**IAudioEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IAudioEffectDefinition)。 每个节点都将公开 **EffectDefinitions** 属性，它表示应用于该节点的效果的列表。 通过将效果的定义对象添加到列表来添加效果。
-   **Windows.Media.Audio** 命名空间中提供了多个效果定义类。 这些问题包括：
    -   [**EchoEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.EchoEffectDefinition)
    -   [**EqualizerEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.EqualizerEffectDefinition)
    -   [**LimiterEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.LimiterEffectDefinition)
    -   [**ReverbEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.ReverbEffectDefinition)
-   你可以创建自己的可实现 [**IAudioEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IAudioEffectDefinition) 的音频效果，并将它们应用到音频图中的任何节点。
-   每个节点类型都将公开 **DisableEffectsByDefinition** 方法，该方法可禁用节点的 **EffectDefinitions** 列表中使用指定定义添加的所有效果。 **EnableEffectsByDefinition** 使用指定定义启用效果。

## <a name="spatial-audio"></a>空间音频
自 Windows 10 版本 1607 开始，**AudioGraph** 支持空间音频，这使你可以根据来自任何输入或子混合节点发出的音频指定 3D 空间中的位置。 还可以指定发出音频的形状和方向、将用于 Doppler 切换节点音频的速率，以及定义描述音频如何随距离衰减的衰减模型。 

若要创建发射器，可以先创建从发射器投射声音所用的形状，该形状可以是锥形或全向。 [  **AudioNodeEmitterShape**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioNodeEmitterShape) 类提供用于创建其中每个形状的静态方法。 接下来，创建一个衰减模型。 该模型定义发射器的音频音量如何随侦听器距离的增加而降低。 [  **CreateNatural**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodeemitterdecaymodel.createnatural) 方法创建衰减模型，可使用距离平方衰减模型模拟声音的自然衰减。 最后，创建 [**AudioNodeEmitterSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioNodeEmitterSettings) 对象。 当前，此对象仅用于启用和禁用基于速率的发射器音频的 Doppler 衰减。 调用 [**AudioNodeEmitter**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodeemitter.-ctor) 构造函数，传入刚创建的初始化对象。 默认情况下，将发射器放置在原点，但你可以使用 [**Position**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodeemitter.position) 属性设置发射器的位置。

> [!NOTE]
> 音频节点发射器只可以处理采样频率为 48kHz 的单声道格式的音频。 尝试使用立体声音频或其他采样频率的音频会导致异常。

当使用适用于所需节点类型的重载创建方法创建音频节点时，请将发射器分配给它。 在此示例中，[**CreateFileInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createfileinputnodeasync) 用于从指定文件创建文件输入节点以及要与该节点关联的 [**AudioNodeEmitter**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioNodeEmitter) 对象。

[!code-cs[CreateEmitter](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateEmitter)]

将音频图中的音频输出到用户的 [**AudioDeviceOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode) 具有侦听器对象（可使用 [**Listener**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiodeviceoutputnode.listener) 属性访问），它表示 3D 空间中用户的位置、方向和速度。 音频图中所有发射器的位置都与发射器对象的位置和方向有关。 默认情况下，侦听器位于原点 (0,0,0)（沿 Z 轴正面向前），但可以使用 [**Position**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodelistener.position) 和 [**Orientation**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodelistener.orientation) 属性设置其位置和方向。

[!code-cs[Listener](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetListener)]

可以更新运行时发射器的位置、速度和方向，以模拟音频源在 3D 空间中的移动。

[!code-cs[UpdateEmitter](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUpdateEmitter)]

还可以更新运行时侦听器对象的位置、速度和方向，以模拟用户在 3D 空间中的移动。

[!code-cs[UpdateListener](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUpdateListener)]

默认情况下，将使用 Microsoft 的头部相关传输函数 (HRTF) 算法计算空间音频，以根据相对于侦听器的音频的形状、速度和位置衰减音频。 可以将 [**SpatialAudioModel**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodeemitter.spatialaudiomodel) 属性设置为 **FoldDown**，以使用简单立体声混音方法模拟空间音频（该方法不太精确，但需要的 CPU 和内存资源也较少）。

## <a name="see-also"></a>请参阅
- [媒体播放](media-playback.md)
 

 





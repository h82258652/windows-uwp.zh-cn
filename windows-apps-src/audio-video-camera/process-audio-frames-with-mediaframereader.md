---
author: drewbatgit
ms.assetid: D6A785C6-DF28-47E6-BDC1-7A7129EC40A0
description: 本文介绍了如何将 MediaFrameReader 和 MediaCapture 结合使用以从捕获源中获取包含音频数据的 AudioFrames。
title: 使用 MediaFrameReader 处理音频帧
ms.author: drewbat
ms.date: 04/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d69e8d8cca3932045d4b43d727210f84e816f30b
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5873402"
---
# <a name="process-audio-frames-with-mediaframereader"></a>使用 MediaFrameReader 处理音频帧

本文介绍了如何将 [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) 和 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) 结合使用以从媒体帧源中获取音频数据。 若要了解如何使用 **MediaFrameReader** 获取图像数据（例如从彩色相机、红外相机或深度相机），请参阅[使用 MediaFrameReader 处理媒体帧](process-media-frames-with-mediaframereader.md)。 本文概述了帧阅读器使用模式，并讨论了 **MediaFrameReader** 类的一些其他功能，如使用 **MediaFrameSourceGroup** 同时从多个源检索帧。 

> [!NOTE] 
> 本文中讨论的功能仅从 Windows 10 版本 1803 开始提供。

> [!NOTE] 
> 文中提供了一个通用 Windows 应用示例，介绍使用 **MediaFrameReader** 显示不同帧源（包括彩色、深度和红外相机）的帧。 有关详细信息，请参阅[相机帧示例](http://go.microsoft.com/fwlink/?LinkId=823230)。

## <a name="setting-up-your-project"></a>设置项目
获取音频帧的过程在很大程度上与获取其他类型的媒体帧相同。 对于使用 **MediaCapture** 的任何应用，在尝试访问任何相机设备前都必须声明应用使用 *webcam* 功能。 如果应用从音频设备捕获音频，还应声明 *microphone* 设备功能。 

**将功能添加到应用清单**

1.  在 Microsoft Visual Studio 的**解决方案资源管理器**中，通过双击 **package.appxmanifest** 项，打开应用程序清单的设计器。
2.  选择**功能**选项卡。
3.  选中**摄像头**框和**麦克风**框。
4.  若要访问图片库和视频库，请选中**图片库**框和**视频库**框。



## <a name="select-frame-sources-and-frame-source-groups"></a>选择帧源和帧源组

捕获音频帧的第一步是初始化表示音频数据源的 [**MediaFrameSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource)，如麦克风或其他音频捕获设备。 若要执行此操作，必须创建 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) 对象的新实例。 此示例中，**MediaCapture** 的唯一初始化设置是设置 [**StreamingCaptureMode**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.streamingcapturemode) 以指示我们要从捕获设备中流式传输音频。 

调用 [**MediaCapture.InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync) 后，可以使用 [**FrameSources**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.framesources) 属性获取可访问媒体帧源的列表。 此示例中使用 Linq 查询选择描述帧源的 [**MediaFrameSourceInfo**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo) 具有**音频**的 [**MediaStreamType**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.mediastreamtype) 的所有帧源，指示媒体源产生音频数据。

如果查询返回一个或多个帧源，可以检查 [**CurrentFormat**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.currentformat) 属性以查看源是否支持所需音频格式 - 此示例中是音频浮点数据。 检查 [**AudioEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframeformat.audioencodingproperties) 以确保源支持所需音频编码。

[!code-cs[InitAudioFrameSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitAudioFrameSource)]

## <a name="create-and-start-the-mediaframereader"></a>创建并启动 MediaFrameReader

通过调用 [**MediaCapture.CreateFrameReaderAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_) 获取 **MediaFrameReader** 的新实例，传递在上一步所选的 **MediaFrameSource** 对象。 默认情况下，音频帧是在缓冲模式下获得的，这降低了丢失帧的可能性，但是如果音频帧处理速度不够快并填满系统分配的内存缓冲区，仍然会出现这种情况。

为音频数据的新帧可用时系统引发的 [**MediaFrameReader.FrameArrived**](*https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) 事件注册处理程序。 调用 [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.startasync) 以开始获取音频帧。 如果帧阅读器无法启动，从调用返回的状态值将包含 [**Success**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereaderstartstatus) 以外的值。

[!code-cs[CreateAudioFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCreateAudioFrameReader)]

在 **FrameArrived** 事件处理程序中，调用作为发件人传递到处理程序的 **MediaFrameReader** 对象上的 [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe) 以尝试检索对最新媒体帧的引用。 请注意，此对象可以为 null，因此在使用对象前应始终进行检查。 从 **TryAcquireLatestFrame** 返回的 **MediaFrameReference** 中包装的媒体帧的类型取决于配置帧阅读器要获取的帧源或源的类型。 由于此示例中的帧阅读器设置为获取音频帧，因此它使用 [**AudioMediaFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference.audiomediaframe) 属性获取基础帧。 

下面示例中的 **ProcessAudioFrame** 帮助程序方法展示了如何获取 [**AudioFrame**](https://docs.microsoft.com/uwp/api/windows.media.audioframe)，它提供了帧的时间戳以及它是否与 **AudioMediaFrame** 对象不连续等信息。 若要阅读或处理音频样本数据，将需要从 **AudioMediaFrame** 对象获取 [**AudioBuffer**](https://docs.microsoft.com/uwp/api/windows.media.audiobuffer)，创建 [**IMemoryBufferReference**](https://docs.microsoft.com/uwp/api/windows.foundation.imemorybufferreference)，然后调用 COM 方法 **IMemoryBufferByteAccess::GetBuffer** 检索数据。 有关访问本机缓冲区的更多信息，请参阅代码列表下的说明。

数据的格式取决于帧源。 在此示例中，在选择媒体帧源时，我们确保所选帧源使用浮点数据的单个频道。 其余的示例代码演示了如何确定帧中音频数据的持续时间和示例计数。  

[!code-cs[ProcessAudioFrame](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetProcessAudioFrame)]

> [!NOTE] 
> 要对音频数据执行操作，必须访问本机内存缓冲区。 为此，必须通过添加以下代码列表来使用 **IMemoryBufferByteAccess** COM 接口。 对本机缓冲区的操作必须在使用**不安全**关键字的方法中执行。 还需要选中该框，以便在**项目 -> 属性**对话框的**版本**选项卡中允许不安全代码。

[!code-cs[IMemoryBufferByteAccess](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetIMemoryBufferByteAccess)]

## <a name="additional-information-on-using-mediaframereader-with-audio-data"></a>有关使用带有音频数据的 MediaFrameReader 的详细信息

可以通过访问 [**MediaFrameSource.Controller**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.controller) 属性来检索与音频帧源相关联的[**AudioDeviceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.AudioDeviceController)。 此对象可用于获取或设置捕获设备的流属性或控制捕获级别。 下面的示例使音频设备静音，以便帧阅读器继续获取帧，但所有样本的值都为 0。

[!code-cs[AudioDeviceControllerMute](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetAudioDeviceControllerMute)]

可以使用 [**AudioFrame**](https://docs.microsoft.com/uwp/api/windows.media.audioframe) 对象将媒体帧捕获的音频数据传递到 [**AudioGraph**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph)。 将帧传递到 [**AudioFrameInputNode**](https://docs.microsoft.com/en-us/uwp/api/windows.media.audio.audioframeinputnode)的 [**AddFrame**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.addframe) 方法。 有关使用音频图捕获、处理和混合音频信号的详细信息，请参阅[音频图](audio-graphs.md)。

## <a name="related-topics"></a>相关主题

* [使用 MediaFrameReader 处理媒体帧](process-media-frames-with-mediaframereader.md)
* [相机](camera.md)
* [使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [相机帧示例](http://go.microsoft.com/fwlink/?LinkId=823230)
* [音频图](audio-graphs.md)
 







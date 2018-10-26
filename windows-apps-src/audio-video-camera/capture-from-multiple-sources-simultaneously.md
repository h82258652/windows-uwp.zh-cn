---
author: drewbatgit
ms.assetid: ''
description: 本文介绍如何使用多个嵌入式视频轨同时从多个来源将视频捕获到一个文件。
title: 使用 MediaFrameSourceGroup 从多个来源捕获
ms.author: drewbat
ms.date: 09/12/2017
ms.topic: article
keywords: windows 10, uwp, 捕获, 视频
ms.localizationpriority: medium
ms.openlocfilehash: ae52026d5fb1ab3c140edfdcd1f92f7d3d0fd143
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5594586"
---
# <a name="capture-from-multiple-sources-using-mediaframesourcegroup"></a>使用 MediaFrameSourceGroup 从多个来源捕获

本文介绍如何使用多个嵌入式视频轨同时从多种来源将视频捕获到一个文件。 从 RS3 开始，可以为单个 **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** 指定多个 **[VideoStreamDescriptor](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor)** 对象。 这样可以同时将多个流编码到单个文件中。 在此操作中编码的视频流必须包含在当前设备上指定了一组可同时使用的摄像机的单个 **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)**。 

有关如何结合使用 **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** 和 **[MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader)** 类来启用多个摄像机的实时计算机视觉方案的信息，请参阅[使用 MediaFrameReader 处理媒体帧](process-media-frames-with-mediaframereader.md)。

本文的其余部分将介绍使用多个视频轨将来自两台彩色摄像机的视频录制到单个文件的步骤。

## <a name="find-available-sensor-groups"></a>查找可用的传感器组
一个 **MediaFrameSourceGroup** 表示一系列可同时访问的帧源（通常是摄像机）。 每个设备的可用帧源组各不相同，因此本示例中的第一步是获取可用帧源组的列表，并查找包含应用场景所需摄像机的帧源组，在本例中需要两个彩色摄像机。

**[MediaFrameSourceGroup.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)** 方法返回当前设备上的所有可用源组。 返回的每个 **MediaFrameSourceGroup** 都具有一系列 **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** 对象，这些对象描述了组中的每一个帧源。 Linq 查询用于查找包含两个彩色摄像机的源组，一个在前面板，一个在背面。 对于每个彩色摄像机都将返回一个包含所选 **MediaFrameSourceGroup** 和 **MediaFrameSourceInfo** 的匿名对象。 不使用 Linq 语法，也可以循环查找每个组，然后循环查找每个 **MediaFrameSourceInfo**，找到满足要求的组。

请注意，不是每个设备都包含具有两个彩色相机的源组，因此在尝试捕获视频之前，应检查以确保找到这样的源组。

[!code-cs[MultiRecordFindSensorGroups](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordFindSensorGroups)]

## <a name="initialize-the-mediacapture-object"></a>初始化 MediaCapture 对象
**[MediaCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)** 类是在 UWP 应用中用于大多数音频、视频和照片捕获操作的主类。 通过调用 **[InitializeAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.InitializeAsync)**、传入包含初始化参数对象的 **[MediaCaptureInitializationSettings](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings)** 对象来初始化对象。 在本示例中，唯一指定的设置是 **[SourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)** 属性，它设置为在上面的示例代码中检索到的 **MediaFrameSourceGroup**。

有关 **MediaCapture** 和其他用于捕获媒体的 UWP 应用功能可以执行的其他操作的信息，请参阅[摄像机](camera.md)。

[!code-cs[MultiRecordInitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordInitMediaCapture)]

## <a name="create-a-mediaencodingprofile"></a>创建 MediaEncodingProfile
**[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** 类告诉媒体捕获管道在将捕获到的音频和视频写入文件时应如何对其进行编码。 对于典型的捕获和转换代码应用场景，此类提供一系列静态方法来创建常用配置文件，如 **[CreateAvi](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi)** 和 **[CreateMp3](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)**。 对于此例，编码配置文件可使用 Mpeg4 容器和 H264 视频编码手动创建。 视频编码设置使用 **[VideoEncodingProperties](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.videoencodingproperties)** 对象指定。 对于此应用场景中使用的每个彩色摄像机，都会配置一个 **VideoStreamDescriptor** 对象。 该描述符使用指定编码的 **VideoEncodingProperties** 对象来构造。 **VideoStreamDescriptor** 的 **[Label](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor.Label)** 属性必须设置为将捕获到流中的媒体帧源的 ID。 这样，捕获管道就能知道对于每个摄像机应该使用的流描述符和编码属性。 帧源的 ID 由在上一节中已选择 **MediaFrameSourceGroup** 的情况下找到的 **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** 对象公布。


自 Windows 10 版本 1709 起，可通过调用 **[SetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.setvideotracks)** 对 **MediaEncodingProfile** 设置多个编码属性。 可以通过调用 **[GetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.GetVideoTracks)** 获取视频流描述符列表。 请注意，如果设置存储单个流描述符的 **[Video](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.Video)** 属性，则通过调用 **SetVideoTracks** 设置的描述符列表将替换为包含你指定的单个描述符的列表。


[!code-cs[MultiRecordMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordMediaEncodingProfile)]

### <a name="encode-timed-metadata-in-media-files"></a>在媒体文件中对计时元数据进行编码

自 Windows 10 版本 1803 起，除了音频和视频，还可将计时元数据编码为支持该数据格式的媒体文件。 例如，GoPro 元数据 (gpmd) 可存储在 MP4 文件中，传递与视频流相关的地理位置。 

进行元数据编码时使用的是与音频或视频编码相似的模式。 [**TimedMetadataEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties) 类可描述元数据的类型、子类型和编码属性，类似于视频中 **VideoEncodingProperties** 的功能。 [**TimedMetadataStreamDescriptor**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatastreamdescriptor) 可标识元数据流，类似于视频流中 **VideoStreamDescriptor** 的功能。  

以下示例演示了如何初始化 **TimedMetadataStreamDescriptor** 对象。 首先，创建 **TimedMetadataEncodingProperties** 对象，并将 **Subtype** 设置为用于识别将包含在流中的元数据类型的 GUID。 本示例使用 GoPro 元数据 (gpmd) 的 GUID。 通过调用 [**SetFormatUserData**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata) 方法设置格式特定的数据。 对于 MP4 文件，格式特定的数据存储在 SampleDescription 框 (stsd) 中。 然后，通过编码属性创建新的 **TimedMetadataStreamDescriptor**。 **Label** 和 **Name** 属性设置为识别要编码的流。 

[!code-cs[GetStreamDescriptor](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetGetStreamDescriptor)]

调用 [MediaEncodingProfile.SetTimedMetadataTracks](**https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks**) 将元数据流描述符添加到编码配置文件。 以下示例介绍的帮助程序方法采用了两个视频流描述符、一个音频流描述符和一个时标元数据流描述符，同时返回可用于对流进行编码的 **MediaEncodingProfile**。

[!code-cs[GetMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetGetMediaEncodingProfile)]

## <a name="record-using-the-multi-stream-mediaencodingprofile"></a>使用多流 MediaEncodingProfile 进行录制
本示例的最后一步是通过调用 **[StartRecordToStorageFileAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.startrecordtostoragefileasync)** 并传入要将所捕获媒体写入其中的 **StorageFile** 以及在前面的示例代码中创建的 **MediaEncodingProfile**，来启动视频捕获。 等待几秒钟后，录制将通过调用 **[StopRecordAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.StopRecordAsync)** 停止。

[!code-cs[MultiRecordToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordToFile)]

操作完成时，应该已创建了一个视频文件，其中包含从每个摄像机捕获的视频（编码为文件中单独的流）。 有关播放包含多个视频轨的媒体文件的信息，请参阅[媒体项、播放列表和轨](media-playback-with-mediasource.md)。

## <a name="related-topics"></a>相关主题

* [相机](camera.md)
* [使用 MediaCapture 进行照片、视频和音频的基本捕获](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [使用 MediaFrameReader 处理媒体帧](process-media-frames-with-mediaframereader.md)
* [媒体项、播放列表和轨](media-playback-with-mediasource.md)


 

 





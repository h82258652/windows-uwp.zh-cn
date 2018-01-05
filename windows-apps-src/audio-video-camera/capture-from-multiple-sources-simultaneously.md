---
author: drewbatgit
ms.assetid: 
description: "本文介绍如何使用多个嵌入式视频轨同时从多个来源将视频捕获到一个文件。"
title: "使用 MediaFrameSourceGroup 从多个来源捕获"
ms.author: drewbat
ms.date: 09/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 捕获, 视频"
ms.localizationpriority: medium
ms.openlocfilehash: 56996cf9edb5aef317421e5ca8eef05153328bf9
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="capture-from-multiple-sources-using-mediaframesourcegroup"></a>使用 MediaFrameSourceGroup 从多个来源捕获

本文介绍如何使用多个嵌入式视频轨同时从多种来源将视频捕获到一个文件。 从 RS3 开始，可以为单个 **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** 指定多个 **[VideoStreamDescriptor](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor)** 对象。 这样可以同时将多个流编码到单个文件中。 在此操作中编码的视频流必须包含在当前设备上指定了一组可同时使用的摄像机的单个 **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)**。 

有关如何结合使用 **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** 和 **[MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader)** 类来启用多个摄像机的实时计算机视觉方案的信息，请参阅[使用 MediaFrameReader 处理媒体帧](process-media-frames-with-mediaframereader.md)。

本文的其余部分将介绍使用多个视频轨将来自两台彩色摄像机的视频录制到单个文件的步骤。

## <a name="find-available-sensor-groups"></a>查找可用的传感器组
一个 **MediaFrameSourceGroup** 表示一系列可同时访问的帧源（通常是摄像机）。 每个设备的可用帧源组各不相同，因此本示例中的第一步是获取可用帧源组的列表，并查找包含应用场景所需摄像机的帧源组，在本例中需要两个彩色摄像机。

**[MediaFrameSourceGroup.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup#Windows_Media_Capture_Frames_MediaFrameSourceGroup_FindAllAsync)** 方法返回当前设备上的所有可用源组。 返回的每个 **MediaFrameSourceGroup** 都具有一系列 **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** 对象，这些对象描述了组中的每一个帧源。 Linq 查询用于查找包含两个彩色摄像机的源组，一个在前面板，一个在背面。 对于每个彩色摄像机都将返回一个包含所选 **MediaFrameSourceGroup** 和 **MediaFrameSourceInfo** 的匿名对象。 不使用 Linq 语法，也可以循环查找每个组，然后循环查找每个 **MediaFrameSourceInfo**，找到满足要求的组。

请注意，不是每个设备都包含具有两个彩色相机的源组，因此在尝试捕获视频之前，应检查以确保找到这样的源组。

[!code-cs[MultiRecordFindSensorGroups](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordFindSensorGroups)]

## <a name="initialize-the-mediacapture-object"></a>初始化 MediaCapture 对象
**[MediaCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)** 类是在 UWP 应用中用于大多数音频、视频和照片捕获操作的主类。 通过调用 **[InitializeAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture#Windows_Media_Capture_MediaCapture_InitializeAsync)**、传入包含初始化参数对象的 **[MediaCaptureInitializationSettings](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings)** 对象来初始化对象。 在本示例中，唯一指定的设置是 **[SourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings#Windows_Media_Capture_MediaCaptureInitializationSettings_SourceGroup)** 属性，它设置为在上面的示例代码中检索到的 **MediaFrameSourceGroup**。

有关 **MediaCapture** 和其他用于捕获媒体的 UWP 应用功能可以执行的其他操作的信息，请参阅[摄像机](camera.md)。

[!code-cs[MultiRecordInitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordInitMediaCapture)]

## <a name="create-a-mediaencodingprofile"></a>创建 MediaEncodingProfile
**[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** 类告诉媒体捕获管道在将捕获到的音频和视频写入文件时应如何对其进行编码。 对于典型的捕获和转换代码应用场景，此类提供一系列静态方法来创建常用配置文件，如 **[CreateAvi](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateAvi_Windows_Media_MediaProperties_VideoEncodingQuality_)** 和 **[CreateMp3](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateMp3_Windows_Media_MediaProperties_AudioEncodingQuality_)**。 对于此例，编码配置文件可使用 Mpeg4 容器和 H264 视频编码手动创建。 视频编码设置使用 **[VideoEncodingProperties](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.videoencodingproperties)** 对象指定。 对于此应用场景中使用的每个彩色摄像机，都会配置一个 **VideoStreamDescriptor** 对象。 该描述符使用指定编码的 **VideoEncodingProperties** 对象来构造。 **VideoStreamDescriptor** 的 **[Label](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor#Windows_Media_Core_VideoStreamDescriptor_Label)** 属性必须设置为将捕获到流中的媒体帧源的 ID。 这样，捕获管道就能知道对于每个摄像机应该使用的流描述符和编码属性。 帧源的 ID 由在上一节中已选择 **MediaFrameSourceGroup** 的情况下找到的 **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** 对象公布。


从 RS3 开始，可以通过调用 **[SetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_SetVideoTracks_Windows_Foundation_Collections_IIterable_Windows_Media_Core_VideoStreamDescriptor__)** 对一个 **MediaEncodingProfile** 设置多个编码属性。 可以通过调用 **[GetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_GetVideoTracks)** 获取视频流描述符列表。 请注意，如果设置存储单个流描述符的 **[Video](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_Video)** 属性，则通过调用 **SetVideoTracks** 设置的描述符列表将替换为包含你指定的单个描述符的列表。


[!code-cs[MultiRecordMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordMediaEncodingProfile)]

## <a name="record-using-the-multi-stream-mediaencodingprofile"></a>使用多个流 MediaEncodingProfile 录制
本示例的最后一步是通过调用 **[StartRecordToStorageFileAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture#Windows_Media_Capture_MediaCapture_StartRecordToStorageFileAsync_Windows_Media_MediaProperties_MediaEncodingProfile_Windows_Storage_IStorageFile_)** 并传入要将所捕获媒体写入其中的 **StorageFile** 以及在前面的示例代码中创建的 **MediaEncodingProfile**，来启动视频捕获。 等待几秒钟后，录制将通过调用 **[StopRecordAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture#Windows_Media_Capture_MediaCapture_StopRecordAsync)** 停止。

[!code-cs[MultiRecordToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordToFile)]

操作完成时，应该已创建了一个视频文件，其中包含从每个摄像机捕获的视频（编码为文件中单独的流）。 有关播放包含多个视频轨的媒体文件的信息，请参阅[媒体项、播放列表和轨](media-playback-with-mediasource.md)。

## <a name="related-topics"></a>相关主题

* [相机](camera.md)
* [使用 MediaCapture 进行照片、视频和音频的基本捕获](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [使用 MediaFrameReader 处理媒体帧](process-media-frames-with-mediaframereader.md)
* [媒体项、播放列表和轨](media-playback-with-mediasource.md)


 

 





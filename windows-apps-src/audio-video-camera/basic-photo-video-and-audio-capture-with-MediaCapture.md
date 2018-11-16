---
author: drewbatgit
ms.assetid: 66d0c3dc-81f6-4d9a-904b-281f8a334dd0
description: 本文介绍使用 MediaCapture 类捕获照片和视频的最简单方法。
title: 使用 MediaCapture 捕获基本的照片、视频和音频
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7204c01cc80c50dacb2009b2138a7c046974dc62
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6674638"
---
# <a name="basic-photo-video-and-audio-capture-with-mediacapture"></a>使用 MediaCapture 捕获基本的照片、视频和音频


本文介绍使用 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) 类捕获照片和视频的最简单方法。 **MediaCapture** 类公布了一组强大的 API，可提供捕获管道的低级别控制和启用高级捕获方案，但本文旨在帮助你将基本的媒体捕获快速且轻松地添加到应用。 若要了解有关 **MediaCapture** 提供的功能的详细信息，请参阅[**相机**](camera.md)。

如果你仅希望捕获照片或视频，不想添加任何其他媒体捕获功能，或者如果你不想创建自己的相机 UI，则可能希望使用 [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CameraCaptureUI) 类，它允许你仅启动 Windows 内置相机应用，并且接收捕获的照片或视频文件。 有关详细信息，请参阅[**使用 Windows 内置相机 UI 捕获照片和视频**](capture-photos-and-video-with-cameracaptureui.md)

本文中的代码源自 [**Camera starter kit**](https://go.microsoft.com/fwlink/?linkid=619479) 示例。 你可以下载该示例以查看上下文中使用的代码，或将该示例用作你自己的应用的起始点。

## <a name="add-capability-declarations-to-the-app-manifest"></a>向应用清单中添加功能声明

为了让你的应用可以访问设备的相机，必须声明你的应用要使用 *webcam* 和 *microphone* 设备功能。 如果你想要将已捕获的照片和视频保存到用户的图片库或视频库，还必须声明 *picturesLibrary* 和 *videosLibrary* 功能。

**将功能添加到应用部件清单**

1.  在 Microsoft Visual Studio 的**解决方案资源管理器**中，通过双击 **package.appxmanifest** 项，打开应用程序清单的设计器。
2.  选择**功能**选项卡。
3.  选中**摄像头**框和**麦克风**框。
4.  若要访问图片库和视频库，请选中**图片库**框和**视频库**框。


## <a name="initialize-the-mediacapture-object"></a>初始化 MediaCapture 对象
本文介绍的所有捕获方法都需要通过依次调用构造函数和 [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.InitializeAsync)，首先初始化 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) 对象。 由于在应用中可从多个位置访问 **MediaCapture** 对象，所以请声明某个类变量以保留该对象。  如果捕获操作失败，请为待通知的 **MediaCapture** 对象的 [**Failed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.Failed) 事件实现处理程序。

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

[!code-cs[InitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitMediaCapture)]

## <a name="set-up-the-camera-preview"></a>设置相机预览
使用 **MediaCapture** 无需显示相机预览即可捕获照片、视频和音频，但通常需要显示预览流，以便用户可以看到捕获的内容。 此外，一些 **MediaCapture** 功能在可以启用前需要运行预览流，这些功能包括自动对焦、自动曝光以及自动白平衡。 若要了解如何设置相机预览，请参阅[**显示相机预览**](simple-camera-preview-access.md)。

## <a name="capture-a-photo-to-a-softwarebitmap"></a>将照片捕获到 SoftwareBitmap
Windows 10 引入了 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap) 类，可跨多项功能提供图像的通用表示形式。 如果你想捕获照片并立即在应用中使用捕获的图像（例如在 XAML 中显示它，而非捕获到某个文件），则应捕获到 **SoftwareBitmap**。 你仍可选择在以后将图像保存到磁盘。

初始化 **MediaCapture** 对象后，可使用 [**LowLagPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture) 类将照片捕获到 **SoftwareBitmap**。 通过调用 [**PrepareLowLagPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagPhotoCaptureAsync)（该函数在指定所需图像格式的 [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties) 对象中传递），获取此类的实例。 [**CreateUncompressed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties.CreateUncompressed) 使用指定的像素格式创建未压缩的编码。 通过调用返回 [**CapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedPhoto) 对象的 [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture.CaptureAsync) 捕获照片。 通过依次访问 [**Frame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedPhoto.Frame) 属性和 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame.SoftwareBitmap) 属性，获取 **SoftwareBitmap**。

如果需要，可重复调用 **CaptureAsync** 来捕获多张照片。 在完成捕获后，调用 [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.FinishAsync) 以关闭 **LowLagPhotoCapture** 会话，并释放关联的资源。 调用 **FinishAsync** 后，若要开始重新捕获照片，需要在调用 [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture.CaptureAsync) 之前重新调用 [**PrepareLowLagPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagPhotoCaptureAsync) 以重新初始化捕获会话。

[!code-cs[CaptureToSoftwareBitmap](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToSoftwareBitmap)]

自 Windows 版本 1803 起，可访问从 **CaptureAsync** 返回的 **CapturedFrame** 类的 [**BitmapProperties**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.bitmapproperties) 属性，以检索与捕获的照片相关的元数据。 可将此数据传入 **BitmapEncoder**，进而将元数据保存到某个文件。 之前无法访问未压缩的图像格式的此数据。 还可访问 [**ControlValues**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.controlvalues) 属性来检索描述捕获帧的控件值（如曝光度和白平衡）的 [**CapturedFrameControlValues**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframecontrolvalues) 对象。

若要了解如何使用 **BitmapEncoder** 和 **SoftwareBitmap** 对象（包括如何在 XAML 页面显示此类对象），请参阅[**创建、编辑和保存位图图像**](imaging.md)。 

若要详细了解如何设置捕获设备控件值，请参阅[用于照片和视频的捕获设备控件](capture-device-controls-for-photo-and-video-capture.md)。

自 Windows 10 版本 1803 起，可通过访问 **MediaCapture** 返回的 **CapturedFrame** 的 [**BitmapProperties**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.bitmapproperties) 属性，获取以未压缩格式捕获的照片的元数据，如 EXIF 信息。 在以前的版本中，仅能在以压缩文件格式捕获的照片的标题中访问此数据。 手动写入图像文件时，可向 [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder) 提供此数据。 有关位图编码的详细信息，请参阅[创建、编辑和保存位图图像](imaging.md)。  还可通过访问 [**ControlValues**](https://docs.microsoft.com/en-us/uwp/api/windows.media.capture.capturedframe.controlvalues) 属性访问捕获图像时使用的帧控件值，如曝光度和闪光设置。 有关详细信息，请参阅[用于照片和视频捕获的捕获设备控件](capture-device-controls-for-photo-and-video-capture.md)。

## <a name="capture-a-photo-to-a-file"></a>将照片捕获到文件
典型的摄影应用会将捕获的照片保存到磁盘或云存储，并且需要将元数据（例如照片方向）添加到文件。 以下示例显示如何将照片捕获到文件。 你仍可选择在以后从图像文件中创建 **SoftwareBitmap**。 

此示例中显示的技术将照片捕获到内存流，然后从该内存流将照片转码到磁盘上的某个文件。 此示例使用 [**GetLibraryAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageLibrary.GetLibraryAsync) 获取用户的图片库，然后使用 [**SaveFolder**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageLibrary.SaveFolder) 属性获取参考默认保存文件夹。 请记住，将**图片库**功能添加到应用部件清单才能访问此文件夹。 [**CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFolder.CreateFileAsync) 创建保存照片的新 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFile)。

创建 [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.Streams.InMemoryRandomAccessStream)，然后调用 [**CapturePhotoToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.CapturePhotoToStreamAsync) 以将照片捕获到流，该照片在流和指定应使用的图像格式的 [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties) 对象中传递。 可通过自行初始化对象来创建自定义编码属性，但类提供用于常见编码格式的静态方法，例如 [**ImageEncodingProperties.CreateJpeg**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties.CreateJpeg)。 接下来，通过调用 [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFile.OpenAsync) 创建输出文件的文件流。 创建 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder) 以解码内存流中的图像，然后创建 [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder) 以通过调用 [**CreateForTranscodingAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.CreateForTranscodingAsync) 将图像编码到文件。

可选择创建 [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapPropertySet) 对象，然后在图像编码器上调用 [**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252.aspx)，以将照片相关元数据包含在图像文件中。 有关编码属性的详细信息，请参阅[**图像元数据**](image-metadata.md)。 大多数摄影应用需要合理处理设备方向。 有关详细信息，请参阅[**使用 MediaCapture 处理设备方向**](handle-device-orientation-with-mediacapture.md)。

最后，在编码器对象上调用 [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.FlushAsync) 以将照片从内存流转码到文件。

[!code-cs[CaptureToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToFile)]

若要详细了解如何使用文件和文件夹，请参阅[**文件、文件夹和库**](https://msdn.microsoft.com/windows/uwp/files/index)。

## <a name="capture-a-video"></a>捕获视频
通过使用 [**LowLagMediaRecording**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording) 类将视频捕获快速添加到应用。 首先，声明对象的类变量。

[!code-cs[LowLagMediaRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetLowLagMediaRecording)]

接下来，创建将保存视频的 **StorageFile** 对象。 请注意，若要保存到用户的视频库（如本示例所示），必须将**视频库**功能添加到应用部件清单。 调用 [**PrepareLowLagRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagRecordToStorageFileAsync) 以初始化媒体录制，该媒体录制在存储文件和指定视频编码的 [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile) 对象中传递。 该类提供用于创建常见视频编码配置文件的静态方法，例如 [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile.CreateMp4)。

最后，调用 [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StartAsync) 以开始捕获视频。

[!code-cs[StartVideoCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartVideoCapture)]

若要停止录制视频，请调用 [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopAsync)。

[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

可继续调用 **StartAsync** 和 **StopAsync** 以捕获其他视频。 完成视频捕获后，调用 [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.FinishAsync) 以释放捕获会话并清理关联的资源。 完成此调用后，必须重新调用 **PrepareLowLagRecordToStorageFileAsync** 以重新初始化捕获会话，然后才可 **StartAsync**。

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]

在捕获视频时，应为 **MediaCapture** 对象的 [**RecordLimitationExceeded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.RecordLimitationExceeded) 事件注册处理程序，如果超过单次录制的时限（当前为三小时），操作系统将引发该事件。 在事件的处理程序中，应通过调用 [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopAsync) 完成录制。

[!code-cs[RecordLimitationExceeded](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceeded)]

[!code-cs[RecordLimitationExceededHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceededHandler)]

### <a name="play-and-edit-captured-video-files"></a>播放和编辑捕获的视频文件
将视频捕获到文件后，你可能想要在应用的 UI 中加载并播放该文件。 可使用 **[MediaPlayerElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)** XAML 控件和关联的 **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** 执行此操作。 若要了解如何在 XAML 页面播放媒体，请参阅[使用 MediaPlayer 播放音频和视频](play-audio-and-video-with-mediaplayer.md)。

还可通过调用 **[CreateFromFileAsync](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromfileasync)** 从视频文件创建 **[MediaClip](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip)** 对象。  **[MediaComposition](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition)** 提供基本的视频编辑功能，如排列 **MediaClip** 对象的序列、剪裁视频长度、创建层、添加背景音乐和应用视频效果。 若要详细了解如何使用媒体合成功能，请参阅[媒体合成和编辑](media-compositions-and-editing.md)。

## <a name="pause-and-resume-video-recording"></a>暂停和恢复视频录制
通过依次调用 [**PauseAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.PauseAsync) 和 [**ResumeAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.ResumeAsync)，可暂停并恢复视频录制，无需创建单独的输出文件。

[!code-cs[PauseRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseRecordingSimple)]

[!code-cs[ResumeRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeRecordingSimple)]

从 Windows 10 版本 1607 开始，可暂停视频录制，并接收暂停录制前最后捕获的帧。 然后可在相机预览上覆盖此帧，以允许用户在恢复录制前将相机与暂停的帧保持一致。 调用 [**PauseWithResultAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.PauseWithResultAsync) 将返回 [**MediaCapturePauseResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapturePauseResult) 对象。 [**LastFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapturePauseResult.LastFrame) 属性是表示最后一帧的 [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.VideoFrame) 对象。 若要在 XAML 中显示帧，请获取视频帧的 **SoftwareBitmap** 表示形式。 目前仅支持预乘或空 alpha 通道且格式为 BGRA8 的图像，因此如果需要获取正确的格式，请调用 [**Convert**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap.Covert)。  创建新 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) 对象，并调用 [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource.SetBitmapAsync) 以对其进行初始化。 最后，设置 XAML [**Image**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Image) 控件的 **Source** 属性以显示该图像。 若要使此技巧有效，图像必须与 **CaptureElement** 控件保持一致，并且不透明度值应小于 1。 请记住，仅可在 UI 线程上修改 UI，因为请在 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher.RunAsync) 内部进行此次调用。

**PauseWithResultAsync** 也会返回在上一段中录制的视频持续时间，以防你需要跟踪录制的总时长。

[!code-cs[PauseCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseCaptureWithResult)]

在恢复录制时，可将图像源设置为 NULL 以将其隐藏。

[!code-cs[ResumeCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeCaptureWithResult)]

请注意，在通过调用 [**StopWithResultAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopWithResultAsync) 停止视频时还可获得结果帧。


## <a name="capture-audio"></a>捕获音频 
使用上述用于捕获视频的相同技术，可将音频捕获快速添加到应用。 以下示例在应用程序数据文件夹中创建 **StorageFile**。 调用 [**PrepareLowLagRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagRecordToStorageFileAsync) 以初始化捕获会话，该会话在文件和 [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile)（本例中由 [**CreateMp3**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile.CreateMp3) 静态方法生成）中传递。 若要开始录制，请调用 [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StartAsync)。

[!code-cs[StartAudioCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartAudioCapture)]


若要停止音频录制，请调用 [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoSequenceCapture.StopAsync)。

## <a name="related-topics"></a>相关主题

* [相机](camera.md)
[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

可多次调用 **StartAsync** 和 **StopAsync** 以录制多个音频文件。 完成音频捕获后，调用 [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.FinishAsync) 以释放捕获会话并清理关联的资源。 完成此调用后，必须重新调用 **PrepareLowLagRecordToStorageFileAsync** 以重新初始化捕获会话，然后才可 **StartAsync**。

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]


## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>检测系统的音频级别更改并做出响应
自 Windows 10 版本 1803 起，应用可检测到系统何时降低其音频捕获和音频渲染流的音频级别或何时将其静音。 例如，应用进入后台状态时，系统可能将应用的流设为静音。 借助 [**AudioStateMonitor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor)，你可以注册以接收系统修改音频流的音量时出现的事件。 通过调用 [**CreateForCaptureMonitoring**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.createforcapturemonitoring#Windows_Media_Audio_AudioStateMonitor_CreateForCaptureMonitoring) 获取用于监视音频捕获流的 **AudioStateMonitor** 实例。 通过调用 [**CreateForRenderMonitoring**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.createforrendermonitoring) 获取用于监视音频渲染流的实例。 针对系统更改相应流类别的音频时要通知的每个监视器的 [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) 事件，注册一个处理程序。

[!code-cs[AudioStateMonitorUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAudioStateMonitorUsing)]

[!code-cs[AudioStateVars](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

[!code-cs[RegisterAudioStateMonitor](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterAudioStateMonitor)]

在捕获流的 **SoundLevelChanged** 处理程序中，可查看 **AudioStateMonitor** 发件人的 [**SoundLevel**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) 属性来确定新的音量。 请注意：系统决不可让捕获流的音量降低或忽高忽低。 系统仅可将其静音或切换回最大音量。 如果音频流已静音，你可以停止正在进行的捕获。 音频流还原到最大音量后，可再次启动捕获。 以下示例使用一些布尔类变量来跟踪应用当前是否正在捕获音频以及捕获是否因音频状态而停止。 这些变量用于确定何时适合以编程方式停止或启动音频捕获。

[!code-cs[CaptureSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureSoundLevelChanged)]

以下代码示例演示了如何将 **SoundLevelChanged** 处理程序用于音频渲染。 根据应用方案以及正在播放的内容类型，你可能想要在音量忽高忽低时暂停音频播放。 若要详细了解如何处理媒体播放的音量更改，请参阅[使用 MediaPlayer 播放音频和视频](play-audio-and-video-with-mediaplayer.md)。

[!code-cs[RenderSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRenderSoundLevelChanged)]


* [使用 Windows 内置相机 UI 捕获照片和视频](capture-photos-and-video-with-cameracaptureui.md)
* [使用 MediaCapture 处理设备方向](handle-device-orientation-with-mediacapture.md)
* [创建、编辑和保存位图图像](imaging.md)
* [文件、文件夹和库](https://msdn.microsoft.com/windows/uwp/files/index)


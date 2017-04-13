---
author: drewbatgit
ms.assetid: 0186EA01-8446-45BA-A109-C5EB4B80F368
description: "本文介绍了如何使用 AdvancedPhotoCapture 类捕获高动态范围 (HDR) 和低亮度照片。"
title: "高动态范围 (HDR) 和低亮度照片捕获"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 32badfb6cc2a069370623357ba96800cebd11c03
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="high-dynamic-range-hdr-and-low-light-photo-capture"></a>高动态范围 (HDR) 和低亮度照片捕获

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本文介绍了如何使用 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 类捕获高动态范围 (HDR) 照片。 此 API 还允许你在完成最终图像的处理之前从 HDR 捕获获取参考帧。

相关 HDR 捕获的其他文章包括：

-   你可以使用 [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902) 以允许系统评估媒体捕获预览流的内容，以便确定 HDR 处理是否会改善捕获结果。 有关详细信息，请参阅 [MediaCapture 的场景分析](scene-analysis-for-media-capture.md)。

-   使用 [**HdrVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926680) 通过 Windows 内置的 HDR 处理算法捕获视频。 有关详细信息，请参阅[用于视频捕获的捕获设备控件](capture-device-controls-for-video-capture.md)。

-   你可以使用 [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564) 捕获一系列照片（每张带有不同的捕获设置），并实现你自己的 HDR 或其他处理算法。 有关详细信息，请参阅[可变照片序列](variable-photo-sequence.md)。

从 Windows 10 版本 1607 开始，可以使用 **AdvancedPhotoCapture** 捕获利用内置算法来增强照片（在低亮度设置下捕获）质量的照片。

> [!NOTE] 
> 不支持使用 **AdvancedPhotoCapture** 同时录制视频和照片捕获。

> [!NOTE] 
> 如果 [**FlashControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.FlashControl.Enabled) 属性设置为 true，它将替代 **AdvancedPhotoCapture** 设置，并使用闪关灯捕获正常照片。 如果 [**Auto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.FlashControl.Auto) 设置为 true，将根据配置使用 **AdvancedPhotoCapture**，但不会使用闪光灯。

> [!NOTE] 
> 本文以[使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)中讨论的概念和代码为基础，详细介绍了实现基本照片和视频捕获的步骤。 我们建议你先熟悉该文中的基本媒体捕获模式，然后再转到更高级的捕获方案。 本文中的代码假设你的应用已有一个正确完成初始化的 MediaCapture 的实例。

有一个通用 Windows 示例展示了 **AdvancedPhotoCapture** 类的用法，你可以使用该类查看用在上下文中或用作自己应用起始点的 API。 有关详细信息，请参阅[相机高级捕获示例](http://go.microsoft.com/fwlink/?LinkID=620517)。

## <a name="advanced-photo-capture-namespaces"></a>高级照片捕获命名空间

本文中的代码示例除了在基本媒体捕获所需的命名空间中之外，还在以下命名空间中使用 API。

[!code-cs[HDRPhotoUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHDRPhotoUsing)]

## <a name="hdr-photo-capture"></a>HDR 照片捕获

### <a name="determine-if-hdr-photo-capture-is-supported-on-the-current-device"></a>确定 HDR 照片捕获在当前设备上是否受支持

本文中介绍的 HDR 捕获技术使用 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 对象进行执行。 并非所有设备都支持使用 **AdvancedPhotoCapture** 的 HDR 捕获。 确定该技术在当前运行你的应用的设备上是否受支持，方法是获取 **MediaCapture** 对象的 [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825)，然后获取 [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) 属性。 检查视频设备控制器的 [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844) 集合以查看它是否包含 [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845)。 如果包含，则支持使用 **AdvancedPhotoCapture** 的 HDR 捕获。

[!code-cs[HdrSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHdrSupported)]

### <a name="configure-and-prepare-the-advancedphotocapture-object"></a>配置和准备 AdvancedPhotoCapture 对象

因为你将需要从代码内的多个位置访问 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 实例，所以你应声明成员变量以保留对象。

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

在你的应用中，在初始化 **MediaCapture** 对象后，创建 [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837) 对象并将模式设置为 [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845)。 调用 [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) 对象的 [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841) 方法，传入创建的 **AdvancedPhotoCaptureSettings** 对象。

调用 **MediaCapture** 对象的 [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403)，传入用于指定该捕获应使用的编码类型的 [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) 对象。 **ImageEncodingProperties** 类提供用于创建受 **MediaCapture** 支持的图像编码的静态方法。

**PrepareAdvancedPhotoCaptureAsync** 返回将用于启动照片捕获的 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 对象。 你可以使用该对象来注册本文稍后将介绍的 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) 和 [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) 的处理程序。

[!code-cs[CreateAdvancedCaptureAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureAsync)]

### <a name="capture-an-hdr-photo"></a>捕获 HDR 照片

通过调用 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 对象的 [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) 方法，捕获 HDR 照片。 此方法将返回在其 [**Frame**](https://msdn.microsoft.com/library/windows/apps/mt181382) 属性中提供已捕获照片的 [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/mt181378) 对象。

[!code-cs[CaptureHdrPhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureHdrPhotoAsync)]

大多数摄影应用都需要将捕获照片的旋转编码为图像文件，这样便可以通过其他应用和设备正确显示该照片。 此示例介绍了如何使用帮助程序类 **CameraRotationHelper** 计算文件的正确方向。 [**使用 MediaCapture 处理设备方向**](handle-device-orientation-with-mediacapture.md)一文中完整地介绍和列出此类。

用于将图像保存到磁盘的 **SaveCapturedFrameAsync** 帮助程序方法将稍后在本文中论述。

### <a name="get-optional-reference-frame"></a>获取可选参考帧

HDR 进程捕获多个帧，然后在已捕获所有帧之后，将它们合成到单个图像中。 你可以在已捕获帧之后，但在完成整个 HDR 进程之前，通过处理 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) 事件来访问该帧。 如果你只对最终 HDR 照片结果感兴趣，你不需要执行此操作。

> [!IMPORTANT]
> 在支持硬件 HDR 的设备上不会引发 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392)，因此不会生成参考帧。 你的应用应对不会引发此事件的这种情况进行处理。

由于参考帧在调用 **CaptureAsync** 的上下文之外到达，因此提供了一个机制来将上下文信息传递到 **OptionalReferencePhotoCaptured** 处理程序。 首先，应调用一个对象，用来包含上下文信息。 此对象的名称和内容由你决定。 此示例定义一个对象，该对象具有可用于跟踪捕获的文件名和相机方向的成员。

[!code-cs[AdvancedCaptureContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAdvancedCaptureContext)]

创建上下文对象的新实例、填充它的成员，然后将其传递到接受对象作为参数的 [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) 的重载。

[!code-cs[CaptureWithContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureWithContext)]

在 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) 事件处理程序中，将 [**OptionalReferencePhotoCapturedEventArgs**](https://msdn.microsoft.com/library/windows/apps/mt181404) 对象的 [**Context**](https://msdn.microsoft.com/library/windows/apps/mt181405) 属性转换为上下文对象类。 此示例修改文件名以区分参考帧图像和最终的 HDR 图像，然后调用 **SaveCapturedFrameAsync** 帮助程序方法以保存该图像。

[!code-cs[OptionalReferencePhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOptionalReferencePhotoCaptured)]

### <a name="receive-a-notification-when-all-frames-have-been-captured"></a>当已捕获所有帧时将收到通知

HDR 照片捕获具有两个步骤。 首先，捕获多个帧，然后将帧处理为最终的 HDR 图像。 仍在捕获源 HDR 帧期间，无法启动另一个捕获，但在捕获所有帧之后，在完成 HDR 后期处理之前，你可以启动一个捕获。 当完成 HDR 捕获时将引发 [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) 事件，通知你可以启动另一个捕获。 典型方案是在 HDR 捕获开始时禁用你的 UI 的捕获按钮，然后在引发 **AllPhotosCaptured** 时重新启用它。

[!code-cs[AllPhotosCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAllPhotosCaptured)]

### <a name="clean-up-the-advancedphotocapture-object"></a>清理 AdvancedPhotoCapture 对象

当你的应用完成捕获时，在释放 **MediaCapture** 对象之前，你应通过调用 [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/mt181391) 并将你的成员变量设置为 Null 来关闭 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 对象。

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]


## <a name="low-light-photo-capture"></a>低亮度照片捕获
当你使用 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture) 类的低亮度功能时，系统将对当前场景进行评估，并根据需要应用一种算法进行低光补偿。 如果系统确定不需要应用该算法，则执行常规捕获。

在使用低亮度照片捕获功能之前，应确定该技术在当前运行你的应用的设备上是否受支持，方法是获取 **MediaCapture** 对象的 [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825)，然后获取 [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) 属性。 检查视频设备控制器的 [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844) 集合以查看它是否包含 [**AdvancedPhotoMode.LowLight**](https://msdn.microsoft.com/library/windows/apps/mt147845)。 如果包含，则支持使用 **AdvancedPhotoCapture** 的低亮度捕获。 
[!code-cs[LowLightSupported1](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetLowLightSupported1)]

[!code-cs[LowLightSupported2](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetLowLightSupported2)]

接下来，声明一个成员变量，用来存储 **AdvancedPhotoCapture** 对象。 

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

在你的应用中，在初始化 **MediaCapture** 对象后，创建 [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837) 对象并将模式设置为 [**AdvancedPhotoMode.LowLight**](https://msdn.microsoft.com/library/windows/apps/mt147845)。 调用 [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) 对象的 [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841) 方法，传入创建的 **AdvancedPhotoCaptureSettings** 对象。

调用 **MediaCapture** 对象的 [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403)，传入用于指定该捕获应使用的编码类型的 [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) 对象。 

[!code-cs[CreateAdvancedCaptureLowLightAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureLowLightAsync)]

若要捕获照片，请调用 [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.CaptureAsync)。

[!code-cs[CaptureLowLight](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureLowLight)]

与上面的 HDR 示例类似，此示例使用名为 **CameraRotationHelper** 的帮助程序类来确定应编码到图像中的旋转值，以便可以通过其他应用和设备正确显示该图像。 [**使用 MediaCapture 处理设备方向**](handle-device-orientation-with-mediacapture.md)一文中完整地介绍和列出此类。

用于将图像保存到磁盘的 **SaveCapturedFrameAsync** 帮助程序方法将稍后在本文中论述。

你可以捕获多张低亮度照片，无需重新配置 **AdvancedPhotoCapture** 对象，但是当你完成捕获时，应调用 [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.FinishAsync) 清除该对象和相关联的资源。

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]

## <a name="working-with-advancedcapturedphoto-objects"></a>使用 AdvancedCapturedPhoto 对象
[**AdvancedPhotoCapture.CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.CaptureAsync) 可返回一个 [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedCapturedPhoto) 对象，用来表示已捕获的照片。 此对象将公开 [**Frame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedCapturedPhoto.Frame) 属性，这可返回一个 [**CapturedFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame) 对象，用来表示该图像。 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.OptionalReferencePhotoCaptured) 事件还将在它的事件参数中提供一个 **CapturedFrame** 对象。 获取此类型的对象后，你可以对它执行各种操作，包括创建 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap) 或将该图像保存到文件。 

## <a name="get-a-softwarebitmap-from-a-capturedframe"></a>从 CapturedFrame 获取 SoftwareBitmap
从 **CapturedFrame** 对象获取 **SoftwareBitmap** 轻而易举，只需访问该对象的 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame.SoftwareBitmap) 属性即可。 不过，大多数编码格式都不会因 **AdvancedPhotoCapture** 而支持 **SoftwareBitmap**，因此在使用之前，应检查并确保该属性不为 null。

[!code-cs[SoftwareBitmapFromCapturedFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmapFromCapturedFrame)]

在当前版本中，因 **AdvancedPhotoCapture** 而支持 **SoftwareBitmap** 的唯一编码格式是未压缩的 NV12。 因此，如果你想要使用此功能，必须在调用 [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403) 时指定该编码格式。 

[!code-cs[UncompressedNv12](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUncompressedNv12)]

当然，你可以始终将图像保存到某个文件，然后在单独的步骤中将该文件加载到 **SoftwareBitmap** 中。 有关使用 **SoftwareBitmap** 的详细信息，请参阅[**创建、编辑和保存位图图像**](imaging.md)。

## <a name="save-a-capturedframe-to-a-file"></a>将 CapturedFrame 保存到文件
[**CapturedFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame) 类可实现 IInputStream 接口，因此可以将它用作对 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder) 的输入，然后可以使用 [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder) 将图像数据写入磁盘。

在以下示例中，在用户的图片库中创建一个新文件夹，并且在此文件夹中创建一个文件。 请注意，你的应用需要包含应用部件清单文件中的**图片库**功能才能访问此目录。 然后针对指定文件打开文件流。 接下来，调用 [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder.CreateAsync) 从 **CapturedFrame** 创建解码器。 然后，[**CreateForTranscodingAsync**](https://msdn.microsoft.com/library/windows/apps/br226214) 即可通过该文件流和解码器创建编码器。

后续步骤使用该编码器的 [**BitmapProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.BitmapProperties)，将照片的方向编码到图像文件中。 有关在捕获图像时处理方向的详细信息，请参阅[**使用 MediaCapture 处理设备方向**](handle-device-orientation-with-mediacapture.md)。

最后，通过调用 [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.FlushAsync) 将图像写入到文件。

[!code-cs[SaveCapturedFrameAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSaveCapturedFrameAsync)]

## <a name="related-topics"></a>相关主题

* [相机](camera.md)
* [使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)

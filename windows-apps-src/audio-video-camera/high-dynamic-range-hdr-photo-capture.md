---
author: drewbatgit
ms.assetid: 0186EA01-8446-45BA-A109-C5EB4B80F368
description: "AdvancedPhotoCapture 类允许你捕获高动态范围 (HDR) 照片。"
title: "高动态范围 (HDR) 照片捕获"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 3015aa4338ddb0c0a006eb631026261a4453f376

---

# 高动态范围 (HDR) 照片捕获

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


[
            **AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 类允许你捕获高动态范围 (HDR) 照片。 此 API 还允许你在完成最终图像的处理之前从 HDR 捕获获取参考帧。

相关 HDR 捕获的其他文章包括：

-   你可以使用 [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902) 以允许系统评估媒体捕获预览流的内容，以便确定 HDR 处理是否会改善捕获结果。 有关详细信息，请参阅[媒体捕获的场景分析](scene-analysis-for-media-capture.md)。

-   使用 [**HdrVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926680) 通过 Windows 内置的 HDR 处理算法捕获视频。 有关详细信息，请参阅[用于视频捕获的捕获设备控件](capture-device-controls-for-video-capture.md)。

-   你可以使用 [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564) 捕获一系列照片（每张带有不同的捕获设置），并实现你自己的 HDR 或其他处理算法。 有关详细信息，请参阅[可变照片序列](variable-photo-sequence.md)。

**注意**
-   不支持使用 **AdvancedPhotoCapture** 同时录制视频和照片捕获。

-   不支持在高级照片捕获期间使用相机闪光灯。

**注意** 本文基于[使用 MediaCapture 捕获照片和视频](capture-photos-and-video-with-mediacapture.md)中讨论的概念和代码，介绍了实现基本照片和视频捕获的步骤。 建议你先熟悉该文中的基本媒体捕获模式，然后再转到更高级的捕获方案。 本文中的代码假设你的应用已有一个正确完成初始化的 MediaCapture 的实例。

## HDR 照片捕获命名空间

若要使用 HDR 照片捕获，则除了基本媒体捕获所需的命名空间外，你的应用还必须包括以下命名空间。

[!code-cs[HDRPhotoUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHDRPhotoUsing)]


## 确定 HDR 照片捕获在当前设备上是否受支持

本文中介绍的 HDR 捕获技术使用 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 对象进行执行。 并非所有设备都支持使用 **AdvancedPhotoCapture** 的 HDR 捕获。 确定该技术在当前运行你的应用的设备上是否受支持，方法是获取 **MediaCapture** 对象的 [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825)，然后获取 [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) 属性。 检查视频设备控制器的 [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844) 集合以查看它是否包含 [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845)。 如果包含，则支持使用 **AdvancedPhotoCapture** 的 HDR 捕获。

[!code-cs[HdrSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHdrSupported)]

## 配置和准备 AdvancedPhotoCapture 对象

因为你将需要从代码内的多个位置访问 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 实例，所以你应声明成员变量以保留对象。

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

在你的应用中，在初始化 **MediaCapture** 对象后，创建 [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837) 对象并将模式设置为 [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845)。 调用 [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) 对象的 [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841) 方法，传入创建的 **AdvancedPhotoCaptureSettings** 对象。

调用 **MediaCapture** 对象的 [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403)，传入用于指定该捕获应使用的编码类型的 [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) 对象。 **ImageEncodingProperties** 类提供用于创建受 **MediaCapture** 支持的图像编码的静态方法。

**PrepareAdvancedPhotoCaptureAsync** 返回将用于启动照片捕获的 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 对象。 你可以使用该对象来注册本文稍后将介绍的 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) 和 [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) 的处理程序。

[!code-cs[CreateAdvancedCaptureAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureAsync)]

## 捕获 HDR 照片

通过调用 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 对象的 [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) 方法，捕获 HDR 照片。 此方法将返回在其 [**Frame**](https://msdn.microsoft.com/library/windows/apps/mt181382) 属性中提供已捕获照片的 [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/mt181378) 对象。

[!code-cs[CaptureHdrPhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureHdrPhotoAsync)]

**ConvertOrientationToPhotoOrientation** 和 **ReencodeAndSavePhotoAsync** 是作为本文[使用 MediaCapture 捕获照片和视频](capture-photos-and-video-with-mediacapture.md)中的基本媒体捕获方案的一部分介绍的帮助程序方法。

## 获取可选参考帧

HDR 进程捕获多个帧，然后在已捕获所有帧之后，将它们合成到单个图像中。 你可以在已捕获帧之后，但在完成整个 HDR 进程之前，通过处理 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) 事件来访问该帧。 如果你只对最终 HDR 照片结果感兴趣，则无需执行此操作。

**重要提示** 在支持硬件 HDR 的设备上不会引发 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392)，因此不会生成参考帧。 你的应用应对不会引发此事件的这种情况进行处理。

由于参考帧在调用 **CaptureAsync** 的上下文之外到达，因此提供了一个机制来将上下文信息传递到 **OptionalReferencePhotoCaptured** 处理程序。 首先你应创建一个将包含上下文信息的对象。 此对象的名称和内容由你决定。 此示例定义一个对象，该对象具有可用于跟踪捕获的文件名和相机方向的成员。

[!code-cs[AdvancedCaptureContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAdvancedCaptureContext)]

创建上下文对象的新实例、填充它的成员，然后将其传递到接受对象作为参数的 [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) 的重载。

[!code-cs[CaptureWithContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureWithContext)]

在 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) 事件处理程序中，将 [**OptionalReferencePhotoCapturedEventArgs**](https://msdn.microsoft.com/library/windows/apps/mt181404) 对象的 [**Context**](https://msdn.microsoft.com/library/windows/apps/mt181405) 属性转换为上下文对象类。 此示例修改文件名以区分参考帧图像和最终的 HDR 图像，然后调用 **ReencodeAndSavePhotoAsync** 帮助程序方法以保存该图像。

[!code-cs[OptionalReferencePhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOptionalReferencePhotoCaptured)]

## 当已捕获所有帧时将收到通知

HDR 照片捕获具有两个步骤。 首先，捕获多个帧，然后将帧处理为最终的 HDR 图像。 仍在捕获源 HDR 帧期间，无法启动另一个捕获，但在捕获所有帧之后，在完成 HDR 后期处理之前，你可以启动一个捕获。 当完成 HDR 捕获时将引发 [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) 事件，通知你可以启动另一个捕获。 典型方案是在 HDR 捕获开始时禁用你的 UI 的捕获按钮，然后在引发 **AllPhotosCaptured** 时重新启用它。

[!code-cs[AllPhotosCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAllPhotosCaptured)]

## 清理 AdvancedPhotoCapture 对象

当你的应用完成捕获时，在释放 **MediaCapture** 对象之前，你应通过调用 [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/mt181391) 并将你的成员变量设置为 Null 来关闭 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 对象。

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]

## 相关主题

* [使用 MediaCapture 捕获照片和视频](capture-photos-and-video-with-mediacapture.md)



<!--HONumber=Jun16_HO4-->



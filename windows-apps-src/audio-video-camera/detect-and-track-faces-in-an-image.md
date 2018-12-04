---
ms.assetid: 84729E44-10E9-4D7D-8575-6A9D97467ECD
description: 本主题介绍如何使用 FaceDetector 检测图像中的人脸。 在视频帧的序列中，将随着时间的推移针对人脸跟踪优化 FaceTracker。
title: 检测图像或视频中的人脸
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 970fbcef984f28e779ea7c133de95f7f7f99be8d
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8469274"
---
# <a name="detect-faces-in-images-or-videos"></a>检测图像或视频中的人脸



\[与在商业发行之前可能会进行实质性修改的预发布产品相关的一些信息。 Microsoft 不对此处提供的信息作任何明示或默示的担保。\]

本主题介绍如何使用 [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) 检测图像中的人脸。 在视频帧的序列中，将随着时间的推移针对人脸跟踪优化 [**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150)。

有关使用 [**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/dn948776) 跟踪人脸的替代方法，请参阅[媒体捕获的场景分析](scene-analysis-for-media-capture.md)。

文本中的代码源自[基本人脸检测](http://go.microsoft.com/fwlink/p/?LinkId=620512&clcid=0x409)和[基本人脸跟踪](http://go.microsoft.com/fwlink/p/?LinkId=620513&clcid=0x409)示例。 你可以下载这些示例以查看上下文中使用的代码，或将该示例用作你自己的应用的起始点。

## <a name="detect-faces-in-a-single-image"></a>检测单张图像中的人脸

[**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) 类允许你检测静止图像中的一张或多张人脸。

此示例使用来自以下命名空间的 API。

[!code-cs[FaceDetectionUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceDetectionUsing)]

为 [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) 对象和将在该图像中检测到的 [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) 对象列表声明一个类成员变量。

[!code-cs[ClassVariables1](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables1)]

人脸检测在 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 对象上运行，可通过多种方法创建该对象。 在此示例中，[**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 用于允许用户选取要检测人脸的图像文件。 有关处理软件位图的详细信息，请参阅[图像处理](imaging.md)。

[!code-cs[Picker](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetPicker)]

使用 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) 类将图像文件解码为 **SoftwareBitmap**。 人脸检测过程在处理较小的图像时速度更快，因此你可能希望将源图像缩小为较小的大小。 可以在解码期间执行此操作，方法是创建一个 [**BitmapTransform**](https://msdn.microsoft.com/library/windows/apps/br226254) 对象、设置 [**ScaledWidth**](https://msdn.microsoft.com/library/windows/apps/br226261) 和 [**ScaledHeight**](https://msdn.microsoft.com/library/windows/apps/br226260) 属性，并将其传递到对 [**GetSoftwareBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn887332) 的调用中，后者返回解码和缩放的 **SoftwareBitmap**。

[!code-cs[Decode](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDecode)]

在当前版本中，**FaceDetector** 类仅支持 Gray8 或 Nv12 格式的图像。 **SoftwareBitmap** 类提供了 [**Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362) 方法，可将位图从一种格式转换为另一种格式。 此示例将源图像转换为 Gray8 像素格式（如果它尚未采用该格式）。 如果需要，可以使用 [**GetSupportedBitmapPixelFormats**](https://msdn.microsoft.com/library/windows/apps/dn974140) 和 [**IsBitmapPixelFormatSupported**](https://msdn.microsoft.com/library/windows/apps/dn974142) 方法在运行时确定某个像素格式是否受支持，以免在将来的版本中扩展受支持的格式集。

[!code-cs[Format](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFormat)]

通过依次调用 [**CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn974132) 和 [**DetectFacesAsync**](https://msdn.microsoft.com/library/windows/apps/dn974134) 来实例化 **FaceDetector**，从而传入已缩放到合理大小并转换为受支持的像素格式的位图。 此方法返回 [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) 对象的列表。 **ShowDetectedFaces** 是一个帮助程序方法（如下所示），用于在图像中的人脸周围绘制正方形。

[!code-cs[Detect](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDetect)]

确保处理在人脸检测过程中创建的对象。

[!code-cs[Dispose](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDispose)]

若要显示图像并在检测到的人脸周围绘制方框，请将 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 元素添加到 XAML 页面。

[!code-xml[Canvas](./code/FaceDetection_Win10/cs/MainPage.xaml#SnippetCanvas)]

定义一些成员变量以设置要绘制的正方形的样式。

[!code-cs[ClassVariables2](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables2)]

在 **ShowDetectedFaces** 帮助程序方法中，将创建新的 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101) 并将源设置为从表示源图像的 **SoftwareBitmap** 创建的 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854)。 XAML **Canvas** 控件的背景设置为图像画笔。

如果传递到帮助程序方法中的人脸列表不为空，循环访问列表中的每张人脸，并使用 [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) 类的 [**FaceBox**](https://msdn.microsoft.com/library/windows/apps/dn974126) 属性来确定图像内包含人脸的的矩形位置和大小。 由于 **Canvas** 控件很可能采用与源图像不同的大小，你应该将 X 和 Y 坐标以及 **FaceBox** 的宽度和高度乘以缩放值，该值是源图像大小与 **Canvas** 控件的实际大小之比。

[!code-cs[ShowDetectedFaces](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetShowDetectedFaces)]

## <a name="track-faces-in-a-sequence-of-frames"></a>在帧序列中跟踪人脸

如果你希望检测视频中的人脸，尽管实现步骤非常相似，但使用 [**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150) 类比 [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) 类更有效。 **FaceTracker** 使用有关以前处理的帧的信息来优化检测过程。

[!code-cs[FaceTrackingUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceTrackingUsing)]

声明 **FaceTracker** 对象的类变量。 此示例使用 [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/br230587) 以定义的间隔启动人脸跟踪。 [SemaphoreSlim](https://msdn.microsoft.com/library/system.threading.semaphoreslim.aspx) 用于确保一次只运行一个人脸跟踪操作。

[!code-cs[ClassVariables3](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables3)]

若要初始化人脸跟踪操作，请通过调用 [**CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn974151) 创建新的 **FaceTracker** 对象。 初始化所需的计时器间隔，然后创建计时器。 每次指定的间隔到期时，都将调用 **ProcessCurrentVideoFrame** 帮助程序方法。

[!code-cs[TrackingInit](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetTrackingInit)]

计时器将以异步方式调用 **ProcessCurrentVideoFrame** 帮助程序，因此该方法首先调用信号灯的 **Wait** 方法来查看跟踪操作是否正在进行；如果正在进行，该方法将在不尝试检测人脸的情况下返回。 在此方法结束时，将调用信号灯的 **Release** 方法，该方法允许继续执行对 **ProcessCurrentVideoFrame** 的后续调用。

[**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150) 类作用于 [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) 对象。 有多种获取 **VideoFrame** 的方法，包括从正在运行的 [MediaCapture](capture-photos-and-video-with-mediacapture.md) 对象捕获预览帧，或通过实现 [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788) 的 [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764784) 方法。 此示例使用未定义的帮助程序方法，该方法返回视频帧 **GetLatestFrame** 作为此操作的占位符。 有关从正在运行的媒体捕获设备的预览流获取视频帧的信息，请参阅[获取预览帧](get-a-preview-frame.md)。

和 **FaceDetector** 一样，**FaceTracker** 支持一组有限的像素格式。 如果所提供的帧未采用 Nv12 格式，则此示例将放弃人脸检测。

调用 [**ProcessNextFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn974157) 来检索表示该帧中的人脸的 [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) 对象列表。 获得人脸列表后，你可以通过上述用于人脸检测的相同方式显示它们。 请注意，由于未在 UI 线程上调用人脸跟踪帮助程序方法，因此你必须在调用 [**CoredDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 内进行任何 UI 更新。

[!code-cs[ProcessCurrentVideoFrame](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetProcessCurrentVideoFrame)]

## <a name="related-topics"></a>相关主题

* [媒体捕获的场景分析](scene-analysis-for-media-capture.md)
* [基本人脸检测示例](http://go.microsoft.com/fwlink/p/?LinkId=620512&clcid=0x409)
* [基本人脸跟踪示例](http://go.microsoft.com/fwlink/p/?LinkId=620513&clcid=0x409)
* [Camera](camera.md)
* [使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [媒体播放](media-playback.md)

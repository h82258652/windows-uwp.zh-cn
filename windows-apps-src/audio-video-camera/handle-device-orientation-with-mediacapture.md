---
author: drewbatgit
ms.assetid: af3941c0-3508-4ba2-a79e-fc71657c605f
description: 本文将向你展示在使用帮助程序类捕获照片和视频时，如何处理设备方向。
title: 使用 MediaCapture 处理设备方向
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1367c880bd6dde573ab4fc30733ed9d1fefa6b0b
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2018
ms.locfileid: "7153080"
---
# <a name="handle-device-orientation-with-mediacapture"></a>使用 MediaCapture 处理设备方向
当你的应用捕获要在应用之外查看的照片或视频时，如保存到用户设备上的某个文件中或在线共享，请务必使用正确的方向元数据对该图像进行编码，以便当其他应用或设备显示该图像时，方向正确。 确定要包含在媒体文件中的正确方向数据可能是项非常复杂的任务，因为有几个变量需要考虑，如设备底盘的方向、屏幕的方向以及相机在底盘上的位置（是前置相机还是后置相机）。 

若要简化处理方向的过程，我们建议使用帮助程序类 **CameraRotationHelper**，该帮助程序类的完整定义在本文末尾提供。 你可以将此类添加到项目中，然后按照本文中的步骤将方向支持添加到相机应用中。 借助该帮助程序类，还可使旋转相机 UI 中的控件更加简单，这样从用户的角度来看，这些控件显示正确。

> [!NOTE] 
> 本文以[**使用 MediaCapture 捕获基本的照片、视频和音频**](basic-photo-video-and-audio-capture-with-mediacapture.md)文章中介绍的代码和概念为基础。 我们建议，在将方向支持添加到应用之前，应熟悉使用 **MediaCapture** 类的基本概念。

## <a name="namespaces-used-in-this-article"></a>本文中使用的命名空间
本文中的示例代码将使用以下应包含在代码中的命名空间中的 API。 

[!code-cs[OrientationUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOrientationUsing)]

将方向支持添加到应用的第一步是锁定屏幕，以便当设备旋转时，屏幕不会自动跟着旋转。 自动 UI 旋转功能可良好适用于大多数类型的应用，但是当相机预览旋转时，该功能将使用户无法直观地进行查看。 锁定屏幕方向的方法是将 [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayInformation.AutoRotationPreferences) 属性设置为 [**DisplayOrientations.Landscape**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayOrientations)。 

[!code-cs[AutoRotationPreference](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAutoRotationPreference)]

## <a name="tracking-the-camera-device-location"></a>跟踪相机设备位置
若要计算捕获的媒体的正确方向，应用必须确定相机在底盘上的位置。 添加一个布尔成员变量来跟踪相机是否在设备外部，如 USB Web 摄像头。 添加另一个布尔变量来跟踪在前置相机已使用的情况下，是否应镜像预览。 此外，再添加一个变量，用来存储表示所选相机的 **DeviceInformation** 对象。

[!code-cs[CameraDeviceLocationBools](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCameraDeviceLocationBools)]
[!code-cs[DeclareCameraDevice](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareCameraDevice)]

## <a name="select-a-camera-device-and-initialize-the-mediacapture-object"></a>选择相机设备并初始化 MediaCapture 对象
文章[**使用 MediaCapture 捕获基本的照片、视频和音频**](basic-photo-video-and-audio-capture-with-mediacapture.md)将向你展示如何仅使用几行代码来初始化 **MediaCapture** 对象。 若要支持相机方向，我们需要向初始化过程添加额外几个步骤。

首先，调用 [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation.FindAllAsync)，传入设备选择器 [**DeviceClass.VideoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceClass)，以获取所有可用视频捕获设备的列表。 然后，在相机面板位置已知并且该位置与提供的值相匹配的列表中选择第一台设备，在本示例中，该设备为前置相机。 如果在期望的面板上未找到任何相机，将使用第一个或默认可用的相机。

如果找到相机设备，将创建一个新的 [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings) 对象，并且 [**VideoDeviceId**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.VideoDeviceId) 属性将设置为所选设备。 接下来，创建 MediaCapture 对象并调用 [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.InitializeAsync)，传入设置对象以告诉系统使用所选相机。

最后，查看所选设备面板是否为 null 或未知。 如果是，则相机在外部，意味着它的旋转与设备的旋转无关。 如果面板为已知，并且在设备底盘的前面，我们知道应该镜像预览，以便设置跟踪此信息的变量。

[!code-cs[InitMediaCaptureWithOrientation](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitMediaCaptureWithOrientation)]
## <a name="initialize-the-camerarotationhelper-class"></a>初始化 CameraRotationHelper 类

现在我们开始使用 **CameraRotationHelper** 类。 声明类成员变量以存储对象。 调用构造函数，传入所选相机的机箱位置。 帮助程序类使用此信息来计算捕获的媒体的正确方向、预览流和 UI。 为该帮助程序类的 **OrientationChanged** 注册一个处理程序，当我们需要更新 UI 或预览流的方向时，就会引发该事件。

[!code-cs[DeclareRotationHelper](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareRotationHelper)]

[!code-cs[InitRotationHelper](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitRotationHelper)]

## <a name="add-orientation-data-to-the-camera-preview-stream"></a>将方向数据添加到相机预览流
将正确方向添加到预览流的元数据不会影响预览向用户显示的方式，但这有助于系统编码从预览流正确捕获的任何帧。

通过调用 [**MediaCapture.StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.StartPreviewAsync) 启动相机预览。 在执行此操作之前，请检查成员变量，以查看是否应镜像预览（针对前置相机）。 如果应镜像，请将 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.CaptureElement) 的 [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.FlowDirection) 属性（在本示例中名为 *PreviewControl*）设置为 [**FlowDirection.RightToLeft**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FlowDirection)。 在启动预览之后，请调用帮助程序方法 **SetPreviewRotationAsync** 来设置预览旋转。 下面是此方法的实现。

[!code-cs[StartPreviewWithRotationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewWithRotationAsync)]

我们单独使用一种方法来设置预览旋转，以便它可以在手机方向发生改变时更新，而无需重新初始化预览流。 如果相机在设备外部，则不采取任何操作。 否则，将调用 **CameraRotationHelper** 方法 **GetCameraPreviewOrientation**，并返回预览流的正确方向。 

若要设置元数据，请通过调用 [**VideoDeviceController.GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.VideoDeviceController.GetMediaStreamProperties) 来检索预览流属性。 然后，为视频流旋转创建表示媒体基础转换 (MFT) 属性的 GUID。 在 C++ 中，你可以使用常量 [**MF_MT_VIDEO_ROTATION**](https://msdn.microsoft.com/library/windows/desktop/hh162880.aspx)，但在 C# 中，必须手动指定 GUID 值。 

向流属性对象添加属性值，指定 GUID 作为项，指定预览旋转作为值。 此属性希望值以逆时针度数为单位，因此 **CameraRotationHelper** 方法 **ConvertSimpleOrientationToClockwiseDegrees** 用于转换简单的方向值。 最后，调用 [**SetEncodingPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.SetEncodingPropertiesAsync(Windows.Media.Capture.MediaStreamType,Windows.Media.MediaProperties.IMediaEncodingProperties,Windows.Media.MediaProperties.MediaPropertySet)) 将新的旋转属性应用到流。

[!code-cs[SetPreviewRotationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSetPreviewRotationAsync)]

接下来，为 **CameraRotationHelper.OrientationChanged** 事件添加处理程序。 此事件将传入一个参数，通过该参数，你可以确定预览流是否需要旋转。 如果设备的方向更改为面朝上或面朝下，此值将为 false。 如果预览需要旋转，请调用之前已定义的 **SetPreviewRotationAsync**。

然后，在 **OrientationChanged** 事件处理程序中，更新 UI（如有需要）。 通过调用 **GetUIOrientation** 从帮助程序类获取当前建议的 UI 方向，并将值转换为顺时针度数，此操作的用途是执行 XAML 转换。 通过方向值创建 [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.RotateTransform)，并设置 XAML 控件的 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.RenderTransform) 属性。 除了简单地旋转控件之外，你可能还需要在此处进行其他调整，具体取决于你的 UI 布局。 另外请记住，对 UI 的所有更新都必须在 UI 线程上进行，因此你应当将此代码放置在对 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority,Windows.UI.Core.DispatchedHandler)) 的调用中。  

[!code-cs[HelperOrientationChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetHelperOrientationChanged)]

## <a name="capture-a-photo-with-orientation-data"></a>使用方向数据捕获照片
文章[**使用 MediaCapture 捕获基本的照片、视频和音频**](basic-photo-video-and-audio-capture-with-mediacapture.md)展示了如何通过以下方式将照片捕获到文件：先捕获到内存中的流，然后使用解码器从该流中读取图像数据，再使用编码器将图像数据转码为文件。 从 **CameraRotationHelper** 类中获取的方向数据可以在执行转码操作期间添加到该图像文件。

在以下示例中，通过调用 [**CapturePhotoToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.CapturePhotoToStreamAsync) 将照片捕获到 [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.Streams.InMemoryRandomAccessStream)，并从该流中创建 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder)。 然后创建并打开 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFile)，检索 [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.Streams.IRandomAccessStream)，以便写入到该文件中。 

在对该文件进行转码之前，请通过帮助程序类方法 **GetCameraCaptureOrientation** 检索照片的方向。 此方法将返回一个 [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Sensors.SimpleOrientation) 对象，该对象可使用帮助程序方法 **ConvertSimpleOrientationToPhotoOrientation** 转换为 [**PhotoOrientation**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.FileProperties.PhotoOrientation) 对象。 接下来，创建新的 **BitmapPropertySet** 对象并添加属性，其中项是“System.Photo.Orientation”，值是照片方向，以 **BitmapTypedValue** 表示。 “System.Photo.Orientation”是许多可以作为元数据添加到图像文件的 Windows 属性之一。 有关所有与照片相关的属性列表，请参阅 [**Windows 属性 - 照片**](https://msdn.microsoft.com/library/windows/desktop/ff516600)。 有关使用图像元数据的详细信息，请参阅[**图像元数据**](image-metadata.md)。

最后，通过调用 [**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/) 为编码器设置属性集（包括方向数据），通过调用 [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.FlushAsync) 对图像进行转码。

[!code-cs[CapturePhotoWithOrientation](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCapturePhotoWithOrientation)]

## <a name="capture-a-video-with-orientation-data"></a>使用方向数据捕获视频
[**使用 MediaCapture 捕获基本的照片、视频和音频**](basic-photo-video-and-audio-capture-with-mediacapture.md)文章中介绍了基本视频捕获。 使用前面将方向数据添加到预览流部分中介绍的相同技术，在编码捕获的视频时添加方向数据。

在以下示例中，创建一个文件，以便将捕获的视频写入其中。 使用静态方法 [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile.CreateMp4) 创建 MP4 编码配置文件。 通过调用 **GetCameraCaptureOrientation** 从 **CameraRotationHelper** 类获取正确的视频方向。由于旋转属性要求方向以逆时针度数表示，因此调用 **ConvertSimpleOrientationToClockwiseDegrees** 帮助程序方法来转换方向值。 然后，为视频流旋转创建表示媒体基础转换 (MFT) 属性的 GUID。 在 C++ 中，你可以使用常量 [**MF_MT_VIDEO_ROTATION**](https://msdn.microsoft.com/library/windows/desktop/hh162880.aspx)，但在 C# 中，必须手动指定 GUID 值。 向流属性对象添加属性值，指定 GUID 作为项，指定旋转作为值。 最后调用 [**StartRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.StartRecordToStorageFileAsync(Windows.Media.MediaProperties.MediaEncodingProfile,Windows.Storage.IStorageFile)) 开始录制使用方向数据编码的视频。

[!code-cs[StartRecordingWithOrientationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartRecordingWithOrientationAsync)]

## <a name="camerarotationhelper-full-code-listing"></a>CameraRotationHelper 完整代码一览
以下代码段列出管理硬件方向传感器、计算照片和视频的正确方向值并提供帮助程序方法在不同 Windows 功能使用的不同方向表示法之间进行转换的 **CameraRotationHelper** 类的完整代码。 如果你按照上文中介绍的指南进行操作，可以将此类按原样添加到项目中，无需进行任何更改。 当然，你可以随意自定义以下代码，以满足特定场景的需求。

此帮助程序类使用设备的 [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Sensors.SimpleOrientationSensor) 来确定设备底盘的当前方向，使用 [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayInformation) 类来确定屏幕的当前方向。 其中每个类都可以提供在当前方向发生变化时引发的事件。 装载捕获设备的面板（前置、后置或外部）用于确定是否应镜像预览流。 此外，受部分设备支持的 [**EnclosureLocation.RotationAngleInDegreesClockwise**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.EnclosureLocation.RotationAngleInDegreesClockwise) 属性用于确定相机在底盘上的装载方向。

可以使用以下方法为指定的相机应用任务获取建议的方向值：

* **GetUIOrientation** - 为相机 UI 元素返回建议的方向。
* **GetCameraCaptureOrientation** - 返回要编码到图像元数据中的建议方向。
* **GetCameraPreviewOrientation** - 为预览流返回建议方向，以提供自然的用户体验。

[!code-cs[CameraRotationHelperFull](./code/SimpleCameraPreview_Win10/cs/CameraRotationHelper.cs#SnippetCameraRotationHelperFull)]



## <a name="related-topics"></a>相关主题

* [相机](camera.md)
* [使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 





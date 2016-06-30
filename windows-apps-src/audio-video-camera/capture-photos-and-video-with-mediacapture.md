---
author: drewbatgit
ms.assetid: 1361E82A-202F-40F7-9239-56F00DFCA54B
description: "本文将介绍使用 MediaCapture API 捕获照片和视频的操作步骤，包括初始化和关闭 MediaCapture 以及处理设备方向的更改。"
title: "使用 MediaCapture 捕获照片和视频"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c20c735d38e6baabe2f8bc0c7c682706d3946ed9

---

# 使用 MediaCapture 捕获照片和视频

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本文将介绍使用 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) API 捕获照片和视频的操作步骤，包括初始化和关闭 **MediaCapture** 以及处理设备方向的更改。

提供 **MediaCapture** 以便支持那些要求对媒体捕获过程进行低级别控制的应用以及需要高级捕获功能的实现方案。 使用 **MediaCapture** 还要求你提供自己的捕获 UI。 如果你的应用仅需要捕获照片或视频，并且不关心高级捕获技术，则只需几行代码，[**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 即可使其轻松捕获照片或视频。 有关详细信息，请参阅[使用 CameraCaptureUI 捕获照片和视频](capture-photos-and-video-with-cameracaptureui.md)。

本文中的代码源自 [CameraStarterKit 示例](http://go.microsoft.com/fwlink/?LinkId=619479)。 你可以下载该示例以查看上下文中使用的代码，或将该示例用作你自己的应用的起始点。

## 配置项目

### 向应用清单中添加功能声明

为了让你的应用可以访问设备的相机，必须声明你的应用要使用 *webcam* 和 *microphone* 设备功能。 如果你想要将已捕获的照片和视频保存到用户的图片库或视频库，你还必须声明 *picturesLibrary* 和 *videosLibrary* 功能。

**将功能添加到应用清单**

1.  在 Microsoft Visual Studio 的“解决方案资源管理器”****中，通过双击“package.appxmanifest”****项，打开应用程序清单的设计器。
2.  选择“功能”****选项卡。
3.  选中“摄像头”****框和“麦克风”****框。
4.  若要访问图片库和视频库，请选中“图片库”****框和“视频库”****框。

### 添加用于媒体捕获相关的 API 的 using 指令

以下列出的代码显示了本文中示例代码引用的命名空间，并介绍了每个命名空间提供了哪些功能。

[!code-cs[使用](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## 初始化 MediaCapture 对象

[
            **Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) 命名空间中的 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 类是所有媒体捕获操作的基本接口。 应用通常将此类型变量的范围声明为单个页面。 你的应用需要跟踪 **MediaCapture** 的当前状态，因此应为对象的初始化、预览和录制状态声明布尔变量。

[!code-cs[MediaCaptureVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMediaCaptureVariables)]

若要帮助正确确定预览视频的方向，请创建成员变量来跟踪相机是否在外部以及应用当前是否正在生成预览流镜像。 当你认为视频源正在捕获用户时，你的应用应生成预览流镜像，因为这是更自然的用户体验。

[!code-cs[PreviewVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewVariables)]

以下示例方法将初始化媒体捕获对象。 首先，该代码搜索可以用于媒体捕获的视频捕获设备。 找到后，初始化 **MediaCapture** 对象并注册其事件的处理程序。 下一步，使用视频捕获设备的 ID 创建 [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/desktop/hh802710) 对象。 然后调用 [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) 初始化 **MediaCapture**。

[!code-cs[InitializeCameraAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitializeCameraAsync)]

-   [
            **DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) 方法可以用于查找指定类型的所有设备。 在此示例中，传入 **DeviceClass.VideoCapture** 枚举值以指示仅限返回视频捕获设备。 请注意，视频捕获设备用于捕获照片和视频。

-   对于请求类型的每个已找到的设备，**FindAllAsync** 将返回包含 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) 对象的 [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) 对象。 来自 **System.Linq** 命名空间的 **FirstOrDefault** 扩展方法提供简单语法，以便基于指定条件从列表中选择项目。 第一次调用将尝试选择列表中第一个具有 **Panel.Back** 的 [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) 值的 **DeviceInformation**，该值指示相机位于设备机箱的后面板上。 如果设备的后面板上没有相机，则使用第一个可用的相机。

-   如果初始化 [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573) 时未指定设备 ID，系统将选择设备的内部列表中的第一个设备。

-   调用 [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) 初始化对象以使用指定的捕获设备。 此调用在 **try** 块内进行，因为如果用户拒绝该调用应用访问相机，它将引发 **UnauthorizedAccessException**。 如果调用成功，则 **\_isInitialized** 变量将设置为 True，以便后续方法调用可以确定是否已初始化捕获设备。

- **重要提示** 对于某些设备系列，在授予应用访问设备相机的权限前，会向用户显示用户同意提示。 出于此原因，必须从主 UI 线程仅调用 [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598)。 尝试从其他线程初始化相机可能会导致初始化失败。

-   如果成功初始化该捕获设备，则设置变量以反映捕获设备是否在外部，或者是否位于设备的前面板上。 这些值将用于为用户正确确定捕获预览的方向。 最后，将更新 UI 以反映可以使用捕获，并且来自捕获设备的预览流已启动。 所有这些任务在本文稍后部分所述的帮助程序方法中执行。

## 启动捕获预览

对于能够看到设备正在捕获内容的用户，你需要提供视频捕获设备当前在你的 UI 中所看到内容的预览。

**重要提示** 必须启动捕获预览，才能使捕获设备启用自动对焦、自动曝光和自动白平衡。

提供 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) 控件以启用捕获预览。 以下内容显示定义捕获元素的示例 XAML 代码。

[!code-xml[CaptureElement](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetCaptureElement)]

用户希望他们在预览视频捕获屏幕期间，屏幕保持打开状态，不会由于不活动而关闭。 若要启用此功能，必须创建 [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) 对象。 声明此变量的页面作用域，以便它一直存在于整个捕获会话中。

[!code-cs[DisplayRequest](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayRequest)]

以下方法启动媒体捕获预览。 首先，它通过调用 [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) 上的 [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818) 来请求将显示保持活动状态。 接下来，通过调用 [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613) 启动预览。

[!code-cs[StartPreviewAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]

-   调用 **DisplayRequest** 对象上的 [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818) 方法来请求系统将屏幕保持打开状态。

-   将 **CaptureElement** 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 属性设置为应用的 **MediaCapture** 对象来定义此预览的源。

-   由 XAML 框架提供 [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 属性以支持双向用户界面。 将 **CaptureElement** 的流方向设置为 [**FlowDirection.RightToLeft**](https://msdn.microsoft.com/library/windows/apps/br242397) 将导致预览视频水平翻转。 当捕获设备位于设备的前面板上时使用此方法，以便从用户的角度来看，预览处于正确方向。

-   [
            **StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613) 方法开始显示 **CaptureElement** 内的预览流。 如果成功启动预览，将设置 **\_isPreviewing** 变量以允许应用的其他部分了解该应用当前正在预览，并调用用于设置预览旋转的帮助程序方法。 将在下一节中定义此方法。

## 检测屏幕和设备方向

有媒体捕获应用（当在类似手机或平板电脑等移动设备上运行时）的多个区域受设备的当前方向的影响。 这些区域包括从相机正确旋转预览流以及正确编码已捕获的图像和视频，以便用户查看时，它们的方向是正确的。

术语“显示方向”指的是系统旋转设备上的 XAML 页面以使其对于用户保持直立的方式。 “设备方向”指的是世界空间中的设备的方向，因而是物理相机设备在世界空间中的方向。 这两种方向与媒体捕获应用有关。 若要处理显示方向，请声明并初始化 [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/dn264258) 类的页面范围变量。 声明类型 [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/br226142) 的另一个变量以跟踪当前显示方向。 声明 [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400) 变量和 [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399) 变量，以跟踪设备方向。

[!code-cs[DisplayInformationAndOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayInformationAndOrientation)]

以下帮助程序方法注册和注销 [**DisplayInformation.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) 和 [**SimpleOrientationSensor.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/br206407) 事件的事件处理程序，并使用当前方向初始化跟踪变量。 请注意，并非所有设备都具有 [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400)，因此在注册该处理程序或尝试获取当前方向之前应进行检查。

[!code-cs[RegisterOrientationEventHandlers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterOrientationEventHandlers)]

[!code-cs[UnregisterOrientationEventHandlers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterOrientationEventHandlers)]

在 **SimpleOrientationSensor.OrientationChanged** 事件的事件处理程序中，会使用当前方向更新设备方向变量。 如果设备面朝上或面朝下，则不应该更新方向。

[!code-cs[SimpleOrientationChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSimpleOrientationChanged)]

在 **DisplayInformation.OrientationChanged** 事件的事件处理程序中，会使用当前方向更新显示方向变量。 如果当前正在显示捕获设备的视频预览，请更新预览视频流的旋转。 **SetPreviewRotationAsync** 帮助程序方法将在以下各节中介绍。

[!code-cs[DisplayOrientationChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayOrientationChanged)]

## 设置媒体捕获预览旋转

用户希望当他们的移动设备的方向更改时会旋转 UI 控件，以便 UI 中的文本是垂直排列并且可读。 然而对于 **CaptureElement** 控件，用户通常不希望在设备旋转时视频预览的方向随之旋转。 为了提供预期的用户体验，应旋转预览流以匹配设备的方向。

必须以度为单位表示预览流方向。 以下帮助程序方法会将 [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/br226142) 枚举值转换为度。

[!code-cs[ConvertDisplayOrientationToDegrees](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertDisplayOrientationToDegrees)]

而以下帮助程序方法会将 [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400) 用以表示设备旋转的 [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399) 枚举值转换为度。

[!code-cs[ConvertDeviceOrientationToDegrees](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertDeviceOrientationToDegrees)]

在底层，实际上由 Microsoft 媒体基础框架执行流的旋转。 使用 [MF\_MT\_VIDEO\_ROTATION](https://msdn.microsoft.com/library/windows/desktop/hh162880) 属性指定旋转。 由于这是 Windows 运行时应用，将使用该属性的 GUID 而不是属性名称指定旋转。 定义以下 GUID 来识别视频旋转属性。

[!code-cs[RotationKey](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRotationKey)]

以下方法设置预览流的旋转。 媒体捕获的 [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) 的 [**GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995) 方法会返回由键/值对组成的属性集。 指定 [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640) 以表明我们需要视频预览流的属性，而不是视频录制流或音频流的属性。 属性集是用于设置流属性的常规用途界面，但对于此任务，上面定义的视频旋转 GUID 添加为属性键，并且视频流的所需方向（以度为单位）指定为该值。 [
            **SetEncodingPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/dn297781) 使用新值更新编码属性。 再次强调，指定 **MediaStreamType.VideoPreview** 以表明正在设置的属性将用于视频预览流。

[!code-cs[SetPreviewRotation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetPreviewRotation)]

-   对于具有外部相机的设备，用户不希望当设备旋转时相机流随之旋转。

-   如果正在对前面板上的相机进行镜像预览，则必须反转旋转方向以匹配设备的旋转。

-   某些设备（通常为手机）支持将 [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/dn264259) 设置为诸如 [**DisplayOrientations.Landscape**](https://msdn.microsoft.com/library/windows/apps/br226142) 的方向值以强制显示随设备旋转。 应设置此值，因为它可以在支持它的设备上提供良好的体验，但你仍应在你的应用中实现上述模式以支持不支持自动旋转首选项的设备。

-   在以前的版本中，[**SetPreviewRotation**](https://msdn.microsoft.com/library/windows/apps/br226611) 方法是用于旋转预览流的唯一方式。 此方法仍存在于 API 图面中以支持现有的应用，但是此方法效率低下，并且不应用于新的应用。

## 捕获照片

以下方法使用 [**CapturePhotoToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/hh700840) 方法捕获照片，并传入请求的编码属性和一个包含捕获操作输出的 [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241720) 对象。 [
            **ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) 类提供帮助程序方法（如 [**CreateJpeg**](https://msdn.microsoft.com/library/windows/apps/hh700994)）来为受媒体捕获支持的文件类型生成编码属性。

[!code-cs[TakePhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetTakePhotoAsync)]

在将照片保存到文件之前，你需要确定照片的正确方向。 **MediaCapture** 对象并不知道设备的方向，因此它在假设捕获设备处于其默认方向情况下对已捕获的照片数据进行编码。 当用户在查看已捕获的照片时，这可能导致负面的用户体验，因为照片方向未进行正确调整。 以下帮助程序方法确定正确的照片方向，然后保存方向正确的文件。

**GetCameraOrientation** 帮助程序方法以当前设备方向为起点，然后根据该设备的本机方向和设备上相机的位置进行旋转。 如果相机安装在设备的前面板上（由本示例中的 **\_mirroringPreview** 变量指示），应反转该相机方向。

[!code-cs[GetCameraOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetCameraOrientation)]

以下帮助程序方法简单地将值从方向传感器使用的 [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399) 枚举值转换为将用于保存文件的位图编码器使用的等效 [**PhotoOrientation**](https://msdn.microsoft.com/library/windows/apps/hh965476) 值。

[!code-cs[ConvertOrientationToPhotoOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertOrientationToPhotoOrientation)]

最后，可以编码并保存已捕获的照片。 从包含已捕获照片数据的输入流创建 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) 对象。 创建新的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 并将其打开以进行读写。 创建 [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206) 对象，传入输出文件和包含图像数据的解码器。 创建新的 [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/hh974338) 并添加新的属性。 该属性的键“System.Photo.Orientation”指定该属性表示照片的方向。 该值是先前计算的 [**PhotoOrientation**](https://msdn.microsoft.com/library/windows/apps/hh965476) 值。 调用 [**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252) 以更新编码器上的属性，然后调用 [**FlushAsync**](https://msdn.microsoft.com/library/dn237883) 将该照片写入存储文件。

[!code-cs[ReencodeAndSavePhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetReencodeAndSavePhotoAsync)]

-   设置“System.Photo.Orientation”位图属性会将照片的方向编码到文件的元数据。 它不会导致以不同的方式编码实际图像数据。 有关将元数据嵌入到图像文件中的详细信息，请参阅[图像元数据](image-metadata.md)。

-   有关处理图像（包括编码和解码图像）的详细信息，请参阅[图像处理](imaging.md)。

## 捕获视频

若要开始捕获视频，首先创建将视频录制到其中的存储文件。 下一步，创建 **MediaCapture** 用于将视频编码到文件的 [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026)。 **MediaEncodingProfile** 类提供为支持的视频格式创建编码配置文件的方法，如 [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/hh701078)。 使用前面所述的帮助程序方法以获取该视频的正确旋转（以度为单位）。 与照片方案不同，视频旋转信息通过 **MediaCapture** 编码到流中。 通过将旋转信息添加到 [**VideoEncodingProperties.Properties**](https://msdn.microsoft.com/library/windows/apps/hh701254) 集合，来将其添加到编码配置文件。 先前为视频旋转定义的 GUID 用作键，并且该旋转（以度为单位）是数值。 最后，调用 [**MediaCapture.StartRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh700863)，指定编码属性和输出文件即可开始录制。

[!code-cs[StartRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartRecordingAsync)]

若要停止录制，只需调用 [**MediaCapture.StopRecordAsync**](https://msdn.microsoft.com/library/windows/apps/br226623)。

[!code-cs[StopRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStopRecordingAsync)]

初始化 **MediaCapture** 时，会注册 [**MediaCapture.RecordLimitationExceeded**](https://msdn.microsoft.com/library/windows/apps/hh973312) 事件的处理程序。 在处理程序中，调用 **StopRecordingAsync** 方法以停止录制视频。

[!code-cs[RecordLimitationExceeded](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceeded)]

## 暂停和恢复视频捕获

在某些情况下，你可能想要暂停和继续正在进行的视频捕获，而不是停止该捕获并启动一个新的捕获。 若要暂停录制，请调用 [**PauseRecordAsync**](https://msdn.microsoft.com/library/windows/apps/dn858102)。 如果指定了 [**MediaCapturePauseBehavior.RetainHardwareResources**](https://msdn.microsoft.com/library/windows/apps/dn926686)，那么在不支持同时进行视频和照片捕获的设备上，该应用将不能在暂停视频时捕获照片。 有关确定设备是否支持同时进行照片和视频捕获的信息，请参阅[相机配置文件](camera-profiles.md)。

[!code-cs[PauseRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPauseRecordingAsync)]

若要恢复先前暂停的视频捕获，请调用 [**ResumeRecordAsync**](https://msdn.microsoft.com/library/windows/apps/dn858103)。

[!code-cs[ResumeRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetResumeRecordingAsync)]

## 清理捕获资源

正确关闭和释放媒体捕获资源非常重要。 你的应用关闭后，如果无法执行上述操作可能会导致异常的相机行为，这会为你的应用造成负面的用户体验。 以下部分演示了关闭相机时应该采取的不同步骤。

### 关闭相机预览

若要关闭捕获预览，首先调用 [**MediaCapture.StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622)。 将 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 属性设置为 Null。 然后，调用 [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) 变量上的 [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819)，以允许系统关闭屏幕（如果需要）。

[!code-cs[StopPreviewAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStopPreviewAsync)]

-   无法从非 UI 线程访问该 UI ，因此必须使用 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 方法设置 **MediaElement.Source** 属性和调用 **RequestRelease**，以便在 UI 线程上执行调用。

### 关闭和释放 MediaCapture 对象

在释放 **MediaCapture** 对象之前，停止任何正在进行的录制，并通过调用先前定义的帮助程序方法停止预览流。 完成此操作后，请删除任何使用 **MediaCapture** 注册的事件处理程序，然后调用 [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858) 来释放任何与该对象关联的资源，并将 **MediaCapture** 设置为 Null

[!code-cs[CleanupCameraAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

应该从 [**MediaCapture.Failed**](https://msdn.microsoft.com/library/windows/apps/br226593) 事件的处理程序内调用此方法来关闭和释放 **MediaCapture** 对象。

[!code-cs[CaptureFailed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureFailed)]

### 处理应用生存期和页面导航事件

应用生存期事件为你的应用提供初始化并释放资源的机会。 这一点对于 **Application.Suspending** 事件尤其重要，其中对于你的应用正确释放媒体捕获资源至关重要。

你可以在页面的构造器中为应用程序生存期事件注册处理程序。

[!code-cs[RegisterAppLifetimeEvents](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterAppLifetimeEvents)]

在 **Application.Suspending** 事件的处理程序中，应注销屏幕和设备方向事件的处理程序，并关闭 **MediaCapture** 对象。 本文中稍后介绍的另一个应用程序生命周期相关的任务将需要此处注销的 [**SystemMediaTransportControls.PropertyChanged**](https://msdn.microsoft.com/library/windows/apps/dn278720) 事件。

**警告** 必须通过在暂停事件处理程序的开头处调用 [**SuspendingOperation.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) 来请求暂停延迟。 这要求系统在关闭你的应用之前等待你发出操作已完成的信号。 这是必须的，因为 **MediaCapture** 关闭操作异步执行，以便 **Application.Suspending** 事件处理程序可以在正确关闭相机之前完成。 处于等待状态的异步调用完成后，应通过调用 [**SuspendingDeferral.Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) 释放延迟。

[!code-cs[暂停中](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSuspending)]

在 **Application.Resuming** 事件的处理程序中，应该为屏幕和设备方向事件注册处理程序、注册 **SystemMediaTransportControls.PropertyChanged** 事件并初始化 **MediaCapture** 对象。

[!code-cs[恢复中](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetResuming)]

[
            **OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) 事件提供一个机会，以便为屏幕和设备方向事件初始注册处理程序，并初始化 **MediaCapture** 对象。

[!code-cs[OnNavigatedTo](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnNavigatedTo)]

如果应用有多个页面，应在 [**OnNavigatingFrom**](https://msdn.microsoft.com/library/windows/apps/br227509) 事件处理程序中清理媒体捕获对象。

[!code-cs[OnNavigatingFrom](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnNavigatingFrom)]

对于要在支持多个同步窗口的设备上正常表现的应用，当最小化或恢复应用时必须立即响应。 若要执行此操作，必须处理 [**SystemMediaTransportControls.PropertyChanged**](https://msdn.microsoft.com/library/windows/apps/dn278720) 事件。 初始化成员变量，以便为你的应用存储对 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 对象的引用。

[!code-cs[DeclareSystemMediaTransportControls](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareSystemMediaTransportControls)]

应将用于注册和注销 **PropertyChanged** 事件的代码添加到上述示例中显示的应用生命周期事件中。 在该事件的处理程序中，检查以查看触发该事件的属性更改是否是 [**SystemMediaTransportControlsProperty.SoundLevel**](https://msdn.microsoft.com/library/windows/apps/dn278721) 属性。 如果由该属性的更改触发，请检查该属性的值。 如果该值是 [**SoundLevel.Muted**](https://msdn.microsoft.com/library/windows/apps/hh700852)，则已最小化你的应用，并且你应相应清理你的媒体捕获资源。 否则，该事件指示已还原你的应用窗口，并且应重新初始化媒体捕获资源。 只有在 UI 线程上才可以访问 **SoundLevel** 属性，因此此方法中的代码包含在对 [**Dispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 的调用中。

[!code-cs[SystemMediaControlsPropertyChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSystemMediaControlsPropertyChanged)]

## 媒体捕获的其他 UI 注意事项

### 设置自动旋转首选项

如有关旋转预览流的上一节中所述，某些设备支持设置 [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/dn264259) 以防止页面（包括显示该预览的 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278)）随设备旋转而旋转。 这可在支持它的设备上提供良好的用户体验。 当你的应用启动或当你开始显示预览时，请设置此值。 请注意，仍应为不支持自动旋转首选项的设备实现预览旋转。

[!code-cs[SetAutoRotationPreferences](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetAutoRotationPreferences)]

### 处理 UI 元素方向

用户通常希望相机应用中的 UI 元素（如启动照片或视频捕获的按钮）跟随视频预览旋转。 以下方法演示如何使用先前定义的方向帮助程序方法来正确定位相机 UI 中的按钮。 应从 [**DisplayInformation.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) 和 [**SimpleOrientationSensor.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/br206407) 事件处理程序和首次启动你的应用时调用此方法。 你的实现可能因应用的 UI 而有所不同。

[!code-cs[UpdateButtonOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateButtonOrientation)]

当正在关闭你的应用时，或如果你导航到与媒体捕获无关的页面时，请将自动旋转首选项设置为 [**None**](https://msdn.microsoft.com/library/windows/apps/br226142) 以允许 UI 正常旋转。

[!code-cs[RevertAutoRotationPreferences](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRevertAutoRotationPreferences)]

### 支持同时进行照片和视频捕获

[
            **Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) API 允许你在支持它的设备上同时捕获照片和视频。 为简化起见，此示例使用 [**ConcurrentRecordAndPhotoSupported**](https://msdn.microsoft.com/library/windows/apps/dn278843) 属性来确定是否支持同时捕获视频和照片，但是执行此操作的更可靠且建议的方法是使用相机配置文件。 有关详细信息，请参阅[相机配置文件](camera-profiles.md)。

以下帮助程序方法更新应用的控件以匹配该应用的当前捕获状态。 每当你的应用的捕获状态更改时（如启动或停止视频捕获时），请调用此方法。

[!code-cs[UpdateCaptureControls](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateCaptureControls)]

### 支持特定于移动的 UI 功能

本文中显示的所有代码将在通用 Windows 应用中使用。 使用其他几行代码，你可以利用仅适用于移动设备的特殊 UI 功能。 若要使用这些功能，你必须将对适用于通用应用平台的 Microsoft 移动扩展 SDK 的引用添加到你的项目中。

**添加对硬件相机按钮支持的移动扩展 SDK 的引用**

1.  在“解决方案资源管理器”****中，右键单击“引用”****，然后选择“添加引用...”****

2.  展开“Windows 通用”****节点，然后选择“扩展”****。

3.  单击“适用于通用应用平台的 Microsoft 移动扩展 SDK”****旁边的复选框。

移动设备具有向用户提供有关设备的状态信息的 [**StatusBar**](https://msdn.microsoft.com/library/windows/apps/dn633864) 控件。 此控件占用屏幕上的空间，这可能会干扰媒体捕获 UI。 你可以通过调用 [**HideAsync**](https://msdn.microsoft.com/library/windows/apps/dn610339) 来隐藏状态栏，但你必须从你使用 [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/dn949016) 方法以确定 API 是否在其中可用的条件块内执行此调用。 此方法将仅在支持状态栏的移动设备上返回 True。 当应用启动时或当你开始从相机预览时，应该隐藏状态栏。

[!code-cs[HideStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHideStatusBar)]

当应用关闭时或者当用户导航离开应用的媒体捕获页面时，使该控件再次可见。

[!code-cs[ShowStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetShowStatusBar)]

某些移动设备具有专用的硬件相机按钮，比起屏幕上的控件来说，一些用户更喜欢使用它们。 若要在按下硬件相机按钮时收到通知，请注册 [**HardwareButtons.CameraPressed**](https://msdn.microsoft.com/library/windows/apps/dn653805) 事件的处理程序。 由于此 API 仅适用于移动设备，必须再次使用 **IsTypePresent** 以确保在尝试访问该 API 之前，它在当前设备上受支持。

[!code-cs[PhoneUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhoneUsing)]

[!code-cs[RegisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterCameraButtonHandler)]

在 **CameraPressed** 事件的处理程序中，你可以启动照片捕获。

[!code-cs[CameraPressed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCameraPressed)]

当应用关闭或者用户离开应用的媒体捕获页面时，请注销该硬件按钮处理程序。

[!code-cs[UnregisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterCameraButtonHandler)]

**注意** 本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你要针对 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。

## 相关主题

* [相机配置文件](camera-profiles.md)
* [高动态范围 (HDR) 照片捕获](high-dynamic-range-hdr-photo-capture.md)
* [用于照片和视频捕获的捕获设备控件](capture-device-controls-for-photo-and-video-capture.md)
* [用于视频捕获的捕获设备控件](capture-device-controls-for-video-capture.md)
* [视频捕获的效果](effects-for-video-capture.md)
* [媒体捕获的场景分析](scene-analysis-for-media-capture.md)
* [可变照片序列](variable-photo-sequence.md)
* [获取预览帧](get-a-preview-frame.md)
* [CameraStarterKit 示例](http://go.microsoft.com/fwlink/?LinkId=619479)



<!--HONumber=Jun16_HO4-->



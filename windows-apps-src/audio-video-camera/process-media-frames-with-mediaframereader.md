---
author: drewbatgit
ms.assetid: 
description: "本文介绍如何将 MediaFrameReader 与 MediaCapture 结合使用，以获取一个或多个可用源提供的媒体帧，这些可用源包括彩色、深度、红外线相机，音频设备，甚至是自定义的帧源（例如生成骨架跟踪帧的帧源）。"
title: "使用 MediaFrameReader 处理媒体帧"
translationtype: Human Translation
ms.sourcegitcommit: e6ab1fc16f150de2fed3797d89375a52b3965182
ms.openlocfilehash: 11e09d9b447e9daa0498377a67ef235bdab168dd

---

# <a name="process-media-frames-with-mediaframereader"></a>使用 MediaFrameReader 处理媒体帧

本文介绍如何将 [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) 与 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) 结合使用，以获取一个或多个可用源提供的媒体帧，这些可用源包括彩色、深度、红外线相机，音频设备，甚至是自定义的帧源（例如生成骨架跟踪帧的帧源）。 此功能旨在由执行实时处理媒体帧的应用使用，例如增强现实和感知深度的相机应用。

如果你对仅捕获视频或照片感兴趣（例如典型的摄影应用），则可能希望使用 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) 支持的其他捕获技术。 有关可用的媒体捕获技术列表和演示如何使用这些技术的文章，请参阅[**相机**](camera.md)。

> [!NOTE] 
> 本文中讨论的功能仅从 Windows 10 版本 1607 开始提供。

> [!NOTE] 
> 文中提供了一个通用 Windows 应用示例，介绍使用 **MediaFrameReader** 显示不同帧源（包括彩色、深度和红外相机）的帧。 有关详细信息，请参阅[相机帧示例](http://go.microsoft.com/fwlink/?LinkId=823230)。

## <a name="setting-up-your-project"></a>设置项目
对于使用 **MediaCapture** 的任何应用，在尝试访问任何相机设备前都必须声明应用使用 *webcam* 功能。 如果应用从音频设备捕获音频，还应声明 *microphone* 设备功能。 

**将功能添加到应用清单**

1.  在 Microsoft Visual Studio 的“解决方案资源管理器”中，通过双击“package.appxmanifest”项，打开应用程序清单的设计器。
2.  选择“功能”选项卡。
3.  选中“摄像头”框和“麦克风”框。
4.  若要访问图片库和视频库，请选中“图片库”框和“视频库”框。

除了默认项目模板包含的这些 API，本文的示例代码还使用来自以下命名空间的 API。

[!code-cs[FramesUsing](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFramesUsing)]

## <a name="select-frame-sources-and-frame-source-groups"></a>选择帧源和帧源组
处理媒体帧的许多应用都需要同时从多个源获取帧，例如设备的彩色和深度相机。 [**MediaFrameSourceGroup**] (https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup) 对象表示一组可同时使用的媒体帧源。 调用静态方法 [**MediaFrameSourceGroup.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.FindAllAsync) 获取当前设备支持的所有帧源组列表。

[!code-cs[FindAllAsync](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFindAllAsync)]

还可以使用 [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceWatcher) 和从 [**MediaFrameSourceGroup.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/br225427) 返回的值创建 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.GetDeviceSelector)，以便在设备上的可用帧源组更改时（例如接入外部相机时）接收通知。 有关详细信息，请参阅[**枚举设备**](https://msdn.microsoft.com/windows/uwp/devices-sensors/enumerate-devices)。

[**MediaFrameSourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup) 具有 [**MediaFrameSourceInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo) 对象集合，用于描述组内包括的帧源。 检索可在设备上使用的帧源组后，可以选择公开所关注帧源的组。

以下示例展示了选择帧源组的最简方法。 此代码只是循环访问所有可用组，然后循环访问 [**SourceInfos**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.SourceInfos) 集合中的每个项目。 检查每个 **MediaFrameSourceInfo**，了解相应项目是否支持我们正在寻找的功能。 在本例中，检查 [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo.MediaStreamType) 属性以查看 [**VideoPreview**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaStreamType) 值（该值表示设备提供视频预览流），并且检查 [**SourceKind**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo.SourceKind) 属性以查看 [**Color**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceKind) 值（该值指示源提供颜色帧）。

[!code-cs[SimpleSelect](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSimpleSelect)]

这种标识所需帧源组和帧源的方法对简单情况有效，但如果要依据更复杂的条件选择帧源，这会很快变得难以实现。 另一种方法是使用 Linq 语法和匿名对象进行选择。 以下示例使用 **Select** 扩展方法将 *frameSourceGroups* 列表中的 **MediaFrameSourceGroup** 对象转换为具有两个字段的匿名对象：*sourceGroup* 表示组本身，*colorSourceInfo* 表示组中的颜色帧源。 *colorSourceInfo* 字段设为 **FirstOrDefault** 的结果，后者用于选择所提供的谓词解析为 true 的第一个对象。 在本例中，如果流类型为 **VideoPreview**、源种类为 **Color**，并且相机是设备的前置相机，则谓词为 true。

在从上述查询返回的匿名对象列表中，**Where** 扩展方法用于仅选择 *colorSourceInfo* 字段不为 null 的对象。 最后，调用 **FirstOrDefault** 以选择列表中的第一个项目。

此时，可以使用所选对象的字段获取所选 **MediaFrameSourceGroup** 和表示彩色相机的 **MediaFrameSourceInfo** 对象的引用。 这些引用将在稍后用于初始化 **MediaCapture** 对象并为所选源创建 **MediaFrameReader**。 最后应进行测试，以查看源组是否为 null，如果是，则意味着当前设备没有所请求的捕获源。

[!code-cs[SelectColor](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSelectColor)]

以下示例使用与上述技术相类似的技术选择包含彩色、深度和红外相机的源组。

[!code-cs[ColorInfraredDepth](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetColorInfraredDepth)]

## <a name="initialize-the-mediacapture-object-to-use-the-selected-frame-source-group"></a>初始化 MediaCapture 对象以使用所选帧源组
下一步是初始化 **MediaCapture** 对象以使用在上一步中所选的帧源组。

**MediaCapture** 对象通常在应用的多个位置中使用，因此应声明某个类成员变量以保留它。

[!code-cs[DeclareMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

通过调用构造函数创建 **MediaCapture** 对象实例。 接下来，创建用于初始化 **MediaCapture** 对象的 [**MediaCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSettings) 对象。 本示例使用以下设置：

* [**SourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.SourceGroup) - 告知系统用于获取帧的源组。 请记住，源组定义可以同时使用的一组媒体帧源。
* [**SharingMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.SharingMode) - 告知系统是否需要捕获源设备的独占控制权。 如果将它设置为 [**ExclusiveControl**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSharingMode)，意味着可以更改捕获设备的设置（例如它生成的帧格式），但也意味着如果其他应用已经拥有独占控制权，你的应用在尝试初始化媒体捕获设备时会失败。 如果将它设置为 [**SharedReadOnly**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSharingMode)，则可以检索帧源的帧，即使其他应用已经在使用这些帧也是如此，但无法更改设备设置。
* [**MemoryPreference**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.MemoryPreference) - 如果指定 [**CPU**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureMemoryPreference)，系统将使用可以保证到达的帧能够用作 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap) 对象的 CPU 内存。 如果指定 [**Auto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureMemoryPreference)，系统将动态选择存储帧的最佳内存位置。 如果系统选择使用 GPU 内存，媒体帧将作为 [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) 对象（而非 **SoftwareBitmap** 对象）到达。
* [**StreamingCaptureMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.StreamingCaptureMode) - 将它设置为 [**Video**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.StreamingCaptureMode)，以指示无需流式处理音频。

调用 [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) 以使用所需设置初始化 **MediaCapture**。 请确保在 *try* 块中调用此函数，防止初始化失败。

[!code-cs[InitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitMediaCapture)]

## <a name="set-the-preferred-format-for-the-frame-source"></a>为帧源设置首选格式
若要为帧源设置首选格式，需要获取表示源的 [**MediaFrameSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource) 对象。 通过访问已初始化的 **MediaCapture** 对象的 [**Frames**](https://msdn.microsoft.com/library/windows/apps/Windows.Phone.Media.Capture.CameraCaptureSequence.Frames) 字典，并指定希望使用的帧源的标识符，获取此对象。 这就是我们在选择帧源组时保存 [**MediaFrameSourceInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo) 对象的原因。

[**MediaFrameSource.SupportedFormats**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource.SupportedFormats) 属性包含 [**MediaFrameFormat**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameFormat) 对象列表，此类对象用于描述帧源的受支持格式。 使用 **Where** Linq 扩展方法以根据所需属性选择某种格式。 在本例中，选择了宽度为 1080 像素并且可以提供 32 位 RGB 格式的帧格式。 **FirstOrDefault** 扩展方法选择列表中的第一个条目。 如果所选格式为 null，则请求的格式不受帧源支持。 如果该格式受支持，可以请求源通过调用 [**SetFormatAsync**](https://msdn.microsoft.com/library/windows/apps/) 来使用此格式。

[!code-cs[GetPreferredFormat](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetPreferredFormat)]

## <a name="create-a-frame-reader-for-the-frame-source"></a>为帧源创建帧阅读器
若要为媒体帧源接收帧，请使用 [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader)。

[!code-cs[DeclareMediaFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareMediaFrameReader)]

通过在已初始化的 **MediaCapture** 对象上调用 [**CreateFrameReaderAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.CreateFrameReaderAsync)，实例化帧阅读器。 此方法的第一个参数是你希望从中接收帧的帧源。 可以为每个要使用的帧源创建单独的帧阅读器。 第二个参数告知系统你希望帧到达时使用的输出格式。 这可以省去在帧到达时需要自行转换到帧的工作。 请注意，如果所指定的格式不受帧源支持，将引发异常，因此请确保该值在 [**SupportedFormats**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource.SupportedFormats) 集合中。  

创建帧阅读器后，请为源提供新帧时引发的 [**FrameArrived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.FrameArrived) 事件注册处理程序。

调用 [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.StartAsync) 以告知系统开始读取源的帧。

[!code-cs[CreateFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCreateFrameReader)]

## <a name="handle-the-frame-arrived-event"></a>处理帧到达事件
新帧可用时即引发 [**MediaFrameReader.FrameArrived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.FrameArrived) 事件。 可以选择处理到达的每个帧，或者仅在需要时使用它们。 因为帧阅读器在自己的线程上引发事件，所以可能需要实现某些同步逻辑，以确保你不会尝试从多个线程访问相同数据。 本节介绍如何在 XAML 页面中将图形颜色帧同步到图像控件。 这套方案解决需要在 UI 线程上执行 XAML 控件的所有更新的其他同步约束。

在 XAML 中显示帧的第一步是创建 Image 控件。 

[!code-xml[ImageElementXAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetImageElementXAML)]

在代码隐藏页面中，声明 **SoftwareBitmap** 类型的类成员变量，该变量用作所有传入图像都会复制到的后台缓冲区。 请注意，图像数据本身不会复制，复制的仅是对象引用。 此外，声明某个布尔值以跟踪 UI 操作当前是否正在运行。

[!code-cs[DeclareBackBuffer](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareBackBuffer)]

由于帧将作为 **SoftwareBitmap** 对象到达，因此需要创建 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) 对象，该对象支持将 **SoftwareBitmap** 用作 XAML **Control** 的源。 在启动帧阅读器前，应该在代码的某个位置设置图像源。

[!code-cs[ImageElementSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetImageElementSource)]

此时可以实现 **FrameArrived** 事件处理程序。 调用处理程序时，*sender* 参数包含对引发该事件的 **MediaFrameReader** 对象的引用。 调用此对象上的 [**TryAcquireLatestFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.TryAcquireLatestFrame) 以尝试获取最新帧。 如名称所示，**TryAcquireLatestFrame** 可能不会成功返回帧。 因此，当依次访问 VideoMediaFrame 和 SoftwareBitmap 属性时，请确保测试它们是否为 null。 在本示例中，null 条件运算符 用于访问 **SoftwareBitmap**，然后检查检索的对象是否为 null。

**Image** 控件仅可以显示格式为 BRGA8 并且已经预乘或者没有 alpha 的图像。 如果到达的帧不是这种格式，则使用静态方法 [**Convert**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap.Covert) 将软件位图转换为正确的格式。

接下来，使用 [**Interlocked.Exchange**](https://msdn.microsoft.com/library/bb337971) 方法来将到达位图的引用交换为后台缓冲区位图。 此方法在线程安全的原子操作中交换这些引用。 交换后，以前的后台缓冲区图像现在位于 *softwareBitmap* 变量中，释放该图像可以清理资源。

接下来，使用与 **Image** 元素关联的 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher) 通过调用 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 创建在 UI 线程上运行的任务。 由于异步任务将在任务中执行，因此传递到 **RunAsync** 的 lambda 表达式使用 *async* 关键字声明。

在任务中，查看 *_taskRunning* 变量以确保一次只运行任务的一个实例。 如果尚未运行该任务，将 *_taskRunning* 设置为 true，以防止任务再次运行。 在 *while* 循环中，调用 **Interlocked.Exchange** 以将后台缓冲区的内容复制到临时 **SoftwareBitmap**，直到后台缓冲区图像为 null。 每次在填充临时位图时，**Image** 的 **Source** 属性转换为 **SoftwareBitmapSource**，然后调用 [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856) 以设置图像源。

最后，*_taskRunning* 变量重新设置为 false，以便下次调用处理程序时任务会重新运行。

> [!NOTE] 
> 如果访问 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.VideoMediaFrame.SoftwareBitmap) 或 [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.VideoMediaFrame.Direct3DSurface) 对象（由 [**MediaFrameReference**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference.VideoMediaFrame) 的 [**VideoMediaFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference) 属性提供），系统将创建对这些对象的强引用，这意味着，当在包含的 **MediaFrameReference** 上调用 [**Dispose**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference.Close) 时，它们将不会释放。 必须直接为要立即释放的对象显式调用 **SoftwareBitmap** 或 **Direct3DSurface** 的 **Dispose** 方法。 否则，垃圾回收器将最终为这些对象释放内存，但无法知道这将何时出现，并且如果分配的位图或曲面的数量超过系统所允许的最大量，将停止新帧的流程。


[!code-cs[FrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFrameArrived)]

## <a name="cleanup-resources"></a>清理资源
在读取帧完成后，通过调用 [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.StopAsync)、注销 **FrameArrived** 处理程序，并释放 **MediaCapture** 对象，确保停止媒体帧阅读器。

[!code-cs[Cleanup](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCleanup)]

有关在挂起应用程序时清理媒体捕获对象的详细信息，请参阅[**显示相机预览**](simple-camera-preview-access.md)。

## <a name="the-framerenderer-helper-class"></a>FrameRenderer 帮助程序类
通用 Windows [相机帧示例](http://go.microsoft.com/fwlink/?LinkId=823230)提供在应用中轻松显示彩色、红外和深度源的帧的帮助程序类。 通常，对深度和红外数据的操作不止将其显示到屏幕，因此这个帮助程序类是显示帧阅读器功能和调试帧阅读器实现的有用工具。

**FrameRenderer** 帮助程序类实现以下方法。

* **FrameRenderer** 构造函数 - 此构造函数初始化帮助程序类以将传递的 XAML [**Image**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Image) 元素用于显示媒体帧。
* **ProcessFrame** - 此方法在传递到构造函数的 **Image** 元素中显示由 [**MediaFrameReference**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference) 表示的媒体帧。 通常应该从 [**FrameArrived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.FrameArrived) 事件处理程序（传递到 [**TryAcquireLatestFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.TryAcquireLatestFrame) 返回的帧）中调用此方法。
* **ConvertToDisplayableImage** - 此方法检查媒体帧的格式，并在需要时将其转换为可显示的格式。 对于彩色图像，这意味着可以确保颜色格式为 BGRA8，并且位图 alpha 模式已预乘。 对于深度或红外帧，每个扫描行都经过处理，以使用也包含在示例中并且在下面列出的 **PsuedoColorHelper** 类将深度或红外值转换为伪色渐变。

> [!NOTE] 
> 为了在 **SoftwareBitmap** 图像上操作像素，必须访问本机内存缓冲区。 为此，必须使用包括在以下代码列表中的 IMemoryBufferByteAccess COM 接口，并且必须更新项目属性，以支持编译不安全代码。 有关详细信息，请参阅[创建、编辑和保存位图图像](imaging.md)。

[!code-cs[FrameArrived](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetFrameRenderer)]

## <a name="related-topics"></a>相关主题

* [相机](camera.md)
* [使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [相机帧示例](http://go.microsoft.com/fwlink/?LinkId=823230)
 

 







<!--HONumber=Dec16_HO3-->



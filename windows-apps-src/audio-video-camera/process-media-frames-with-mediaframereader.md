---
ms.assetid: a128edc8-8a80-4645-ac29-908ede2d1c72
description: 本文介绍如何将 MediaFrameReader 与 MediaCapture 结合使用，以获取一个或多个可用源提供的媒体帧，这些可用源包括彩色、深度、红外线相机，音频设备，甚至是自定义的帧源（例如生成骨架跟踪帧的帧源）。
title: 使用 MediaFrameReader 处理媒体帧
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1e9db0960070c77485fbe8b2f3231f7ce8035b5c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361484"
---
# <a name="process-media-frames-with-mediaframereader"></a>使用 MediaFrameReader 处理媒体帧

本文介绍如何将 [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) 与 [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) 结合使用，以获取一个或多个可用源提供的媒体帧，这些可用源包括彩色、深度、红外线相机，音频设备，甚至是自定义的帧源（例如生成骨架跟踪帧的帧源）。 此功能旨在由执行实时处理媒体帧的应用使用，例如增强现实和感知深度的相机应用。

如果你对仅捕获视频或照片感兴趣（例如典型的摄影应用），则可能希望使用 [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) 支持的其他捕获技术。 有关可用的媒体捕获技术列表和演示如何使用这些技术的文章，请参阅[**相机**](camera.md)。

> [!NOTE] 
> 本文中讨论的功能仅从 Windows 10 版本 1607 开始提供。

> [!NOTE] 
> 文中提供了一个通用 Windows 应用示例，介绍使用 **MediaFrameReader** 显示不同帧源（包括彩色、深度和红外相机）的帧。 有关详细信息，请参阅[相机帧示例](https://go.microsoft.com/fwlink/?LinkId=823230)。

> [!NOTE] 
> Windows 10 版本 1803 中采用了一组新的 API，借助其可将 **MediaFrameReader** 与音频数据结合使用。 有关详细信息，请参阅[使用 MediaFrameReader 处理音频帧](process-audio-frames-with-mediaframereader.md)。


## <a name="setting-up-your-project"></a>设置项目
对于使用 **MediaCapture** 的任何应用，在尝试访问任何相机设备前都必须声明应用使用 *webcam* 功能。 如果应用从音频设备捕获音频，还应声明 *microphone* 设备功能。 

**将功能添加到应用程序清单**

1.  在 Microsoft Visual Studio 的“解决方案资源管理器”中，通过双击“package.appxmanifest”项，打开应用程序清单的设计器。  
2.  选择**功能**选项卡。
3.  选中“摄像头”框和“麦克风”框。  
4.  若要访问图片库和视频库，请选中“图片库”框和“视频库”框。  

除了默认项目模板包含的这些 API，本文的示例代码还使用来自以下命名空间的 API。

[!code-cs[FramesUsing](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFramesUsing)]

## <a name="select-frame-sources-and-frame-source-groups"></a>选择帧源和帧源组
处理媒体帧的许多应用都需要同时从多个源获取帧，例如设备的彩色和深度相机。 [ **MediaFrameSourceGroup** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup)对象都表示一组可同时使用的媒体帧源。 调用静态方法 [**MediaFrameSourceGroup.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync) 获取当前设备支持的所有帧源组列表。

[!code-cs[FindAllAsync](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFindAllAsync)]

此外可以创建[ **DeviceWatcher** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)使用[ **DeviceInformation.CreateWatcher** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher) 从返回的值和[ **MediaFrameSourceGroup.GetDeviceSelector** ](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector)可用帧源分组的设备更改，例如当外部照相机已接通电源时接收通知。 有关详细信息，请参阅[**枚举设备**](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices)。

[  **MediaFrameSourceGroup**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) 具有 [**MediaFrameSourceInfo**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceInfo) 对象集合，用于描述组内包括的帧源。 检索可在设备上使用的帧源组后，可以选择公开所关注帧源的组。

以下示例展示了选择帧源组的最简方法。 此代码只是循环访问所有可用组，然后循环访问 [**SourceInfos**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.sourceinfos) 集合中的每个项目。 检查每个 **MediaFrameSourceInfo**，了解相应项目是否支持我们正在寻找的功能。 在本例中，检查 [**MediaStreamType**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.mediastreamtype) 属性以查看 [**VideoPreview**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaStreamType) 值（该值表示设备提供视频预览流），并且检查 [**SourceKind**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.sourcekind) 属性以查看 [**Color**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceKind) 值（该值指示源提供颜色帧）。

[!code-cs[SimpleSelect](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSimpleSelect)]

这种标识所需帧源组和帧源的方法对简单情况有效，但如果要依据更复杂的条件选择帧源，这会很快变得难以实现。 另一种方法是使用 Linq 语法和匿名对象进行选择。 以下示例使用 **Select** 扩展方法将 *frameSourceGroups* 列表中的 **MediaFrameSourceGroup** 对象转换为具有两个字段的匿名对象：*sourceGroup* 表示组本身，*colorSourceInfo* 表示组中的颜色帧源。 *colorSourceInfo* 字段设为 **FirstOrDefault** 的结果，后者用于选择所提供的谓词解析为 true 的第一个对象。 在本例中，如果流类型为 **VideoPreview**、源种类为 **Color**，并且相机是设备的前置相机，则谓词为 true。

在从上述查询返回的匿名对象列表中，**Where** 扩展方法用于仅选择 *colorSourceInfo* 字段不为 null 的对象。 最后，调用 **FirstOrDefault** 以选择列表中的第一个项目。

此时，可以使用所选对象的字段获取所选 **MediaFrameSourceGroup** 和表示彩色相机的 **MediaFrameSourceInfo** 对象的引用。 这些引用将在稍后用于初始化 **MediaCapture** 对象并为所选源创建 **MediaFrameReader**。 最后应进行测试，以查看源组是否为 null，如果是，则意味着当前设备没有所请求的捕获源。

[!code-cs[SelectColor](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSelectColor)]

以下示例使用与上述技术相类似的技术选择包含彩色、深度和红外相机的源组。

[!code-cs[ColorInfraredDepth](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetColorInfraredDepth)]

> [!NOTE]
> 从 Windows 10 版本 1803 开始，可以通过一组所需的功能使用 [**MediaCaptureVideoProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile) 类选择媒体帧源。 有关详细信息，请参阅本文后面的**使用视频配置文件选择帧源**部分。


## <a name="initialize-the-mediacapture-object-to-use-the-selected-frame-source-group"></a>初始化 MediaCapture 对象以使用所选帧源组
下一步是初始化 **MediaCapture** 对象以使用在上一步中所选的帧源组。

**MediaCapture** 对象通常在应用的多个位置中使用，因此应声明某个类成员变量以保留它。

[!code-cs[DeclareMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

通过调用构造函数创建 **MediaCapture** 对象实例。 接下来，创建用于初始化 **MediaCapture** 对象的 [**MediaCaptureSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureSettings) 对象。 本示例使用以下设置：

* [**组**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.sourcegroup) -这会告诉你将用于获取帧的源组系统。 请记住，源组定义可以同时使用的一组媒体帧源。
* [**SharingMode** ](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.sharingmode) -这将告知系统是否需要捕获源设备的独有控制。 如果将它设置为 [**ExclusiveControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureSharingMode)，意味着可以更改捕获设备的设置（例如它生成的帧格式），但也意味着如果其他应用已经拥有独占控制权，你的应用在尝试初始化媒体捕获设备时会失败。 如果将它设置为 [**SharedReadOnly**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureSharingMode)，则可以检索帧源的帧，即使其他应用已经在使用这些帧也是如此，但无法更改设备设置。
* [**MemoryPreference** ](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.memorypreference) -如果你指定[ **CPU**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureMemoryPreference)，则系统将使用 CPU 内存用于保证，帧到达时，它们可用作[ **SoftwareBitmap** ](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)对象。 如果指定 [**Auto**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureMemoryPreference)，系统将动态选择存储帧的最佳内存位置。 如果系统选择使用 GPU 内存，媒体帧将作为 [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) 对象（而非 **SoftwareBitmap** 对象）到达。
* [**StreamingCaptureMode** ](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.streamingcapturemode) -将此设置为[**视频**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.StreamingCaptureMode)以指示不需要的音频进行流式处理。

调用 [**InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync) 以使用所需设置初始化 **MediaCapture**。 请确保在 *try* 块中调用此函数，防止初始化失败。

[!code-cs[InitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitMediaCapture)]

## <a name="set-the-preferred-format-for-the-frame-source"></a>为帧源设置首选格式
若要为帧源设置首选格式，需要获取表示源的 [**MediaFrameSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource) 对象。 通过访问已初始化的 **MediaCapture** 对象的 [**Frames**](https://docs.microsoft.com/previous-versions/windows/apps/phone/jj207578(v=win.10)) 字典，并指定希望使用的帧源的标识符，获取此对象。 这就是我们在选择帧源组时保存 [**MediaFrameSourceInfo**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceInfo) 对象的原因。

[  **MediaFrameSource.SupportedFormats**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.supportedformats) 属性包含 [**MediaFrameFormat**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameFormat) 对象列表，此类对象用于描述帧源的受支持格式。 使用 **Where** Linq 扩展方法以根据所需属性选择某种格式。 在本例中，选择了宽度为 1080 像素并且可以提供 32 位 RGB 格式的帧格式。 **FirstOrDefault** 扩展方法选择列表中的第一个条目。 如果所选格式为 null，则请求的格式不受帧源支持。 如果该格式受支持，可以请求源通过调用 [**SetFormatAsync**](https://developer.microsoft.com/windows/apps/develop) 来使用此格式。

[!code-cs[GetPreferredFormat](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetPreferredFormat)]

## <a name="create-a-frame-reader-for-the-frame-source"></a>为帧源创建帧阅读器
若要为媒体帧源接收帧，请使用 [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader)。

[!code-cs[DeclareMediaFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareMediaFrameReader)]

通过在已初始化的 **MediaCapture** 对象上调用 [**CreateFrameReaderAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync)，实例化帧阅读器。 此方法的第一个参数是你希望从中接收帧的帧源。 可以为每个要使用的帧源创建单独的帧阅读器。 第二个参数告知系统你希望帧到达时使用的输出格式。 这可以省去在帧到达时需要自行转换到帧的工作。 请注意，如果所指定的格式不受帧源支持，将引发异常，因此请确保该值在 [**SupportedFormats**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.supportedformats) 集合中。  

创建帧阅读器后，请为源提供新帧时引发的 [**FrameArrived**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) 事件注册处理程序。

调用 [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.startasync) 以告知系统开始读取源的帧。

[!code-cs[CreateFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCreateFrameReader)]

## <a name="handle-the-frame-arrived-event"></a>处理帧到达事件
新帧可用时即引发 [**MediaFrameReader.FrameArrived**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) 事件。 可以选择处理到达的每个帧，或者仅在需要时使用它们。 因为帧阅读器在自己的线程上引发事件，所以可能需要实现某些同步逻辑，以确保你不会尝试从多个线程访问相同数据。 本节介绍如何在 XAML 页面中将图形颜色帧同步到图像控件。 这套方案解决需要在 UI 线程上执行 XAML 控件的所有更新的其他同步约束。

在 XAML 中显示帧的第一步是创建 Image 控件。 

[!code-xml[ImageElementXAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetImageElementXAML)]

在代码隐藏页面中，声明 **SoftwareBitmap** 类型的类成员变量，该变量用作所有传入图像都会复制到的后台缓冲区。 请注意，图像数据本身不会复制，复制的仅是对象引用。 此外，声明某个布尔值以跟踪 UI 操作当前是否正在运行。

[!code-cs[DeclareBackBuffer](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareBackBuffer)]

由于帧将作为 **SoftwareBitmap** 对象到达，因此需要创建 [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) 对象，该对象支持将 **SoftwareBitmap** 用作 XAML **Control** 的源。 在启动帧阅读器前，应该在代码的某个位置设置图像源。

[!code-cs[ImageElementSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetImageElementSource)]

此时可以实现 **FrameArrived** 事件处理程序。 调用处理程序时，*sender* 参数包含对引发该事件的 **MediaFrameReader** 对象的引用。 调用此对象上的 [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe) 以尝试获取最新帧。 如名称所示，**TryAcquireLatestFrame** 可能不会成功返回帧。 因此，当依次访问 VideoMediaFrame 和 SoftwareBitmap 属性时，请确保测试它们是否为 null。 在本示例中，null 条件运算符 用于访问 **SoftwareBitmap**，然后检查检索的对象是否为 null。

**Image** 控件仅可以显示格式为 BRGA8 并且已经预乘或者没有 alpha 的图像。 如果到达的帧不是这种格式，则使用静态方法 [**Convert**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.windows) 将软件位图转换为正确的格式。

接下来，使用 [**Interlocked.Exchange**](https://docs.microsoft.com/dotnet/api/system.threading.interlocked.exchange?redirectedfrom=MSDN#System_Threading_Interlocked_Exchange__1___0____0_) 方法来将到达位图的引用交换为后台缓冲区位图。 此方法在线程安全的原子操作中交换这些引用。 交换后，以前的后台缓冲区图像现在位于 *softwareBitmap* 变量中，释放该图像可以清理资源。

接下来，使用与 **Image** 元素关联的 [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) 通过调用 [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.windows) 创建在 UI 线程上运行的任务。 由于异步任务将在任务中执行，因此传递到 **RunAsync** 的 lambda 表达式使用 *async* 关键字声明。

在任务中，查看 *_taskRunning* 变量以确保一次只运行任务的一个实例。 如果尚未运行该任务，将 *_taskRunning* 设置为 true，以防止任务再次运行。 在 *while* 循环中，调用 **Interlocked.Exchange** 以将后台缓冲区的内容复制到临时 **SoftwareBitmap**，直到后台缓冲区图像为 null。 每次在填充临时位图时，**Image** 的 **Source** 属性转换为 **SoftwareBitmapSource**，然后调用 [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) 以设置图像源。

最后， *_taskRunning* 变量重新设置为 false，以便下次调用处理程序时任务会重新运行。

> [!NOTE] 
> 如果访问 [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.videomediaframe.softwarebitmap) 或 [**Direct3DSurface**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.videomediaframe.direct3dsurface) 对象（由 [**MediaFrameReference**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference.videomediaframe) 的 [**VideoMediaFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReference) 属性提供），系统将创建对这些对象的强引用，这意味着，当在包含的 **MediaFrameReference** 上调用 [**Dispose**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference.close) 时，它们将不会释放。 必须直接为要立即释放的对象显式调用 **SoftwareBitmap** 或 **Direct3DSurface** 的 **Dispose** 方法。 否则，垃圾回收器将最终为这些对象释放内存，但无法知道这将何时出现，并且如果分配的位图或曲面的数量超过系统所允许的最大量，将停止新帧的流程。 可以拷贝检索的帧（如使用 [**SoftwareBitmap.Copy**](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.imaging.softwarebitmap.copy) 方法），然后释放原始帧来解决此限制问题。 另外，如果使用重载 [CreateFrameReaderAsync(Windows.Media.Capture.Frames.MediaFrameSource inputSource, System.String outputSubtype, Windows.Graphics.Imaging.BitmapSize outputSize)](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_System_String_Windows_Graphics_Imaging_BitmapSize_) 或 [CreateFrameReaderAsync(Windows.Media.Capture.Frames.MediaFrameSource inputSource, System.String outputSubtype)](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_System_String_) 创建 **MediaFrameReader**，则返回的帧为原始帧数据的副本，因此在保留这些帧时不会导致帧采集停止。 


[!code-cs[FrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFrameArrived)]

## <a name="cleanup-resources"></a>清理资源
在读取帧完成后，通过调用 [**StopAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.stopasync)、注销 **FrameArrived** 处理程序，并释放 **MediaCapture** 对象，确保停止媒体帧阅读器。

[!code-cs[Cleanup](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCleanup)]

有关在挂起应用程序时清理媒体捕获对象的详细信息，请参阅[**显示相机预览**](simple-camera-preview-access.md)。

## <a name="the-framerenderer-helper-class"></a>FrameRenderer 帮助程序类
通用 Windows [相机帧示例](https://go.microsoft.com/fwlink/?LinkId=823230)提供在应用中轻松显示彩色、红外和深度源的帧的帮助程序类。 通常，对深度和红外数据的操作不止将其显示到屏幕，因此这个帮助程序类是显示帧阅读器功能和调试帧阅读器实现的有用工具。

**FrameRenderer** 帮助程序类实现以下方法。

* **FrameRenderer** 构造函数 - 此构造函数初始化帮助程序类以将传递的 XAML [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) 元素用于显示媒体帧。
* **ProcessFrame** - 此方法在传递到构造函数的 **Image** 元素中显示由 [**MediaFrameReference**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReference) 表示的媒体帧。 通常应该从 [**FrameArrived**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) 事件处理程序（传递到 [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe) 返回的帧）中调用此方法。
* **ConvertToDisplayableImage** - 此方法检查媒体帧的格式，并在需要时将其转换为可显示的格式。 对于彩色图像，这意味着可以确保颜色格式为 BGRA8，并且位图 alpha 模式已预乘。 对于深度或红外帧，每个扫描行都经过处理，以使用也包含在示例中并且在下面列出的 **PsuedoColorHelper** 类将深度或红外值转换为伪色渐变。

> [!NOTE] 
> 为了在 **SoftwareBitmap** 图像上操作像素，必须访问本机内存缓冲区。 为此，必须使用包括在以下代码列表中的 IMemoryBufferByteAccess COM 接口，并且必须更新项目属性，以支持编译不安全代码。 有关详细信息，请参阅[创建、编辑和保存位图图像](imaging.md)。

[!code-cs[IMemoryBufferByteAccess](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetIMemoryBufferByteAccess)]

[!code-cs[FrameArrived](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetFrameRenderer)]

## <a name="use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources"></a>使用 MultiSourceMediaFrameReader 从多个源获取与时间关联的帧
从 Windows 10 版本 1607 开始，你可以使用 [**MultiSourceMediaFrameReader**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader) 从多个源接收与时间关联的帧。 此 API 便于更加轻松地执行在接近时间获取的多个源的帧的处理，例如使用 [**DepthCorrelatedCoordinateMapper**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.depthcorrelatedcoordinatemapper) 类。 使用此新方法的一个限制是，只有以最慢的捕获源速率，才能引发帧到达事件。 来自更快的源的额外帧将被丢弃。 此外，由于系统预计来自不同源的帧以不同的速率到达，因此它不会自动识别一个源是否已完全停止生成帧。 此部分的示例代码显示如何使用事件自行创建在关联的帧在应用定义的时间限制内未到达时调用的超时逻辑。

使用 [**MultiSourceMediaFrameReader**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader) 的步骤与本文前面所述的使用 [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) 的步骤类似。 此示例中将使用色源和深度源。 声明一些字符串变量以存储将被用来从每个源选择帧的媒体帧源 ID。 下一步，声明将用来实施示例中的超时逻辑的 [**ManualResetEventSlim**](https://docs.microsoft.com/dotnet/api/system.threading.manualreseteventslim?view=netframework-4.7)、[**CancellationTokenSource**](https://docs.microsoft.com/dotnet/api/system.threading.cancellationtokensource?redirectedfrom=MSDN) 和 [**EventHandler**](https://docs.microsoft.com/dotnet/api/system.eventhandler?redirectedfrom=MSDN)。 

[!code-cs[MultiFrameDeclarations](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameDeclarations)]

使用本文前面所述的方法，查询包含此示例方案所需的颜色和深度源的 [**MediaFrameSourceGroup**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup)。 选择所需的帧源组后，获取每个帧源的 [**MediaFrameSourceInfo**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceInfo)。

[!code-cs[SelectColorAndDepth](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSelectColorAndDepth)]

创建并初始化 **MediaCapture** 对象，在初始化设置中传递所选的帧源组。

[!code-cs[MultiFrameInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameInitMediaCapture)]

初始化 **MediaCapture** 对象后，检索彩色和深度相机的 [**MediaFrameSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource) 对象。 存储每个源的 ID，以便可以选择相应源的到达帧。

[!code-cs[GetColorAndDepthSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetColorAndDepthSource)]

通过调用 [**CreateMultiSourceFrameReaderAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createmultisourceframereaderasync) 和传递阅读器将使用的一组帧源创建并初始化 **MultiSourceMediaFrameReader**。 为 [**FrameArrived**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader.FrameArrived) 事件注册事件处理程序。 此示例创建本文前面所述的 **FrameRenderer** 帮助程序类的实例，以将帧呈现给**图像**控件。 通过调用 [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader.StartAsync) 启动帧阅读器。

为在此示例前面所声明的 **CorellationFailed** 事件注册事件处理程序。 如果正在使用的其中一个媒体帧源停止生成帧，我们将发出此事件的信号。 最后调用 [**Task.Run**](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task.run?redirectedfrom=MSDN#System_Threading_Tasks_Task_Run_System_Action_) 以在单独的线程上调用超时帮助程序方法 **NotifyAboutCorrelationFailure**。 本文稍后将展示此方法的实现方式。

[!code-cs[InitMultiFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitMultiFrameReader)]

每当由 **MultiSourceMediaFrameReader** 管理的所有媒体帧源都提供可用的新帧时，引发 **FrameArrived** 事件。 这意味着事件将按照最慢的媒体源的节奏引发。 如果一个源在较慢的源生成一个帧的时间内生成多个帧，则来自快源的额外帧将被丢弃。 

通过调用 [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader.TryAcquireLatestFrame) 获取与事件关联的 [**MultiSourceMediaFrameReference**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereference)。 通过调用 [**TryGetFrameReferenceBySourceId**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereference.trygetframereferencebysourceid) 获取与每个媒体帧源相关联的 **MediaFrameReference**，同时在初始化帧阅读器时在存储的 ID 字符串中传递。

调用 **ManualResetEventSlim** 对象的[**设置**](https://docs.microsoft.com/dotnet/api/system.threading.manualreseteventslim.set?redirectedfrom=MSDN#System_Threading_ManualResetEventSlim_Set) 方法以发出帧已到达的信号。 我们将在单独的线程中运行的 **NotifyCorrelationFailure** 方法中检查此事件。 

最后，对与时间关联的媒体帧执行任何处理。 此示例仅显示来自深度源的帧。

[!code-cs[MultiFrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameArrived)]

在启动帧阅读器后，**NotifyCorrelationFailure** 帮助程序方法在单独的线程上运行。 在此方法中，检查以查看是否已发出帧已收到事件的信号。 请记住，在 **FrameArrived** 处理程序中，每当一组关联的帧到达时，我们都将设置此事件。 如果在某些应用定义的时间期限（5 秒是合理的值）内未发出事件信号，且未使用 **CancellationToken** 取消任务，则有可能其中一个媒体帧源已停止阅读帧。 在这种情况下，你通常需要关闭帧阅读器，以便引发应用定义的 **CorrelationFailed** 事件。 在此事件的处理程序中，你可以按照本文前面所述停止帧阅读器并清理与它关联的资源。

[!code-cs[NotifyCorrelationFailure](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetNotifyCorrelationFailure)]

[!code-cs[CorrelationFailure](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCorrelationFailure)]

## <a name="use-buffered-frame-acquisition-mode-to-preserve-the-sequence-of-acquired-frames"></a>使用缓冲的帧采集模式保留获取的帧的序列
从 Windows 10 版本 1709 开始，可以将 **MediaFrameReader** 或 **MultiSourceMediaFrameReader** 的 **[AcquisitionMode](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.AcquisitionMode)** 属性设置为 **Buffered** 来保留从帧源传入应用的帧的序列。

[!code-cs[SetBufferedFrameAcquisitionMode](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSetBufferedFrameAcquisitionMode)]

在默认采集模式 **Realtime** 下，当应用仍在处理前一帧的 **FrameArrived** 事件时，如果从源采集了多个帧，系统将向应用发送最新采集的帧并删除在缓冲区中等待的其他帧。 这样可以始终向应用提供最新可用的帧。 对于实时计算机视觉应用程序，这通常是最有用的模式。 

在 **Buffered** 采集模式下，系统将所有帧保留在缓冲区中，并按照帧的接收顺序，通过 **FrameArrived** 事件将帧提供给应用。 请注意，在此模式下，当用于保留帧的系统缓冲区填满后，系统将停止采集新的帧，直至应用完成前面的帧的 **FrameArrived** 事件并在缓冲区中释放更多空间为止。

## <a name="use-mediasource-to-display-frames-in-a-mediaplayerelement"></a>使用 MediaSource 在 MediaPlayerElement 中显示帧
从 Windows 版本 1709 开始，可以在 XAML 页面的 **[MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement)** 控件中直接显示从 **MediaFrameReader** 采集的帧。 实现此操作的方法是使用 **[MediaSource.CreateFromMediaFrameSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediaframesource)** 创建 **[MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)** 对象，且与 **MediaPlayerElement** 关联的 **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** 可直接使用该对象。 有关使用 **MediaPlayer** 和 **MediaPlayerElement** 的详细信息，请参阅[使用 MediaPlayer 播放音频和视频](play-audio-and-video-with-mediaplayer.md)。

以下代码示例演示了一个简单实现：在 XAML 页面中同时显示前置和后置相机的帧。

首先，将两个 **MediaPlayerElement** 控件添加到 XAML 页面。

[!code-xml[MediaPlayerElement1XAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetMediaPlayerElement1XAML)]

[!code-xml[MediaPlayerElement2XAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetMediaPlayerElement2XAML)]

然后，使用前文中所介绍的技术为前置和后置彩色相机选择包含 **MediaFrameSourceInfo** 对象的 **MediaFrameSourceGroup**。 请注意，**MediaPlayer** 不会自动将帧从非颜色格式（如深度或红外线数据）转换成颜色数据。 使用其他传感器类型可能会产生意外结果。 

[!code-cs[MediaSourceSelectGroup](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMediaSourceSelectGroup)]

初始化 **MediaCapture** 对象以使用所选的 **MediaFrameSourceGroup**。

[!code-cs[MediaSourceInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMediaSourceInitMediaCapture)]

最后，通过使用关联的 **MediaFrameSourceInfo** 对象的 **[Id](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.Id)** 属性，在 **MediaCapture** 对象的 **[FrameSources](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.FrameSources)** 集合中选择一个帧源，来调用 **[MediaSource.CreateFromMediaFrameSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediaframesource)** ，为每个帧源创建一个 **MediaSource**。 通过调用 **[SetMediaPlayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.MediaPlayer)** 初始化新的 **MediaPlayer** 对象并将其分配给 **MediaPlayerElement**。 然后，将 **[Source](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Source)** 属性设置为新创建的 **MediaSource** 对象。

[!code-cs[MediaSourceMediaPlayer](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMediaSourceMediaPlayer)]

## <a name="use-video-profiles-to-select-a-frame-source"></a>使用视频配置文件选择帧源

由 [**MediaCaptureVideoProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile) 对象表示的相机配置文件表示特定捕获设备所提供的一组功能，如帧速率、分辨率或 HDR 捕获这类的高级功能。 捕获设备可支持多个配置文件，这样用户便可选择最适合所使用的捕获方案的配置文件。 从 Windows 10 版本 1803 开始，可借助特定功能，先使用 **MediaCaptureVideoProfile** 来选择媒体帧源，然后再初始化 **MediaCapture** 对象。 以下示例方法查找支持 HDR 和广色域 (WCG) 的视频配置文件，然后返回可用于初始化 **MediaCapture** 的 **MediaCaptureInitializationSettings** 对象，以使用所选的设备和配置文件。

首先，通过调用 [**MediaFrameSourceGroup.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync) 获取当前设备上可用的所有媒体帧源组的列表。 循环访问每个源组，并通过调用 [**MediaCapture.FindKnownVideoProfiles**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) 获取当前源组中支持特定配置文件（本例中为带 WCG 照片的 HDR）的所有视频配置文件的列表。 如果找到符合条件的配置文件，则新建一个 **MediaCaptureInitializationSettings** 对象，将 **VideoProfile** 设置为选择配置文件并将 **VideoDeviceId** 设置为当前媒体帧源组的 **Id** 属性。

[!code-cs[GetSettingsWithProfile](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetSettingsWithProfile)]

有关使用相机配置文件的详细信息，请参阅[相机配置文件](camera-profiles.md)。

## <a name="related-topics"></a>相关主题

* [摄像头](camera.md)
* [基本的照片、 视频和音频捕获与 MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [相机帧示例](https://go.microsoft.com/fwlink/?LinkId=823230)
 

 





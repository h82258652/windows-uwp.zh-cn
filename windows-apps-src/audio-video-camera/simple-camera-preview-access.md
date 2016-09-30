---
author: drewbatgit
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: "本文描述了如何在通用 Windows 平台 (UWP) 应用的 XAML 页面内快速显示相机预览流。"
title: "简单相机预览访问"
translationtype: Human Translation
ms.sourcegitcommit: 72abc006de1925c3c06ecd1b78665e72e2ffb816
ms.openlocfilehash: 05e752925c07b0e3720fbdd42d785381aa08b99c

---

# 简单相机预览访问

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本文描述了如何在通用 Windows 平台 (UWP) 应用的 XAML 页面内快速显示相机预览流。 创建可使用相机捕获照片和视频的应用需要你执行如下任务：处理设备和相机方向或者设置捕获文件的编码选项。 有关某些应用方案，可能只需显示来自相机的预览流，而无需担心其他注意事项。 本文将向你显示如何使用最少的代码执行相关操作。 请注意，在你使用它按照下列步骤完成操作时，应当始终正确关闭预览流。

有关编写可捕获照片或视频的相机应用的信息，请参阅[使用 MediaCapture 捕获照片和视频](capture-photos-and-video-with-mediacapture.md)。

## 向应用清单中添加功能声明

为了让你的应用可以访问设备的相机，必须声明你的应用要使用 *webcam* 和 *microphone* 设备功能。 如果你想要将已捕获的照片和视频保存到用户的图片库或视频库，你还必须声明 *picturesLibrary* 和 *videosLibrary* 功能。

**将功能添加到应用清单**

1.  在 Microsoft Visual Studio 的“解决方案资源管理器”****中，通过双击“package.appxmanifest”****项，打开应用程序清单的设计器。
2.  选择“功能”****选项卡。
3.  选中“摄像头”****框和“麦克风”****框。
4.  若要访问图片库和视频库，请选中“图片库”****框和“视频库”****框。

## 将 CaptureElement 添加到你的页面

使用 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) 以在你的 XAML 页面内显示预览流。

[!code-xml[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]

## 使用 MediaCapture 启动预览流

[**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 对象为你的设备相机的应用界面。 此类是 Windows.Media.Capture 命名空间的成员。 除了默认项目模板包含的这些 API，本文的示例还使用来自 [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691) 和 [System.Threading.Tasks](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.aspx) 命名空间的 API。

添加 using 指令以将以下命名空间包含在你的页面的 .cs 文件中。

[!code-cs[SimpleCameraPreviewUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSimpleCameraPreviewUsing)]

声明 **MediaCapture** 对象的类变量。

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

创建 **MediaCapture** 类的一个新实例并调用 [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) 以初始化捕获设备。 例如，此方法在没有相机的设备上可能会失败，所以你应当从 **try** 块内调用它。 如果用户已在该设备的隐私设置中禁用相机访问，则在尝试初始化相机时会引发 **UnauthorizedAccessException**。 如果你忘记将合适的功能添加到你的应用清单，在开发期间也会看到此异常。

**重要提示** 对于某些设备系列，在授予应用访问设备相机的权限前，会向用户显示用户同意提示。 出于此原因，必须从主 UI 线程仅调用 [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598)。 尝试从其他线程初始化相机可能会导致初始化失败。

通过设置 [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280) 属性来将 **MediaCapture** 连接到 **CaptureElement**。 最后，通过调用 [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613) 启动预览。

[!code-cs[StartPreviewAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]


## 关闭预览流

当你使用预览流完成操作时，应当始终关闭该流并适当地释关联资源，以确保该相机可用于设备上的其他应用。 关闭预览流所需的步骤如下：

-   调用 [**StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622) 以停止预览流。
-   将 **CaptureElement** 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280) 属性设置为 null。
-   调用 **MediaCapture** 对象的 [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858) 方法以发布该对象。
-   将 **MediaCapture** 成员变量设置为 null。

[!code-cs[CleanupCameraAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

当用户通过替代 [**OnNavigatedFrom**](https://msdn.microsoft.com/library/windows/apps/br227507) 方法导航离开你的页面时，应当关闭预览流。

[!code-cs[OnNavigatedFrom](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

你还应当在应用暂停时正确关闭预览流。 若要执行此操作，请在你的页面的构造函数中注册 [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) 事件的处理程序。

[!code-cs[RegisterSuspending](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterSuspending)]

在 **Suspending** 事件处理程序中，首先进行查看以通过比较 [**CurrentSourcePageType**](https://msdn.microsoft.com/library/windows/apps/hh702390) 属性的页面类型，确保应用程序的 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 正显示该页面。 如果当前未显示该页面，则应当已引发 **OnNavigatedFrom** 事件并关闭预览流。 如果当前显示该页面，从已传入到处理程序中的事件参数中获取 [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) 对象，以确保直到关闭预览流系统才暂停你的应用。 关闭流后，调用延迟的 [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) 方法以使系统继续暂停你的应用。

[!code-cs[SuspendingHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSuspendingHandler)]

## 从预览流捕获静止图像

只需以 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 形式从媒体捕获预览流获取静止图像。 有关详细信息，请参阅[获取预览帧](get-a-preview-frame.md)。

## 相关主题

* [使用 MediaCapture 捕获照片和视频](capture-photos-and-video-with-mediacapture.md)
* [获取预览帧](get-a-preview-frame.md)



<!--HONumber=Jun16_HO4-->



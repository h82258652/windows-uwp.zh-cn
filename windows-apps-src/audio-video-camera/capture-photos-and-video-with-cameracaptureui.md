---
author: drewbatgit
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: 本文介绍如何使用 CameraCaptureUI 类来使用内置于 Windows 的相机 UI 捕获照片或视频。
title: 使用 Windows 内置相机 UI 捕获照片和视频
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3ba33a1e79a2447c5dac546ce0f1caeaf16929a3
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "4020627"
---
# <a name="capture-photos-and-video-with-windows-built-in-camera-ui"></a>使用 Windows 内置相机 UI 捕获照片和视频



本文介绍如何通过 CameraCaptureUI 类使用 Windows 内置的相机 UI 捕获照片或视频。 此功能易于使用，并允许你的应用只需几行代码即可获取用户捕获的照片或视频。

如果你希望提供你自己的相机 UI 或者你的方案需要对捕获操作进行更可靠的低级别控制，你应使用 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 对象并实现自己的捕获体验。 有关详细信息，请参阅 [使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)。

> [!NOTE]
> 如果你的应用仅使用 CameraCaptureUI，你不应在应用清单文件中指定**网络摄像头**或**麦克风**功能。 如果指定了这些功能，应用将显示在设备的摄像头隐私设置中，但是即使用户拒绝摄像头访问应用，也不会阻止 CameraCaptureUI 捕获媒体。 这是因为 Windows 内置的摄像头应用是受信任的第一方应用，需要用户按下按钮来启动照片、音频和视频捕获。 你的应用可能无法通过 Windows 应用认证工具包认证提交到应用商店中，如果作为唯一的照片捕获机制使用 CameraCaptureUI 时指定了网络摄像头或麦克风功能时。
> 如果使用 MediaCapture 以编程方式捕获音频、照片或视频，必须指定应用清单文件中的网络摄像头或麦克风功能。

## <a name="capture-a-photo-with-cameracaptureui"></a>使用 CameraCaptureUI 捕获照片

若要使用相机捕获 UI，请将 [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) 命名空间包含在你的项目中。 若要针对返回的图像文件执行文件操作，请包含 [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346)。

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

若要捕获照片，请创建新的 [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 对象。 使用对象的 [**PhotoSettings**](https://msdn.microsoft.com/library/windows/apps/br241058) 属性，你可以为返回的照片指定属性，例如照片的图像格式。 默认情况下，相机捕获 UI 允许用户针对返回之前的照片进行裁剪，尽管可以使用 [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042) 属性禁止此操作。 此示例设置 [**CroppedSizeInPixels**](https://msdn.microsoft.com/library/windows/apps/br241044) 以要求返回的图像像素为 200 x 200。

> [!NOTE]
> 移动设备系列中的设备不支持 **CameraCaptureUI** 中的图像处理剪裁。 在这些设备上运行应用时，将忽略 [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042) 属性的值。

调用 [**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057) 并指定 [**CameraCaptureUIMode.Photo**](https://msdn.microsoft.com/library/windows/apps/br241040) 以指定应捕获的照片。 如果捕获成功，该方法将返回包含该图像的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 实例。 如果用户取消捕获，则返回的对象为 NULL。

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

为包含捕获的照片的 **StorageFile** 提供一个动态生成的名称，并将其保存在应用的本地文件夹中。 若要更好地整理所捕获的照片，可能需要将该文件移动到其他文件夹。

[!code-cs[CopyAndDeletePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCopyAndDeletePhoto)]

若要在应用中使用你的照片，你可能希望创建可以与多个不同的通用 Windows 应用功能结合使用的 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 对象。

首先，应在项目中包含 [**Windows.Graphics.Imaging**](https://msdn.microsoft.com/library/windows/apps/br226400) 命名空间。

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

调用 [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116) 以从图像文件获得一个流。 调用 [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226182) 以获取流的位图解码器。 然后，调用 [**GetSoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887332) 来获取该图像的 **SoftwareBitmap** 表示形式。

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

若要在 UI 中显示该图像，请在 XAML 页面中声明 [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) 控件。

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

若要在 XAML 页面中使用软件位图，请在项目中包含正在使用的 [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258) 命名空间。

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

**Image** 控件要求该图像源是带有预乘 alpha 或没有 alpha 的 BGRA8 格式，因此调用静态方法 [**SoftwareBitmap.Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362) 以使用所需格式创建一个新的软件位图。 接下来，创建一个新的 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) 对象并调用 [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856) 将软件位图分配给该源。 最后，设置 **Image** 控件的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br242760) 属性以显示 UI 中已捕获的照片。

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## <a name="capture-a-video-with-cameracaptureui"></a>使用 CameraCaptureUI 捕获视频

若要捕获视频，请创建新的 [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 对象。 使用对象的 [**VideoSettings**](https://msdn.microsoft.com/library/windows/apps/br241059) 属性，你可以为返回的视频指定属性，例如视频的格式。

调用 [**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057) 并指定 [**Video**](https://msdn.microsoft.com/library/windows/apps/br241059) 以指定应捕获的视频。 如果捕获成功，该方法将返回包含该视频的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 实例。 如果用户取消捕获，则返回的对象为 NULL。

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

根据应用方案处理所捕获视频文件。 本文的其余部分向你介绍了如何从一个或多个已捕获的视频快速创建媒体组合并将其显示在你的 UI 中。

首先，添加 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) 控件，其中的视频组合将在 XAML 页面上显示。

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]


对于从相机捕获 UI 返回的视频文件，通过调用 **[CreateFromStorageFile](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile)** 创建一个新的 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)。 调用与 **MediaPlayerElement** 关联的默认 **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** 的 **[Play](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play)** 方法以播放视频。

[!code-cs[PlayVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetPlayVideo)]
 

## <a name="related-topics"></a>相关主题

* [相机](camera.md)
* [使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 
 

 





---
ms.assetid: 7DBEE5E2-C3EC-4305-823D-9095C761A1CD
description: 本文向你演示如何捕获可变照片序列，允许你快速连续捕获图像的多个帧，并将每个帧配置为使用不同的焦点、闪光灯、ISO、曝光和曝光补偿设置。
title: 可变照片序列
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 506ea5f7fe199c3df0b6089da73a108d53d98ef3
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361379"
---
# <a name="variable-photo-sequence"></a>可变照片序列



本文向你演示如何捕获可变照片序列，允许你快速连续捕获图像的多个帧，并将每个帧配置为使用不同的焦点、闪光灯、ISO、曝光和曝光补偿设置。 此功能启用了创建高动态范围 (HDR) 图像等方案。

如果你想要捕获 HDR 图像，但不想要实现你自己的处理算法，你可以使用 [**AdvancedPhotoCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) API 来使用内置于 Windows 的 HDR 功能。 有关详细信息，请参阅[高动态范围 (HDR) 照片捕获](high-dynamic-range-hdr-photo-capture.md)。

> [!NOTE] 
> 本文以[使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)中讨论的概念和代码为基础，该文章介绍了实现基本照片和视频捕获的步骤。 建议你先熟悉该文中的基本媒体捕获模式，然后再转到更高级的捕获方案。 本文中的代码假设你的应用已有一个正确完成初始化的 MediaCapture 的实例。

## <a name="set-up-your-app-to-use-variable-photo-sequence-capture"></a>设置你的应用以使用可变照片序列捕获

除了基本媒体捕获所需的命名空间，实现可变照片序列捕获还需要以下命名空间。

[!code-cs[VPSUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSUsing)]

声明成员变量以存储 [**VariablePhotoSequenceCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture) 对象，该对象用于启动照片序列捕获。 声明 [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 对象的数组以在序列中存储每个捕获的图像。 此外，声明数组以针对每个帧存储 [**CapturedFrameControlValues**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CapturedFrameControlValues) 对象。 可通过图像处理算法使用它，以确定哪些设置已用于捕获每个帧。 最后，声明一个将用于跟踪序列中当前正在捕获的图像的索引。

[!code-cs[VPSMemberVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSMemberVariables)]

## <a name="prepare-the-variable-photo-sequence-capture"></a>准备可变照片序列捕获

初始化你的 [MediaCapture](capture-photos-and-video-with-mediacapture.md) 之后，通过从媒体捕获的 [**VideoDeviceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.VideoDeviceController) 获取 [**VariablePhotoSequenceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.VariablePhotoSequenceController) 的实例并检查 [**Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.variablephotosequencecontroller.supported) 属性，确保可变照片序列在当前设备上受支持。

[!code-cs[IsVPSSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsVPSSupported)]

从可变照片序列控制器获取 [**FrameControlCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.FrameControlCapabilities) 对象。 此对象具有可针对照片序列的每帧配置的每个设置的属性。 这些问题包括：

-   [**风险**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.exposure)
-   [**ExposureCompensation**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.exposurecompensation)
-   [**Flash**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.flash)
-   [**Focus**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.focus)
-   [**IsoSpeed**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.isospeed)
-   [**PhotoConfirmation**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.photoconfirmationsupported)

此示例将为每个帧设置一个不同的曝光补偿值。 若要验证照片序列的曝光补偿在当前设备上是否受支持，请检查通过 **ExposureCompensation** 属性访问的 [**FrameExposureCompensationCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.FrameExposureCompensationCapabilities) 对象的 [**Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.exposurecompensationcontrol.supported) 属性。

[!code-cs[IsExposureCompensationSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsExposureCompensationSupported)]

为要捕获的每个帧创建一个新的 [**FrameController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.FrameController) 对象。 此示例将捕获三个帧。 为你希望针对每个帧而变化的控件设置值。 接着，清除 **VariablePhotoSequenceController** 的 [**DesiredFrameControllers**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.variablephotosequencecontroller.desiredframecontrollers) 集合，然后将每个帧控制器添加到该集合。

[!code-cs[InitFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitFrameControllers)]

创建 [**ImageEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) 对象以设置要用于捕获的图像的编码。 调用静态方法 [**MediaCapture.PrepareVariablePhotoSequenceCaptureAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.preparevariablephotosequencecaptureasync)，从而传入编码属性。 此方法返回 [**VariablePhotoSequenceCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture) 对象。 最后，为 [**PhotoCaptured**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.photocaptured) 和 [**Stopped**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.stopped) 事件注册事件处理程序。

[!code-cs[PrepareVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPrepareVPS)]

## <a name="start-the-variable-photo-sequence-capture"></a>启动可变照片序列捕获

若要启动可变照片序列捕获，请调用 [**VariablePhotoSequenceCapture.StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.startasync)。 请务必初始化用于存储捕获的图像和帧控件值的数组，并将当前索引设置为 0。 设置你的应用的录制状态变量并更新你的 UI，以禁止在执行此捕获时启动另一个捕获。

[!code-cs[StartVPSCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartVPSCapture)]

## <a name="receive-the-captured-frames"></a>接收捕获的帧

针对每个捕获的帧引发 [**PhotoCaptured**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.photocaptured) 事件。 保存帧控件值和该帧的已捕获图像，然后增加当前帧索引。 此示例显示了如何获取每个帧的 [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 表示形式。 有关使用 **SoftwareBitmap** 的详细信息，请参阅[图像处理](imaging.md)。

[!code-cs[OnPhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnPhotoCaptured)]

## <a name="handle-the-completion-of-the-variable-photo-sequence-capture"></a>处理可变照片序列捕获的完成

当已捕获序列中的所有帧时，将引发 [**Stopped**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.stopped) 事件。 更新你的应用的录制状态并更新你的 UI，以允许用户启动新的捕获。 此时，你可以将捕获的图像和帧控件值传递给图像处理代码。

[!code-cs[OnStopped](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnStopped)]

## <a name="update-frame-controllers"></a>更新帧控制器

如果你想要使用不同的每帧设置执行另一个可变照片序列捕获，无需完全重新初始化 **VariablePhotoSequenceCapture**。 你可以清除 [**DesiredFrameControllers**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.variablephotosequencecontroller.desiredframecontrollers) 集合并添加新的帧控制器，也可以修改现有的帧控制器值。 以下示例将检查 [**FrameFlashCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.FrameFlashCapabilities) 对象，以验证当前设备是否支持可变照片序列帧的闪光灯和闪光电源。 如果支持，将更新每个帧，以便在电源已满时启用闪光灯。 之前为每个帧设置的曝光补偿值仍然处于活动状态。

[!code-cs[UpdateFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateFrameControllers)]

## <a name="clean-up-the-variable-photo-sequence-capture"></a>清理可变照片序列捕获

当你完成捕获可变照片序列或你的应用暂停时，通过调用 [**FinishAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.finishasync) 清理可变照片序列对象。 取消注册对象的事件处理程序，并将其设置为 null。

[!code-cs[CleanUpVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpVPS)]

## <a name="related-topics"></a>相关主题

* [摄像头](camera.md)
* [基本的照片、 视频和音频捕获与 MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 





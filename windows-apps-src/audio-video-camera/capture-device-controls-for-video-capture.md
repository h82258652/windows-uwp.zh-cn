---
author: drewbatgit
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: 本文介绍如何使用手动设备控件实现增强的视频捕获方案，包括 HDR 视频和曝光优先级。
title: 用于视频捕获的手动相机控件
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 91f9d21f6df09bf8f3cad3e5a43f3ed94049338e
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5982089"
---
# <a name="manual-camera-controls-for-video-capture"></a>用于视频捕获的手动相机控件



本文介绍如何使用手动设备控件实现增强的视频捕获方案，包括 HDR 视频和曝光优先级。

本文中讨论的视频设备控件全部使用相同模式添加到你的应用中。 首先，检查运行你的应用的当前设备是否支持该控件。 如果控件受支持，则为控件设置所需模式。 通常，如果特定控件在当前设备上不受支持，你应禁用或隐藏允许用户启用该功能的 UI 元素。

本文中讨论的所有设备控件 API 都是 [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902) 命名空间的成员。

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

> [!NOTE] 
> 本文以[使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)中讨论的概念和代码为基础，该文章介绍了实现基本照片和视频捕获的步骤。 我们建议你先熟悉该文中的基本媒体捕获模式，然后再转到更高级的捕获方案。 本文中的代码假设你的应用已有一个正确完成初始化的 MediaCapture 的实例。

## <a name="hdr-video"></a>HDR 视频

高动态范围 (HDR) 视频功能将 HDR 处理应用到捕获设备的视频流。 通过选择 [**HdrVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926682) 属性来确定 HDR 视频是否受支持。

HDR 视频控件支持以下三种模式：开、关和自动。这意味着设备以动态方式确定 HDR 视频处理是否会改进媒体捕获；如果会改进，则启用 HDR 视频。 若要确定特定模式在当前设备上是否受支持，请检查以查看 [**HdrVideoControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926683) 集合是否包含所需模式。

通过将 [**HdrVideoControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926681) 设置为所需模式，启用或禁用 HDR 视频处理。

[!code-cs[SetHdrVideoMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetHdrVideoMode)]

## <a name="exposure-priority"></a>曝光优先级

启用 [**ExposurePriorityVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926644) 时，将评估来自捕获设备的视频帧以确定视频是否正在捕获光线较暗的场景。 如果是，该控件将降低已捕获视频的帧速率，以便增加每个帧的曝光时间并改进已捕获视频的视觉质量。

通过检查 [**ExposurePriorityVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926647) 属性，确定曝光优先级控件在当前设备上是否受支持。

通过将 [**ExposurePriorityVideoControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926646) 设置为所需模式，启用或禁用曝光优先级控件。

[!code-cs[EnableExposurePriority](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEnableExposurePriority)]

## <a name="temporal-denoising"></a>临时降噪
自 Windows 10 版本 1803 起，可在支持临时降噪的设备上为视频启用此功能。 此功能实时融合来自多个相邻帧的图像数据，生成视觉噪音较少的视频帧。

应用可通过 [**VideoTemporalDenoisingControl**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol) 确定当前设备是否支持临时降噪，如果支持，则还确定设备支持哪种降噪模式。 三种可用的降噪模式为：[**关**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingmode)、[**开**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingmode)和[**自动**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingmode)。设备可能不支持所有模式，但每台设备必须支持**自动**或支持**开**和**关**。

以下示例使用简单的 UI 来提供单选按钮，让用户切换降噪模式。

[!code-cs[SnippetDenoiseXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetDenoiseXAML)]

以下方法检查了 [**VideoTemporalDenoisingControl.Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.supported) 属性以确定当前设备是否支持临时降噪。 如果支持，则检查确保设备支持**关**和**自动**或**开**模式，我们将在此情况下将单选按钮设置为可见。 接下来，如果设备支持这些模式，则**自动**和**开**按钮为可见状态。

[!code-cs[SnippetUpdateDenoiseCapabilities](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetUpdateDenoiseCapabilities)]

在单选按钮的 **Checked** 事件处理程序中，已选中按钮名称，并通过设置 [**VideoTemporalDenoisingControl.Mode**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.mode) 属性设置了相应模式。

[!code-cs[SnippetDenoiseButtonChecked](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseButtonChecked)]

### <a name="disabling-temporal-denoising-while-processing-frames"></a>处理帧时禁用临时降噪
使用临时降噪处理的视频的视觉效果更棒。 但是临时降噪会影响图像一致性并减少帧中的细节数量，因此对帧执行图像处理（如注册或光学字符识别）的应用可能会在启用图像处理时以编程方式禁用降噪功能。

以下示例确定设备支持的降噪模式并将此信息存储在某些类变量中。

[!code-cs[SnippetDenoiseFrameReaderVars](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseFrameReaderVars)]

[!code-cs[SnippetDenoiseCapabilitiesForFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseCapabilitiesForFrameProcessing)]

应用启用帧处理后，会将降噪模式设置为**关**（如果支持该模式），让帧处理能够使用未经过降噪的原始帧。

[!code-cs[SnippetEnableFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEnableFrameProcessing)]

应用禁用帧处理后，会根据设备支持的模式将降噪模式设置为**开**或**自动**。

[!code-cs[SnippetDisableFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDisableFrameProcessing)]

若要详细了解如何获取用于图像处理的视频帧，请参阅[使用 MediaFrameReader 处理媒体帧](process-media-frames-with-mediaframereader.md)。

## <a name="related-topics"></a>相关主题

* [相机](camera.md)
* [使用 MediaCapture 进行照片、视频和音频的基本捕获](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [使用 MediaFrameReader 处理媒体帧](process-media-frames-with-mediaframereader.md)
*  [**VideoTemporalDenoisingControl**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol)
 





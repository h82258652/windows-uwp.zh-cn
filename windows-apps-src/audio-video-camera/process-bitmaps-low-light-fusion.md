---
author: laurenhughes
description: "本文介绍了如何使用 Low Light Fusion 类处理位图。"
title: "使用 Low Light Fusion API 处理位图"
ms.author: lahugh
ms.date: 11/02/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, low light fusion, 位图, 图像处理"
localizationpriority: medium
ms.openlocfilehash: 989571063603a7133f39961b4b32bdc60fc373dc
ms.sourcegitcommit: d0c93d734639bd31f264424ae5b6fead903a951d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2017
---
# <a name="process-bitmaps-with-the-lowlightfusion-api"></a>使用 Low Light Fusion API 处理位图

低亮度图像难以获得好的图像质量，尤其是在孔距和传感器尺寸固定的移动设备上更是如此。 为弥补暗淡的光线，设备可能会增加曝光时间或传感器增益，这会导致图像中出现运动模糊和噪点增多。 

[LowLightFusion 类](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion)通过从接近的时间的多个帧（如短时间闪光图像）中采样像素信息来减少噪点和运动模糊，改进了低亮度图像的质量。 这个功能很有用，举例来说，它可添加到照片编辑应用中。

此功能也可通过 [AdvancedPhotoCapture 类](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture)提供，这可以在捕获一系列图像之后立即将低亮度合成算法应用于一系列图像。 请参阅[低亮度照片](https://docs.microsoft.com/windows/uwp/audio-video-camera/high-dynamic-range-hdr-photo-capture#low-light-photo-capture)捕获，以了解如何实现此功能。

## <a name="prepare-the-images-for-processing"></a>准备要处理的图像

在本示例中，我们将展示如何使用 [LowLightFusion 类](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion)，以及 [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 来允许用户选择多个要执行低亮度合成的图像。

首先，我们将需要确定该算法接受多少个图像（也称为帧），并创建一个包含这些帧的列表。

[!code-cs[SnippetGetMaxLLFFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetGetMaxLLFFrames)]

在确定低亮度合成算法接受多少帧之后，我们可以使用 [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 来允许用户选择应在该算法中使用哪些图像。

[!code-cs[SnippetGetFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetGetFrames)]

现在，我们已选择正确的帧数，我们需要将这些帧解码到 [SoftwareBitmap](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 中，并确保 SoftwareBitmap 是适合 LowLightFusion 的正确格式。

[!code-cs[SnippetDecodeFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetDecodeFrames)]


## <a name="fuse-the-bitmaps-into-a-single-bitmap"></a>将多个位图合成为一个位图

现在，我们已经有了格式可接受、数量也正确的帧，我们可以使用 **[FuseAsync](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion#Windows_Media_Core_LowLightFusion_FuseAsync_Windows_Foundation_Collections_IIterable_Windows_Graphics_Imaging_SoftwareBitmap__)** 方法来应用低亮度合成算法。 结果将是处理好的图像，它具有更高的清晰度，采用 SoftwareBitmap 格式。 

[!code-cs[SnippetFuseFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetFuseFrames)]

最后，我们将清理生成的 SoftwareBitmap，具体方法是将该位图编码并保存为用户友好的“常规”图像，类似于我们开始使用的输入图像。

[!code-cs[SnippetEncodeFrame](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetEncodeFrame)]


## <a name="before-and-after"></a>之前和之后

下面是两幅输入图像以及应用低亮度合成算法之后产生的输出图像的示例。

> [!div class="mx-tableFixed"] 
| 帧 1 | 帧 2 | 低亮度合成输出 | 
|---------|---------|-------------------------|
| ![低亮度合成算法的第一个输入帧](./images/LLF-Input1.png) | ![低亮度合成算法的第二个输入帧](./images/LLF-Input2.png) | ![低亮度合成算法的结果帧](./images/LLF-Output.png) |

可以从输入帧和输出帧看出，杯子的光照和清晰度都已增强，周围物体的清晰度也得到了改善，如地毯旁边的模型。

## <a name="related-topics"></a>相关主题 
[LowLightFusion 类](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion)  
[LowLightFusionResult 类](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusionresult)
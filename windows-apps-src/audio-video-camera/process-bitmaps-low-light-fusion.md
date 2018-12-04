---
description: 本文介绍了如何使用 Low Light Fusion 类处理位图。
title: 使用 Low Light Fusion API 处理位图
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, low light fusion, 位图, 图像处理
ms.localizationpriority: medium
ms.openlocfilehash: e0677d2e4ce5e412c158b8a00da3a6338bae6c46
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8467395"
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

现在，我们已经有了格式可接受、数量也正确的帧，我们可以使用 **[FuseAsync](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion.fuseasync)** 方法来应用低亮度合成算法。 结果将是处理好的图像，它具有更高的清晰度，采用 SoftwareBitmap 格式。 

[!code-cs[SnippetFuseFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetFuseFrames)]

最后，我们将清理生成的 SoftwareBitmap，具体方法是将该位图编码并保存为用户友好的“常规”图像，类似于我们开始使用的输入图像。

[!code-cs[SnippetEncodeFrame](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetEncodeFrame)]


## <a name="before-and-after"></a>之前和之后

下面是一幅输入图像以及应用低亮度合成算法之后产生的输出图像的示例。

> [!div class="mx-tableFixed"] 
| 输入帧 | 低亮度合成输出 | 
|-------------|-------------------------|
| ![低亮度合成算法的输入帧](./images/LLF-Input.png) | ![低亮度合成算法的结果帧](./images/LLF-Output.png) |

从输入帧可以看出，横幅周围的光和阴影的清晰度已经得到了改善。

## <a name="related-topics"></a>相关主题 
[LowLightFusion 类](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion)  
[LowLightFusionResult 类](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusionresult)
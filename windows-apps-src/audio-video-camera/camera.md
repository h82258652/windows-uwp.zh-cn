---
ms.assetid: 370f2c14-4f1e-47b3-9197-24205ab255a3
description: 本文列出了 UWP 应用可用的相机功能以及指向显示如何使用这些功能的操作方法文章的链接。
title: 相机
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d7d7fcdeb3740ac4c6851170796243392676d1d1
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254293"
---
# <a name="camera"></a>相机

本节提供有关创建使用相机或麦克风捕获照片、视频或音频的通用 Windows 平台 (UWP) 应用的指南。

## <a name="use-the-windows-built-in-camera-ui"></a>使用 Windows 内置相机 UI

| 主题 | 描述 |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [利用 Windows 内置照相机 UI 捕获照片和视频](capture-photos-and-video-with-cameracaptureui.md) | 展示如何使用 [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 类来使用内置于 Windows 的相机 UI 捕获照片或视频。 如果仅希望使用户能够捕获照片或视频，并将结果返回到应用，这就是达到此目的的最快且最简单的方法。  |

## <a name="basic-mediacapture-tasks"></a>MediaCapture 基本任务

| 主题 | 描述 |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [显示相机预览](simple-camera-preview-access.md) | 展示如何在 UWP 应用的 XAML 页面内快速显示相机预览流。 |
| [带有 MediaCapture 的基本照片、视频和音频捕获](basic-photo-video-and-audio-capture-with-MediaCapture.md) | 展示使用 [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) 类捕获照片和视频的最简单方法。 **MediaCapture** 类公布了一组强大的 API，可提供捕获管道的低级别控制和启用高级捕获方案，但本文旨在帮助你将基本的媒体捕获快速且轻松地添加到应用。 |
| [适用于移动设备的照相机 UI 功能](camera-ui-features-for-mobile-devices.md) | 展示如何利用仅在移动设备上提供的特殊相机 UI 功能。  |
                                                                                                               
## <a name="advanced-mediacapture-tasks"></a>MediaCapture 高级任务   
                                                                                                               
| 主题                                                                                             | 描述                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [通过 MediaCapture 处理设备和屏幕方向](handle-device-orientation-with-mediacapture.md) | 展示在使用帮助程序类捕获照片和视频时，如何处理设备方向。 | 
| [发现并选择相机功能配置文件](camera-profiles.md) | 展示如何使用相机配置文件来发现和管理不同视频捕获设备的功能。 这包括如下任务：选择支持特定分辨率或帧速率的配置文件、选择支持同时访问多台相机的配置文件，以及选择支持 HDR 的配置文件。 |
| [设置 MediaCapture 的格式、分辨率和帧速率](set-media-encoding-properties.md) | 展示如何使用 [**IMediaEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.IMediaEncodingProperties) 界面设置相机预览流以及已捕获照片和视频的分辨率和帧速率。 还将展示如何确保预览流的纵横比与已捕获媒体的纵横比相匹配。 |
| [HDR 和低轻型照片捕获](high-dynamic-range-hdr-photo-capture.md) | 展示如何使用 [**AdvancedPhotoCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) 类捕获高动态范围 (HDR) 照片和光线较暗的照片。 |
| [照片和视频捕获的手动相机控制](capture-device-controls-for-photo-and-video-capture.md) | 展示如何使用手动设备控件实现增强的照片和视频捕获方案，包括光学图像防抖动和平滑缩放。 |
| [视频捕获的手动相机控件](capture-device-controls-for-video-capture.md) | 展示如何使用手动设备控件实现增强的视频捕获方案，包括 HDR 视频和曝光优先级。  |
| [视频捕获的视频抖动效果](effects-for-video-capture.md) | 展示如何使用视频防抖动效果。  |
| [适用于 MediaCapture 的场景 anlysis](scene-analysis-for-media-capture.md) | 展示如何使用 [**SceneAnalysisEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffect) 和 [**FaceDetectionEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffect) 分析媒体捕获预览流的内容。  |
| [使用 VariablePhotoSequence 捕获照片序列](variable-photo-sequence.md) | 展示如何捕获可变照片序列，这允许你快速连续捕获图像的多个帧，并将每个帧配置为使用不同的焦点、闪光灯、ISO、曝光和曝光补偿设置。  |
| [用 MediaFrameReader 处理媒体帧](process-media-frames-with-mediaframereader.md) | 展示如何将 [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) 与 [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) 结合使用，以获取一个或多个可用源提供的媒体帧，这些可用源包括颜色、深度、红外线相机、音频设备，甚至是自定义的帧源（例如生成骨架跟踪帧的帧源）。 此功能旨在由执行实时处理媒体帧的应用使用，例如增强现实和感知深度的相机应用。  |
| [获取预览框架](get-a-preview-frame.md) | 展示如何从媒体捕获预览流获取单个预览帧。  |                                                                                                   


## <a name="uwp-app-samples-for-camera"></a>相机的 UWP 应用示例

* [照相机面部检测示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFaceDetection)
* [相机预览框架示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraGetPreviewFrame)
* [照相机 HDR 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraAdvancedCapture)
* [照相机手动控制示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraManualControls)
* [照相机配置文件示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraProfile)
* [照相机分辨率示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraResolution)
* [照相机初学者工具包](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraStarterKit)
* [照相机视频抖动示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraVideoStabilization)

## <a name="related-topics"></a>相关主题

* [音频、视频和相机](index.md)
 

 





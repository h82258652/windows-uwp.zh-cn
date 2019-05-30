---
ms.assetid: 0fc12d26-f1cf-4da7-b5a7-735a5074b74a
description: 本部分将提供有关创建可捕获、播放或编辑照片、视频或音频的通用 Windows 平台 (UWP) 应用的信息。
title: 音频、视频和相机
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 98bc1e93c918310b902c70709df1c2cc1b38a238
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360844"
---
# <a name="audio-video-and-camera"></a>音频、视频和相机


本部分将提供有关创建可捕获、播放或编辑照片、视频或音频的通用 Windows 平台 (UWP) 应用的信息。
 
| 主题                                                                                             | 描述                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [摄像头](camera.md) | 列出了 UWP 应用可用的相机功能以及指向显示如何使用这些功能的操作方法文章的链接。 |
| [媒体播放](media-playback.md) | 提供有关创建使用音频和视频播放的 UWP 应用的信息。 |
| [检测图像或视频中的人脸](detect-and-track-faces-in-an-image.md) | 介绍如何使用 [FaceTracker](https://docs.microsoft.com/uwp/api/Windows.Media.FaceAnalysis.FaceTracker) 在视频帧序列中随着时间的推移跟踪人脸。 |
| [媒体合成和编辑](media-compositions-and-editing.md) | 介绍如何使用 [**Windows.Media.Editing**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing) 命名空间中的 API 来快速开发应用，从而使用户从音频和视频源文件创建媒体合成。 |
| [自定义视频效果](custom-video-effects.md) | 介绍如何创建可实现 **IBasicVideoEffect** 接口的 Windows 运行时组件，以允许你为视频流创建自定义效果。 |
| [自定义音频效果](custom-audio-effects.md) | 介绍如何创建可实现 **IBasicAudioEffect** 接口的 Windows 运行时组件，以允许为音频流创建自定义效果。 |
| [创建、编辑和保存位图图像](imaging.md) | 介绍如何通过使用 [SoftwareBitmap](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 对象表示位图图像来加载和保存图像文件。  |
| [音频设备信息属性](audio-device-information-properties.md)  | 列出了与音频设备相关的设备信息属性。 |
| [检测和响应音频的状态变化](detect-and-respond-to-audio-state-changes.md)  | 介绍了 UWP 应用如何检测并响应音频流级别中的系统初始化变化。 |
| [对媒体文件进行转码](transcode-media-files.md) | 介绍如何使用 [Windows.Media.Transcoding](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding) API 将视频文件代码从一种格式转换为另一种格式。 |
| [在后台处理媒体文件](process-media-files-in-the-background.md) | 介绍如何使用 [MediaProcessingTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTrigger) 和后台任务在后台处理媒体文件。 |
| [音频图](audio-graphs.md) | 介绍如何使用 [Windows.Media.Audio](https://docs.microsoft.com/uwp/api/Windows.Media.Audio) 命名空间中的 API 为音频路由、混合和处理方案创建音频图。 |
| [MIDI](midi.md) | 介绍如何枚举 MIDI（乐器数字接口）设备以及从 UWP 应用发送和接收 MIDI 消息。 |
| [从设备导入媒体](import-media-from-a-device.md) | 介绍如何从设备导入媒体，包括搜索可用媒体源、导入文件（如视频、照片和 sidecar 文件）以及从源设备中删除导入的文件。 |
| [独立于相机的手电筒](camera-independent-flashlight.md) | 介绍如何访问和使用设备灯（如果存在）。 灯功能通过设备的相机和相机闪光灯功能单独管理。 |
| [支持的编解码器](supported-codecs.md) | 列出 UWP 应用的音频、视频和图像编解码器以及格式支持。 |
| [查询已安装的编解码器](codec-query.md) | 演示了如何查询设备上所安装的音频和视频编码器和解码器。 |
| [屏幕捕获](screen-capture.md) | 介绍了如何使用 [Windows.Graphics.Capture 命名空间](https://docs.microsoft.com/uwp/api/windows.graphics.capture)从屏幕或应用程序窗口获取帧，以创建用于生成协作和交互式体验的视频流或快照。 |

## <a name="see-also"></a>另请参阅
- [开发 UWP 应用](https://developer.microsoft.com/windows/develop)

 

 

 





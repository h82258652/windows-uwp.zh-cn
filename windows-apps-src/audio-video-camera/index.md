---
author: drewbatgit
ms.assetid: 0fc12d26-f1cf-4da7-b5a7-735a5074b74a
description: "本部分将提供有关创建可捕获、播放或编辑照片、视频或音频的通用 Windows 应用的信息。"
title: "音频、视频和相机"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 8480eb181b6af26eae5950a1f1bf040d0cb5db99

---

# 音频、视频和相机

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本部分将提供有关创建可捕获、播放编辑照片、视频或音频的通用 Windows 应用的信息。
 
| 主题                                                                                             | 说明                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [使用 CameraCaptureUI 捕获照片和视频](capture-photos-and-video-with-cameracaptureui.md) | 本文将介绍如何使用 [CameraCaptureUI](capture-photos-and-video-with-cameracaptureui.md) 类来使用内置于 Windows 的相机 UI 捕获照片或视频。                                                                                                            |
| [使用 MediaCapture 捕获照片和视频](capture-photos-and-video-with-mediacapture.md)       | 本文将介绍使用 [MediaCapture](https://msdn.microsoft.com/library/windows/apps/br241124) API 捕获照片和视频的操作步骤，包括初始化和关闭 MediaCapture 以及处理设备方向的更改。                                  |
| [检测图像或视频中的人脸](detect-and-track-faces-in-an-image.md)                         | 本主题介绍如何使用 [FaceTracker](https://msdn.microsoft.com/library/windows/apps/dn974150)，该功能针对随视频帧序列中的时间推移跟踪人脸进行了优化。                                                                                                               |
| [媒体合成和编辑](media-compositions-and-editing.md)                               | 本文向你介绍如何使用 [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565) 命名空间中的 API 来快速开发应用，从而使用户从音频和视频源文件创建媒体合成。                                    |
                                                                                                                                        | [自定义视频效果](custom-video-effects.md)                               | 本文将介绍如何创建可实现 IBasicVideoEffect 接口的 Windows 运行时组件，以允许你为视频流创建自定义效果。                                                                                                                                |
| [图像处理](imaging.md)                                                                             | 本文介绍了如何使用 [SoftwareBitmap](https://msdn.microsoft.com/library/windows/apps/dn887358) 对象加载和保存图像文件以表示位图图像。                                                                                                                     |
| [音频设备信息属性](audio-device-information-properties.md)                                                                             | 本文列出了与音频设备相关的设备信息属性。                                                                                                                      |
| [转换媒体文件的代码](transcode-media-files.md)                                                 | 你可以使用 [Windows.Media.Transcoding](https://msdn.microsoft.com/library/windows/apps/br207105) API 将视频文件代码从一种格式转换为另一种格式。                                                                                                                                |
| [在后台处理媒体文件](process-media-files-in-the-background.md)                 | 本文将向你介绍如何使用 [MediaProcessingTrigger](https://msdn.microsoft.com/library/windows/apps/dn806005) 和后台任务在后台处理媒体文件。                                                                                             |
| [使用 MediaSource 的媒体播放](media-playback-with-mediasource.md)                             | [MediaSource](https://msdn.microsoft.com/library/windows/apps/dn930905) 类提供从不同的源（例如本地或远程文件）引用和播放媒体的常用方法，并公开用于访问媒体数据的常用模型，而不考虑基础媒体格式。  |
| [自适应流](adaptive-streaming.md)                                                       | 本文介绍如何将自适应流多媒体内容的播放添加到通用 Windows 平台 (UWP) 应用。 此功能当前支持 Http 实时流 (HLS) 和 HTTP 动态流 (DASH) 内容的播放。                                          |
| [后台音频](background-audio.md)                                                           | 本文介绍如何创建可在后台播放音频的 UWP 应用。                                                                                                                                                                                                               |
| [系统媒体传输控件](system-media-transport-controls.md)                             | 通过 [SystemMediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn278677) 类，你的应用可以使用内置于 Windows 的系统媒体传输控件，并更新控件显示的有关你的应用当前播放的媒体的元数据。 |
| [媒体转换](media-casting.md)                                                                 | 本文向你演示了如何将媒体从通用 Windows 应用转换到远程设备。                                                                                                                                                                                                       |
| [音频图](audio-graphs.md)                                                                   | 本文将介绍如何使用 [Windows.Media.Audio](https://msdn.microsoft.com/library/windows/apps/dn914341) 命名空间中的 API 来创建音频路由、混合和处理方案的音频图。                                                                            |
| [MIDI](midi.md)                                                                                   | 本文向你演示了如何枚举 MIDI（乐器数字接口）设备以及从通用 Windows 应用发送和接收 MIDI 消息。                                                                                                                                   |
| [独立于相机的手电筒](camera-independent-flashlight.md)                                 | 本文介绍如何访问和使用设备的灯（如果存在）。 灯功能的管理独立于设备的相机和相机闪光灯功能。                                                                                                                 |
| [支持的编解码器](supported-codecs.md)                                                           | 本文将列出 UWP 应用的音频和视频编解码器以及格式支持。                                                                                                                                                                                                                  |
| [PlayReady DRM](playready-client-sdk.md)                                                          | 本主题介绍如何向通用 Windows 平台 (UWP) 应用添加 PlayReady 保护的媒体内容。                                                                                                                                                                                |
| [PlayReady 加密媒体扩展](playready-encrypted-media-extension.md)                     | 本部分将介绍如何修改 PlayReady Web 应用以支持从以前的 Windows 8.1 版本到 Windows 10 版本所做的更改。                                                                                                                                       |

 

 

 







<!--HONumber=Jun16_HO4-->



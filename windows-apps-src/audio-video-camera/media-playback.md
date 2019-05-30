---
ms.assetid: 25553c4d-fa4f-4130-af9b-97f993fefd43
description: 本部分将提供有关创建可播放音频和视频的通用 Windows 应用的信息。
title: 媒体播放
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1c945c273a119a50af2a4d99477617bd0af39a36
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360729"
---
# <a name="media-playback"></a>媒体播放


本部分将提供有关创建可播放音频和视频的通用 Windows 应用的信息。 

## <a name="media-playback-developer-features"></a>媒体播放开发人员功能

下表列出了操作方法文章，这些文章提供有关将媒体播放功能添加到应用的详细指南。
 
| 主题                                                                                             | 描述                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [播放音频和视频使用 MediaPlayer](play-audio-and-video-with-mediaplayer.md) | 本文介绍了如何针对 UWP 应用充分利用媒体播放系统的新功能和改进。 从 Windows 10 版本 1607 开始，播放媒体的推荐最佳做法是将 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) 类（而不是 [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement)）用于媒体播放。 引入了轻型 XAML 控件 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)，该控件允许你在 XAML 页面中呈现媒体内容。 **MediaPlayer** 提供多项优势，包括与系统媒体传输控件的自动集成以及后台音频的更简单的单进程模型。 本文还介绍了如何将视频呈现给 Windows.UI.Composition 图面以及如何使用 [**MediaTimelineController**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaTimelineController) 同步多个媒体播放器。                                                                                                          |
| [媒体项，播放列表，其中跟踪](media-playback-with-mediasource.md)                         | 本文介绍了如何使用 [**MediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource) 类，该类提供从不同的源（例如本地或远程文件）引用和播放媒体的常用方法，并公开用于访问媒体数据的常用模型，而不考虑基础媒体格式。 [  **MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem) 类扩展 **MediaSource** 的功能，从而允许你管理并从媒体项中所含的多个音频、视频和元数据曲目中进行选择。 [**MediaPlaybackList** ](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList)允许你创建从一个或多个媒体的播放列表播放的项。                                                                                                               |
| [将与集成系统媒体传输控件](integrate-with-systemmediatransportcontrols.md)                               | 本文介绍了如何将你的应用与系统媒体传输控件 (SMTC) 集成。 从 Windows 10 版本 1607 开始，为播放媒体而创建的每个 **MediaPlayer** 实例将自动由 SMTC 显示。 本文介绍了如何向 SMTC 提供关于你正在播放的内容的元数据，以及如何扩大或完全覆盖 SMTC 控件的默认行为。                                   |
| [系统支持超时元数据提示](system-supported-metadata-cues.md)                               | 本文介绍如何利用可以在媒体文件或流中嵌入的多种格式的计时元数据。                                   |
| [创建、 安排和管理媒体分页符](create-schedule-and-manage-media-breaks.md)                                                                             | 本文介绍了如何创建、计划和管理媒体播放应用的媒体中断。 从 Windows 10 版本 1607 开始，可使用 [**MediaBreakManager**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreakManager) 类快速轻松地将媒体中断添加到使用 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) 播放的任何 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem)。 媒体中断通常用于在媒体内容中插入音频或视频广告。 安排一个或多个媒体中断后，系统将在播放期间的指定时间自动播放你的媒体内容。 **MediaBreakManager** 提供事件，以便应用可在媒体中断开始、结束或用户跳过时做出反应。 你还可以访问媒体中断的 [**MediaPlaybackSession**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackSession)，以监视诸如下载和缓冲进度更新等事件。                                                                                                                     |
| [在背景中播放媒体](background-audio.md)                                                                             | 本文介绍了如何配置应用，以便在应用从前台移至后台后，媒体可以继续播放。 这意味着，即使在用户已最小化你的应用、返回到主屏幕，或已以其他方式离开你的应用后，你的应用仍可继续播放音频。 在 Windows 10 版本 1607 中，已引入新的用于后台媒体播放的单进程模型，从而比传统的双进程模型实现速度更快且更简单。 本文包含的信息涉及处理新的应用程序生命周期事件 [**EnteredBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) 和[**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground)，以便在后台运行时管理应用的内存使用量。                                                                                                                    |
| [自适应流式处理](adaptive-streaming.md)                                                       | 本文介绍如何将自适应流多媒体内容的播放添加到通用 Windows 平台 (UWP) 应用。 此功能当前支持 Http 实时流 (HLS) 和 HTTP 动态流 (DASH) 内容的播放。                                          |
| [媒体强制转换](media-casting.md)                                                                 | 本文向你演示了如何将媒体从通用 Windows 应用转换到远程设备。                                                                                                                                                                                                       |
| [PlayReady DRM](playready-client-sdk.md)                                                          | 本主题介绍如何向通用 Windows 平台 (UWP) 应用添加 PlayReady 保护的媒体内容。                                                                                                                                                                                |
| [PlayReady 加密媒体扩展](playready-encrypted-media-extension.md)                     | 本部分介绍了如何修改 PlayReady Web 应用以支持从以前的 Windows 8.1 版本到 Windows 10 版本所做的更改。                                                                                                                                       |

## <a name="media-playback-sdk-samples"></a>媒体播放 SDK 示例

以下 SDK 示例演示了适用于 Windows 10 上的 UWP 应用的媒体播放功能。 可以将这些示例用于查看上下文中所使用的媒体播放 API，或者用作你自己的应用的起始点。

* [自适应流式处理示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming)
* [背景音频示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)
* [系统媒体传输示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls)                                                                                               
 





---
author: drewbatgit
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: "本文介绍如何将自适应流多媒体内容的播放添加到通用 Windows 平台 (UWP) 应用。 此功能当前支持 Http 实时流 (HLS) 和 HTTP 动态流 (DASH) 内容的播放。"
title: "自适应流式处理"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 3afd0440d8e552ebc3459c5fe30dd766db3ae8b9
ms.lasthandoff: 02/07/2017

---

# <a name="adaptive-streaming"></a>自适应流式处理

\[ 已针对 Windows 10 上的 UWP 应用进行了更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本文介绍如何将自适应流多媒体内容的播放添加到通用 Windows 平台 (UWP) 应用。 此功能当前支持 Http 实时流 (HLS) 和 HTTP 动态流 (DASH) 内容的播放。

有关受支持的 HLS 协议标记的列表，请参阅 [HLS 标记支持](hls-tag-support.md)。 

> [!NOTE] 
> 本文中的代码改编自 UWP [自适应流式处理示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming)。

## <a name="simple-adaptive-streaming-with-mediaplayer-and-mediaplayerelement"></a>使用 MediaPlayer 和 MediaPlayerElement 的简单自适应流式处理

若要在 UWP 应用中播放自适应流式处理媒体，请创建一个指向 DASH 或 HLS 清单文件的 **Uri** 对象。 创建 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 类的实例。 调用 [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912) 以创建一个新的 **MediaSource** 对象，然后将该对象设置为 **MediaPlayer** 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) 属性。 调用 [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play) 以开始播放媒体内容。

[!code-cs[DeclareMediaPlayer](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-cs[ManifestSourceNoUI](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSourceNoUI)]

上述示例将播放媒体内容的音频，但它不会在 UI 中自动呈现该内容。 大多数播放视频内容的应用都将希望在 XAML 页面中呈现该内容。  为此，请将一个 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) 控件添加到 XAML 页面。

[!code-xml[MediaPlayerElementXAML](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

调用 [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912) 以从 DASH 或 HLS 清单文件的 URI 创建一个 **MediaSource**。 然后设置 **MediaPlayerElement** 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227420) 属性。 **MediaPlayerElement** 将为该内容自动创建一个新的 **MediaPlayer** 对象。 你可以在 **MediaPlayer** 上调用 **Play** 以开始播放内容。

[!code-cs[ManifestSource](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSource)]

> [!NOTE] 
> 从 Windows 10 版本 1607 开始，建议你使用 **MediaPlayer** 类播放媒体项。 **MediaPlayerElement** 是一种轻型 XAML 控件，用于在 XAML 页面中呈现 **MediaPlayer** 的内容。 为了实现向后兼容性，继续支持 **MediaElement** 控件。 有关使用 **MediaPlayer** 和 **MediaPlayerElement** 播放媒体内容的详细信息，请查阅[使用 MediaPlayer 播放音频和视频](play-audio-and-video-with-mediaplayer.md)。 有关使用 **MediaSource** 和相关 API 处理媒体内容的信息，请参阅[媒体项、播放列表和轨](media-playback-with-mediasource.md)。

## <a name="adaptive-streaming-with-adaptivemediasource"></a>使用 AdaptiveMediaSource 的自适应流式处理

如果你的应用需要更多高级自适应流式处理功能（如提供自定义 HTTP 标头、监视当前下载和播放比特率或调整用于确定系统何时切换自适应流的比特率的时间），请使用 [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912) 对象。

在 [**Windows.Media.Streaming.Adaptive**](https://msdn.microsoft.com/library/windows/apps/dn931279) 命名空间中可以找到自适应流式处理 API。

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

通过调用 [**CreateFromUriAsync**](https://msdn.microsoft.com/library/windows/apps/dn931261) 来使用自适应流式处理清单文件的 URI 初始化 **AdaptiveMediaSource**。 从此方法返回的 [**AdaptiveMediaSourceCreationStatus**](https://msdn.microsoft.com/library/windows/apps/dn946917) 值可使你知道媒体源是否已成功创建。 如果成功，你可以通过调用 [**SetMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn652653) 来将该对象设置为你的 **MediaPlayer** 的流源。 在此示例中，将查询 [**AvailableBitrates**](https://msdn.microsoft.com/library/windows/apps/dn931257) 属性来确定此流的受支持的最大比特率，然后将该值设置为初始比特率。 本示例还为 [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272)、[**DownloadBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931269) 和 [**PlaybackBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931278) 事件注册处理程序，这些事件将在本文后面进行讨论。

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

如果你需要设置自定义 HTTP 标头以用于获取清单文件，你可以创建 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 对象、设置所需的标头，然后将该对象传递到 **CreateFromUriAsync** 的重载中。

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

当系统即将从服务器检索资源时，会引发 [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272) 事件。 传递到事件处理程序的 [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn946935) 会公开提供有关要请求的资源的信息（资源的类型和 URI）的属性。

你可以通过更新事件参数所提供的 [**AdaptiveMediaSourceDownloadResult**](https://msdn.microsoft.com/library/windows/apps/dn946942) 对象的属性来使用 **DownloadRequested** 事件处理程序修改资源请求。 在以下示例中，将从中检索资源的 URI 通过更新结果对象的 [**ResourceUri**](https://msdn.microsoft.com/library/windows/apps/dn931250) 属性来进行修改。

通过设置结果对象的 [**Buffer**](https://msdn.microsoft.com/library/windows/apps/dn946943) 或 [**InputStream**](https://msdn.microsoft.com/library/windows/apps/dn931249) 属性，你可以替代所请求的资源的内容。 在以下示例中，清单资源的内容通过设置 **Buffer** 属性来替换。 请注意，如果要使用以异步方式获取的数据更新资源请求（如从远程服务器或异步用户身份验证检索数据），必须调用 [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn946936) 来获取延迟，然后在操作完成时调用 [**Complete**](https://msdn.microsoft.com/library/windows/apps/dn946934) 以向系统发出下载请求操作可以继续的信号。

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

**AdaptiveMediaSource** 对象提供可使你在下载或播放比特率发生更改时作出反应的事件。 在此示例中，当前比特率仅在 UI 中更新。 请注意，你可以修改用于确定系统何时切换自适应流的比特率的比率。 有关详细信息，请参阅 [**AdvancedSettings**](https://msdn.microsoft.com/library/windows/apps/mt628697) 属性。

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

## <a name="related-topics"></a>相关主题
* [媒体播放](media-playback.md)
* [HLS 标记支持](hls-tag-support.md) 
* [使用 MediaPlayer 播放音频和视频](play-audio-and-video-with-mediaplayer.md)
* [在后台播放媒体](background-audio.md) 







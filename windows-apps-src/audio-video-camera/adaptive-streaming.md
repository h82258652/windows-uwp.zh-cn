---
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: 本文介绍如何将自适应流多媒体内容的播放添加到通用 Windows 平台 (UWP) 应用。 此功能当前支持 Http 实时流 (HLS) 和 HTTP 动态流 (DASH) 内容的播放。
title: 自适应流式处理
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9ee2f8fd670da6bf843962e6b4dcac7a4b0c516d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359134"
---
# <a name="adaptive-streaming"></a>自适应流式处理


本文介绍如何将自适应流多媒体内容的播放添加到通用 Windows 平台 (UWP) 应用。 此功能支持 HTTP Live Streaming (HLS) 和基于 HTTP 的动态流式处理 (DASH) 内容的播放。 从 Windows 10 版本 1803 开始， **[AdaptiveMediaSource](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)** 支持平滑流式处理。

有关受支持的 HLS 协议标记的列表，请参阅 [HLS 标记支持](hls-tag-support.md)。 

有关受支持的 DASH 配置文件的列表，请参阅 [DASH 配置文件支持](dash-profile-support.md)。 

> [!NOTE] 
> 本文中的代码改编自 UWP [自适应流式处理示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming)。

## <a name="simple-adaptive-streaming-with-mediaplayer-and-mediaplayerelement"></a>使用 MediaPlayer 和 MediaPlayerElement 的简单自适应流式处理

若要在 UWP 应用中播放自适应流式处理媒体，请创建一个指向 DASH 或 HLS 清单文件的 **Uri** 对象。 创建 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) 类的实例。 调用 [**MediaSource.CreateFromUri**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromuri) 以创建一个新的 **MediaSource** 对象，然后将该对象设置为 **MediaPlayer** 的 [**Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.source) 属性。 调用 [**Play**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.play) 以开始播放媒体内容。

[!code-cs[DeclareMediaPlayer](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-cs[ManifestSourceNoUI](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSourceNoUI)]

上述示例将播放媒体内容的音频，但它不会在 UI 中自动呈现该内容。 大多数播放视频内容的应用都将希望在 XAML 页面中呈现该内容。  为此，请将一个 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) 控件添加到 XAML 页面。

[!code-xml[MediaPlayerElementXAML](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

调用 [**MediaSource.CreateFromUri**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromuri) 以从 DASH 或 HLS 清单文件的 URI 创建一个 **MediaSource**。 然后设置 **MediaPlayerElement** 的 [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.sourceproperty) 属性。 **MediaPlayerElement** 将为该内容自动创建一个新的 **MediaPlayer** 对象。 你可以在 **MediaPlayer** 上调用 **Play** 以开始播放内容。

[!code-cs[ManifestSource](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSource)]

> [!NOTE] 
> 从 Windows 10 版本 1607 开始，建议你使用 **MediaPlayer** 类播放媒体项。 **MediaPlayerElement** 是一种轻型 XAML 控件，用于在 XAML 页面中呈现 **MediaPlayer** 的内容。 为了实现向后兼容性，继续支持 **MediaElement** 控件。 有关使用 **MediaPlayer** 和 **MediaPlayerElement** 播放媒体内容的详细信息，请查阅[使用 MediaPlayer 播放音频和视频](play-audio-and-video-with-mediaplayer.md)。 有关使用 **MediaSource** 和相关 API 处理媒体内容的信息，请参阅[媒体项、播放列表和轨](media-playback-with-mediasource.md)。

## <a name="adaptive-streaming-with-adaptivemediasource"></a>使用 AdaptiveMediaSource 的自适应流式处理

如果你的应用需要更多高级自适应流式处理功能（如提供自定义 HTTP 标头、监视当前下载和播放比特率或调整用于确定系统何时切换自适应流的比特率的时间），请使用 **[AdaptiveMediaSource](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)** 对象。

在 [**Windows.Media.Streaming.Adaptive**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive) 命名空间中可以找到自适应流式处理 API。 本文中的示例使用以下命名空间中的 API。

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

## <a name="initialize-an-adaptivemediasource-from-a-uri"></a>通过 URI 初始化 AdaptiveMediaSource。

通过调用 [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync) 来使用自适应流式处理清单文件的 URI 初始化 **AdaptiveMediaSource**。 从此方法返回的 [**AdaptiveMediaSourceCreationStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceCreationStatus) 值可使你知道媒体源是否已成功创建。 如果成功，则通过调用 [**MediaSource.CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource.AdaptiveMediaSource) 来创建 **MediaSource** 对象，然后将该对象分配给媒体播放器的 [**Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Source) 属性，你可以将该对象设置为你的 **MediaPlayer** 的流源。 在此示例中，将查询 [**AvailableBitrates**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.availablebitrates) 属性来确定此流的受支持的最大比特率，然后将该值设置为初始比特率。 此示例中还为在本文中后面部分讨论的几个 **AdaptiveMediaSource** 事件注册了处理程序。

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

## <a name="initialize-an-adaptivemediasource-using-httpclient"></a>使用 HttpClient 初始化 AdaptiveMediaSource

如果你需要设置自定义 HTTP 标头以用于获取清单文件，你可以创建 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 对象、设置所需的标头，然后将该对象传递到 **CreateFromUriAsync** 的重载中。

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

当系统即将从服务器检索资源时，会引发 [**DownloadRequested**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.downloadrequested) 事件。 传递到事件处理程序的 [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadRequestedEventArgs) 会公开提供有关要请求的资源的信息（资源的类型和 URI）的属性。

## <a name="modify-resource-request-properties-using-the-downloadrequested-event"></a>使用 DownloadRequested 事件修改资源请求属性

你可以通过更新事件参数所提供的 [**AdaptiveMediaSourceDownloadResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadResult) 对象的属性来使用 **DownloadRequested** 事件处理程序修改资源请求。 在以下示例中，将从中检索资源的 URI 通过更新结果对象的 [**ResourceUri**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.resourceuri) 属性来进行修改。 还可以重写媒体段的字节范围偏移和长度，或者如以下示例所示，更改资源 URI 以下载完整资源并将字节范围偏移和长度设置为 null。

通过设置结果对象的 [**Buffer**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.buffer) 或 [**InputStream**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.inputstream) 属性，你可以替代所请求的资源的内容。 在以下示例中，清单资源的内容通过设置 **Buffer** 属性来替换。 请注意，如果要使用以异步方式获取的数据更新资源请求（如从远程服务器或异步用户身份验证检索数据），必须调用 [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadrequestedeventargs.getdeferral) 来获取延迟，然后在操作完成时调用 [**Complete**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadrequesteddeferral.complete) 以向系统发出下载请求操作可以继续的信号。

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

## <a name="use-bitrate-events-to-manage-and-respond-to-bitrate-changes"></a>使用比特率事件管理和响应比特率更改

**AdaptiveMediaSource** 对象提供可使你在下载或播放比特率发生更改时作出反应的事件。 在此示例中，当前比特率仅在 UI 中更新。 请注意，你可以修改用于确定系统何时切换自适应流的比特率的比率。 有关详细信息，请参阅 [**AdvancedSettings**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.advancedsettings) 属性。

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

## <a name="handle-download-completion-and-failure-events"></a>处理下载完成和失败事件
当请求的资源下载失败时，**AdaptiveMediaSource** 对象会引发 [**DownloadFailed**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadFailed) 事件。 可以使用此事件更新 UI 以响应失败。 还可以使用此事件记录有关下载操作和失败的统计信息。 

传入事件处理程序中的 [**AdaptiveMediaSourceDownloadFailedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs) 对象包含有关失败的资源下载的元数据，如资源类型、资源的 URI 以及流中发生下载失败的位置。 [  **RequestId**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.RequestId) 会获取系统为请求生成的唯一标识符，它可以用于跨多个事件使有关单个请求的状态信息相关联。

[  **Statistics**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.Statistics) 属性返回 [**AdaptiveMediaSourceDownloadStatistics**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadstatistics) 对象，它提供有关发生事件时接收的字节数和下载操作中各种里程碑的时序的详细信息。 可记录此信息以识别自适应流式处理实现中的性能问题。

[!code-cs[AMSDownloadFailed](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadFailed)]


[  **DownloadCompleted**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadCompleted) 事件在资源下载完成时发生，可提供与 **DownloadFailed** 事件类似的数据。 会再次提供 **RequestId** 以便为单个请求关联事件。 还会提供 **AdaptiveMediaSourceDownloadStatistics** 对象以启用下载统计数据的日志记录。

[!code-cs[AMSDownloadCompleted](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadCompleted)]

## <a name="gather-adaptive-streaming-telemetry-data-with-adaptivemediasourcediagnostics"></a>使用 AdaptiveMediaSourceDiagnostics 收集自适应流式处理遥测数据
**AdaptiveMediaSource** 会公开 [**Diagnostics**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource) 属性，它返回 [**AdaptiveMediaSourceDiagnostics**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics) 对象。 使用此对象可为 [**DiagnosticAvailable**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics.DiagnosticAvailable) 事件进行注册。 此事件旨在用于遥测收集，而不应用于在运行时修改应用行为。 多种不同原因都会引发此诊断事件。 检查传入到事件中的 [**AdaptiveMediaSourceDiagnosticAvailableEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs) 对象的 [**DiagnosticType**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs.DiagnosticType) 属性，以确定引发事件的原因。 可能原因包括访问请求的资源时出错以及分析流式处理清单文件时出错。 有关可以触发诊断事件的情况的列表，请参阅 [**AdaptiveMediaSourceDiagnosticType**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostictype)。 与其他自适应流式处理事件的参数一样，**AdaptiveMediaSourceDiagnosticAvailableEventArgs** 提供 **RequestId** 属性，用于在不同事件之间关联请求信息。

[!code-cs[AMSDiagnosticAvailable](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDiagnosticAvailable)]

## <a name="defer-binding-of-adaptive-streaming-content-for-items-in-a-playback-list-by-using-mediabinder"></a>使用 MediaBinder 为播放列表中的项延迟自适应流式处理内容的绑定
[  **MediaBinder**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder) 类支持延迟 [**MediaPlaybackList**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList) 中的媒体内容的绑定。 从 Windows 10 版本 1703 开始，可以提供 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) 作为绑定内容。 自适应媒体源的延迟绑定的过程与绑定其他类型的媒体（在[媒体项、播放列表和曲目](media-playback-with-mediasource.md)中进行了介绍）大致相同。 

创建 **MediaBinder** 实例，设置应用定义的 [**Token**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Token) 字符串以标识要绑定的内容，然后为 [**Binding**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Binding) 事件进行注册。 通过调用 [**MediaSource.CreateFromMediaBinder**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediabinder)，从 **Binder** 创建 **MediaSource**。 然后，通过 **MediaSource** 创建一个 **MediaPlaybackItem** 并将它添加到播放列表。

[!code-cs[InitMediaBinder](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetInitMediaBinder)]

在 **Binding** 事件处理程序中，使用标记字符串标识要绑定的内容，然后通过调用 **[CreateFromStreamAsync](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromstreamasync)** 或 **[CreateFromUriAsync](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync)** 的一个重载来创建自适应媒体源。 由于这些是异步方法，因此应首先调用 [**MediaBindingEventArgs.GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) 方法以指示系统等待操作完成，然后再继续。  通过调用 **[SetAdaptiveMediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource)** 将自适应媒体源设置为绑定内容。 最后，在操作完成后调用 [**Deferral.Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.Complete) 以指示系统继续进行。

[!code-cs[BinderBindingAMS](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetBinderBindingAMS)]

如果要为绑定的自适应媒体源注册事件处理程序，可在 **MediaPlaybackList** 的 [**CurrentItemChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacklist.CurrentItemChanged) 事件的处理程序中执行此操作。 [  **CurrentMediaPlaybackItemChangedEventArgs.NewItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.NewItem) 属性包含列表中当前正在播放的新 **MediaPlaybackItem**。 通过访问 **MediaPlaybackItem** 的 [**Source**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem.Source) 属性，然后访问媒体源的 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.AdaptiveMediaSource) 属性，来获取表示新项的 **AdaptiveMediaSource** 实例。 如果新的播放项不是 **AdaptiveMediaSource**，则此属性为 NULL，因此应在尝试为对象的任何事件注册处理程序之前测试是否为 NULL。

[!code-cs[AMSBindingCurrentItemChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAMSBindingCurrentItemChanged)]

## <a name="related-topics"></a>相关主题
* [媒体播放](media-playback.md)
* [HLS 标记支持](hls-tag-support.md) 
* [Dash 配置文件的支持](dash-profile-support.md) 
* [播放音频和视频使用 MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [在背景中播放媒体](background-audio.md) 






---
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
本文介绍了如何将自适应流式处理多媒体内容的播放添加到通用 Windows 平台 (UWP) 应用。 此功能当前支持 Http 实时流 (HLS) 和 HTTP 动态流 (DASH) 内容的播放。
自适应流式处理
---

# 自适应流式处理

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本文介绍了如何将自适应流式处理多媒体内容的播放添加到通用 Windows 平台 (UWP) 应用。 此功能当前支持 Http 实时流 (HLS) 和 HTTP 动态流 (DASH) 内容的播放。

## 使用 MediaElement 的简单自适应流

若要在基于 XAML 的应用中显示自适应流式处理多媒体，请将 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 控件添加到页面。

[!code-xml[MediaElementXAML](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml#SnippetMediaElementXAML)]

将 **MediaElement** 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227420) 属性设置为 DASH 或 HLS 清单文件的 URI。

[!code-cs[ManifestSource](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetManifestSource)]

## 使用 AdaptiveMediaSource 的自适应流式处理

如果你的应用需要更多高级自适应流功能（如提供自定义 HTTP 标头、监视当前下载和播放比特率或调整用于确定系统何时切换自适应流的比特率的时间），请使用 [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912) 对象。

在 [**Windows.Media.Streaming.Adaptive**](https://msdn.microsoft.com/library/windows/apps/dn931279) 命名空间中可以找到自适应流式处理 API。

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

通过调用 [**CreateFromUriAsync**](https://msdn.microsoft.com/library/windows/apps/dn931261) 来使用自适应流式处理清单文件的 URI 初始化 **AdaptiveMediaSource**。 从此方法返回的 [**AdaptiveMediaSourceCreationStatus**](https://msdn.microsoft.com/library/windows/apps/dn946917) 值可使你知道媒体源是否已成功创建。 如果成功，你可以通过调用 [**SetMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn299029) 来将该对象设置为你的 **MediaElement** 的流源。 在此示例中，将查询 [**AvailableBitrates**](https://msdn.microsoft.com/library/windows/apps/dn931257) 属性来确定此流的受支持的最大比特率，然后将该值设置为初始比特率。 本示例还为 [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272)、[**DownloadBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931269) 和 [**PlaybackBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931278) 事件注册处理程序，这些事件将在本文的后面部分进行讨论。

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

如果你需要设置自定义 HTTP 标头以用于获取清单文件，你可以创建 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 对象、设置所需的标头，然后将该对象传递到 **CreateFromUriAsync** 的重载中。

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

当系统即将从服务器检索资源时，会引发 [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272) 事件。 传递到事件处理程序的 [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn946935) 会公开提供有关要请求的资源的信息（资源的类型和 URI）的属性。

你可以通过更新事件参数所提供的 [**AdaptiveMediaSourceDownloadResult**](https://msdn.microsoft.com/library/windows/apps/dn946942) 对象的属性来使用 **DownloadRequested** 事件处理程序修改资源请求。 在以下示例中，将从中检索资源的 URI 通过更新结果对象的 [**ResourceUri**](https://msdn.microsoft.com/library/windows/apps/dn931250) 属性来进行修改。

通过设置结果对象的 [**Buffer**](https://msdn.microsoft.com/library/windows/apps/dn946943) 或 [**InputStream**](https://msdn.microsoft.com/library/windows/apps/dn931249) 属性，你可以替代所请求的资源的内容。 在以下示例中，清单资源的内容通过设置 **Buffer** 属性来替换。 请注意，如果要使用以异步方式获取的数据更新资源请求（如从远程服务器或异步用户身份验证检索数据），必须调用 [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn946936) 来获取延迟，然后在操作完成时调用 [**Complete**](https://msdn.microsoft.com/library/windows/apps/dn946934) 以向系统发送下载请求操作可以继续的信号。

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

**AdaptiveMediaSource** 对象提供可使你在下载或播放比特率发生更改时作出反应的事件。 在此示例中，当前比特率仅在 UI 中更新。 请注意，你可以修改用于确定系统何时切换自适应流的比特率的比率。 有关详细信息，请参阅 [**AdvancedSettings**](https://msdn.microsoft.com/library/windows/apps/mt628697) 属性。

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

 

 






<!--HONumber=Mar16_HO1-->



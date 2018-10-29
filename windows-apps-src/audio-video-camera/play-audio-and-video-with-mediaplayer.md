---
author: drewbatgit
ms.assetid: 58af5e9d-37a1-4f42-909c-db7cb02a0d12
description: 本文介绍了如何在通用 Windows 应用中使用 MediaPlayer 播放媒体。
title: 使用 MediaPlayer 播放音频和视频
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6b83be1dee4e23fa6974e39fbfb0f9ce26529274
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2018
ms.locfileid: "5749396"
---
# <a name="play-audio-and-video-with-mediaplayer"></a>使用 MediaPlayer 播放音频和视频

本文介绍了如何在通用 Windows 应用中使用 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 类播放媒体。 Windows10 版本 1607 对媒体播放 API 进行了显著改进，包括简化了后台音频的单进程设计、自动与系统媒体传输控件 (SMTC) 集成、能够同步多个媒体播放器、支持 Windows.UI.Composition 表面，并且提供用于创建和计划内容的媒体中断的简单界面。 若要充分利用这些改进功能，推荐用于播放媒体的最佳做法是将 **MediaPlayer** 类（而非 **MediaElement**）用于媒体播放。 引入了轻型 XAML 控件 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement)，该控件允许你在 XAML 页面中呈现媒体内容。 **MediaElement** 提供的许多播放控件和状态 API 现在都可通过新 [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession) 对象获取。 **MediaElement** 将继续运行以支持向后兼容，但不会向此类添加其他功能。

本文将向你介绍典型的媒体播放应用所使用的 **MediaPlayer** 功能。 请注意，**MediaPlayer** 将 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource) 类用作所有媒体项目的容器。 此类允许你加载并播放来自许多不同源的媒体，这些来源包括本地文件、内存流和网络源等，但使用的都是同一界面。 此外，还有更高级的类能够与 **MediaSource** 一同使用，例如 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) 和 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList)，这些类提供更加高级的功能，例如播放列表，以及通过音频、视频和元数据多轨道管理媒体源的功能。 有关 **MediaSource** 和相关 API 的详细信息，请参阅[媒体项、播放列表和曲目](media-playback-with-mediasource.md)。

> [!NOTE] 
> Windows 10 N 和 Windows 10 KN 版本不包括使用 **MediaPlayer** 进行播放所需的媒体功能。 可以手动安装这些功能。 有关详细信息，请参阅 [Windows 10 N 和 Windows 10 KN 版本的媒体功能包](https://support.microsoft.com/en-us/help/3010081/media-feature-pack-for-windows-10-n-and-windows-10-kn-editions)。

## <a name="play-a-media-file-with-mediaplayer"></a>使用 MediaPlayer 播放媒体文件  
通过 **MediaPlayer** 进行基本媒体播放非常易于实现。 首先，创建 **MediaPlayer** 类的新实例。 应用可以同时使用多个 **MediaPlayer** 实例。 接下来，将播放器的 [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) 属性设为实现 [**IMediaPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.IMediaPlaybackSource) 的对象，例如 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource)、[**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) 或 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList)。 在本例中，**MediaSource** 创建于应用本地存储中的文件，**MediaPlaybackItem** 创建于源，随后分配到播放器的 **Source** 属性。

与 **MediaElement** 不同，默认情况下，**MediaPlayer** 不会自动开始播放。 可以通过调用 [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play)、将 [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) 属性设为 True，或者等到用户使用内置媒体控件启动播放时开始播放。

[!code-cs[SimpleFilePlayback](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSimpleFilePlayback)]

当应用结束使用 **MediaPlayer** 后，应该调用 [**Close**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Close) 方法（对应 C# 中的 **Dispose**）清理播放器使用的资源。

[!code-cs[CloseMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCloseMediaPlayer)]

## <a name="use-mediaplayerelement-to-render-video-in-xaml"></a>使用 MediaPlayerElement 在 XAML 中呈现视频
可以在 **MediaPlayer** 中播放媒体，无需在 XAML 中显示，但是许多媒体播放应用会希望在 XAML 页面中呈现媒体。 为此，请使用轻型 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) 控件。 与 **MediaElement** 一样，**MediaPlayerElement** 支持指定是否应显示内置传输控件。

[!code-xml[MediaPlayerElementXAML](./code/MediaPlayer_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

可以通过调用 [**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764)，设置绑定元素的 **MediaPlayer** 实例。

[!code-cs[SetMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetMediaPlayer)]

还可以在 **MediaPlayerElement** 上设置播放源，元素将自动创建可以使用 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.MediaPlayer) 属性访问的新 **MediaPlayer** 实例。

[!code-cs[GetPlayerFromElement](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetGetPlayerFromElement)]

> [!NOTE] 
> 如果你通过将 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) 设置为 false 禁用 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 的 [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager)，它将中断 **MediaPlayer** 和 **MediaPlayerElement** 提供的 [**TransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.TransportControls) 之间的链接，以致内置传输控件不再自动控制播放器的播放。 作为替代方法，你必须实现自己的控件才能控制 **MediaPlayer**。

## <a name="common-mediaplayer-tasks"></a>MediaPlayer 常见任务
本部分介绍如何使用 **MediaPlayer** 的一些功能。

### <a name="set-the-audio-category"></a>设置音频类别
将 **MediaPlayer** 的 [**AudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioCategory) 属性设为 [**MediaPlayerAudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerAudioCategory) 枚举的其中一个值，让系统知道播放的媒体种类。 游戏应该将其音乐流归类为 **GameMedia**，以使游戏音乐在其他应用程序在后台播放音乐时自动静音。 音乐或视频应用程序应该将它们的流归类为 **Media** 或 **Movie**，以使它们能够优先于 **GameMedia** 流。

[!code-cs[SetAudioCategory](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioCategory)]

### <a name="output-to-a-specific-audio-endpoint"></a>输出到特定音频终结点
默认情况下，**MediaPlayer** 的音频输出路由到系统的默认音频终结点，但是可以指定 **MediaPlayer** 应该用于输出的特定音频终结点。 在以下示例中，[**MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.MediaDevice.GetAudioRenderSelector) 返回了唯一标识设备音频呈现类别的字符串。 接下来，调用 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation) 方法 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation.FindAllAsync) 获取所选类型的所有可用设备列表。 可以通过编程方式确定希望使用的设备，或者将返回的设备添加到 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox) 以允许用户选择设备。

[!code-cs[SetAudioEndpointEnumerate](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpointEnumerate)]

在用于设备组合框的 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged) 事件中，**MediaPlayer** 的 [**AudioDevice**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioDevice) 属性设置为所选设备，该属性存储在 **ComboBoxItem** 的 [**Tag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Tag) 属性中。

[!code-cs[SetAudioEndpontSelectionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpontSelectionChanged)]

### <a name="playback-session"></a>播放会话
如前文所述，许多由 **MediaElement** 类公开的函数都已移到 [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession) 类。 这包括有关播放器播放状态的信息，例如当前播放位置、播放器是已暂停还是正在播放，以及当前播放速度。 **MediaPlaybackSession** 还会提供几个事件，用于在状态更改时通知用户，这些更改包括所播放内容的当前缓冲和下载状态，以及当前播放视频内容的自然大小和纵横比。

以下示例演示了如何实现向前跳过 10 秒内容的按钮单击处理程序。 首先，使用 [**PlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.PlaybackSession) 属性检索播放器的 **MediaPlaybackSession** 对象。 接下来，将 [**Position**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.Position) 属性设置为当前播放位置向后 10 秒的值。

[!code-cs[SkipForwardClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSkipForwardClick)]

下一个示例介绍通过设置会话的 [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.PlaybackRate) 属性，使用切换按钮在正常播放速度和 2 倍播放速度之间切换。

[!code-cs[SpeedChecked](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSpeedChecked)]

从 Windows 10 版本 1803 开始，可以对在 **MediaPlayer** 中显示的视频设置旋转（以 90 度为增量）。

[!code-cs[SetRotation](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetRotation)]

### <a name="detect-expected-and-unexpected-buffering"></a>检测预期和非预期缓冲
上一节中所述的 **MediaPlaybackSession** 对象提供了两个事件（**[BufferingStarted](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.BufferingStarted)** 和 **[BufferingEnded](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.BufferingEnded)**）用于检测当前播放的媒体文件何时开始和结束缓冲。 这允许更新 UI 以向用户显示正在发生缓冲。 当媒体文件首次打开或用户切换到播放列表中的新项时，需要进行初始缓冲。 当网络速度降低或提供内容的内容管理系统遇到技术问题时，可能会发生非预期缓冲。 从 RS3 开始，可以使用 **BufferingStarted** 事件来确定缓冲事件是预期的还是非预期的并会中断播放。 此信息可以用作应用或媒体传送服务的遥测数据。 

为 **BufferingStarted** 和 **BufferingEnded** 事件注册处理程序以接收缓冲状态通知。

[!code-cs[RegisterBufferingHandlers](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterBufferingHandlers)]

在 **BufferingStarted** 事件处理程序中，将传入事件的事件参数投放到 **[MediaPlaybackSessionBufferingStartedEventArgs](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs)** 对象并检查 **[IsPlaybackInterruption](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs.IsPlaybackInterruption)** 属性。 如果此值为 true，则触发事件的缓冲为非预期并中断播放。 否则为预期初始缓冲。 

[!code-cs[BufferingHandlers](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetBufferingHandlers)]


### <a name="pinch-and-zoom-video"></a>使用收缩手势缩放视频
**MediaPlayer** 支持在视频内容中指定应该呈现的源矩形，这可有效地支持视频放大。 所指定的矩形与规范化矩形 (0,0,1,1) 成比例，其中 0,0 是帧的左上角，1,1 指定帧的全宽和全高。 因此，如果要设置缩放矩形以呈现视频的右上角象限，需指定矩形 (.5,0,.5,.5)。  请务必检查这些值，确保源矩形位于 (0,0,1,1) 规范化矩形内。 尝试设置超出此范围的值将引发异常。

若要使用多点触控手势实现收缩以缩放，必须先指定要支持的手势。 在本例中，请求了缩放和平移手势。 发生一个订阅手势即会引发 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.ManipulationDelta) 事件。 [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped) 事件用于将缩放重置为完整的帧。 

[!code-cs[RegisterPinchZoomEvents](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPinchZoomEvents)]

接下来，声明存储当前缩放源矩形的 **Rect** 对象。

[!code-cs[DeclareSourceRect](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareSourceRect)]

**ManipulationDelta** 处理程序调整缩放矩形的缩放或平移。 如果增量缩放值不为 1，则意味着用户执行了收缩手势。 如果值大于 1，源矩形应变小以放大内容。 如果值小于 1，则应使源矩形变大以缩小。在设置新的缩放值之前，将检查生成的矩形以确保其完全位于 (0,0,1,1) 限制内。

如果缩放值为 1，则处理平移手势。 矩形仅根据手势的像素数除以控件的宽度和高度所得结果平移。 同样，检查生成的矩形，确保它位于 (0,0,1,1) 边界内。

最后，将 **MediaPlaybackSession** 的 [**NormalizedSourceRect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NormalizedSourceRect) 设置为新调整的矩形，从而指定视频帧中应该呈现的区域。

[!code-cs[ManipulationDelta](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

在 [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped) 事件处理程序中，源矩形重新设为 (0,0,1,1)，以呈现整个视频帧。

[!code-cs[DoubleTapped](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDoubleTapped)]

### <a name="handling-policy-based-playback-degradation"></a>处理基于策略的播放降级

在某些情况下，系统可能会根据策略而不是性能问题对媒体项的播放进行降级，例如降低分辨率（压缩）。 例如，如果使用未签名的视频驱动程序播放视频，则系统可能会对视频进行降级。 可以调用 [**MediaPlaybackSession.GetOutputDegradationPolicyState**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.getoutputdegradationpolicystate#Windows_Media_Playback_MediaPlaybackSession_GetOutputDegradationPolicyState) 来确定是否以及为什么会发生这种基于策略的降级，并提醒用户或记录原因用于遥测。

下面的示例显示了 **MediaPlayer.MediaOpened** 事件的处理程序的实现，该事件是在播放机打开新媒体项时引发的。 在传递给处理程序的 **MediaPlayer** 上调用 **GetOutputDegradationPolicyState**。 [**VideoConstrictionReason**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksessionoutputdegradationpolicystate.videoconstrictionreason#Windows_Media_Playback_MediaPlaybackSessionOutputDegradationPolicyState_VideoConstrictionReason) 的值指示视频被压缩的策略原因。 如果值不是 **None**，则此示例记录降级原因用于遥测。 此示例还显示将当前播放的 **AdaptiveMediaSource** 的比特率设置为最低带宽以节省数据使用量，因为视频被压缩，并且不会以高分辨率显示。 有关使用 **AdaptiveMediaSource** 的详细信息，请参阅[自适应流式处理](adaptive-streaming.md)。

[!code-cs[PolicyDegradation](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPolicyDegradation)]
        
## <a name="use-mediaplayersurface-to-render-video-to-a-windowsuicomposition-surface"></a>使用 MediaPlayerSurface 将视频呈现到 Windows.UI.Composition 界面
从 Windows10 版本 1607 开始，可以使用 **MediaPlayer** 将视频呈现到 [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ICompositionSurface)，这可以支持播放器与 [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition) 命名空间中的 API 进行互操作。 合成框架允许你在 XAML 与低级别 DirectX 图形 API 之间的可视化层处理图形。 这可以使任意 XAML 控件都能够执行呈现视频等方案。 有关使用合成 API 的详细信息，请参阅[可视化层](https://msdn.microsoft.com/windows/uwp/composition/visual-layer)。

以下示例介绍了如何将视频播放器内容呈现到 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Canvas) 控件。 本例中特定于媒体播放器的调用为 [**SetSurfaceSize**](https://msdn.microsoft.com/library/windows/apps/mt489968) 和 [**GetSurface**](https://msdn.microsoft.com/library/windows/apps/mt489963)。 **SetSurfaceSize** 告知系统为呈现内容应该分配的缓冲区大小。 **GetSurface** 将 [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.Compositor) 视作参数，并且检索 [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface) 类的实例。 使用此类可以访问用于通过 [**CompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface.CompositionSurface) 属性创建表面和公开表面自身的 **MediaPlayer** 和 **Compositor**。

本例中的其余代码创建视频要呈现到的 [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual)，还将大小设为显示视觉效果的画布元素大小。 接下来，从 [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface) 创建 [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.CompositionBrush)，并将其分配到视觉效果的 [**Brush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual.Brush) 属性。 然后，创建 [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ContainerVisual)，在它的可视树顶部插入 **SpriteVisual**。 最后，调用 [**SetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/mt608981)，将容器视觉效果分配到 **Canvas**。

[!code-cs[Compositor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCompositor)]
        
## <a name="use-mediatimelinecontroller-to-synchronize-content-across-multiple-players"></a>使用 MediaTimelineController 跨多台播放器同步内容。
如前文所述，应用可以同时使用多个 **MediaPlayer** 对象。 默认情况下，所创建的每个 **MediaPlayer** 都独立操作。 在某些情况下（例如同步视频的解说音轨），你可能希望同步多台播放器的状态、播放位置和播放速度。 从 Windows10 版本 1607 开始，可以使用 [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController) 类实现这种行为。

### <a name="implement-playback-controls"></a>实现播放控件
以下示例展示了如何使用 **MediaTimelineController** 控制 **MediaPlayer** 的两个实例。 首先，**MediaPlayer** 的每个实例均进行实例化，并且 **Source** 设置为媒体文件。 接下来，创建新 **MediaTimelineController**。 对于每个 **MediaPlayer**，将 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) 属性设为 False 禁用与每台播放器关联的 [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager)。 然后，将 [**TimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineController) 属性设置为时间线控制器对象。

[!code-cs[DeclareMediaTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaTimelineController)]

[!code-cs[SetTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetTimelineController)]

**警告** [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) 在 **MediaPlayer** 和系统媒体传输控件 (SMTC) 之间提供自动集成，但是这种自动集成无法和使用 **MediaTimelineController** 控制的媒体播放器一起使用。 因此，必须先禁用媒体播放器的命令管理器才能设置播放器的时间线控制器。 否则将引发异常，并且显示以下消息：“由于对象的当前状态，附加媒体时间线控制器的操作被阻止”。 有关媒体播放器与 SMTC 集成的详细信息，请参阅[与系统媒体传输控件集成](integrate-with-systemmediatransportcontrols.md)。 使用 **MediaTimelineController** 时仍然可以手动控制 SMTC。 有关详细信息，请参阅[手动控制系统媒体传输控件](system-media-transport-controls.md)。

将 **MediaTimelineController** 附加到一个或多个媒体播放器后，可以通过使用控制器公开的方法控制播放状态。 以下示例调用 [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController.Start) 使所有关联的媒体播放器从媒体开头处开始播放。

[!code-cs[PlayButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPlayButtonClick)]

本例介绍暂停并恢复所有附加的媒体播放器。

[!code-cs[PauseButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPauseButtonClick)]

为了使所有连接的媒体播放器快进，请将播放速度设置为大于 1 的值。

[!code-cs[FastForwardButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFastForwardButtonClick)]

下一个示例介绍如何使用 **Slider** 控件显示时间线控制器的当前播放位置，该位置与某一个连接的媒体播放器播放的内容持续时间有关。 首先，创建新 **MediaSource**，并且为媒体源的 [**OpenOperationCompleted**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource.OpenOperationCompleted) 注册处理程序。 

[!code-cs[CreateSourceWithOpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCreateSourceWithOpenCompleted)]

使用 **OpenOperationCompleted** 处理程序有机会发现媒体源内容的持续时间。 确定持续时间后，**Slider** 控件的最大值设置为媒体项的总秒数。 此值在 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 调用中设置，以确保它在 UI 线程上运行。

[!code-cs[DeclareDuration](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareDuration)]

[!code-cs[OpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenCompleted)]

接下来，为时间线控制器的 [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController.PositionChanged) 事件注册处理程序。 系统将定期调用它，频率大约为每秒 4 次。

[!code-cs[RegisterPositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPositionChanged)]

在 **PositionChanged** 的处理程序中，更新滑块值以反映时间线控制器的当前位置。

[!code-cs[PositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPositionChanged)]

### <a name="offset-the-playback-position-from-the-timeline-position"></a>使播放位置偏离时间线位置
在某些情况下，与时间线控制器关联的一个或多个媒体播放器的播放位置可能需要偏离其他播放器。 为此，可以设置要偏离的 **MediaPlayer** 对象的 [**TimelineControllerPositionOffset**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineControllerPositionOffset) 属性。 以下示例使用两个媒体播放器的内容持续时间设置两个滑块控件的最小值和最大值，以增加或减少项目的时间长度。  

[!code-cs[OffsetSliders](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOffsetSliders)]

在每个滑块的 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.RangeBase.ValueChanged) 事件中，将每个播放器的 **TimelineControllerPositionOffset** 设置为相应值。

[!code-cs[TimelineOffset](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetTimelineOffset)]

请注意，如果播放器的偏离值映射到反向播放位置，剪辑将保持暂停状态，直到偏离值达到零后才开始播放。 同样地，如果偏离值映射到的播放位置大于媒体项的持续时间，将显示最后一帧，就像单个媒体播放器播放完内容一样。

## <a name="play-spherical-video-with-mediaplayer"></a>使用 MediaPlayer 播放球面视频
从 Windows 10 版本 1703 开始，**MediaPlayer** 支持用于球面视频播放的 equirectangular 投影。 由于只要视频编码受支持，**MediaPlayer** 便会呈现视频，因此球面视频内容与常规的平面视频没什么不同。 对于包含的元数据标记指定视频使用 equirectangular 投影的球面视频，**MediaPlayer** 可以使用指定视野和视图方向呈现视频。 这样可实现使用头盔显示屏的虚拟现实视频播放或是仅仅允许用户使用鼠标或键盘输入在球面视频中四处平移这类方案。

若要播放球面视频，请使用本文前面介绍的用于播放视频内容的步骤。 还有一个步骤是为 [**MediaPlayer.MediaOpened**])https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer#Windows_Media_Playback_MediaPlayer_MediaOpened) 事件注册处理程序。 此事件使你可以启用并控制球面视频播放参数。

[!code-cs[OpenSphericalVideo](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenSphericalVideo)]

在 **MediaOpened** 处理程序中，首先通过检查 [**PlaybackSession.SphericalVideoProjection.FrameFormat**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.FrameFormat) 属性来检查新打开的媒体项的帧格式。 如果此值为 [**SphericaVideoFrameFormat.Equirectangular**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat)，则系统可以自动投影视频内容。 首先，将 [**PlaybackSession.SphericalVideoProjection.IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.IsEnabled) 属性设置为 **true**。 还可以调整媒体播放器会用于投影视频内容的属性（如视图方向和视野）。 在此示例中，通过设置 [**HorizontalFieldOfViewInDegrees**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.HorizontalFieldOfViewInDegrees) 属性，将视野设置为较宽值 120 度。

如果视频内容是球面的，但采用 equirectangular 以外的格式，则可以通过使用媒体播放器的帧服务器模式接收和处理各个帧来实现自己的投影算法。

[!code-cs[SphericalMediaOpened](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalMediaOpened)]

以下示例代码演示如何使用向左和向右箭头键来调整球面视频视图方向。

[!code-cs[SphericalOnKeyDown](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalOnKeyDown)]

如果应用支持视频的播放列表，则可能要在 UI 中标识包含球面视频的播放项。 媒体播放列表在文章[媒体项、播放列表和轨](media-playback-with-mediasource.md)中进行了详细讨论。 以下示例演示如何创建新播放列表、添加项以及为 [**MediaPlaybackItem.VideoTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.VideoTracksChanged) 事件（解析媒体项的视频轨时发生）注册处理程序。

[!code-cs[SphericalList](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalList)]

在 **VideoTracksChanged** 事件处理程序中，通过调用 [**VideoTrack.GetEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.core.videotrack.GetEncodingProperties) 来获取任何已添加视频轨的编码属性。 如果编码属性的 [**SphericalVideoFrameFormat**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.videoencodingproperties.SphericalVideoFrameFormat) 属性是 [**SphericaVideoFrameFormat.None**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat) 之外的值，则视频轨包含球面视频，可以相应地更新 UI（如果选择）。

[!code-cs[SphericalTracksChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalTracksChanged)]

## <a name="use-mediaplayer-in-frame-server-mode"></a>在帧服务器模式中使用 MediaPlayer
从 Windows 10 版本 1703 开始，可以在帧服务器模式中使用 **MediaPlayer**。 在此模式中，**MediaPlayer** 不会自动将帧呈现到关联的 **MediaPlayerElement**。 相反，应用会将当前帧从 **MediaPlayer** 复制到实现 [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface) 的对象。 此功能实现的主要方案是使用像素着色器处理 **MediaPlayer** 提供的视频帧。 应用负责在处理之后显示每个帧，如通过在 XAML [**Image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) 控件中显示帧。

在以下示例中，会初始化新 **MediaPlayer** 并加载视频内容。 接下来，会注册 [**VideoFrameAvailable**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.VideoFrameAvailable) 的处理程序。 通过将 **MediaPlayer** 对象的 [**IsVideoFrameServerEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.IsVideoFrameServerEnabled) 属性设置为 **true** 来启用帧服务器模式。 最后，使用 [**Play**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play) 调用开始媒体播放。

[!code-cs[FrameServerInit](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFrameServerInit)]

下一个示例演示 **VideoFrameAvailable** 的处理程序，它使用 [Win2D](https://github.com/Microsoft/Win2D) 将简单模糊效果添加到视频的每个帧，然后在 XAML [Image](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) 控件中显示经过处理的帧。

每当调用 **VideoFrameAvailable** 处理程序时，都会使用 [**CopyFrameToVideoSurface**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.copyframetovideosurface) 方法将帧的内容复制到 [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface)。 还可以使用 [**CopyFrameToStereoscopicVideoSurfaces**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.copyframetostereoscopicvideosurfaces) 将 3D 内容复制到两个图面，以便单独处理左眼和右眼内容。 为了获取实现 **IDirect3DSurface** 的对象，此示例创建一个 [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap)，然后使用该对象创建实现所需接口的 Win2D **CanvasBitmap**。 **CanvasImageSource** 是 Win2D 对象，可以用作 **Image** 控件的源，因此会新建一个并设置为将在其中显示内容的 **Image** 的源。 接下来创建一个 **CanvasDrawingSession**。 这由 Win2D 用于呈现模糊效果。

实例化了所有所需对象之后，会调用 **CopyFrameToVideoSurface**，它将当前帧从 **MediaPlayer** 复制到 **CanvasBitmap** 中。 接下来，创建一个 Win2D **GaussianBlurEffect**，其 **CanvasBitmap** 设置为操作的源。 最后，调用 **CanvasDrawingSession.DrawImage** 以将应用了模糊效果的源图像绘制到与 **Image** 控件关联的 **CanvasImageSource** 中，从而在 UI 中绘制它。

[!code-cs[VideoFrameAvailable](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetVideoFrameAvailable)]

有关 Win2D 的详细信息，请参阅 [Win2D GitHub 存储库](https://github.com/Microsoft/Win2D)。 若要尝试上面显示的示例代码，需要按照以下说明将 Win2D NuGet 程序包添加到项目。

**将 Win2D NuGet 程序包添加到你的效果项目**

1.  在**解决方案资源管理器**中，右键单击项目，然后选择**管理 NuGet 程序包**。
2.  在窗口顶部，选择**浏览**选项卡。
3.  在搜索框中，输入 **Win2D**。
4.  选择 **Win2D.uwp**，然后选择右侧窗格中的**安装**。
5.  **查看更改**对话框将向你显示要安装的程序包。 单击**确定**。
6.  接受程序包许可证。

## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>检测系统的音频级别更改并做出响应
从 Windows 10 版本 1803 开始，应用可检测到系统何时降低或静音当前播放的 **MediaPlayer** 的音频级别。 例如，当警报响起时，系统可能降低（或者“闪避”）音频播放级别。 如果应用没有在应用清单中声明 *backgroundMediaPlayback* 功能，系统将在应用进入后台时将其静音。 [**AudioStateMonitor**](https://docs.microsoft.comuwp/api/windows.media.audio.audiostatemonitor) 类可用于注册以接收系统修改音频流的音量时出现的事件。 访问 **MediaPlayer** 的 **AudioStateMonitor** 属性并为 [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged)事件注册处理程序，以在系统更改该 **MediaPlayer** 的音频级别时收到通知。

[!code-cs[RegisterAudioStateMonitor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterAudioStateMonitor)]

在处理 **SoundLevelChanged** 事件时，可以根据正在播放的内容的类型采取不同的操作。 如果当前正在播放音乐，可能希望在音量闪避时继续播放音乐。 但是，如果正在播放播客，那么可能希望在音频闪避时暂停播放，这样用户就不会错过任何内容。

此示例声明一个变量，该变量跟踪当前播放的内容是否为播客，假设在为 **MediaPlayer** 选择内容时将其设置为适当的值。 我们还创建类变量来跟踪在音频级别发生变化时何时以编程方式暂停播放。

[!code-cs[AudioStateVars](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

在 **SoundLevelChanged** 事件处理程序中，检查 **AudioStateMonitor** 发件人的 [**SoundLevel**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) 属性，以确定新的音量。 此示例检查新的音量是否为完整音量，即系统已停止静音或闪避音量，或者音量是否已经降低，但正在播放非播客内容。 如果发生其中任何一种情况，并且内容以前以编程方式暂停，则恢复播放。 如果新的音量为静音，或者如果当前内容是播客，并且音量较低，则暂停播放，并将变量设置为跟踪，暂停以编程方式启动。

[!code-cs[SoundLevelChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSoundLevelChanged)]

用户可以决定想要暂停还是继续播放，即使系统闪避音频。 此示例显示播放和暂停按钮的事件处理程序。 在暂停按钮中单击处理程序暂停，如果播放已经以编程方式暂停，则更新变量以指示用户已暂停内容。 在播放按钮中单击处理程序，继续播放并清除跟踪变量。

[!code-cs[ButtonUserClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetButtonUserClick)]

## <a name="related-topics"></a>相关主题
* [媒体播放](media-playback.md)
* [媒体项、播放列表和曲目](media-playback-with-mediasource.md)
* [与系统媒体传输控件集成](integrate-with-systemmediatransportcontrols.md)
* [创建、计划和管理媒体中断](create-schedule-and-manage-media-breaks.md)
* [在后台播放媒体](background-audio.md)





 

 





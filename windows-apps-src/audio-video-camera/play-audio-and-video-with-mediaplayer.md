---
author: drewbatgit
ms.assetid: 
description: "本文介绍了如何在通用 Windows 应用中使用 MediaPlayer 播放媒体。"
title: "使用 MediaPlayer 播放音频和视频"
translationtype: Human Translation
ms.sourcegitcommit: 3d6f79ea55718d988415557bc4ac9a1f746f9053
ms.openlocfilehash: 32df2810710e78eeb8c257548c39c0d5d978e888

---

# 使用 MediaPlayer 播放音频和视频

本文介绍了如何在通用 Windows 应用中使用 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 类播放媒体。 Windows 10 版本 1607 对媒体播放 API 进行了显著改进，包括简化了后台音频的单进程设计、自动与系统媒体传输控件 (SMTC) 集成、能够同步多个媒体播放器、支持 Windows.UI.Composition 表面，并且提供用于创建和计划内容的媒体中断的简单界面。 若要充分利用这些改进功能，推荐用于播放媒体的最佳做法是将 **MediaPlayer** 类（而非 **MediaElement**）用于媒体播放。 引入了轻型 XAML 控件 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement)，该控件允许你在 XAML 页面中呈现媒体内容。 **MediaElement** 提供的许多播放控件和状态 API 现在都可通过新 [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession) 对象获取。 **MediaElement** 将继续运行以支持向后兼容，但不会向此类添加其他功能。

本文将向你介绍典型的媒体播放应用所使用的 **MediaPlayer** 功能。 请注意，**MediaPlayer** 将 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource) 类用作所有媒体项目的容器。 此类允许你加载并播放来自许多不同源的媒体，这些来源包括本地文件、内存流和网络源等，但使用的都是同一界面。 此外，还有更高级的类能够与 **MediaSource** 一同使用，例如 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) 和 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList)，这些类提供更加高级的功能，例如播放列表，以及通过音频、视频和元数据多轨道管理媒体源的功能。 有关 **MediaSource** 和相关 API 的详细信息，请参阅[媒体项、播放列表和曲目](media-playback-with-mediasource.md)。


##使用 MediaPlayer 播放媒体文件  
通过 **MediaPlayer** 进行基本媒体播放非常易于实现。 首先，创建 **MediaPlayer** 类的新实例。 应用可以同时使用多个 **MediaPlayer** 实例。 接下来，将播放器的 [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) 属性设为实现 [**IMediaPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.IMediaPlaybackSource) 的对象，例如 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource)、[**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) 或 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList)。 在本例中，**MediaSource** 创建于应用本地存储中的文件，**MediaPlaybackItem** 创建于源，随后分配到播放器的 **Source** 属性。

与 **MediaElement** 不同，默认情况下，**MediaPlayer** 不会自动开始播放。 可以通过调用 [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play)、将 [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) 属性设为 True，或者等到用户使用内置媒体控件启动播放时开始播放。

[!code-cs[SimpleFilePlayback](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSimpleFilePlayback)]

当应用结束使用 **MediaPlayer** 后，应该调用 [**Close**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Close) 方法（对应 C# 中的 **Dispose**）清理播放器使用的资源。

[!code-cs[CloseMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCloseMediaPlayer)]

##使用 MediaPlayerElement 在 XAML 中呈现视频
可以在 **MediaPlayer** 中播放媒体，无需在 XAML 中显示，但是许多媒体播放应用会希望在 XAML 页面中呈现媒体。 为此，请使用轻型 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) 控件。 与 **MediaElement** 一样，**MediaPlayerElement** 支持指定是否应显示内置传输控件。

[!code-xml[MediaPlayerElementXAML](./code/MediaPlayer_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

可以通过调用 [**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764)，设置绑定元素的 **MediaPlayer** 实例。

[!code-cs[SetMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetMediaPlayer)]

还可以在 **MediaPlayerElement** 上设置播放源，元素将自动创建可以使用 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.MediaPlayer) 属性访问的新 **MediaPlayer** 实例。

[!code-cs[GetPlayerFromElement](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetGetPlayerFromElement)]

##MediaPlayer 常见任务
本部分介绍如何使用 **MediaPlayer** 的一些功能。

###设置音频类别
将 **MediaPlayer** 的 [**AudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioCategory) 属性设为 [**MediaPlayerAudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerAudioCategory) 枚举的其中一个值，让系统知道播放的媒体种类。 游戏应该将其音乐流归类为 **GameMedia**，以使游戏音乐在其他应用程序在后台播放音乐时自动静音。 音乐或视频应用程序应该将它们的流归类为 **Media** 或 **Movie**，以使它们能够优先于 **GameMedia** 流。

[!code-cs[SetAudioCategory](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioCategory)]

###输出到特定音频终结点
默认情况下，**MediaPlayer** 的音频输出路由到系统的默认音频终结点，但是可以指定 **MediaPlayer** 应该用于输出的特定音频终结点。 在以下示例中，[**MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.MediaDevice.GetAudioRenderSelector) 返回了唯一标识设备音频呈现类别的字符串。 接下来，调用 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation) 方法 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation.FindAllAsync) 获取所选类型的所有可用设备列表。 可以通过编程方式确定希望使用的设备，或者将返回的设备添加到 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox) 以允许用户选择设备。

[!code-cs[SetAudioEndpointEnumerate](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpointEnumerate)]

在用于设备组合框的 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged) 事件中，**MediaPlayer** 的 [**AudioDevice**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioDevice) 属性设置为所选设备，该属性存储在 **ComboBoxItem** 的 [**Tag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Tag) 属性中。

[!code-cs[SetAudioEndpontSelectionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpontSelectionChanged)]

###播放会话
如前文所述，许多由 **MediaElement** 类公开的函数都已移到 [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession) 类。 这包括有关播放器播放状态的信息，例如当前播放位置、播放器是已暂停还是正在播放，以及当前播放速度。 **MediaPlaybackSession** 还会提供几个事件，用于在状态更改时通知用户，这些更改包括所播放内容的当前缓冲和下载状态，以及当前播放视频内容的自然大小和纵横比。

以下示例演示了如何实现向前跳过 10 秒内容的按钮单击处理程序。 首先，使用 [**PlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.PlaybackSession) 属性检索播放器的 **MediaPlaybackSession** 对象。 接下来，将 [**Position**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.Position) 属性设置为当前播放位置向后 10 秒的值。

[!code-cs[SkipForwardClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSkipForwardClick)]

下一个示例介绍通过设置会话的 [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.PlaybackRate) 属性，使用切换按钮在正常播放速度和 2 倍播放速度之间切换。

[!code-cs[SpeedChecked](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSpeedChecked)]

###使用收缩手势缩放视频
**MediaPlayer** 支持在视频内容中指定应该呈现的源矩形，这可有效地支持视频放大。 所指定的矩形与规范化矩形 (0,0,1,1) 成比例，其中 0,0 是帧的左上角，1,1 指定帧的全宽和全高。 因此，如果要设置缩放矩形以呈现视频的右上角象限，需指定矩形 (.5,0,.5,.5)。  请务必检查这些值，确保源矩形位于 (0,0,1,1) 规范化矩形内。 尝试设置超出此范围的值将引发异常。

若要使用多点触控手势实现收缩以缩放，必须先指定要支持的手势。 在本例中，请求了缩放和平移手势。 发生一个订阅手势即会引发 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.ManipulationDelta) 事件。 [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped) 事件用于将缩放重置为完整的帧。 

[!code-cs[RegisterPinchZoomEvents](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPinchZoomEvents)]

接下来，声明存储当前缩放源矩形的 **Rect** 对象。

[!code-cs[DeclareSourceRect](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareSourceRect)]

**ManipulationDelta** 处理程序调整缩放矩形的缩放或平移。 如果增量缩放值不为 1，则意味着用户执行了收缩手势。 如果值大于 1，源矩形应变小以放大内容。 如果值小于 1，源矩形应变大以缩小内容。 在设置新缩放值前，请检查生成的矩形，确保它完全处于 (0,0,1,1) 限定范围内。

如果缩放值为 1，则处理平移手势。 矩形仅根据手势的像素数除以控件的宽度和高度所得结果平移。 同样，检查生成的矩形，确保它位于 (0,0,1,1) 边界内。

最后，将 **MediaPlaybackSession** 的 [**NormalizedSourceRect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NormalizedSourceRect) 设置为新调整的矩形，从而指定视频帧中应该呈现的区域。

[!code-cs[ManipulationDelta](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

在 [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped) 事件处理程序中，源矩形重新设为 (0,0,1,1)，以呈现整个视频帧。

[!code-cs[DoubleTapped](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDoubleTapped)]
        
##使用 MediaPlayerSurface 将视频呈现到 Windows.UI.Composition 界面
从 Windows 10 版本 1607 开始，可以使用 **MediaPlayer** 将视频呈现到 [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ICompositionSurface)，这可以支持播放器与 [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition) 命名空间中的 API 进行互操作。 合成框架允许你在 XAML 与低级别 DirectX 图形 API 之间的可视化层处理图形。 这可以使任意 XAML 控件都能够执行呈现视频等方案。 有关使用合成 API 的详细信息，请参阅[可视化层](https://msdn.microsoft.com/windows/uwp/graphics/visual-layer)。

以下示例介绍了如何将视频播放器内容呈现到 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Canvas) 控件。 本例中特定于媒体播放器的调用为 [**SetSurfaceSize**](https://msdn.microsoft.com/library/windows/apps/mt489968) 和 [**GetSurface**](https://msdn.microsoft.com/library/windows/apps/mt489963)。 **SetSurfaceSize** 告知系统为呈现内容应该分配的缓冲区大小。 **GetSurface** 将 [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.Compositor) 视作参数，并且检索 [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface) 类的实例。 使用此类可以访问用于通过 [**CompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface.CompositionSurface) 属性创建表面和公开表面自身的 **MediaPlayer** 和 **Compositor**。

本例中的其余代码创建视频要呈现到的 [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual)，还将大小设为显示视觉效果的画布元素大小。 接下来，从 [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface) 创建 [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.CompositionBrush)，并将其分配到视觉效果的 [**Brush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual.Brush) 属性。 然后，创建 [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ContainerVisual)，在它的可视树顶部插入 **SpriteVisual**。 最后，调用 [**SetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/mt608981)，将容器视觉效果分配到 **Canvas**。

[!code-cs[Compositor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCompositor)]
        
##使用 MediaTimelineController 跨多台播放器同步内容。
如前文所述，应用可以同时使用多个 **MediaPlayer** 对象。 默认情况下，所创建的每个 **MediaPlayer** 都独立操作。 在某些情况下（例如同步视频的解说音轨），你可能希望同步多台播放器的状态、播放位置和播放速度。 从 Windows 10 版本 1607 开始，可以使用 [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController) 类实现这种行为。

###实现播放控件
以下示例展示了如何使用 **MediaTimelineController** 控制 **MediaPlayer** 的两个实例。 首先，**MediaPlayer** 的每个实例均进行实例化，并且 **Source** 设置为媒体文件。 接下来，创建新 **MediaTimelineController**。 对于每个 **MediaPlayer**，将 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) 属性设为 False 禁用与每台播放器关联的 [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager)。 然后，[**TimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineController) 属性设置为时间线控制器对象。

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

###使播放位置偏离时间线位置
在某些情况下，与时间线控制器关联的一个或多个媒体播放器的播放位置可能需要偏离其他播放器。 为此，可以设置要偏离的 **MediaPlayer** 对象的 [**TimelineControllerPositionOffset**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineControllerPositionOffset) 属性。 以下示例使用两个媒体播放器的内容持续时间设置两个滑块控件的最小值和最大值，以增加或减少项目的时间长度。  

[!code-cs[OffsetSliders](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOffsetSliders)]

在每个滑块的 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.RangeBase.ValueChanged) 事件中，将每个播放器的 **TimelineControllerPositionOffset** 设置为相应值。

[!code-cs[TimelineOffset](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetTimelineOffset)]

请注意，如果播放器的偏离值映射到反向播放位置，剪辑将保持暂停状态，直到偏离值达到零后才开始播放。 同样地，如果偏离值映射到的播放位置大于媒体项的持续时间，将显示最后一帧，就像单个媒体播放器播放完内容一样。

## 相关主题
* [媒体播放](media-playback.md)
* [媒体项、播放列表和曲目](media-playback-with-mediasource.md)
* [与系统媒体传输控件集成](integrate-with-systemmediatransportcontrols.md)
* [创建、计划和管理媒体中断](create-schedule-and-manage-media-breaks.md)
* [在后台播放媒体](background-audio.md)





 

 







<!--HONumber=Aug16_HO3-->



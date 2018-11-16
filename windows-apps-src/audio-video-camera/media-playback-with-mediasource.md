---
author: drewbatgit
ms.assetid: C5623861-6280-4352-8F22-80EB009D662C
description: 本文介绍了如何使用 MediaSource，该类提供从不同的源（例如本地或远程文件）引用和播放媒体的常用方法，并公开用于访问媒体数据的常用模型，而不考虑基础媒体格式。
title: 媒体项、播放列表和轨
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 73b6a19e2385f1a9b8afa4672df50d17ac16ec97
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2018
ms.locfileid: "6835036"
---
# <a name="media-items-playlists-and-tracks"></a>媒体项、播放列表和轨


 本文介绍了如何使用 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource) 类，该类提供从不同的源（例如本地或远程文件）引用和播放媒体的常用方法，并公开用于访问媒体数据的常用模型，而不考虑基础媒体格式。 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) 类扩展 **MediaSource** 的功能，从而允许你管理并从媒体项中所含的多个音频、视频和元数据曲目中进行选择。 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) 允许你从一个或多个媒体播放项中创建播放列表。


## <a name="create-and-play-a-mediasource"></a>创建并播放 MediaSource

通过调用由该类公开的工厂方法之一来创建 **MediaSource** 的新实例：

-   [**CreateFromAdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930906)
-   [**CreateFromIMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn965527)
-   [**CreateFromMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930907)
-   [**CreateFromMseStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930908)
-   [**CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909)
-   [**CreateFromStream**](https://msdn.microsoft.com/library/windows/apps/dn930910)
-   [**CreateFromStreamReference**](https://msdn.microsoft.com/library/windows/apps/dn930911)
-   [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912)
-   [**CreateFromDownloadOperation**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromdownloadoperation)

创建 **MediaSource** 后，可以通过设置 [**Source**](https://msdn.microsoft.com/library/windows/apps/dn987010) 属性来使用 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652535) 播放它。 从 Windows10 版本 1607 开始，你可以通过调用 [**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764) 将 **MediaPlayer** 分配给 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement)，以便在 XAML 页面中呈现媒体播放器内容。 这是优先于使用 **MediaElement** 的方法。 有关使用 **MediaPlayer** 的详细信息，请查阅[**使用 MediaPlayer 播放音频和视频**](play-audio-and-video-with-mediaplayer.md)。

以下示例演示如何在 **MediaPlayer** 中使用 **MediaSource** 播放用户选择的媒体文件。

将需要包含 [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) 和 [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) 命名空间才能完成此方案。

[!code-cs[Using](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetUsing)]

声明类型 **MediaSource** 的变量。 对于本文中的示例，媒体源声明为类成员，以便可以从多个位置访问它。

[!code-cs[DeclareMediaSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaSource)]

声明一个变量来存储 **MediaPlayer** 对象，并向页面添加 **MediaPlayerElement** 控件（如果你希望在 XAML 中呈现媒体内容）。

[!code-cs[DeclareMediaPlayer](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-xml[MediaPlayerElement](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetMediaPlayerElement)]

若要允许用户选取要播放的媒体文件，请使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)。 使用从选取器的 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) 方法返回的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 对象，通过调用 [**MediaSource.CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909) 初始化新的 MediaObject。 最后，通过调用 [**SetPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/dn899085) 方法将媒体源设置为 **MediaElement** 的播放源。

[!code-cs[PlayMediaSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaSource)]

默认情况下，当设置媒体源时，**MediaPlayer** 不会自动开始播放。 可以通过调用 [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play) 手动开始播放。

[!code-cs[Play](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlay)]

你还可以将 **MediaPlayer** 的 [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) 属性设置为 true 以告知播放器在设置媒体源后立即开始播放。

[!code-cs[AutoPlay](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAutoPlay)]

### <a name="create-a-mediasource-from-a-downloadoperation"></a>从 DownloadOperation 创建 MediaSource
从 Windows 版本 1803 开始，可以从 **DownloadOperation** 创建 **MediaSource** 对象。

[!code-cs[CreateMediaSourceFromDownload](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetCreateMediaSourceFromDownload)]

请注意，虽然可以从下载中创建 **MediaSource**，无需启动该下载或将其 **IsRandomAccessRequired** 属性设置为 true，但在尝试将 **MediaSource** 附加到 **MediaPlayer** 或 **MediaPlayerElement** 以进行播放之前，必须同时执行这两项操作。

[!code-cs[StartDownload](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetStartDownload)]


## <a name="handle-multiple-audio-video-and-metadata-tracks-with-mediaplaybackitem"></a>使用 MediaPlaybackItem 处理多个音频、视频和元数据轨

使用 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930905) 进行播放很便利，因为它提供了播放来自不同类型的源的媒体的常用方法，但可以通过从 **MediaSource** 创建 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) 访问更高级的行为。 这包括为媒体项访问和管理多个音频、视频和数据轨的功能。

声明用于存储 **MediaPlaybackItem** 的变量。

[!code-cs[DeclareMediaPlaybackItem](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackItem)]

通过调用构造函数和传递初始化的 **MediaSource** 对象来创建 **MediaPlaybackItem**。

如果你的应用在媒体播放项中支持多个音频、视频或数据轨，请为 [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948)、[**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) 或 [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952) 事件注册事件处理程序。

最后，请将 **MediaElement** 或 **MediaPlayer** 的播放源设置为 **MediaPlaybackItem**。

[!code-cs[PlayMediaPlaybackItem](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackItem)]

> [!NOTE] 
> 一个 **MediaSource** 只能与单个 **MediaPlaybackItem** 相关联。 在从某个源创建 **MediaPlaybackItem** 后，尝试从同一个源创建另一个播放项会导致错误。 此外，在从媒体源创建 **MediaPlaybackItem** 后，你无法将 **MediaSource** 对象直接设置为 **MediaPlayer** 的源，但应改用 **MediaPlaybackItem**。

在将包含多个视频轨的 **MediaPlaybackItem** 分配为播放源后，将引发 [**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) 事件，并且如果视频轨列表因项更改而更改，将可能再次引发该事件。 此事件的处理程序使你有机会更新 UI 以允许用户在可用轨之间切换。 此示例使用 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348) 来显示可用的视频轨。

[!code-xml[VideoComboBox](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetVideoComboBox)]

在 **VideoTracksChanged** 处理程序中，循环访问播放项的 [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953) 列表中的所有轨。 对于每个轨，将创建新的 [**ComboBoxItem**](https://msdn.microsoft.com/library/windows/apps/br209349)。 如果该轨尚未有标签，将从轨索引中生成标签。 组合框项的 [**Tag**](https://msdn.microsoft.com/library/windows/apps/br208745) 属性设置为曲目索引，以便以后可以标识它。 最后，该项将添加到组合框。 请注意，这些操作在 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 调用内执行，因为必须在 UI 线程上进行所有 UI 更改，并且此事件在不同的线程上引发。

[!code-cs[VideoTracksChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetVideoTracksChanged)]

在组合框的 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) 处理程序中，从所选项的 **Tag** 属性检索轨索引。 设置媒体播放项的 [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953) 列表的 [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn956634) 属性会导致 **MediaElement** 或 **MediaPlayer** 将活动视频轨切换为指定索引。

[!code-cs[VideoTracksSelectionChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetVideoTracksSelectionChanged)]

管理带有多个音频轨的媒体项的方式与视频轨完全相同。 处理 [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948) 以使用在播放项的 [**AudioTracks**](https://msdn.microsoft.com/library/windows/apps/dn930947) 列表中找到的音频轨更新 UI。 当用户选择音频轨时，设置 **AudioTracks** 列表的 [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn930937) 属性可导致 **MediaElement** 或 **MediaPlayer** 将活动音频轨切换到指定索引。

[!code-xml[AudioComboBox](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetAudioComboBox)]

[!code-cs[AudioTracksChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksChanged)]

[!code-cs[AudioTracksSelectionChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksSelectionChanged)]

除了音频和视频外，**MediaPlaybackItem** 对象还可能包含零个或多个 [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580) 对象。 计时元数据轨可以包含副标题或描述文字，或者可以包含专属于你的应用的自定义数据。 计时元数据曲目包含由继承自 [**IMediaCue**](https://msdn.microsoft.com/library/windows/apps/dn930899) 的对象表示的提示列表，例如 [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) 或 [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655)。 每个提示都有开始时间和持续时间，用于确定何时激活提示以及持续多久。

与音频轨和视频轨类似，可以通过处理 **MediaPlaybackItem** 的 [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952) 事件来发现媒体项的计时元数据轨。 但是，对于计时元数据轨，用户可能希望同时启用多个元数据轨。 此外，根据你的应用方案，你可能希望自动启用或禁用元数据轨，而无需用户干预。 出于说明目的，该示例为媒体项中的每个元数据轨添加 [**ToggleButton**](https://msdn.microsoft.com/library/windows/apps/br209795)，使用户可以启用和禁用轨。将每个按钮的 **Tag** 属性设置为关联的元数据轨索引，以便切换按钮时可以识别它。

[!code-xml[MetaStackPanel](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetMetaStackPanel)]

[!code-cs[TimedMetadataTrackschanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedMetadataTrackschanged)]

由于多个元数据轨可以同时处于活动状态，因此你不仅要设置元数据轨列表的活动索引。 而且还要调用 **MediaPlaybackItem** 对象的 [**SetPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn986977) 方法，从而传入你希望切换的轨的索引，然后从 [**TimedMetadataTrackPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn987016) 枚举中提供一个值。 你选择的演示模式取决于你的应用的实现。 在此示例中，元数据轨在启用时设置为 **PlatformPresented**。 对于基于文本的轨，这意味着系统会自动显示轨中的文本线索。当切换按钮关闭时，演示模式设置为**禁用**，这意味着不显示文本也不引发提示事件。 将在本文的后面部分讨论提示事件。

[!code-cs[ToggleChecked](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetToggleChecked)]

[!code-cs[ToggleUnchecked](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetToggleUnchecked)]

当你处理元数据轨时，可以通过访问 [**Cues**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.TimedMetadataTrack.Cues) 或 [**ActiveCues**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.TimedMetadataTrack.ActiveCues) 属性来访问该轨内的提示集。 你可以执行此操作来更新你的 UI，从而为媒体项显示提示位置。

## <a name="handle-unsupported-codecs-and-unknown-errors-when-opening-media-items"></a>在打开媒体项时，处理不受支持的编解码器和未知错误
从 Windows10 版本 1607 开始，你可以检查播放媒体项所需的编解码器在运行应用的设备上是否受支持或部分受支持。 在 **MediaPlaybackItem** 轨更改事件的事件处理程序中，例如 [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.AudioTracksChanged)，先检查轨更改是否是插入新轨。如果是这样，可以通过使用 **IVectorChangedEventArgs.Index** 参数中传递的索引和 **MediaPlaybackItem** 参数的适当轨集合（如 [**AudioTracks**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.AudioTracks) 集合）获得对正在插入的轨的引用。

获取对插入的轨的引用后，检查该轨的 [**SupportInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.SupportInfo) 属性的 [**DecoderStatus**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrackSupportInfo.DecoderStatus)。 如果值为 [**FullySupported**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus)，则设备上存在播放该轨所需的相应编解码器。 如果值为 [**Degraded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus)，则该轨可以由系统播放，但播放将以某种方式降级。 例如，5.1 音频轨可能作为 2 通道立体声进行播放。 如果是这种情况，你可能希望更新你的 UI 以提醒用户发生降级。 如果值为 [**UnsupportedSubtype**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus) 或 [**UnsupportedEncoderProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus)，则该轨完全无法使用设备上的当前编解码器进行播放。 你可能希望提醒用户并跳过播放该项目，或实现允许用户下载正确编解码器的 UI。 可以使用该轨的 [**GetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.GetEncodingProperties) 方法来确定播放所需的编解码器。

最后，你可以注册该轨的 [**OpenFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.OpenFailed) 事件，当该轨在设备上受支持，但由于管道中的未知错误而无法打开时，将引发该事件。

[!code-cs[AudioTracksChanged_CodecCheck](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksChanged_CodecCheck)]

在 [**OpenFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.OpenFailed) 事件处理程序中，你可以检查 **MediaSource** 状态是否为未知，如果是，你可以以编程方式选择其他轨来播放、允许用户选择其他轨或放弃播放。

[!code-cs[OpenFailed](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetOpenFailed)]

## <a name="set-display-properties-used-by-the-system-media-transport-controls"></a>设置系统媒体传输控件使用的显示属性
从 Windows10 版本 1607 开始，[**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 中播放的媒体默认自动集成到系统媒体传输控件 (SMTC) 中。 你可以通过更新 **MediaPlaybackItem** 的显示属性来指定将由 SMTC 显示的元数据。 通过调用 [**GetDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.GetDisplayProperties) 获取表示某个项目的显示属性的对象。 通过设置 [**Type**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.Type) 属性来设置播放项目是音乐还是视频。 然后，设置对象的 [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.VideoProperties) 或 [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.MusicProperties) 的属性。 调用 [**ApplyDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/mt489923) 以将项目的属性更新为你提供的值。 通常，应用将从 Web 服务动态检索显示值，但以下示例使用硬编码的值演示此过程。

[!code-cs[SetVideoProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetVideoProperties)]

[!code-cs[SetMusicProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetMusicProperties)]

## <a name="add-external-timed-text-with-timedtextsource"></a>使用 TimedTextSource 添加外部计时文本

对于某些方案，你可能有包含与媒体项相关联的计时文本的外部文件，例如包含用于不同区域设置的副标题的单独文件。 在来自流或 URI 的外部计时文本文件中使用要加载的 [**TimedTextSource**](https://msdn.microsoft.com/library/windows/apps/dn956679) 类。

此示例使用 **Dictionary** 集合存储媒体项的计时文本源的列表，其中使用源 URI 和 **TimedTextSource** 对象作为键/值对，以便在解析轨后标识轨。

[!code-cs[TimedTextSourceMap](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSourceMap)]

通过调用 [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn708190) 来为每个外部计时文本文件创建新的 **TimedTextSource**。 将计时文本源的项添加到 **Dictionary**。 添加 [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540) 事件的处理程序，以在项加载失败时进行处理，或在项成功加载后设置其他属性。

向 **MediaSource** 注册你的所有 **TimedTextSource** 对象，方法是将它们添加到 [**ExternalTimedTextSources**](https://msdn.microsoft.com/library/windows/apps/dn930916) 集合。 请注意，外部计时文本源直接添加到 **MediaSource**，而不是从源创建的 **MediaPlaybackItem**。 若要更新 UI 以反映外部文本轨，请注册并处理 **TimedMetadataTracksChanged** 事件，如本文之前所述。

[!code-cs[TimedTextSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSource)]

在 [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540) 事件的处理程序中，检查传递到处理程序中的 [**TimedTextSourceResolveResultEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn965537) 的 **Error** 属性，以确定是否在尝试加载计时文本数据时出错。 如果项目已成功解决，则可以使用此处理程序更新已解决轨的其他属性。此示例根据先前存储在**字典**中的 URI 为每个轨添加一个标签。

[!code-cs[TimedTextSourceResolved](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSourceResolved)]

## <a name="add-additional-metadata-tracks"></a>添加其他元数据轨

你可以在代码中动态创建自定义元数据曲目，并将其与媒体源相关联。 你创建的曲目可以包含副标题或描述文字，或者可以包含你的专属应用数据。

通过调用构造函数并指定 ID、语言标识符和 [**TimedMetadataKind**](https://msdn.microsoft.com/library/windows/apps/dn956578) 枚举中的值来创建新的 [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580)。 为 [**CueEntered**](https://msdn.microsoft.com/library/windows/apps/dn956583) 和 [**CueExited**](https://msdn.microsoft.com/library/windows/apps/dn956584) 事件注册处理程序。 当已达到提示的开始时间和提示的持续时间到期时，将分别引发这些事件。

创建新的适合已创建元数据轨类型的提示对象，并设置轨的 ID、开始时间和持续时间。此示例创建数据轨，因此生成一组 [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) 对象，并为每个提示提供一个包含特定于应用的数据的缓冲区。 若要注册新轨，请将其添加到 **MediaSource** 对象的 [**ExternalTimedMetadataTracks**](https://msdn.microsoft.com/library/windows/apps/dn930915) 集合。

从 Windows 10 版本 1703 开始，**DataCue.Properties** 属性会公开 [**PropertySet**](https://docs.microsoft.com/uwp/api/windows.foundation.collections.propertyset)，它可以用于在可以在 **CueEntered** 和 **CueExited** 事件中检索的键/数据对中存储自定义属性。  

[!code-cs[AddDataTrack](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAddDataTrack)]

当已达到提示的开始时间时，将引发 **CueEntered** 事件，前提是相关联的轨具有 **ApplicationPresented**、**Hidden** 或 **PlatformPresented.** 的演示模式。 对于元数据轨，当轨的演示模式为**已禁用**时，不会引发提示事件。 此示例仅将与该提示相关联的自定义数据输出到调试窗口。

[!code-cs[DataCueEntered](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDataCueEntered)]

本示例将添加自定义文本轨，方法是在创建该轨和使用 [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655) 对象向该轨添加提示时指定 **TimedMetadataKind.Caption**。

[!code-cs[AddTextTrack](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAddTextTrack)]

[!code-cs[TextCueEntered](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTextCueEntered)]

## <a name="play-a-list-of-media-items-with-mediaplaybacklist"></a>使用 MediaPlaybackList 播放媒体项的列表


            [
              **MediaPlaybackList**
            ](https://msdn.microsoft.com/library/windows/apps/dn930955) 允许你创建媒体项的播放列表，这些项由 **MediaPlaybackItem** 对象表示。

**注意** [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955)中的项使用无缝播放进行呈现。 系统将使用 MP3 或 AAC 编码的文件中所提供的元数据来确定无缝播放所需的延迟或填充补偿。 如果 MP3 或 AAC 编码的文件不提供此元数据，则系统将试探性确定延迟或填充。 对于无损格式（如 PCM、FLAC 或 ALAC），系统不执行任何操作，因为这些编码器不会引入延迟或填充。

若要开始使用，请声明用于存储 **MediaPlaybackList** 的变量。

[!code-cs[DeclareMediaPlaybackList](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackList)]

使用本文之前所述的相同过程为希望添加到列表中的每个媒体项创建 **MediaPlaybackItem**。 初始化你的 **MediaPlaybackList** 对象并向其添加媒体播放项。 为 [**CurrentItemChanged**](https://msdn.microsoft.com/library/windows/apps/dn930957) 事件注册处理程序。 此事件允许你更新 UI 以反映当前正在播放的媒体项。 还可以为 [ItemOpened](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemOpened) 事件（在列表中的项目成功打开时引发）和 [ItemFailed](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemFailed) 事件（在列表中的项目无法打开时引发）进行注册。

从 Windows 10 版本 1703 开始，可以指定 **MediaPlaybackList** 中的最大 **MediaPlaybackItem** 对象数，系统会通过设置 [MaxPlayedItemsToKeepOpen](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.MaxPlayedItemsToKeepOpen) 属性来使它们在播放之后保持打开。 当某个 **MediaPlaybackItem** 保持打开时，可以在用户切换到该项目时立即开始播放该项目，因为无需重载该项目。 但是使项目保持打开也会增加应用的内存消耗，因此在设置此值时应考虑响应能力与内存使用量之间的平衡。 

若要启用列表的播放，请将 **MediaPlayer** 的播放源设置为 **MediaPlaybackList**。

[!code-cs[PlayMediaPlaybackList](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackList)]

在 **CurrentItemChanged** 事件处理程序中，更新 UI 以反映当前正在播放的项，可使用传递到该事件中的 [**CurrentMediaPlaybackItemChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn930929) 对象的 [**NewItem**](https://msdn.microsoft.com/library/windows/apps/dn930930) 属性来检索该项。 请记住，如果你从此事件更新 UI，你应在对 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 的调用中执行此操作，以便在 UI 线程上进行更新。

从 Windows 10 版本 1703 开始，可以检查 [CurrentMediaPlaybackItemChangedEventArgs.Reason](https://docs.microsoft.com/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.Reason) 属性以获取指示项目更改原因（如应用以编程方式切换项目、以前播放的项目到达其结尾或发生错误）的值。

[!code-cs[MediaPlaybackListItemChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetMediaPlaybackListItemChanged)]


调用 [**MovePrevious**](https://msdn.microsoft.com/library/windows/apps/mt146455) 或 [**MoveNext**](https://msdn.microsoft.com/library/windows/apps/mt146454) 以使媒体播放器播放 **MediaPlaybackList** 中的上一项或下一项。

[!code-cs[PrevButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPrevButton)]

[!code-cs[NextButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetNextButton)]

设置 [**ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146457) 属性来指定媒体播放器是否应以随机顺序播放列表中的项。

[!code-cs[ShuffleButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetShuffleButton)]

设置 [**AutoRepeatEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146452) 属性来指定媒体播放器是否应循环播放列表。

[!code-cs[RepeatButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetRepeatButton)]


### <a name="handle-the-failure-of-media-items-in-a-playback-list"></a>处理播放列表中的媒体项的故障
当列表中的某个项目无法打开时，将引发 [**ItemFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList.ItemFailed) 事件。 传入处理程序中的 [**MediaPlaybackItemError**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItemError) 对象的 [**ErrorCode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItemError.ErrorCode) 属性将枚举故障的具体原因（如果可能），包括网络错误、解码错误或加密错误。

[!code-cs[ItemFailed](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetItemFailed)]

### <a name="disable-playback-of-items-in-a-playback-list"></a>禁用播放列表中的项目的播放
从 Windows 10 版本 1703 开始，可以通过将 [MediaPlaybackItem](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem) 的 [IsDisabledInPlaybackList](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem.IsDisabledInPlaybackList) 属性设置为 false，来禁用 **MediaPlaybackItemList** 中一个或多个项目的播放。 

此功能的典型方案是用于可播放从 Internet 流式处理的音乐的应用。 应用可以侦听设备网络连接状态的更改，并禁用未完整下载的项目的播放。 在以下示例中，为 [NetworkInformation.NetworkStatusChanged](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation.NetworkStatusChanged) 事件注册了一个事件处理程序。

[!code-cs[RegisterNetworkStatusChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetRegisterNetworkStatusChanged)]

在 **NetworkStatusChanged** 的处理程序中，检查以查看 [GetInternetConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation.GetInternetConnectionProfile) 是否返回 null，这指示网络未连接。 如果是这种情况，则循环访问播放列表中的所有项目，并且项目的 [TotalDownloadProgress](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TotalDownloadProgress) 小于 1（表示项目未完整下载），则禁用项目。 如果网络连接处于启用状态，则循环访问播放列表中的所有项目，并启用每个项目。

[!code-cs[NetworkStatusChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetNetworkStatusChanged)]

### <a name="defer-binding-of-media-content-for-items-in-a-playback-list-by-using-mediabinder"></a>使用 MediaBinder 为播放列表中的项目延迟媒体内容的绑定
在以前的示例中，会从文件、URL 或流创建 **MediaSource**，在此之后会创建 **MediaPlaybackItem** 并将它添加到 **MediaPlaybackList**。 对于某些方案（例如如果对用户收取查看内容的费用），则可能要延迟 **MediaSource** 内容的检索，直到播放列表中的项目准备好进行实际播放。 若要实现此方案，请创建 [**MediaBinder**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder) 类的实例。 将 [**Token**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Token) 属性设置为可标识要对其延迟检索的内容的应用定义字符串，然后为 [**Binding**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Binding) 事件注册处理程序。 接下来，通过调用 [**MediaSource.CreateFromMediaBinder**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediabinder)，从 **Binder** 创建 **MediaSource**。 然后，从 **MediaSource** 创建一个 **MediaPlaybackItem** 并像往常一样将它添加到播放列表。

[!code-cs[InitMediaBinder](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetInitMediaBinder)]

当系统确定需要检索与 **MediaBinder** 关联的内容时，它会引发 **Binding** 事件。 在此事件的处理程序中，可以从传入事件的 [**MediaBindingEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs) 检索 **MediaBinder** 实例。 检索为 **Token** 属性指定的字符串，然后使用它确定应检索的内容。 **MediaBindingEventArgs** 提供了方法用于采用多种不同的表示形式设置绑定内容，包括 [**SetStorageFile**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstoragefile)、[**SetStream**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstream)、[**SetStreamReference**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstreamreference) 和 [**SetUri**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.seturi)。 

[!code-cs[BinderBinding](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetBinderBinding)]

请注意，如果在 **Binding** 事件处理程序中执行异步操作（如 Web 请求），则应调用 [**MediaBindingEventArgs.GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) 方法以指示系统等待操作完成，然后再继续进行。 在操作完成之后调用 [**Deferral.Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.Complete) 以指示系统继续进行。

从 Windows 10 版本 1703 开始，可以通过调用 [**SetAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource)，提供 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) 作为绑定内容。 有关在应用中使用自适应流式处理的详细信息，请参阅[自适应流式处理](adaptive-streaming.md)。



## <a name="related-topics"></a>相关主题
* [媒体播放](media-playback.md)
* [使用 MediaPlayer 播放音频和视频](play-audio-and-video-with-mediaplayer.md)
* [与系统媒体传输控件集成](integrate-with-systemmediatransportcontrols.md)
* [在后台播放媒体](background-audio.md)


---
ms.assetid: F28162D4-AACC-4EE0-B243-5878F870F87F
description: 在媒体播放过程中处理系统支持的元数据提示
title: 系统支持的计时元数据提示
ms.date: 04/18/2017
ms.topic: article
keywords: windows 10, uwp, 元数据, 提示, 语言, 章节
ms.localizationpriority: medium
ms.openlocfilehash: 2b3753e92524e300252930f48433f91e175353c9
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2019
ms.locfileid: "9046104"
---
# <a name="system-supported-timed-metadata-cues"></a>系统支持的计时元数据提示
本文介绍如何利用可以在媒体文件或流中嵌入的多种格式的计时元数据。 UWP 应用可以注册在播放过程中每当遇到这些元数据提示时由媒体管道引发的事件。 通过使用 [**DataCue**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.DataCue) 类，应用可以实现自己的自定义元数据提示，但是本文重点介绍由媒体管道自动检测的几种元数据标准，包括：

* VobSub 格式的基于图像的字幕
* 语音提示，包括单词边界、句子边界和语音合成器标记语言 (SSML) 书签
* 章节提示
* 扩展 M3U 注释
* ID3 标记
* 零碎的 mp4 emsg 箱


本文建立在文章[媒体项、播放列表和轨](media-playback-with-mediasource.md)中讨论的概念，这包括使用 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)、[**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) 和 [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580) 类的基础知识以及有关在应用中使用计时元数据的通用指南。

对于本文介绍的所有不同类型的计时元数据，基本实现步骤是相同的：

1. 创建一个 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)，然后创建一个 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) 以用于要播放的内容。
2. 注册在媒体管道解析媒体项的子轨时发生的 [**MediaPlaybackItem.TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 事件。
3. 为要使用的计时元数据轨注册 [**TimedMetadataTrack.CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 和 [**TimedMetadataTrack.CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 事件。
4. 在 **CueEntered** 事件处理程序中，基于事件参数中传递的元数据更新 UI。 可以在 **CueExited** 事件中再次更新 UI，例如用于删除当前字幕文本。

在本文中，每种类型元数据的处理作为不同方案进行演示，但是可以使用大部分共享的代码处理（或忽略）不同类型的元数据。 可以在过程中的多个时间点检查 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 对象的 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 属性。 因此，例如可以选择为具有值 **TimedMetadataKind.ImageSubtitle** 的元数据轨注册 **CueEntered** 事件，但是不为具有值 **TimedMetadataKind.Speech** 的轨进行注册。 或是相反，可以为所有元数据轨类型注册处理程序，然后在 **CueEntered** 处理程序中检查 **TimedMetadataKind** 值以确定为响应提示而执行的操作。

## <a name="image-based-subtitles"></a>基于图像的字幕
从 Windows 10 版本 1703 开始，UWP 应用可以支持 VobSub 格式的基于图像的外部字幕。 若要使用此功能，请首先为会为其显示图像字幕的媒体内容创建 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 对象。 接下来，通过调用 [**CreateFromUriWithIndex**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource.CreateFromUriWithIndex) 或 [**CreateFromStreamWithIndex**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource.CreateFromStreamWithIndex)（传入包含字幕图像数据的 .sub 文件的 Uri，以及包含字幕的计时信息的 .idx 文件），来创建 [**TimedTextSource**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource) 对象。 通过将 **TimedTextSource** 添加到源的 [**ExternalTimedTextSources**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.ExternalTimedTextSources) 集合，将它添加到 **MediaSource**。 从 **MediaSource** 创建 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)。

[!code-cs[ImageSubtitleLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleLoadContent)]

使用在上一步中创建的 **MediaPlaybackItem** 对象注册图像字幕元数据事件。 此示例使用帮助程序方法 **RegisterMetadataHandlerForImageSubtitles** 注册事件。 一个 lambda 表达式用于为 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 事件注册处理程序，该事件在系统检测到与 **MediaPlaybackItem** 关联的元数据轨发生更改时发生。 在某些情况下，元数据轨在最初解析播放项目时可用，因此在 **TimedMetadataTracksChanged** 处理程序外部，我们也会循环访问可用媒体轨并调用 **RegisterMetadataHandlerForImageSubtitles**。

[!code-cs[ImageSubtitleTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleTracksChanged)]

注册图像字幕元数据事件之后，**MediaItem** 会分配给 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) 以便在 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 中播放。

[!code-cs[ImageSubtitlePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitlePlay)]

在 **RegisterMetadataHandlerForImageSubtitles** 帮助程序方法中，通过在 **MediaPlaybackItem** 的 **TimedMetadataTracks** 集合中进行索引来获取 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 类的实例。 注册 [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 事件和 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 事件。 随后必须对播放项目的 **TimedMetadataTracks** 集合调用 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode)，以指示系统应用要接收此播放项目的媒体提示事件。

[!code-cs[RegisterMetadataHandlerForImageSubtitles](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForImageSubtitles)]

在 **CueEntered** 事件的处理程序中，可以检查传入处理程序中的 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 对象的 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 属性，以查看元数据是否用于图像字幕。 如果对多种类型的元数据使用相同的数据提示事件程序，则这是必需的。 如果关联元数据轨属于类型 **TimedMetadataKind.ImageSubtitle**，则将 [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) 的 **Cue** 属性中包含的数据提示强制转换为 [**ImageCue**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue)。 **ImageCue** 的 [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue.SoftwareBitmap) 属性包含字幕图像的 [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap) 表示形式。 创建 [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource) 并调用 [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.SetBitmapAsync) 以将图像分配给 XAML [**Image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) 控件。 **ImageCue** 的 [**Extent**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue.Extent) 和 [**Position**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue.Position) 属性可提供有关字幕图像的大小和位置的信息。

[!code-cs[ImageSubtitleCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleCueEntered)]

## <a name="speech-cues"></a>语音提示
从 Windows 10 版本 1703 开始，UWP 应用可以注册以接收事件，来响应播放的媒体中的单词边界、句子边界和语音合成标记语言 (SSML) 书签。 这使你可以播放使用 [**SpeechSynthesizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer) 类生成的音频流并基于这些事件更新 UI，如显示当前播放的单词或句子的文本。

此部分中显示的示例使用类成员变量存储将进行合成和播放的文本字符串。

[!code-cs[SpeechInputText](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechInputText)]

创建 **SpeechSynthesizer** 类的新实例。 将合成器的 [**IncludeWordBoundaryMetadata**](https://docs.microsoft.com/uwp/api/windows.media.speechsynthesis.speechsynthesizeroptions.IncludeWordBoundaryMetadata) 和 [**IncludeSentenceBoundaryMetadata**](https://docs.microsoft.com/uwp/api/windows.media.speechsynthesis.speechsynthesizeroptions.IncludeSentenceBoundaryMetadata) 选项设置为 **true**，以指定元数据应包含在生成的媒体流中。 调用 [**SynthesizeTextToStreamAsync**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer.SynthesizeTextToStreamAsync) 以生成包含合成语音和对应元数据的流。 从合成流创建 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 和 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)。

[!code-cs[SynthesizeSpeech](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSynthesizeSpeech)]

使用 **MediaPlaybackItem** 对象注册语音元数据事件。 此示例使用帮助程序方法 **RegisterMetadataHandlerForSpeech** 注册事件。 一个 lambda 表达式用于为 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 事件注册处理程序，该事件在系统检测到与 **MediaPlaybackItem** 关联的元数据轨发生更改时发生。  在某些情况下，元数据轨在最初解析播放项目时可用，因此在 **TimedMetadataTracksChanged** 处理程序外部，我们也会循环访问可用媒体轨并调用 **RegisterMetadataHandlerForSpeech**。

[!code-cs[SpeechTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechTracksChanged)]

注册语音元数据事件之后，**MediaItem** 会分配给 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) 以便在 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 中播放。

[!code-cs[SpeechPlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechPlay)]

在 **RegisterMetadataHandlerForSpeech** 帮助程序方法中，通过在 **MediaPlaybackItem** 的 **TimedMetadataTracks** 集合中进行索引来获取 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 类的实例。 注册 [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 事件和 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 事件。 随后必须对播放项目的 **TimedMetadataTracks** 集合调用 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode)，以指示系统应用要接收此播放项目的媒体提示事件。

[!code-cs[RegisterMetadataHandlerForWords](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForWords)]

在 **CueEntered** 事件的处理程序中，可以检查传入处理程序中的 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 对象的 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 属性，以查看元数据是否为语音。 如果对多种类型的元数据使用相同的数据提示事件程序，则这是必需的。 如果关联元数据轨属于类型 **TimedMetadataKind.Speech**，则将 [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) 的 **Cue** 属性中包含的数据提示强制转换为 [**SpeechCue**](https://docs.microsoft.com/uwp/api/windows.media.core.speechcue)。 对于语音提示，元数据轨中包含的语音提示的类型通过检查 **Label** 属性来确定。 此属性的值会是用于单词边界的“SpeechWord”、用于句子边界的“SpeechSentence”或用于 SSML 书签的“SpeechBookmark”。 在此示例中，我们检查是否存在“SpeechWord”值，如果找到此值，则 **SpeechCue** 的 [**StartPositionInInput**](https://docs.microsoft.com/uwp/api/windows.media.core.speechcue.StartPositionInInput) 和 [**EndPositionInInput**](https://docs.microsoft.com/uwp/api/windows.media.core.speechcue.EndPositionInInput) 属性用于确定当前播放的单词在输入文本中的位置。 此示例只将每个单词输出到调试输出。

[!code-cs[SpeechWordCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechWordCueEntered)]

## <a name="chapter-cues"></a>章节提示
从 Windows 10 版本 1703 开始，UWP 应用可以注册与媒体项中的章节对应的提示。 若要使用此功能，请为媒体内容创建 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 对象，然后从 **MediaSource** 创建 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)。

[!code-cs[ChapterCueLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueLoadContent)]

使用在上一步中创建的 **MediaPlaybackItem** 对象注册章节元数据事件。 此示例使用帮助程序方法 **RegisterMetadataHandlerForChapterCues** 注册事件。 一个 lambda 表达式用于为 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 事件注册处理程序，该事件在系统检测到与 **MediaPlaybackItem** 关联的元数据轨发生更改时发生。 在某些情况下，元数据轨在最初解析播放项目时可用，因此在 **TimedMetadataTracksChanged** 处理程序外部，我们也会循环访问可用媒体轨并调用 **RegisterMetadataHandlerForChapterCues**。

[!code-cs[ChapterCueTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueTracksChanged)]

注册章节元数据事件之后，**MediaItem** 会分配给 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) 以便在 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 中播放。

[!code-cs[ChapterCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCuePlay)]

在 **RegisterMetadataHandlerForChapterCues** 帮助程序方法中，通过在 **MediaPlaybackItem** 的 **TimedMetadataTracks** 集合中进行索引来获取 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 类的实例。 注册 [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 事件和 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 事件。 随后必须对播放项目的 **TimedMetadataTracks** 集合调用 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode)，以指示系统应用要接收此播放项目的媒体提示事件。

[!code-cs[RegisterMetadataHandlerForChapterCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForChapterCues)]

在 **CueEntered** 事件的处理程序中，可以检查传入处理程序中的 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 对象的 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 属性，以查看元数据是否用于章节提示。如果对多种类型的元数据使用相同的数据提示事件处理程序，则这是必要的操作。 如果关联元数据轨属于类型 **TimedMetadataKind.Chapter**，则将 [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) 的 **Cue** 属性中包含的数据提示强制转换为 [**ChapterCue**](https://docs.microsoft.com/uwp/api/windows.media.core.chaptercue)。 **ChapterCue** 的 [**Title**](https://docs.microsoft.com/uwp/api/windows.media.core.chaptercue.Title) 属性包含播放中刚刚到达的章节的标题。

[!code-cs[ChapterCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueEntered)]

### <a name="seek-to-the-next-chapter-using-chapter-cues"></a>使用章节提示寻找下一个章节
除了在当前章节在播放项目中更改时接收通知之后，还可以使用章节提示寻找播放项目中的下一个章节。 下面显示的示例方法采用 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) 和 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)（表示当前播放的媒体项）作为参数。 会搜索 [**TimedMetadataTracks**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracks) 集合以查看是否有任何轨具有 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 值为 **TimedMetadataKind.Chapter** 的 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 属性。  如果找到章节轨，则该方法会循环访问轨的 [**Cues**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.Cues) 集合中的每个提示，以查找 [**StartTime**](https://docs.microsoft.com/uwp/api/windows.media.core.chaptercue.StartTime) 大于媒体播放器播放会话的当前 [**Position**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.Position) 的第一个提示。 找到正确提示之后，播放会话的位置会进行更新，并且章节标题会在 UI 中进行更新。

[!code-cs[GoToNextChapter](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetGoToNextChapter)]

## <a name="extended-m3u-comments"></a>扩展 M3U 注释
从 Windows 10 版本 1703 开始，UWP 应用可以注册与扩展 M3U 清单文件中的注释对应的提示。 此示例使用 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) 播放媒体内容。 有关详细信息，请参阅[自适应流式处理](adaptive-streaming.md)。 通过调用 [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) 或 [**CreateFromStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync) 为内容创建 **AdaptiveMediaSource**。 通过调用 [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) 来创建 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 对象，然后从 **MediaSource** 创建 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)。

[!code-cs[EXTM3ULoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3ULoadContent)]

使用在上一步中创建的 **MediaPlaybackItem** 对象注册 M3U 元数据事件。 此示例使用帮助程序方法 **RegisterMetadataHandlerForEXTM3UCues** 注册事件。 一个 lambda 表达式用于为 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 事件注册处理程序，该事件在系统检测到与 **MediaPlaybackItem** 关联的元数据轨发生更改时发生。 在某些情况下，元数据轨在最初解析播放项目时可用，因此在 **TimedMetadataTracksChanged** 处理程序外部，我们也会循环访问可用媒体轨并调用 **RegisterMetadataHandlerForEXTM3UCues**。

[!code-cs[EXTM3UCueTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCueTracksChanged)]

注册 M3U 元数据事件之后，**MediaItem** 会分配给 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) 以便在 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 中播放。

[!code-cs[EXTM3UCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCuePlay)]

在 **RegisterMetadataHandlerForEXTM3UCues** 帮助程序方法中，通过在 **MediaPlaybackItem** 的 **TimedMetadataTracks** 集合中进行索引来获取 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 类的实例。 检查元数据轨的 DispatchType 属性，如果轨表示 M3U 注释，则该属性的值为“EXTM3U”。 注册 [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 事件和 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 事件。 随后必须对播放项目的 **TimedMetadataTracks** 集合调用 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode)，以指示系统应用要接收此播放项目的媒体提示事件。

[!code-cs[RegisterMetadataHandlerForEXTM3UCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForEXTM3UCues)]

在 **CueEntered** 事件的处理程序中，将 [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) 的  **Cue** 属性中包含的数据提示强制转换为 [**DataCue**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue)。  检查以确保提示的 **DataCue** 和 [**Data**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue.Data) 属性不为 null。 扩展 EMU 注册采用 UTF-16、little endian、以 null 结尾的字符串的形式提供。 通过调用 [**DataReader.FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.FromBuffer) 来创建新 **DataReader** 以读取提示数据。 将读取器的 [**UnicodeEncoding**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.UnicodeEncoding) 属性设置为 [**Utf16LE**](https://docs.microsoft.com/uwp/api/windows.storage.streams.unicodeencoding) 以采用正确格式读取数据。 调用 [**ReadString**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.ReadString) 以读取数据，其中指定 **Data** 字段长度的一半，因为每个字符的大小为两个字节，并减去一以删除尾部 null 字符。 在此示例中，M3U 注释只写入到调试输出。

[!code-cs[EXTM3UCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCueEntered)]

## <a name="id3-tags"></a>ID3 标记
从 Windows 10 版本 1703 开始，UWP 应用可以注册与 Http 实时流 (HLS) 内容中的 ID3 标记对应的提示。 此示例使用 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) 播放媒体内容。 有关详细信息，请参阅[自适应流式处理](adaptive-streaming.md)。 通过调用 [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) 或 [**CreateFromStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync) 为内容创建 **AdaptiveMediaSource**。 通过调用 [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) 来创建 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 对象，然后从 **MediaSource** 创建 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)。

[!code-cs[EXTM3ULoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3ULoadContent)]

使用在上一步中创建的 **MediaPlaybackItem** 对象注册 ID3 标记事件。 此示例使用帮助程序方法 **RegisterMetadataHandlerForID3Cues** 注册事件。 一个 lambda 表达式用于为 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 事件注册处理程序，该事件在系统检测到与 **MediaPlaybackItem** 关联的元数据轨发生更改时发生。 在某些情况下，元数据轨在最初解析播放项目时可用，因此在 **TimedMetadataTracksChanged** 处理程序外部，我们也会循环访问可用媒体轨并调用 **RegisterMetadataHandlerForID3Cues**。

[!code-cs[ID3LoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3LoadContent)]

注册 ID3 元数据事件之后，**MediaItem** 会分配给 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) 以便在 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 中播放。

[!code-cs[ID3CuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3CuePlay)]


在 **RegisterMetadataHandlerForID3Cues** 帮助程序方法中，通过在 **MediaPlaybackItem** 的 **TimedMetadataTracks** 集合中进行索引来获取 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 类的实例。 检查元数据轨的 DispatchType 属性，如果轨表示 ID3 标记，则该属性的值包含 GUID 字符串“15260DFFFF49443320FF49443320000F”。 注册 [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 事件和 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 事件。 随后必须对播放项目的 **TimedMetadataTracks** 集合调用 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode)，以指示系统应用要接收此播放项目的媒体提示事件。

[!code-cs[RegisterMetadataHandlerForID3Cues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForID3Cues)]

在 **CueEntered** 事件的处理程序中，将 [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) 的  **Cue** 属性中包含的数据提示强制转换为 [**DataCue**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue)。  检查以确保提示的 **DataCue** 和 [**Data**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue.Data) 属性不为 null。 扩展的 EMU 注释在传输流中采用原始字节的形式提供（请参阅[http://id3.org/id3v2.4.0-structure](https://id3.org/id3v2.4.0-structure)）。 通过调用 [**DataReader.FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.FromBuffer) 来创建新 **DataReader** 以读取提示数据。  在此示例中，ID3 标记中的标头值从提示数据进行读取并写入调试输出。

[!code-cs[ID3CueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3CueEntered)]

## <a name="fragmented-mp4-emsg-boxes"></a>零碎的 mp4 emsg 箱
从 Windows 10 版本 1703 开始，UWP 应用可以注册与零碎的 mp4 流中的 emsg 箱对应的提示。 此类型的元数据的一种示例用法是供内容提供程序向客户端应用程序发送信号，以便在实时流式处理内容过程中播发广告。 此示例使用 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) 播放媒体内容。 有关详细信息，请参阅[自适应流式处理](adaptive-streaming.md)。 通过调用 [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) 或 [**CreateFromStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync) 为内容创建 **AdaptiveMediaSource**。 通过调用 [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) 来创建 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 对象，然后从 **MediaSource** 创建 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)。

[!code-cs[EmsgLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgLoadContent)]

使用在上一步中创建的 **MediaPlaybackItem** 对象注册 emsg 箱事件。 此示例使用帮助程序方法 **RegisterMetadataHandlerForEmsgCues** 注册事件。 一个 lambda 表达式用于为 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 事件注册处理程序，该事件在系统检测到与 **MediaPlaybackItem** 关联的元数据轨发生更改时发生。 在某些情况下，元数据轨在最初解析播放项目时可用，因此在 **TimedMetadataTracksChanged** 处理程序外部，我们也会循环访问可用媒体轨并调用 **RegisterMetadataHandlerForEmsgCues**。

[!code-cs[ID3LoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3LoadContent)]

注册 emsg 箱元数据事件之后，**MediaItem** 会分配给 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) 以便在 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 中播放。

[!code-cs[EmsgCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgCuePlay)]


在 **RegisterMetadataHandlerForEmsgCues** 帮助程序方法中，通过在 **MediaPlaybackItem** 的 **TimedMetadataTracks** 集合中进行索引来获取 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 类的实例。 检查元数据轨的 DispatchType 属性，如果轨表示 emsg 箱，则该属性的值为“emsg:mp4”。 注册 [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 事件和 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 事件。 随后必须对播放项目的 **TimedMetadataTracks** 集合调用 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode)，以指示系统应用要接收此播放项目的媒体提示事件。


[!code-cs[RegisterMetadataHandlerForEmsgCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForEmsgCues)]


在 **CueEntered** 事件的处理程序中，将 [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) 的  **Cue** 属性中包含的数据提示强制转换为 [**DataCue**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue)。  检查以确保 **DataCue** 对象不为 null。 emsg 箱的属性由媒体管道作为自定义属性在 DataCue 对象的 [**Properties**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue.Properties) 集合中提供。 此示例尝试使用 **[TryGetValue](https://docs.microsoft.com/uwp/api/windows.foundation.collections.propertyset.trygetvalue)** 方法提取几个不同的属性值。 如果此方法返回 null，则表示请求的属性在 emsg 箱中不存在，因此改为设置默认值。

示例的下一个部分演示触发广告播放的应用情景，当上一步中获取的 *scheme_id_uri* 属性的值为“urn:scte:scte35:2013:xml”时会发生这种情况（请参阅[http://dashif.org/identifiers/event-schemes/](https://dashif.org/identifiers/event-schemes/)）。 请注意，标准建议多次发送此 emsg 以便实现冗余，因此此示例维护已处理的 emsg ID 的列表，仅处理新消息。 通过调用 [**DataReader.FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.FromBuffer) 来创建新 **DataReader** 以读取提示数据，并通过设置 [**UnicodeEncoding**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.UnicodeEncoding) 属性来将编码设置为 UTF-8，然后读取数据。 在此示例中，消息负载会写入调试输出。 实际应用会使用负载数据安排广告的播放。

[!code-cs[EmsgCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgCueEntered)]


## <a name="related-topics"></a>相关主题

* [媒体播放](media-playback.md)
* [媒体项、播放列表和曲目](media-playback-with-mediasource.md)


 





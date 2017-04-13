---
author: drewbatgit
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: "你可以使用 Windows.Media.Transcoding API 将视频文件代码从一种格式转换为另一种格式。"
title: "转换媒体文件代码"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: c9e6a6437e427ca6b5bd063d467fd713526ea8ee
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="transcode-media-files"></a>转换媒体文件代码

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


你可以使用 [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105) API 将视频文件代码从一种格式转换为另一种格式。

*转换代码*是指转换数字媒体文件，例如将视频或音频文件从一种格式转换为另一种格式。 这通常是通过解码文件，然后再重新编码文件实现的。 例如，你可以将 Windows Media 文件转换为 MP4 文件，以使此文件可以在支持 MP4 格式的便携式设备上播放。 或者，你可以将高清视频文件转换为较低分辨率的文件。 在此情况下，重新编码的文件可能使用与原始文件相同的编解码器，但是其编码配置文件不同。

## <a name="set-up-your-project-for-transcoding"></a>设置你的项目以进行转换代码

除了由默认项目模板引用的命名空间，你还需要引用这些命名空间，以便使用本文中的代码转换媒体文件代码。

[!code-cs[使用](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="select-source-and-destination-files"></a>选择源文件和目标文件

应用确定要转换代码的源文件和目标文件的方式取决于实现。 此示例使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 和 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)，以允许用户选取源文件和目标文件。

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## <a name="create-a-media-encoding-profile"></a>创建媒体编码配置文件

编码配置文件包含确定如何编码目标文件的设置。 转换文件代码时，此文件中包含的选项最多。

[**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) 类提供用于创建预定义编码配置文件的静态方法：

### <a name="methods-for-creating-audio-only-encoding-profiles"></a>用于创建纯音频编码配置文件的方法

方法  |配置文件  |
---------|---------|
[CreateAlac](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateAlac_Windows_Media_MediaProperties_AudioEncodingQuality_)     |Apple 无损音频编解码器 (ALAC) 音频         |
[CreateFlac](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateFlac_Windows_Media_MediaProperties_AudioEncodingQuality_)     |免费无损音频编解码器 (FLAC) 音频。         |
[CreateM4a](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateM4a_Windows_Media_MediaProperties_AudioEncodingQuality_)     |AAC 音频 (M4A)         |
[CreateMp3](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateMp3_Windows_Media_MediaProperties_AudioEncodingQuality_)     |MP3 音频         |
[CreateWav](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateWav_Windows_Media_MediaProperties_AudioEncodingQuality_)     |WAV 音频         |
[CreateWmv](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateWmv_Windows_Media_MediaProperties_VideoEncodingQuality_)     |Windows Media 音频 (WMA)         |

### <a name="methods-for-creating-audio--video-encoding-profiles"></a>用于创建音频/视频编码配置文件的方法

方法  |配置文件  |
---------|---------|
[CreateAvi](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateAvi_Windows_Media_MediaProperties_VideoEncodingQuality_)     |AVI         |
[CreateHevc](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateHevc_Windows_Media_MediaProperties_VideoEncodingQuality_)     |高效率视频编码 (HEVC) 视频，也称为 H.265 视频         |
[CreateMp4](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateMp4_Windows_Media_MediaProperties_VideoEncodingQuality_)     |MP4 视频（H.264 视频加 AAC 音频）         |

[CreateWmv](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateWmv_Windows_Media_MediaProperties_VideoEncodingQuality_)     |Windows Media 视频 (WMV)         |


以下代码为 MP4 视频创建配置文件。

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

静态 [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/hh701078) 方法创建 MP4 编码配置文件。 此方法的参数为视频指定目标分辨率。 在这种情况下，[**VideoEncodingQuality.hd720p**](https://msdn.microsoft.com/library/windows/apps/hh701290) 表示在每秒 30 帧情况下的 1280 x 720 像素。 （“720p”表示每帧 720 个逐行扫描行。）用于创建预定义配置文件的其他方法均遵循此模式。

或者，你可以通过使用 [**MediaEncodingProfile.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701047) 方法来创建与现有媒体文件相匹配的配置文件。 或者，如果你知道所需的确切编码设置，则可以创建一个新的 [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) 对象，然后填写配置文件详细信息。

## <a name="transcode-the-file"></a>转换文件代码

若要转码文件，请创建新的 [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080) 对象并调用 [**MediaTranscoder.PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936) 方法。 传入源文件、目标文件和编码配置文件。 然后，调用从异步转换代码操作中返回的 [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) 对象中的 [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) 方法。

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## <a name="respond-to-transcoding-progress"></a>响应转换代码进度

你可以注册要在异步进度 [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) 发生更改时响应的事件。 这些事件是通用 Windows 平台 (UWP) 应用的异步编程框架的一部分，且不特定于转换 API 代码。

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]

 

 





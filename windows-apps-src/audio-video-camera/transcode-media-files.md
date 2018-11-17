---
author: drewbatgit
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: 你可以使用 Windows.Media.Transcoding API 将视频文件代码从一种格式转换为另一种格式。
title: 转换媒体文件代码
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: babf91e681004942bb3b66eb43622742fa183125
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2018
ms.locfileid: "7149464"
---
# <a name="transcode-media-files"></a>转换媒体文件代码



可以使用 [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105) API 将视频文件代码从一种格式转换为另一种格式。

*转换代码*是指转换数字媒体文件，例如将视频或音频文件从一种格式转换为另一种格式。 这通常是通过解码文件，然后再重新编码文件实现的。 例如，你可以将 Windows Media 文件转换为 MP4 文件，以使此文件可以在支持 MP4 格式的便携式设备上播放。 或者，你可以将高清视频文件转换为较低分辨率的文件。 在此情况下，重新编码的文件可能使用与原始文件相同的编解码器，但是其编码配置文件不同。

## <a name="set-up-your-project-for-transcoding"></a>设置你的项目以进行转换代码

除了由默认项目模板引用的命名空间，你还需要引用这些命名空间，以便使用本文中的代码转换媒体文件代码。

[!code-cs[Using](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="select-source-and-destination-files"></a>选择源文件和目标文件

应用确定要转换代码的源文件和目标文件的方式取决于实现。 此示例通过使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 和 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) 让用户能够选取源文件和目标文件。

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## <a name="create-a-media-encoding-profile"></a>创建媒体编码配置文件

编码配置文件包含确定如何编码目标文件的设置。 转换文件代码时，此文件中包含的选项最多。

[**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) 类提供用于创建预定义编码配置文件的静态方法：

### <a name="methods-for-creating-audio-only-encoding-profiles"></a>用于创建纯音频编码配置文件的方法

方法  |配置文件  |
---------|---------|
[**CreateAlac**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createalac)     |Apple 无损音频编解码器 (ALAC) 音频         |
[**CreateFlac**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createflac)     |免费无损音频编解码器 (FLAC) 音频。         |
[**CreateM4a**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createm4a)     |AAC 音频 (M4A)         |
[**CreateMp3**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)     |MP3 音频         |
[**CreateWav**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwav)     |WAV 音频         |
[**CreateWmv**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv)     |Windows Media 音频 (WMA)         |

### <a name="methods-for-creating-audio--video-encoding-profiles"></a>用于创建音频/视频编码配置文件的方法

方法  |配置文件  |
---------|---------|
[**CreateAvi**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi) |AVI |
[**CreateHevc**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createhevc) |高效率视频编码 (HEVC) 视频，也称为 H.265 视频 |
[**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) |MP4 视频（H.264 视频加 AAC 音频） |
[**CreateWmv**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv) |Windows Media 视频 (WMV) |


以下代码为 MP4 视频创建配置文件。

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

静态 [**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) 方法创建 MP4 编码配置文件。 此方法的参数为视频指定目标分辨率。 在这种情况下，[**VideoEncodingQuality.hd720p**](https://msdn.microsoft.com/library/windows/apps/hh701290) 表示在每秒 30 帧情况下的 1280 x 720 像素。 （“720p”表示每帧 720 个逐行扫描行。）用于创建预定义配置文件的其他方法均遵循此模式。

或者，你可以通过使用 [**MediaEncodingProfile.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701047) 方法来创建与现有媒体文件相匹配的配置文件。 或者，如果你知道所需的确切编码设置，则可以创建一个新的 [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) 对象，然后填写配置文件详细信息。

## <a name="transcode-the-file"></a>转换文件代码

若要转码文件，请创建新的 [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080) 对象并调用 [**MediaTranscoder.PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936) 方法。 传入源文件、目标文件和编码配置文件。 然后，调用从异步转换代码操作中返回的 [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) 对象上的 [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) 方法。

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## <a name="respond-to-transcoding-progress"></a>响应代码转换进度

你可以注册要在异步进度 [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) 发生更改时响应的事件。 这些事件是通用 Windows 平台 (UWP) 应用的异步编程框架的一部分，且不特定于 API 代码转换操作。

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]


## <a name="encode-a-metadata-stream"></a>对元数据流进行编码
从 Windows 10，版本 1803年开始，你可以包含计时元数据时转换媒体文件。 与上面的视频转换代码示例中，它使用内置媒体编码配置文件创建方法，如[**MediaEncodingProfile.CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4)，你必须手动创建的元数据编码配置文件以支持类型的元数据编码.

创建元数据 incoding 配置文件此第一步是创建描述要转换代码的元数据编码的 [**TimedMetadataEncodingProperties**] 对象。 子类型属性是指定的元数据类型的 GUID。 每个元数据类型的编码详细信息，专并不由 Windows 提供。 在此示例中，使用 GoPro 元数据 (gprs) 的 GUID。 接下来，调用[**SetFormatUserData**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata)以设置描述特定于元数据格式的流格式的数据的二进制文件 blob。 下一步， **TimedMetadataStreamDescriptor**(https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatastreamdescriptor)创建从编码属性，并且轨道标签和名称允许应用程序读取 endcoded 流标识元数据流和 （可选） 在 UI 中显示的流名称。 
 
[!code-cs[GetStreamDescriptor](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetStreamDescriptor)]

创建后**TimedMetadataStreamDescriptor**，你可以创建**MediaEncodingProfile**描述视频、 音频和元数据文件中进行编码。 在最后一个示例中创建**TimedMetadataStreamDescriptor**传递到此示例帮助程序函数，并通过调用[**SetTimedMetadataTracks**](https://docs.microsoft.com/en-us/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks)添加到**MediaEncodingProfile** 。

[!code-cs[GetMediaEncodingProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetMediaEncodingProfile)]
 

 





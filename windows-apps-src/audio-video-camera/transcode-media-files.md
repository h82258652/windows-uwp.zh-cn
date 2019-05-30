---
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: 你可以使用 Windows.Media.Transcoding API 将视频文件代码从一种格式转换为另一种格式。
title: 转换媒体文件代码
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 17eaa79a65bb19156efd230a3442cfde059e5503
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360601"
---
# <a name="transcode-media-files"></a>转换媒体文件代码



你可以使用 [**Windows.Media.Transcoding**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding) API 将视频文件代码从一种格式转换为另一种格式。

*转换代码*是指转换数字媒体文件，例如将视频或音频文件从一种格式转换为另一种格式。 这通常是通过解码文件，然后再重新编码文件实现的。 例如，你可以将 Windows Media 文件转换为 MP4 文件，以使此文件可以在支持 MP4 格式的便携式设备上播放。 或者，你可以将高清视频文件转换为较低分辨率的文件。 在此情况下，重新编码的文件可能使用与原始文件相同的编解码器，但是其编码配置文件不同。

## <a name="set-up-your-project-for-transcoding"></a>设置你的项目以进行转换代码

除了由默认项目模板引用的命名空间，你还需要引用这些命名空间，以便使用本文中的代码转换媒体文件代码。

[!code-cs[Using](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="select-source-and-destination-files"></a>选择源文件和目标文件

应用确定要转换代码的源文件和目标文件的方式取决于实现。 此示例使用 [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 和 [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker)，以允许用户选取源文件和目标文件。

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## <a name="create-a-media-encoding-profile"></a>创建媒体编码配置文件

编码配置文件包含确定如何编码目标文件的设置。 转换文件代码时，此文件中包含的选项最多。

[  **MediaEncodingProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) 类提供用于创建预定义编码配置文件的静态方法：

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

静态 [**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) 方法创建 MP4 编码配置文件。 此方法的参数为视频指定目标分辨率。 在这种情况下，[**VideoEncodingQuality.hd720p**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.VideoEncodingQuality) 表示在每秒 30 帧情况下的 1280 x 720 像素。 （"720p"代表每帧 720 渐进式扫描行数。）用于创建所有预定义配置文件的其他方法遵循此模式。

或者，你可以通过使用 [**MediaEncodingProfile.CreateFromFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createfromfileasync) 方法来创建与现有媒体文件相匹配的配置文件。 或者，如果你知道所需的确切编码设置，则可以创建一个新的 [**MediaEncodingProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) 对象，然后填写配置文件详细信息。

## <a name="transcode-the-file"></a>转换文件代码

若要转码文件，请创建新的 [**MediaTranscoder**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding.MediaTranscoder) 对象并调用 [**MediaTranscoder.PrepareFileTranscodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.transcoding.mediatranscoder.preparefiletranscodeasync) 方法。 传入源文件、目标文件和编码配置文件。 然后，调用从异步转换代码操作中返回的 [**PrepareTranscodeResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding.PrepareTranscodeResult) 对象中的 [**TranscodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) 方法。

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## <a name="respond-to-transcoding-progress"></a>响应转换代码进度

你可以注册要在异步进度 [**TranscodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) 发生更改时响应的事件。 这些事件是通用 Windows 平台 (UWP) 应用的异步编程框架的一部分，且不特定于转换 API 代码。

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]


## <a name="encode-a-metadata-stream"></a>对元数据流进行编码
从 Windows 10，版本 1803，开始可以包含超时元数据时转码媒体文件。 与视频转码不同示例更高版本，使用内置的媒体编码配置文件创建方法，如[ **MediaEncodingProfile.CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4)，您必须手动创建的元数据编码配置文件以支持要编码的元数据的类型。

在创建元数据 incoding 配置文件的此第一步是创建 [**TimedMetadataEncodingProperties**] 对象，描述要转码的元数据的编码。 子类型属性是指定的元数据类型的 GUID。 每个元数据类型的编码详细信息是专有并不由 Windows 提供。 在此示例中，使用 GoPro 元数据 (gprs) 的 GUID。 下一步， [ **SetFormatUserData** ](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata)调用来设置描述特定于元数据格式的流格式的数据的二进制 blob。 下一步， **TimedMetadataStreamDescriptor**(https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatastreamdescriptor)的编码属性，从创建和跟踪标签和名称包括允许读取编码流应用程序，以确定元数据的流和 （可选）在 UI 中显示的流名称。 
 
[!code-cs[GetStreamDescriptor](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetStreamDescriptor)]

在创建后**TimedMetadataStreamDescriptor**，可以创建**MediaEncodingProfile**描述视频、 音频和元数据文件中进行编码。 **TimedMetadataStreamDescriptor**创建的最后一个示例传递到此示例的帮助器函数，并添加到**MediaEncodingProfile**通过调用[ **SetTimedMetadataTracks**](https://docs.microsoft.com/en-us/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks)。

[!code-cs[GetMediaEncodingProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetMediaEncodingProfile)]
 

 





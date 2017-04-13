---
author: drewbatgit
ms.assetid: 0A360481-B649-4E90-9BC4-4449BA7445EF
description: "查询设备上所安装的音频和视频编码器和解码器。"
title: "查询已安装的编解码器"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 编解码器, 编码器, 解码器, 查询"
ms.openlocfilehash: 1bde2c24db6faa843d9118f330867e7f66b22e7e
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="query-for-installed-codecs"></a>查询已安装的编解码器
本文介绍如何查询设备上所安装的音频和视频编码器和解码器。 本文[支持的编解码器](supported-codecs.md) 列出了每个设备系列的操作系统中包含的编解码器。 在单个设备上，用户或应用可以安装其他编解码器。 [CodecQuery](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codecquery) 类可用于列出安装在运行应用的设备上的当前编码器和解码器集。

**CodecQuery** 类包含在 [windows.media.core](https://docs.microsoft.com/en-us/uwp/api/windows.media.core) 命名空间中，并提供不采用任何参数的构造函数。

[!code-cs[CodecQueryUsing](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetCodecQueryUsing)]

[!code-cs[NewCodecQuery](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetNewCodecQuery)]

**CodecQuery** 类的 [FindAllAsync](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codecquery#Windows_Media_Core_CodecQuery_FindAllAsync_Windows_Media_Core_CodecKind_Windows_Media_Core_CodecCategory_System_String_) 方法返回符合指定参数的所有已安装的编解码器。 这些参数包括 [CodecKind](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codeckind) 枚举中一个指定是应该返回音频编解码器还是视频编解码器的值，以及一个指定是应该返回音频编解码器还是视频编解码器的 [CodecCategory](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codeccategory) 值。

最终参数是一个表示媒体子类型（例如 H264 视频或 FLAC 音频）的字符串，从 **FindAllAsync** 中返回的任何编解码器都必须支持该字符串。 你可以为此参数指定 null 或空字符串以返回适用于所有媒体子类型的编解码器。 以下示例列出了当前设备上安装的所有视频编码器。

[!code-cs[FindAllEncoders](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetFindAllEncoders)]

如果你指定媒体子类型，则必须使用[音频子类型 GUID](https://msdn.microsoft.com/library/windows/desktop/aa372553(v=vs.85).aspx) 或[视频子类型 GUID](https://msdn.microsoft.com/library/windows/desktop/aa370819(v=vs.85).aspx) 中列出的其中一个子类型 GUID 的字符串表示形式。 [CodecSubtypes](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codecsubtypes) 类为返回子类型 GUID 的字符串表示形式的大多数受支持媒体子类型提供属性。 你也可以为媒体子类型参数指定 FOURCC 代码。 有关详细信息，请参阅 [FOURCC 代码](https://msdn.microsoft.com/library/windows/desktop/dd375802(v=vs.85).aspx)。 

以下示例使用 FOURCC 代码查询支持 H.264 视频的视频解码器。 此操作的示例方案是在尝试播放视频文件之前检查视频格式是否受支持。

[!code-cs[IsH264Supported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsH264Supported)]

以下示例查询支持 FLAC 媒体子类型的音频编码器。 然后，该示例会为媒体子类型创建一个 AudioEncodinAudioEncodingProperties 对象和 MediaEncodingProfile。 这可用于按指定格式录制音频或转换另一种格式的音频代码。 有关详细信息，请参阅[使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md) 以及[转换媒体文件代码](transcode-media-files.md)。

[!code-cs[IsFLACSupported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsFLACSupported)]

## <a name="related-topics"></a>相关主题

* [支持的编解码器](supported-codecs.md)
* [使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [转换媒体文件代码](transcode-media-files.md)
 

 





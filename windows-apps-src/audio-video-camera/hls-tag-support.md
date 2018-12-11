---
ms.assetid: 66a9cfe2-b212-4c73-8a36-963c33270099
description: 本文列出 UWP 应用支持的 HTTP Live Streaming (HLS) 协议标记。
title: HTTP Live Streaming (HLS) 标记支持
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7c6664d13e76a5774172094d632de9db25109fdc
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8923374"
---
# <a name="http-live-streaming-hls-tag-support"></a>HTTP Live Streaming (HLS) 标记支持
下表列出了 UWP 应用支持的 HLS 标记。

> [!NOTE] 
> 以“X-”开头的自定义标记可以作为定时元数据加以访问，如文章[媒体项、播放列表和曲目](media-playback-with-mediasource.md)中所述。

|标记 |已引入 HLS 协议版本中|HLS 协议文档草案版本|客户端要求|7 月发布的 Windows 10|Windows 10 版本 1511|Windows 10 版本 1607 |
|---------------------|-----------|--------------|---------|--------------|-----|-----|
|4.3.1.  基本标记                 |             |                   |         |             |     |    |
| 4.3.1.1.  EXTM3U |1|0|必填|支持|支持|支持|
| 4.3.1.2.  EXT-X-VERSION |2|3|必填|支持|支持|支持
|4.3.2.  媒体段标记                 |             |                   |         |             |     |    | 
| 4.3.2.1.  EXTINF  |1|0|必填|支持|支持|支持
| 4.3.2.2.  EXT-X-BYTERANGE |4|7|可选|支持|支持|支持|
| 4.3.2.3.  EXT-X-DISCONTINUITY |1|2|可选|支持|支持|支持|
| 4.3.2.4.  EXT-X-KEY |1|0|可选|支持|支持|支持|
|&nbsp;&nbsp;&nbsp; METHOD|1|0|属性|“NONE、AES-128”|“NONE、AES-128”|“NONE、AES-128、SAMPLE-AES”|
|&nbsp;&nbsp;&nbsp; URI|1|0|属性|支持|支持|支持|
|&nbsp;&nbsp;&nbsp; IV|2|3|属性|支持|支持|支持|
|&nbsp;&nbsp;&nbsp; KEYFORMAT|5|9|属性|不支持|不支持|不支持|
|&nbsp;&nbsp;&nbsp; KEYFORMATVERSIONS|5|9|属性|不支持|不支持|不支持|
| 4.3.2.5.  EXT-X-MAP |5|9|可选|不支持|不支持|不支持|
|&nbsp;&nbsp;&nbsp; URI|5|9|属性|不支持|不支持|不支持|
|&nbsp;&nbsp;&nbsp; BYTERANGE|5|9|属性|不支持|不支持|不支持|
| 4.3.2.6.  EXT-X-PROGRAM-DATE-TIME |1|0|可选|不支持|不支持|不支持|
|4.3.3.  媒体播放列表标记                 |             |                   |         |             |     |    | 
| 4.3.3.1.  EXT-X-TARGETDURATION  |1|0|必填|支持|支持|支持|
| 4.3.3.2.  EXT-X-MEDIA-SEQUENCE  |1|0|可选|支持|支持|支持|
| 4.3.3.3.  EXT-X-DISCONTINUITY-SEQUENCE|6|12|可选|不支持|不支持|不支持|
| 4.3.3.4.  EXT-X-ENDLIST |1|0|可选|支持|支持|支持|
| 4.3.3.5.  EXT-X-PLAYLIST-TYPE |3|6|可选|支持|支持|支持|
| 4.3.3.6.  EXT-X-I-FRAMES-ONLY |4|7|可选|不支持|不支持|不支持|
|4.3.4.  主播放列表标记                 |             |                   |         |             |     |    |
| 4.3.4.1.  EXT-X-MEDIA |4|7|可选|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  TYPE|4|7|属性|“AUDIO、VIDEO”|“AUDIO、VIDEO”|“AUDIO、VIDEO、SUBTITLES”|
|&nbsp;&nbsp;&nbsp;  URI|4|7|属性|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  GROUP-ID|4|7|属性|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  LANGUAGE|4|7|属性|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  ASSOC-LANGUAGE|6|13|属性|不支持|不支持|不支持|
|&nbsp;&nbsp;&nbsp;  NAME|4|7|属性|不支持|不支持|支持|
|&nbsp;&nbsp;&nbsp;  DEFAULT|4|7|属性|不支持|不支持|不支持|
|&nbsp;&nbsp;&nbsp;  AUTOSELECT|4|7|属性|不支持|不支持|不支持|
|&nbsp;&nbsp;&nbsp;  FORCED|5|9|属性|不支持|不支持|不支持|
|&nbsp;&nbsp;&nbsp;  INSTREAM-ID|6|12|属性|不支持|不支持|不支持|
|&nbsp;&nbsp;&nbsp;  CHARACTERISTICS|5|9|属性|不支持|不支持|不支持|
| 4.3.4.2.  EXT-X-STREAM-INF  |1|0|必填|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  BANDWIDTH|1|0|属性|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  PROGRAM-ID|1|0|属性|NA|NA|NA|
|&nbsp;&nbsp;&nbsp;  AVERAGE-BANDWIDTH|7|14|属性|不支持|不支持|不支持|
|&nbsp;&nbsp;&nbsp;  CODECS|1|0|属性|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  RESOLUTION|2|3|属性|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  FRAME-RATE|7|15|属性|NA|NA|NA|
|&nbsp;&nbsp;&nbsp;  AUDIO|4|7|属性|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  VIDEO|4|7|属性|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  SUBTITLES|5|9|属性|不支持|不支持|支持|
|&nbsp;&nbsp;&nbsp;  CLOSED-CAPTIONS|6|12|属性|不支持|不支持|不支持|
| 4.3.4.3.  EXT-X-I-FRAME-STREAM-INF  |4|7|可选|不支持|不支持|不支持|
| 4.3.4.4.  EXT-X-SESSION-DATA  |7|14|可选|不支持|不支持|不支持|
| 4.3.4.5.  EXT-X-SESSION-KEY |7|17|可选|不支持|不支持|不支持|
|4.3.5.  媒体或主播放列表标记                  |             |                   |         |             |     |    |
| 4.3.5.1.  EXT-X-INDEPENDENT-SEGMENTS |6|13|可选|不支持|支持|支持|
| 4.3.5.2.  EXT-X-START  |6|12|可选|不支持|部分支持|部分支持|
|&nbsp;&nbsp;&nbsp;  TIME-OFFSET|6|12|属性|不支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  PRECISE|6|12|属性|不支持|默认“不”支持|默认“不”支持|



## <a name="related-topics"></a>相关主题

* [媒体播放](media-playback.md)
* [自适应流式处理](adaptive-streaming.md)
 

 





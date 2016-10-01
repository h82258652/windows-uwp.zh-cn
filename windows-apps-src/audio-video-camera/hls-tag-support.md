---
author: drewbatgit
ms.assetid: 
description: "本文列出 UWP 应用支持的 HTTP Live Streaming (HLS) 协议标记。"
title: "HTTP Live Streaming (HLS) 标记支持"
translationtype: Human Translation
ms.sourcegitcommit: 599e7dd52145d695247b12427c1ebdddbfc4ffe1
ms.openlocfilehash: 779e5d0da7186a6f94251b89cf27636170923d5c

---

# HTTP Live Streaming (HLS) 标记支持
下表列出了 UWP 应用支持的 HLS 标记。

> [!NOTE] 
> 以“X-”开头的自定义标记可以作为定时元数据加以访问，如文章[媒体项、播放列表和曲目](media-playback-with-mediasource.md)中所述。

|标记 |已引入 HLS 协议版本中|HLS 协议文档草案版本|客户端要求|7 月发布的 Windows 10|Windows 10 版本 1511|Windows 10 版本 1606 |
|---------------------|-----------|--------------|---------|--------------|
|4.3.1.  基本标记                 |             |                   |         |             |     |    |
| 4.3.1.1.  EXTM3U |1|0|必填|支持|支持|支持|
| 4.3.1.2.  EXT-X-VERSION |2|3|必填|支持|支持|支持
|4.3.2.  媒体段标记                 |             |                   |         |             |     |    | 
| 4.3.2.1.  EXTINF  |1|0|必填|支持|支持|支持
| 4.3.2.2.  EXT-X-BYTERANGE |4|7|可选|支持|支持|支持|
| 4.3.2.3.  EXT-X-DISCONTINUITY |1|2|可选|支持|支持|支持|
| 4.3.2.4.  EXT-X-KEY |1|0|可选|支持|支持|支持|
|&nbsp;&nbsp;&nbsp; 方法|1|0|属性|“NONE、AES-128”|“NONE、AES-128”|“NONE、AES-128、SAMPLE-AES”|
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
|&nbsp;&nbsp;&nbsp;  类型|4|7|属性|“AUDIO、VIDEO”|“AUDIO、VIDEO”|“AUDIO、VIDEO、SUBTITLES”|
|&nbsp;&nbsp;&nbsp;  URI|4|7|属性|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  GROUP-ID|4|7|属性|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  语言|4|7|属性|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  ASSOC-LANGUAGE|6|13|属性|不支持|不支持|不支持|
|&nbsp;&nbsp;&nbsp;  名称|4|7|属性|不支持|不支持|支持|
|&nbsp;&nbsp;&nbsp;  默认值|4|7|属性|不支持|不支持|不支持|
|&nbsp;&nbsp;&nbsp;  自动选择|4|7|属性|不支持|不支持|不支持|
|&nbsp;&nbsp;&nbsp;  强制|5|9|属性|不支持|不支持|不支持|
|&nbsp;&nbsp;&nbsp;  INSTREAM-ID|6|12|属性|不支持|不支持|不支持|
|&nbsp;&nbsp;&nbsp;  特性|5|9|属性|不支持|不支持|不支持|
| 4.3.4.2.  EXT-X-STREAM-INF  |1|0|必填|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  带宽|1|0|属性|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  PROGRAM-ID|1|0|属性|NA|NA|NA|
|&nbsp;&nbsp;&nbsp;  平均带宽|7|14|属性|不支持|不支持|不支持|
|&nbsp;&nbsp;&nbsp;  编解码器|1|0|属性|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  解决方案|2|3|属性|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  帧速率|7|15|属性|NA|NA|NA|
|&nbsp;&nbsp;&nbsp;  音频|4|7|属性|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  视频|4|7|属性|支持|支持|支持|
|&nbsp;&nbsp;&nbsp;  字幕|5|9|属性|不支持|不支持|支持|
|&nbsp;&nbsp;&nbsp;  隐藏字幕|6|12|属性|不支持|不支持|不支持|
| 4.3.4.3.  EXT-X-I-FRAME-STREAM-INF  |4|7|可选|不支持|不支持|不支持|
| 4.3.4.4.  EXT-X-SESSION-DATA  |7|14|可选|不支持|不支持|不支持|
| 4.3.4.5.  EXT-X-SESSION-KEY |7|17|可选|不支持|不支持|不支持|




## 相关主题

* [媒体播放](media-playback.md)
* [自适应流式处理](adaptive-streaming.md)
 

 







<!--HONumber=Aug16_HO3-->



---
author: drewbatgit
ms.assetid: 3E0FBB43-F6A4-4558-AA89-20E7760BA73F
description: "本文列出了 UWP 应用支持的 HTTP 动态自适应流式处理 (DASH) 配置文件。"
title: "HTTP 动态自适应流式处理 (DASH) 配置文件支持"
ms.author: drewbat
ms.date: 02/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 9ca8f2d1783a73ae38fed1d3ee1e58b809ddd2aa
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="dynamic-adaptive-streaming-over-http-dash-profile-support"></a>HTTP 动态自适应流式处理 (DASH) 配置文件支持
下表列出了 UWP 应用支持的 DASH 配置文件。



|标记 | 清单类型 | 备注|7 月发布的 Windows 10|Windows 10 版本 1511|Windows 10 版本 1607 |Windows 10 版本 1607 |Windows 10 版本 1703|
|----------------|------|-------|-----------|--------------|---------|-------|--------|
|urn:mpeg:dash:profile:isoff-live:2011 | 静态 |     |支持            |  支持              | 支持        |支持| 支持|
|urn:mpeg:dash:profile:isoff-main:2011 |        | 最大努力 | 支持            |  支持              | 支持        |支持| 支持|
|urn:mpeg:dash:profile:isoff-live:2011 | 动态 | $Time$ 受支持，但 $Number$ 在类别模板中不受支持 | 不支持            | 不支持              | 不支持        |不支持| 支持|


## <a name="related-topics"></a>相关主题

* [媒体播放](media-playback.md)
* [自适应流式处理](adaptive-streaming.md)
 

 





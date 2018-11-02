---
author: drewbatgit
ms.assetid: 3E0FBB43-F6A4-4558-AA89-20E7760BA73F
description: 本文列出了 UWP 应用支持的 HTTP 动态自适应流式处理 (DASH) 配置文件。
title: HTTP 动态自适应流式处理 (DASH) 配置文件支持
ms.author: drewbat
ms.date: 02/15/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7a4ec9f9e81010d39af496da156afa676f4b3714
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5968721"
---
# <a name="dynamic-adaptive-streaming-over-http-dash-profile-support"></a>HTTP 动态自适应流式处理 (DASH) 配置文件支持


## <a name="supported-dash-profiles"></a>支持的 DASH 配置文件
下表列出了 UWP 应用支持的 DASH 配置文件。

|标记 | 清单类型 | 备注|7 月发布的 Windows 10|Windows 10 版本 1511|Windows 10 版本 1607 |Windows 10 版本 1607 |Windows 10 版本 1703|
|----------------|------|-------|-----------|--------------|---------|-------|--------|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | 静态 |     |支持            |  支持              | 支持        |支持| 受支持|
|urn:mpeg&#58;dash:profile:isoff-main:2011 |        | 最大努力 | 支持            |  支持              | 支持        |支持| 受支持|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | 动态 | $Time$ 受支持，但 $Number$ 在类别模板中不受支持 | 不支持            | 不支持              | 不支持        |不支持| 受支持|


## <a name="unsupported-dash-profiles"></a>不支持的 DASH 配置文件
上表中未列出的即为不支持的配置文件，包括但不限于以下配置文件：

* urn:mpeg&#58;dash:profile:full:2011
* urn:mpeg&#58;dash:profile:isoff-on-demand:2011
* urn:mpeg&#58;dash:profile:mp2t-main:2011
* urn:mpeg&#58;dash:profile:mp2t-simple:2011


## <a name="related-topics"></a>相关主题

* [媒体播放](media-playback.md)
* [自适应流式处理](adaptive-streaming.md)
 

 





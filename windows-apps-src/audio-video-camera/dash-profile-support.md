---
ms.assetid: 3E0FBB43-F6A4-4558-AA89-20E7760BA73F
description: 本文列出了 UWP 应用支持的 HTTP 动态自适应流式处理 (DASH) 配置文件。
title: HTTP 动态自适应流式处理 (DASH) 配置文件支持
ms.date: 02/15/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 268020c06be57d8ac300f1202046e52b6e1d2507
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867386"
---
# <a name="dynamic-adaptive-streaming-over-http-dash-profile-support"></a>HTTP 动态自适应流式处理 (DASH) 配置文件支持


## <a name="supported-dash-profiles"></a>支持的 DASH 配置文件
下表列出了 UWP 应用支持的 DASH 配置文件。

|Tag | 清单类型 | 说明|7 月发布的 Windows 10|Windows 10 版本 1511|Windows 10 版本 1607 |Windows 10 版本 1607 |Windows 10 版本 1703| Windows 10 版本1809
|----------------|------|-------|-----------|--------------|---------|-------|--------|--------|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | Static |     |支持            |  支持              | 支持        |支持| 支持| 支持|
|urn:mpeg&#58;dash:profile:isoff-main:2011 |        | 最大努力 | 支持            |  支持              | 支持        |支持| 支持| 支持|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | 动态 | $Time$ 受支持，但 $Number$ 在类别模板中不受支持 | 不支持            | 不支持              | 不支持        |不支持| 支持| 支持|
|urn:mpeg&#58;dash:profile:isoff-on-demand:2011 |        |  | 不支持            |  不支持              | 不支持        |不支持| 不支持| 支持|


## <a name="unsupported-dash-profiles"></a>不支持的 DASH 配置文件
上表中未列出的即为不支持的配置文件，包括但不限于以下配置文件：

* urn:mpeg&#58;dash:profile:full:2011
* urn:mpeg&#58;dash:profile:mp2t-main:2011
* urn:mpeg&#58;dash:profile:mp2t-simple:2011


## <a name="related-topics"></a>相关主题

* [媒体播放](media-playback.md)
* [自适应流式处理](adaptive-streaming.md)
 

 





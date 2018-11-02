---
title: SessionEntry (JSON)
assetID: b5cf5c3d-83b8-635f-d1a5-0be5d9434ea5
permalink: en-us/docs/xboxlive/rest/json-sessionentry.html
author: KevinAsgari
description: " SessionEntry (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a8ec59202300912d4646b0e759e7bbf037853608
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5928929"
---
# <a name="sessionentry-json"></a>SessionEntry (JSON)
用于健身会话中包含的数据。 
<a id="ID4EN"></a>

 
## <a name="sessionentry"></a>SessionEntry
 
SessionEntry 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| durationInSeconds| 32 位有符号整数 | 持续时间，以秒为单位 — 的会话。 | 
| 焦耳为单位| 32 位有符号整数 | 能源 — 以焦耳为单位 — 刻录在会话中。 | 
| 满足| 单精度浮点数| 会话持续时间的平均满足的值。 满足值是相对于单个新陈代谢速率的其余部分活动期间个人新陈代谢速率的比率。 因为 resting 新陈代谢率是个人的权重，无论 1.0 和满足的值为相对于单个停留新陈代谢速率，它们可以用于比较的不同粗细个人正在执行活动的强度。| 
| serverTimestamp| DateTime| 时间，具体取决于 UTC — 入口服务器上输入。 | 
| 源| 8 位无符号的整数| 会话源。| 
| 时间戳| DateTime| 时间--基于在协调世界时 （utc 时间）-在客户端上创建条目。 | 
| titleId| 64 位无符号的整数| 游戏-以十进制 — 创建条目。| 
  
<a id="ID4EFE"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
   "titleId" : "1234567",
   "timestamp" : "2011-11-18T08:08:46Z",
   "serverTimestamp" : "2011-11-20T04:04:23Z",
   "durationInSeconds" : 240,
   "joules" :  1600,
   "met" :  "124"
   "source" :  "1"
}
    
```

  
<a id="ID4EOE"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EQE"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   
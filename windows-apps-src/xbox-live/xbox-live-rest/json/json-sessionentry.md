---
title: SessionEntry (JSON)
assetID: b5cf5c3d-83b8-635f-d1a5-0be5d9434ea5
permalink: en-us/docs/xboxlive/rest/json-sessionentry.html
description: " SessionEntry (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 73133f898ff219477cb60f54798cbd81acb87ebe
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2018
ms.locfileid: "8747611"
---
# <a name="sessionentry-json"></a>SessionEntry (JSON)
对于的适用性会话包含的数据。 
<a id="ID4EN"></a>

 
## <a name="sessionentry"></a>SessionEntry
 
SessionEntry 对象具有以下规范。
 
| 成员| 类型| 描述| 
| --- | --- | --- | 
| durationInSeconds| 32 位有符号整数 | 持续时间，以秒为单位 — 的会话。 | 
| 焦耳为单位| 32 位有符号整数 | 能源 — 以焦耳为单位 — 刻录在会话中。 | 
| 满足| 单精度浮点数| 会话持续时间的平均满足的值。 满足值是相对于静态的个人新陈代谢速率活动期间的个人新陈代谢速率的比率。 因为休眠新陈代谢率是个人的权重，无论 1.0 和满足的值为相对于个人停留新陈代谢速率，它们可以用于比较的不同粗细个人正在执行活动的强度。| 
| serverTimestamp| DateTime| 时间，具体取决于 UTC — 入口服务器上输入。 | 
| 源| 8 位无符号的整数| 会话源。| 
| 时间戳| DateTime| 时间--基于上协调世界时 （utc 时间）-在客户端上创建条目。 | 
| titleId| 64 位无符号的整数| 游戏-以十进制表示 — 创建条目。| 
  
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

   
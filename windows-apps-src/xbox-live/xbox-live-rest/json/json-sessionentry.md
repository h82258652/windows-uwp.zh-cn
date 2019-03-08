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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589732"
---
# <a name="sessionentry-json"></a>SessionEntry (JSON)
包含适用性会话数据。 
<a id="ID4EN"></a>

 
## <a name="sessionentry"></a>SessionEntry
 
SessionEntry 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| durationInSeconds| 32 位有符号的整数 | 持续时间，以秒为单位，会话。 | 
| 焦耳为单位| 32 位有符号的整数 | 能源 — 以焦耳为单位，在会话中刻录。 | 
| 满足| 单精度浮点数| 平均会话持续时间内满足的值。 点。 值是相对于个人的静态代谢速率活动期间的个人代谢率的比率。 因为停留的代谢率是 1.0 而不考虑个人的权重，点。 值是相对于个人的停留代谢速率，它们可用于比较不同权重的个人正在执行的活动的强度。| 
| serverTimestamp| DateTime| 时间-基于 UTC — 条目输入服务器上。 | 
| 源| 8 位无符号的整数| 会话源。| 
| timestamp| DateTime| 时间-基于协调世界时 (UTC)，在客户端上创建条目。 | 
| titleId| 64 位无符号的整数| 标题-十进制 — 创建条目。| 
  
<a id="ID4EFE"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   
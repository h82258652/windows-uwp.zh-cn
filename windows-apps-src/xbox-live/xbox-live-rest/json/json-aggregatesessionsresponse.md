---
title: AggregateSessionsResponse (JSON)
assetID: 020ee9b2-c96c-2e65-4e6d-f9f4bd25a374
permalink: en-us/docs/xboxlive/rest/json-aggregatesessionsresponse.html
author: KevinAsgari
description: " AggregateSessionsResponse (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 590acdd89d83fa21a401b5573053ba341f7f05c7
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5922085"
---
# <a name="aggregatesessionsresponse-json"></a>AggregateSessionsResponse (JSON)
用户的适用性会话都包含聚合的数据。 
<a id="ID4EN"></a>

 
## <a name="aggregatesessionsresponse"></a>AggregateSessionsResponse
 
AggregateSessionsResponse 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| totalDurationInSeconds| 64 位有符号整数| 以秒为单位聚合时段的会话的总持续时间。| 
| totalJoules| 64 位有符号整数| 总能耗刻录 — 以焦耳为单位，聚合时段。 | 
| totalSessions| 64 位有符号整数| 聚合时段的会话的总次数。| 
| weightedAverageMets| 单精度浮点数 | 聚合时段的任务 （满足） 值的加权平均值新陈代谢等效。 满足值是相对于单个新陈代谢速率的其余部分活动期间个人新陈代谢速率的比率。 因为 resting 新陈代谢率是个人的权重，无论 1.0 和满足的值为相对于单个停留新陈代谢速率，它们可以用于比较的不同粗细个人正在执行活动的强度。| 
  
<a id="ID4ESC"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
   "totalSessions" : 300,
   "totalDurationInSeconds" : 1240,
   "totalJoules" :  21600,
   "weightedAvgMet" : 21,
}

    
```

  
<a id="ID4E2C"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E4C"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   
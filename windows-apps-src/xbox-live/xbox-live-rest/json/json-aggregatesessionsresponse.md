---
title: AggregateSessionsResponse (JSON)
assetID: 020ee9b2-c96c-2e65-4e6d-f9f4bd25a374
permalink: en-us/docs/xboxlive/rest/json-aggregatesessionsresponse.html
description: " AggregateSessionsResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ef026fd5096d047b2014faaf95a667c69827e043
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608302"
---
# <a name="aggregatesessionsresponse-json"></a>AggregateSessionsResponse (JSON)
包含聚合的数据的用户的适用性会话。 
<a id="ID4EN"></a>

 
## <a name="aggregatesessionsresponse"></a>AggregateSessionsResponse
 
AggregateSessionsResponse 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| totalDurationInSeconds| 64 位有符号的整数| 以秒为单位的聚合时间段内的会话的总持续时间。| 
| totalJoules| 64 位有符号的整数| 总能源 — 以焦耳为单位，聚合期间。 | 
| totalSessions| 64 位有符号的整数| 聚合期间的会话的总数。| 
| weightedAverageMets| 单精度浮点数 | 在聚合时间段内的任务 （点。） 值的加权平均代谢等效。 点。 值是相对于个人的静态代谢速率活动期间的个人代谢率的比率。 因为停留的代谢率是 1.0 而不考虑个人的权重，点。 值是相对于个人的停留代谢速率，它们可用于比较不同权重的个人正在执行的活动的强度。| 
  
<a id="ID4ESC"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   
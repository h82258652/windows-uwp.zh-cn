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
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2018
ms.locfileid: "8332383"
---
# <a name="aggregatesessionsresponse-json"></a>AggregateSessionsResponse (JSON)
为用户的适用性会话包含聚合的数据。 
<a id="ID4EN"></a>

 
## <a name="aggregatesessionsresponse"></a>AggregateSessionsResponse
 
AggregateSessionsResponse 对象具有以下规范。
 
| 成员| 类型| 描述| 
| --- | --- | --- | 
| totalDurationInSeconds| 64 位有符号整数| 以秒为单位聚合时段的会话的总持续时间。| 
| totalJoules| 64 位有符号整数| 总能耗刻录-以焦耳为单位，聚合时段。 | 
| totalSessions| 64 位有符号整数| 聚合时段的会话的总次数。| 
| weightedAverageMets| 单精度浮点数 | 聚合时段的任务 （满足） 值的加权平均值新陈代谢等效。 满足值是相对于静态的个人新陈代谢速率活动期间个人新陈代谢率的比率。 因为休眠新陈代谢率是个人的权重，无论 1.0，并满足值是相对于个人休眠新陈代谢速率，它们可以用于比较的不同粗细的个人正在执行活动的强度。| 
  
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

   
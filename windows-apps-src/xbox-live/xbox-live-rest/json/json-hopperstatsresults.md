---
title: HopperStatsResults (JSON)
assetID: 91927da1-2e97-f7bc-ae62-7e0e9966b98e
permalink: en-us/docs/xboxlive/rest/json-hopperstatsresults.html
author: KevinAsgari
description: " HopperStatsResults (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b6f1322d2c22e65c33667ea409b10d9209628d3b
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5873056"
---
# <a name="hopperstatsresults-json"></a>HopperStatsResults (JSON)
表示漏斗的统计信息的 JSON 对象。 
<a id="ID4EN"></a>

  
 
HopperStatsResults JSON 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| hopperName| 字符串| 所选的漏斗的名称。| 
| waitTime| 32 位有符号整数| 匹配漏斗 （不可或缺秒数） 时间的平均。 | 
| 填充| 32 位有符号整数| 等待匹配漏斗中的用户数。| 
  
<a id="ID4EW"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例 
 

```json
{
      "hopperName":"contosoawesome2",
      "waitTime":30,
      "population":1
    }
  
    
```

  
<a id="ID4EGB"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EIB"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EUB"></a>

 
##### <a name="reference"></a>参考 

[GET (/serviceconfigs/{scid}/hoppers/{name}/stats)](../uri/matchtickets/uri-serviceconfigsscidhoppershoppernamestatsget.md)

   
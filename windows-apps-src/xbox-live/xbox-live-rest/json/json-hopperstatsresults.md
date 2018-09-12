---
title: HopperStatsResults (JSON)
assetID: 91927da1-2e97-f7bc-ae62-7e0e9966b98e
permalink: en-us/docs/xboxlive/rest/json-hopperstatsresults.html
author: KevinAsgari
description: " HopperStatsResults (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 17b6bf856dd87abc72a000cb92724baf91452d73
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3935617"
---
# <a name="hopperstatsresults-json"></a>HopperStatsResults (JSON)
表示漏斗的统计信息的 JSON 对象。 
<a id="ID4EN"></a>

  
 
HopperStatsResults JSON 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| hopperName| 字符串| 所选的漏斗的名称。| 
| waitTime| 32 位有符号的整数| 匹配漏斗 （不可或缺秒数） 时间的平均。 | 
| 填充| 32 位有符号的整数| 等待匹配漏斗中的用户数。| 
  
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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EUB"></a>

 
##### <a name="reference"></a>参考 

[获取 (/serviceconfigs/ {scid} /hoppers/ {name} / 统计数据)](../uri/matchtickets/uri-serviceconfigsscidhoppershoppernamestatsget.md)

   
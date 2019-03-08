---
title: HopperStatsResults (JSON)
assetID: 91927da1-2e97-f7bc-ae62-7e0e9966b98e
permalink: en-us/docs/xboxlive/rest/json-hopperstatsresults.html
description: " HopperStatsResults (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 38e345fc20e92cdf6446c6ae1100e347fe634eff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646202"
---
# <a name="hopperstatsresults-json"></a>HopperStatsResults (JSON)
表示 hopper 的统计信息的 JSON 对象。 
<a id="ID4EN"></a>

  
 
HopperStatsResults JSON 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| hopperName| 字符串| 所选 hopper 的名称。| 
| waitTime| 32 位有符号的整数| 平均匹配 hopper （整数秒数） 的时间。 | 
| 填充| 32 位有符号的整数| 等待 hopper 中的匹配项的人员数。| 
  
<a id="ID4EW"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法 
 

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

[GET (/serviceconfigs/{scid}/hoppers/{name}/stats)](../uri/matchtickets/uri-serviceconfigsscidhoppershoppernamestatsget.md)

   
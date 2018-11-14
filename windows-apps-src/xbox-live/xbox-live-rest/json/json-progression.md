---
title: Progression (JSON)
assetID: cdff6415-f12b-0a45-61f2-26dbf47b1b56
permalink: en-us/docs/xboxlive/rest/json-progression.html
author: KevinAsgari
description: " Progression (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 173c7dd42e0e5fda0d18e270c32a594171ebd7b6
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6267185"
---
# <a name="progression-json"></a>Progression (JSON)
在用户解锁成就的进度。 
<a id="ID4EN"></a>

 
## <a name="progression"></a>进度
 
进度对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| 要求| 要求的数组| 若要获得成就的要求，沿用户远距离解锁它。| 
| timeUnlocked| DateTime| 成就是第一家解锁时间。| 
  
<a id="ID4ETB"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
  "requirements":
  [{
    "id":"12345678-1234-1234-1234-123456789012",
    "current":null,
    "target":"100"
  }],
  "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
}
    
```

  
<a id="ID4E3B"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E5B"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   
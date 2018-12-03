---
title: Progression (JSON)
assetID: cdff6415-f12b-0a45-61f2-26dbf47b1b56
permalink: en-us/docs/xboxlive/rest/json-progression.html
description: " Progression (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dda124e5be9a4d21a1ee5b9d6130290207e31921
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8340392"
---
# <a name="progression-json"></a>Progression (JSON)
在用户解锁成就的进度。 
<a id="ID4EN"></a>

 
## <a name="progression"></a>进度
 
进度对象具有以下规范。
 
| 成员| 类型| 描述| 
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

   
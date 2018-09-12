---
title: 进度 (JSON)
assetID: cdff6415-f12b-0a45-61f2-26dbf47b1b56
permalink: en-us/docs/xboxlive/rest/json-progression.html
author: KevinAsgari
description: " 进度 (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5a0e534d92e4bcb77565f59de5252afcbbe3eef5
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3932745"
---
# <a name="progression-json"></a>进度 (JSON)
用户解锁成就的进度。 
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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   
---
title: ActivityRecord (JSON)
assetID: e3a7465b-3451-0266-f8ba-b7602b59f7af
permalink: en-us/docs/xboxlive/rest/json-activityrecord.html
author: KevinAsgari
description: " ActivityRecord (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: bb78941f2ab9cab4395dbb1ba5eb8fb3b09dee08
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5764109"
---
# <a name="activityrecord-json"></a>ActivityRecord (JSON)
有关一个或多个用户的完整状态格式化和本地化字符串。 
<a id="ID4EN"></a>

 
## <a name="activityrecord"></a>ActivityRecord
 
ActivityRecord 对象具有以下规范。
 
| 成员| 类型| 描述| 
| --- | --- | --- | 
| richPresence| 字符串| 完整状态字符串格式化和本地化。| 
| 媒体| MediaRecord| 哪些用户观看或收听。| 
  
<a id="ID4ETB"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
        richPresence:"Team Deathmatch on Nirvana"
      }
    
```

  
<a id="ID4E3B"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E5B"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   
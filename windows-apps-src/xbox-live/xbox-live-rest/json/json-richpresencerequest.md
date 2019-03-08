---
title: RichPresenceRequest (JSON)
assetID: 599266be-f747-0be1-fadf-f8e0262dc27f
permalink: en-us/docs/xboxlive/rest/json-richpresencerequest.html
description: " RichPresenceRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4c49da63ecd091a886a68f508af09e33fb9c58ac
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654122"
---
# <a name="richpresencerequest-json"></a>RichPresenceRequest (JSON)
丰富的状态显示信息应使用其中的相关信息的请求。 
<a id="ID4EN"></a>

 
## <a name="richpresencerequest"></a>RichPresenceRequest
 
RichPresenceRequest 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| id| 字符串| <b>FriendlyName</b>的要使用的丰富的状态显示字符串。| 
| scid| 字符串| 告诉我们其中定义丰富的状态显示字符串的 Scid。| 
| params| 字符串数组| 数组<b>friendlyName</b>字符串用来完成的丰富的状态显示字符串。 应该指定仅枚举友好名称，不统计信息。将此留空，则将删除以前的任何值。| 
  
<a id="ID4EDC"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

```json
{
      id:"playingMapWeapon",
      scid:"abba0123-08ba-48ca-9f1a-21627b189b0f",
    }
    
```

  
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   
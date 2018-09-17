---
title: RichPresenceRequest (JSON)
assetID: 599266be-f747-0be1-fadf-f8e0262dc27f
permalink: en-us/docs/xboxlive/rest/json-richpresencerequest.html
author: KevinAsgari
description: " RichPresenceRequest (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9d1158832623b88efb0a614680f0c0fb579f79d4
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2018
ms.locfileid: "3984005"
---
# <a name="richpresencerequest-json"></a>RichPresenceRequest (JSON)
完整状态信息应使用哪些信息请求。 
<a id="ID4EN"></a>

 
## <a name="richpresencerequest"></a>RichPresenceRequest
 
RichPresenceRequest 对象具有以下规范。
 
| 成员| 类型| 描述| 
| --- | --- | --- | 
| id| 字符串| 要使用的完整状态字符串<b>friendlyName</b> 。| 
| scid| 字符串| 告诉我们定义的完整状态字符串的位置的 Scid。| 
| 参数| 字符串的数组| 用来完成的完整状态字符串<b>friendlyName</b>字符串数组。 应指定仅枚举友好名称，不统计数据。保留此为空，则将删除以前的任何值。| 
  
<a id="ID4EDC"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

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

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   
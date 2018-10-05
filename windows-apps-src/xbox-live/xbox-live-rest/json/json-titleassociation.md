---
title: TitleAssociation (JSON)
assetID: 05f4edbb-cc3d-c17d-0979-aac9e44a4988
permalink: en-us/docs/xboxlive/rest/json-titleassociation.html
author: KevinAsgari
description: " TitleAssociation (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c9d42f4285cd20785f5606d3e2ac6094a874acba
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4359292"
---
# <a name="titleassociation-json"></a>TitleAssociation (JSON)
与成就关联的标题。 
<a id="ID4EN"></a>

 
## <a name="titleassociation"></a>TitleAssociation
 
TitleAssociation 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| name| 字符串| 本地化的内容的名称。| 
| id| 字符串| TitleId （32 位无符号整数、 小数中返回）。| 
| version| 字符串| （如果适用） 相关联的游戏的特定版本。| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
  "name":"Microsoft Achievements Sample",
  "id":3051199919,
  "version":"abc"
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   
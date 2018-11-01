---
title: TitleAssociation (JSON)
assetID: 05f4edbb-cc3d-c17d-0979-aac9e44a4988
permalink: en-us/docs/xboxlive/rest/json-titleassociation.html
author: KevinAsgari
description: " TitleAssociation (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c90dbca0e16cf1dcebc53fd8fa90006ca7ae7caf
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5863788"
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

   
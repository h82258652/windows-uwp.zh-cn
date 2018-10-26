---
title: PagingInfo (JSON)
assetID: 645e575d-3e8e-d954-90e6-e51dd83da93b
permalink: en-us/docs/xboxlive/rest/json-paginginfo.html
author: KevinAsgari
description: " PagingInfo (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c9479488961de40ef5019d1ebe067f29dffb16dd
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5569407"
---
# <a name="paginginfo-json"></a>PagingInfo (JSON)
包含分页信息中的数据的页面将返回的结果。 
<a id="ID4EN"></a>

 
## <a name="paginginfo"></a>PagingInfo
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| ContinuationToken| 字符串| 用于访问结果的下一步页面不透明延续令牌。 最大 32 个字符。调用方可以提供<b>continuationToken</b>查询参数中的此值，以检索下一组的集合中的项。 如果此属性为<b>null</b>，则没有任何其他项目集合中。 此属性是必需的并且即使与<b>skipItems</b>调时集合提供。| 
| totalItems| 32 位有符号整数| 集合中项的总数。 这不会提供服务是否处于无法提供实时概况集合的大小。| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
   "continuationToken":"16354135464161213-0708c1ba-147f-48cc-9ad9-546gaadg648"
   "totalItems":5,
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   
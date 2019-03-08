---
title: PagingInfo (JSON)
assetID: 645e575d-3e8e-d954-90e6-e51dd83da93b
permalink: en-us/docs/xboxlive/rest/json-paginginfo.html
description: " PagingInfo (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0e773d73499e79fe23f736a536027932ca1a07b4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651212"
---
# <a name="paginginfo-json"></a>PagingInfo (JSON)
包含分页信息的数据的页面中返回的结果。 
<a id="ID4EN"></a>

 
## <a name="paginginfo"></a>PagingInfo
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| ContinuationToken| 字符串| 用于访问下一页结果的不透明继续标记。 最多 32 个字符。调用方可以提供此值在<b>continuationToken</b>查询以检索下一组集合中的项的参数。 如果此属性为<b>null</b>，然后在集合中没有其他项。 此属性是必需的即使使用换集合时提供<b>skipItems</b>。| 
| totalItems| 32 位有符号的整数| 集合中项的总数。 如果该服务不能提供到集合的大小的实时视图不提供该参数。| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   
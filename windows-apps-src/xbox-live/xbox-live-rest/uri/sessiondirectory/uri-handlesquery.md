---
title: /handles/query
assetID: e00d31ad-b9ba-8e52-1333-83192eab0446
permalink: en-us/docs/xboxlive/rest/uri-handlesquery.html
description: " /handles/query"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: eaa148972ce1e65056470a6c4082cb4e50de3f09
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8918314"
---
# <a name="handlesquery"></a>/handles/query
支持 POST 操作以创建用于会话句柄查询。 

> [!NOTE] 
> 此 URI 使用 2015年多人游戏和应用仅向该多人游戏版本及更高版本。 它旨在使用模板合约 104/105 或更高版本。  

 
<a id="ID4EQ"></a>

 
## <a name="domain"></a>域
sessiondirectory.xboxlive.com  
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
此 URI 支持的句柄查询。 会话与查询不同，这些字符串和批处理查询句柄查询使用查询处理器样式。 最多 100 句柄均受支持。  
<a id="ID4E2"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
无   
<a id="ID4EEB"></a>

 
## <a name="valid-methods"></a>有效的方法

[POST (/handles/query)](uri-handlesquerypost.md)

&nbsp;&nbsp;创建查询会话句柄。

[POST (/handles/query?include=relatedInfo)](uri-handlesqueryincludepost.md)

&nbsp;&nbsp;创建会话句柄包含相关的会话信息的查询。
 
<a id="ID4EQB"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ESB"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[会话目录 URI](atoc-reference-sessiondirectory.md)

   
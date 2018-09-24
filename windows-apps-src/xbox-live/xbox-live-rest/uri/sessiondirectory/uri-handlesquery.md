---
title: /handles/query
assetID: e00d31ad-b9ba-8e52-1333-83192eab0446
permalink: en-us/docs/xboxlive/rest/uri-handlesquery.html
author: KevinAsgari
description: " /handles/query"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fbb8a823581f357e42cd13bb1331808584301f5e
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "4153725"
---
# <a name="handlesquery"></a>/handles/query
支持创建会话句柄查询 POST 操作。 

> [!NOTE] 
> 此 URI 由 2015年多人游戏和应用仅向该多人游戏版本及更高版本。 它旨在使用模板合约 104/105 或更高版本。  

 
<a id="ID4EQ"></a>

 
## <a name="domain"></a>域
sessiondirectory.xboxlive.com  
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
此 URI 支持的句柄查询。 会话与查询不同，字符串和批处理查询，即句柄查询使用查询处理器样式。 最多 100 句柄均受支持。  
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

   
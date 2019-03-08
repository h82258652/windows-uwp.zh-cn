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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598562"
---
# <a name="handlesquery"></a>/handles/query
支持 POST 操作创建的会话句柄的查询。 

> [!NOTE] 
> 此 URI 由 2015年之多人游戏，并且仅适用于该多玩家版本和更高版本。 它旨在用于模板协定 104/105 或更高版本。  

 
<a id="ID4EQ"></a>

 
## <a name="domain"></a>域
sessiondirectory.xboxlive.com  
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
此 URI 支持的句柄的查询。 与会话查询，这是字符串和批处理的查询，不同句柄的查询使用查询处理器样式。 支持多达 100 句柄。  
<a id="ID4E2"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
无   
<a id="ID4EEB"></a>

 
## <a name="valid-methods"></a>有效的方法

[POST （/处理/查询）](uri-handlesquerypost.md)

&nbsp;&nbsp;创建查询的会话句柄。

[POST (/handles/query?include=relatedInfo)](uri-handlesqueryincludepost.md)

&nbsp;&nbsp;创建包含相关的会话信息的会话句柄的查询。
 
<a id="ID4EQB"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ESB"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[会话目录 Uri](atoc-reference-sessiondirectory.md)

   
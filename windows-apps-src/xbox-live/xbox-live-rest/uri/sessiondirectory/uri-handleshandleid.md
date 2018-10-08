---
title: /handles/{handleId}
assetID: 5b722d3e-fe80-fec5-a26b-8b3db6422004
permalink: en-us/docs/xboxlive/rest/uri-handleshandleid.html
author: KevinAsgari
description: " /handles/{handleId}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 47dda291a9a86ccbee69e1e51ca71be373f5dc1d
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/08/2018
ms.locfileid: "4417511"
---
# <a name="handleshandleid"></a>/handles/{handleId}
支持会话句柄由标识符指定的删除和 GET 的操作。 

> [!NOTE] 
> 此 URI 由 2015年多人游戏和应用仅向该多人游戏版本及更高版本。 它旨在与模板合约 104/105 或更高版本一起使用。  

 
<a id="ID4EQ"></a>

 
## <a name="domain"></a>域
sessiondirectory.xboxlive.com  
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 描述| 
| --- | --- | --- | --- | 
| handleId| GUID| 会话句柄的唯一 ID。| 
  
<a id="ID4ERB"></a>

 
## <a name="valid-methods"></a>有效的方法

[DELETE (/handles/{handleId})](uri-handleshandleiddelete.md)

&nbsp;&nbsp;删除由句柄 ID 指定的句柄

[GET (/handles/{handle-id})](uri-handleshandleidget.md)

&nbsp;&nbsp;检索句柄 ID 指定的句柄
 
<a id="ID4E4B"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E6B"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[会话目录 URI](atoc-reference-sessiondirectory.md)

   
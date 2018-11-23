---
title: /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
assetID: edcb19bd-87a5-732b-0c45-6f7355fc2dd1
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameindex.html
author: KevinAsgari
description: " /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3d95d3f0f171fa0e529d57ab5deca8160ddc3c43
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2018
ms.locfileid: "7572677"
---
# <a name="usersxuidxuidlistspinslistnameindexindexinsertindexinsertindex"></a>/users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
将移动列表中的一项。 这些 Uri 的域是`eplists.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数 
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| XUID| 字符串| 用户的 XUID。| 
| listname| 字符串| 列表来操作的名称。| 
| 索引| 字符串| 指定要移动的项的当前索引。 零或正整数索引值时，这是指项的当前索引和请求正文中调用的应为空。 但是，如果的索引值为"-1"，必须由 ItemId 或提供程序/ProviderID 调用，请求正文中指定要移动的项。 | 
  
<a id="ID4EHC"></a>

 
## <a name="valid-methods"></a>有效的方法

[POST](uri-usersxuidlistspinslistnameindexpost.md)

&nbsp;&nbsp;将列表中的项移到列表中的不同位置。
 
<a id="ID4ERC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ETC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[统一资源标识符 (URI) 参考](../atoc-xboxlivews-reference-uris.md)

   
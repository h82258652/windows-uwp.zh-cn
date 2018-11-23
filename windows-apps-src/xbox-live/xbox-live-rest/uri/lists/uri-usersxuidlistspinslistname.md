---
title: /users/xuid(xuid)/lists/PINS/{listname}
assetID: b6421b11-fcd1-cfdb-c1fa-6cab3dab89d9
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistname.html
author: KevinAsgari
description: " /users/xuid(xuid)/lists/PINS/{listname}"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3b76637d0c88e4ef2bc8905ff1b4dd894fd819a0
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2018
ms.locfileid: "7579580"
---
# <a name="usersxuidxuidlistspinslistname"></a>/users/xuid(xuid)/lists/PINS/{listname}
访问列表中的项目。 这些 Uri 的域是`eplists.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| xuid| 字符串| Xbox 用户 ID (XUID)。| 
| listtype| 字符串| 列表 （如何使用和其工作原理） 的类型。 始终"固定"对于这些相关的方法。| 
| listname| 字符串| 列表的名称 （给定 listtype 执行哪些列表）。 始终为"XBLPins"的 Pin 中的项目。| 
  
<a id="ID4EGC"></a>

 
## <a name="valid-methods"></a>有效的方法

[DELETE](uri-usersxuidlistspinslistnamedelete.md)

&nbsp;&nbsp;从列表中删除项目。

[GET](uri-usersxuidlistspinslistnameget.md)

&nbsp;&nbsp;返回列表的内容。

[POST](uri-usersxuidlistspinslistnamepost.md)

&nbsp;&nbsp;项目插入到列表中基于查询字符串参数**insertIndex**的索引。

[PUT](uri-usersxuidlistspinslistnameput.md)

&nbsp;&nbsp;更新根据指定每个项目，请求正文中的索引列表中的项。
 
<a id="ID4EZC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E2C"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[统一资源标识符 (URI) 参考](../atoc-xboxlivews-reference-uris.md)

   
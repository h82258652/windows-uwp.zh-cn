---
title: /users/xuid(xuid)/lists/PINS/{listname}
assetID: b6421b11-fcd1-cfdb-c1fa-6cab3dab89d9
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistname.html
author: KevinAsgari
description: " /users/xuid(xuid)/lists/PINS/{listname}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 052a83f47dc2d5b692c811850e41381c4745815c
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2018
ms.locfileid: "4209410"
---
# <a name="usersxuidxuidlistspinslistname"></a>/users/xuid(xuid)/lists/PINS/{listname}
访问列表中的项目。 这些 Uri 的域是`eplists.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| xuid| 字符串| Xbox 用户 ID (XUID)。| 
| listtype| 字符串| 列表 （它如何使用和其工作原理） 的类型。 对于这些"固定"始终相关的方法。| 
| listname| 字符串| 列表的名称 （给定 listtype 执行哪些列表）。 始终为"XBLPins"的 Pin 中的项。| 
  
<a id="ID4EGC"></a>

 
## <a name="valid-methods"></a>有效的方法

[DELETE](uri-usersxuidlistspinslistnamedelete.md)

&nbsp;&nbsp;从列表中删除项目。

[GET](uri-usersxuidlistspinslistnameget.md)

&nbsp;&nbsp;返回列表的内容。

[POST](uri-usersxuidlistspinslistnamepost.md)

&nbsp;&nbsp;插入位于基于查询字符串参数**insertIndex**索引列表项。

[PUT](uri-usersxuidlistspinslistnameput.md)

&nbsp;&nbsp;更新根据指定每个项目，请求正文中的索引列表中的项。
 
<a id="ID4EZC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E2C"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[统一资源标识符 (URI) 参考](../atoc-xboxlivews-reference-uris.md)

   
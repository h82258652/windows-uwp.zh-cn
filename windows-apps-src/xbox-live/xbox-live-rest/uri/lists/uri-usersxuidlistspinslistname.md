---
title: /users/xuid(xuid)/lists/PINS/{listname}
assetID: b6421b11-fcd1-cfdb-c1fa-6cab3dab89d9
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistname.html
description: " /users/xuid(xuid)/lists/PINS/{listname}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0f9610b400e9530f86e264cea30bfdfdd1b09c8d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627992"
---
# <a name="usersxuidxuidlistspinslistname"></a>/users/xuid(xuid)/lists/PINS/{listname}
访问列表中的项。 这些 Uri 的域是`eplists.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| xuid| 字符串| Xbox 用户 ID (XUID)。| 
| listtype| 字符串| 列表 （如何使用和处理方式） 的类型。 始终"固定"这些相关方法。| 
| listname| 字符串| 列表的名称 （若要对给定 listtype 的列表）。 始终为"XBLPins"Pin 中的项。| 
  
<a id="ID4EGC"></a>

 
## <a name="valid-methods"></a>有效的方法

[DELETE](uri-usersxuidlistspinslistnamedelete.md)

&nbsp;&nbsp;从列表中移除项。

[GET](uri-usersxuidlistspinslistnameget.md)

&nbsp;&nbsp;返回列表的内容。

[POST](uri-usersxuidlistspinslistnamepost.md)

&nbsp;&nbsp;将项插入到列表根据查询字符串参数的索引处**insertIndex**。

[PUT](uri-usersxuidlistspinslistnameput.md)

&nbsp;&nbsp;更新列表根据指定的请求正文中的每个项的索引中的项。
 
<a id="ID4EZC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E2C"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[通用资源标识符 (URI) 引用](../atoc-xboxlivews-reference-uris.md)

   
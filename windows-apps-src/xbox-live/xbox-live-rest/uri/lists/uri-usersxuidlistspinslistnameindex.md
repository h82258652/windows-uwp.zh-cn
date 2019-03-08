---
title: /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
assetID: edcb19bd-87a5-732b-0c45-6f7355fc2dd1
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameindex.html
description: " /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b56b563c72c206340aa2c1ce9f73aa8dfe50809d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641222"
---
# <a name="usersxuidxuidlistspinslistnameindexindexinsertindexinsertindex"></a>/users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
将移动列表中的项。 这些 Uri 的域是`eplists.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数 
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| XUID| 字符串| 用户的 XUID。| 
| listname| 字符串| 要操作的列表的名称。| 
| index| 字符串| 指定要移动的项的当前索引。 如果索引值为零个或一个正整数，这是指当前项的索引，并且在调用的请求正文应为空。 但是，如果索引值为"-1"，必须由 ItemId 或提供程序/ProviderID 调用的请求正文中指定要移动的项。 | 
  
<a id="ID4EHC"></a>

 
## <a name="valid-methods"></a>有效的方法

[POST](uri-usersxuidlistspinslistnameindexpost.md)

&nbsp;&nbsp;将在列表中的项移至列表中的其他位置。
 
<a id="ID4ERC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ETC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[通用资源标识符 (URI) 引用](../atoc-xboxlivews-reference-uris.md)

   
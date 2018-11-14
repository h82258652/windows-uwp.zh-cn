---
title: 列表 URI
assetID: 84dcbd11-86a0-8a1e-7db9-bcecf9b7f853
permalink: en-us/docs/xboxlive/rest/atoc-reference-lists.html
author: KevinAsgari
description: " 列表 URI"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 63c84b68f990392d17342e333b18e5f5d38d2266
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6257832"
---
# <a name="lists-uris"></a>列表 URI
 
本部分提供有关统一资源标识符 (URI) 地址和关联的超文本传输协议 (HTTP) 方法的详细信息，从 Xbox Live 服务的*Pin*。
 
仅游戏和 Xbox 360 上，Windows Phone 设备、 SmartGlass，或在 Xbox.com 上运行的应用程序可以使用此服务。
 
这些 Uri 的域是 eplists.xboxlive.com。
 
<a id="ID4EPB"></a>

 
## <a name="in-this-section"></a>本部分内容

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

&nbsp;&nbsp;访问列表中的项目。

[/users/xuid(xuid)/lists/PINS/{listname}/ContainsItems](uri-usersxuidlistspinslistnamecontainsitems.md)

&nbsp;&nbsp;确定是否项目 （由 itemId 指定） 的一组包含在列表中而不检索整个列表。

[/users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}](uri-usersxuidlistspinslistnameindex.md)

&nbsp;&nbsp;将移动列表中的一项。

[/users/xuid(xuid)/lists/PINS/{listname}/RemoveItems](uri-usersxuidlistspinslistnameremoveitems.md)

&nbsp;&nbsp;从列表中删除项目。
 
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[统一资源标识符 (URI) 参考](../atoc-xboxlivews-reference-uris.md)

   
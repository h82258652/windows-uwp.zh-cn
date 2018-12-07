---
title: /json/users/xuid({xuid})/scids/{scid}/data/{path}
assetID: c2745955-5e52-ea2b-7389-cb85202e01c3
permalink: en-us/docs/xboxlive/rest/uri-jsonusersxuidscidssciddatapath.html
description: " /json/users/xuid({xuid})/scids/{scid}/data/{path}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 275842420b73abc6c2fd8a8fafa265777e2fa00f
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2018
ms.locfileid: "8756234"
---
# <a name="jsonusersxuidxuidscidssciddatapath"></a>/json/users/xuid({xuid})/scids/{scid}/data/{path}
列出了在指定的路径的文件信息。 这些 Uri 的域是`titlestorage.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 描述| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 的玩家谁发出请求。| 
| scid| guid| 若要查找的服务配置 ID。| 
| path| 字符串| 要返回的数据项路径。 获取返回所有匹配的目录和子目录。 有效字符包括大写字母 (A-Z)、 小写字母 (a-z)、 数字 (0-9)、 下划线 (_) 和正斜杠 （/）。 可能为空。 256 的最大长度。| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET](uri-jsonusersxuidscidssciddatapath-get.md)

&nbsp;&nbsp;列出了在指定的路径的文件信息。
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[标题存储 URI](atoc-reference-storagev2.md)

   
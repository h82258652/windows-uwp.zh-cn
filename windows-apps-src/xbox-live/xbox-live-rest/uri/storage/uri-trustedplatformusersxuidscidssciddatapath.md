---
title: /trustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}
assetID: a60a231c-359a-ee6a-6d18-f9e8c6afd0fc
permalink: en-us/docs/xboxlive/rest/uri-trustedplatformusersxuidscidssciddatapath.html
description: " /trustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ff0d2589eb970ef05f8746bb407857de7ebb3256
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589722"
---
# <a name="trustedplatformusersxuidxuidscidssciddatapath"></a>/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}
列出指定路径处的文件信息。 这些 Uri 的域是`titlestorage.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 播放机的用户发出请求。| 
| scid| GUID| 要查找服务配置的 ID。| 
| path| 字符串| 若要返回的数据项目的路径。 返回所有匹配的目录和子目录。 有效字符包括大写字母 (A-Z)、 小写字母 (a-z)、 数字 (0-9)、 下划线 (_) 和正斜杠 （/）。 可能为空。 最大长度为 256。| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET](uri-trustedplatformusersxuidscidssciddatapath-get.md)

&nbsp;&nbsp;列出指定路径处的文件信息。
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[标题存储 Uri](atoc-reference-storagev2.md)

   
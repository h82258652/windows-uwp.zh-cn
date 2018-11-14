---
title: /json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json
assetID: d004d715-d73c-693e-2a0b-ce64f482642b
permalink: en-us/docs/xboxlive/rest/uri-jsonusersxuidscidssciddatapathandfilenametype.html
author: KevinAsgari
description: " /json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fb630c8537fa4190585b6f95274a6cd4760a2987
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6182773"
---
# <a name="jsonusersxuidxuidscidssciddatapathandfilenamejson"></a>/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json
下载、 上传，或删除某个文件。 这些 Uri 的域是`titlestorage.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 的玩家用户发出请求。| 
| scid| guid| 若要查找的服务配置 ID。| 
| pathAndFileName| 字符串| 若要访问该项目的路径和文件名。 有效的字符 （至当前阶段并包括最终正斜杠） 的路径部分包括 (A-Z) 的大写字母、 小写字母 (a-z)、 下划线 (_) 的数字 (0-9)，并且反斜杠 （/）。路径部分可能为空。有效的字符 （所有最终正斜杠后） 的文件名称部分包含大写字母 (A-Z)、 小写字母 (a-z)、 数字 (0-9)、 下划线 (_)，句点 （.） 和连字符 （-）。 文件名称不能为空，以句号结尾或包含两个连续句点。| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>有效的方法

[DELETE](uri-jsonusersxuidscidssciddatapathandfilenametype-delete.md)

&nbsp;&nbsp;删除文件。 

[GET](uri-jsonusersxuidscidssciddatapathandfilenametype-get.md)

&nbsp;&nbsp;下载文件。

[PUT](uri-jsonusersxuidscidssciddatapathandfilenametype-put.md)

&nbsp;&nbsp;将文件上传。 多块上载不受支持的类型 json 数据。 
 
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[标题存储 URI](atoc-reference-storagev2.md)

   
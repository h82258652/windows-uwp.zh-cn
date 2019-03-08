---
title: /trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}
assetID: be789e70-517d-383e-ea35-b0c39e846081
permalink: en-us/docs/xboxlive/rest/uri-trustedplatformusersxuidscidssciddatapathandfilenametype.html
description: " /trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 035f30279321b7b386f59e014fa6d185e6b71b57
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603712"
---
# <a name="trustedplatformusersxuidxuidscidssciddatapathandfilenametype"></a>/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}
下载、 上载，或删除的文件。 这些 Uri 的域是`titlestorage.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 播放机的用户发出请求。| 
| scid| GUID| 要查找服务配置的 ID。| 
| pathAndFileName| 字符串| 要访问的项的路径和文件名称。 有效的字符 （直至并包括最后的正斜杠） 的路径部分包括大写字母 (A-Z)、 小写字母 (a-z)、 数字 (0-9)、 下划线 (_)，和正斜杠 （/）。路径部分可能为空。有效的字符 （最后一个正斜杠之后的所有内容） 的文件部分包括大写字母 (A-Z)、 小写字母 (a-z)、 数字 (0-9)、 下划线 (_)、 句点 （.） 和连字符 （-）。 文件名称不为空、 以句点结尾或包含两个连续句点。| 
| type| 字符串| 数据的格式。 可能的值为二进制或 json。| 
  
<a id="ID4EOC"></a>

 
## <a name="valid-methods"></a>有效的方法

[DELETE](uri-trustedplatformusersxuidscidssciddatapathandfilenametype-delete.md)

&nbsp;&nbsp;删除的文件。 

[GET](uri-trustedplatformusersxuidscidssciddatapathandfilenametype-get.md)

&nbsp;&nbsp;将文件下载。

[PUT](uri-trustedplatformusersxuidscidssciddatapathandfilenametype-put.md)

&nbsp;&nbsp;将文件上载。 可以在其中的数据和元数据发送一条消息，或在其中的数据和元数据发送一系列较小的块中多块上传完整上传上载数据。 小于四个兆字节为单位的文件可以作为单个消息发送。 数据类型 json 的情况下，不支持多块上传。 
 
<a id="ID4E5C"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EAD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[标题存储 Uri](atoc-reference-storagev2.md)

   
---
title: /untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}
assetID: f7de98c3-e6d1-2c40-00f0-d45e418af8bf
permalink: en-us/docs/xboxlive/rest/uri-untrustedplatformusersxuidscidssciddatapathandfilenametype.html
author: KevinAsgari
description: " /untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d5c799fa54f6458881046eb74411f8338dde65ad
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5946896"
---
# <a name="untrustedplatformusersxuidxuidscidssciddatapathandfilenametype"></a>/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}
下载、 上传，或删除某个文件。 这些 Uri 的域是`titlestorage.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 的玩家发出请求者。| 
| scid| guid| 若要查找的服务配置 ID。| 
| pathAndFileName| 字符串| 要访问的项的路径和文件名。 有效的字符 （至当前阶段并包括最终正斜杠） 的路径部分包括 (A-Z) 的大写字母、 小写字母 (a-z)、 数字 (0-9)、 下划线 (_)，并且正斜杠 （/）。路径部分可能为空。有效的字符 （最终正斜杠后的所有内容） 的文件名称部分包括 (A-Z) 的大写字母、 小写字母 (a-z)、 数字 (0-9) 下划线 (_)，句点 （.） 和连字符 （-）。 文件名称不能为空，以句号结尾或包含两个连续句点。| 
| type| 字符串| 数据的格式。 可能的值为二进制文件或 json。| 
  
<a id="ID4EOC"></a>

 
## <a name="valid-methods"></a>有效的方法

[DELETE](uri-untrustedplatformusersxuidscidssciddatapathandfilenametype-delete.md)

&nbsp;&nbsp;删除文件。 

[GET](uri-untrustedplatformusersxuidscidssciddatapathandfilenametype-get.md)

&nbsp;&nbsp;下载文件。

[PUT](uri-untrustedplatformusersxuidscidssciddatapathandfilenametype-put.md)

&nbsp;&nbsp;将文件上传。 可在其中的数据和元数据发送一条消息，或在其中的数据和元数据发送一系列的较小的块多块上载的完整上载上传数据。 可以为一条消息发送小于 4 兆字节的文件。 数据类型 json 不支持多块上载。 
 
<a id="ID4E5C"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EAD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[标题存储 URI](atoc-reference-storagev2.md)

   
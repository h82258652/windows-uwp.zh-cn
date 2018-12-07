---
title: /json/users/batch/scids/{scid}/data/{pathAndFileName},json
assetID: 06ae159f-7739-1330-df15-871d260e6ba1
permalink: en-us/docs/xboxlive/rest/uri-jsonusersbatchscidssciddatapathandfilenametype.html
description: " /json/users/batch/scids/{scid}/data/{pathAndFileName},json"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2b28b0faad1c321137344455bc7cd471f7cb6eba
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8786687"
---
# <a name="jsonusersbatchscidssciddatapathandfilenamejson"></a>/json/users/batch/scids/{scid}/data/{pathAndFileName},json
将多个文件下载从多个用户具有相同的文件名。 这些 Uri 的域是`titlestorage.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 描述| 
| --- | --- | --- | 
| scid| guid| 若要查找的服务配置 ID。| 
| pathAndFileName| 字符串| 要访问的项的路径和文件名。 有效的字符 （至当前阶段并包括最终正斜杠） 的路径部分包括 (A-Z) 的大写字母、 小写字母 (a-z)、 数字 (0-9) 下划线 (_)，并且反斜杠 （/）。路径部分可能为空。有效的字符的文件名称部分 （最终正斜杠后面的所有内容） 包含大写字母 (A-Z)、 小写字母 (a-z)、 数字 (0-9)、 下划线 (_)，句点 （.） 和连字符 （-）。 文件名称不能为空，以句号结尾或包含两个连续句点。| 
  
<a id="ID4E3B"></a>

 
## <a name="valid-methods"></a>有效的方法

[POST](uri-jsonusersbatchscidssciddatapathandfilenametype-post.md)

&nbsp;&nbsp;将多个文件下载从多个用户具有相同的文件名。 请求 URI 由确定要下载的文件。 请求正文中的包含的用户的 Xuid 列表以下载的文件。 响应正文将很多部分的 MIME 消息，与每个表示的特定用户的组其自己的标头文件的一部分。 很可能是成功和失败的组合的响应的部分。
 
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[标题存储 URI](atoc-reference-storagev2.md)

   
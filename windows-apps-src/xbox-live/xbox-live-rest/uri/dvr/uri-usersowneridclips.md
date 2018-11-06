---
title: /users/{ownerId}/clips
assetID: cc200b89-dc63-9ab5-b037-7a910f46dae9
permalink: en-us/docs/xboxlive/rest/uri-usersowneridclips.html
author: KevinAsgari
description: " /users/{ownerId}/clips"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 755d30796c7e948a2fe97e84986a74a461795f83
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2018
ms.locfileid: "6023596"
---
# <a name="usersowneridclips"></a>/users/{ownerId}/clips
访问用户的剪辑的列表。 这些 Uri 的域是`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，根据问题的 URI 的函数。
 
  * [URI 参数](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| ownerId| 字符串| 用户的用户身份的所访问的资源。 支持的格式:"me"或"xuid(123456789)"。 最大长度： 16。| 
  
<a id="ID4EVB"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/users/{ownerId}/clips)](uri-usersowneridclipsget.md)

&nbsp;&nbsp;检索用户的剪辑的列表。
 
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[游戏 DVR URI](atoc-reference-dvr.md)

   
---
title: /users/{ownerId}/clips
assetID: cc200b89-dc63-9ab5-b037-7a910f46dae9
permalink: en-us/docs/xboxlive/rest/uri-usersowneridclips.html
description: " /users/{ownerId}/clips"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cd711777bcdf0b073dd0821222049b03aa35a23c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599512"
---
# <a name="usersowneridclips"></a>/users/{ownerId}/clips
访问用户的剪辑列表。 这些 Uri 的域是`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，取决于 URI 相关的函数。
 
  * [URI 参数](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| ownerId| 字符串| 其资源的访问的用户的用户标识。 支持的格式:"me"或"xuid(123456789)"。 最大长度：16.| 
  
<a id="ID4EVB"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/users/{ownerId}/clips)](uri-usersowneridclipsget.md)

&nbsp;&nbsp;检索用户的剪辑列表。
 
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[游戏 DVR Uri](atoc-reference-dvr.md)

   
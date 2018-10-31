---
title: /users/xuid({xuid})/history/titles
assetID: 2f8eb79a-42c2-0267-cbf2-8682bb28f270
permalink: en-us/docs/xboxlive/rest/uri-titlehistoryusersxuidhistorytitlesv2.html
author: KevinAsgari
description: " /users/xuid({xuid})/history/titles"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b404b6b48139236397e765f885caef953acd87ff
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5814179"
---
# <a name="usersxuidxuidhistorytitles"></a>/users/xuid({xuid})/history/titles
 
此通用资源标识符 (URI) 提供了访问用户的成就相关的游戏历史记录。
 
这些 Uri 的域是`achievements.xboxlive.com`。
 
<a id="ID4E1"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 所访问其游戏历史记录的用户。| 
  
<a id="ID4EAC"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET](uri-titlehistoryusersxuidhistorytitlesgetv2.md)

&nbsp;&nbsp;获取一份标题为其用户已解锁或对其成就的进度。 此 API 不会返回用户的游戏播放或启动完整历史记录。
 
<a id="ID4EKC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EMC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[游戏成就历史记录 URI](atoc-reference-titlehistoryv2.md)

   
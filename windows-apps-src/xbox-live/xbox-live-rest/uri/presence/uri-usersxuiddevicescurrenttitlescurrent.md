---
title: /users/xuid({xuid})/devices/current/titles/current
assetID: f149c68b-9874-e348-4e1d-6acf5d305c49
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrent.html
author: KevinAsgari
description: " /users/xuid({xuid})/devices/current/titles/current"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1fe5e366c48dc5cc7586604584adb6af00207b85
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "4154537"
---
# <a name="usersxuidxuiddevicescurrenttitlescurrent"></a>/users/xuid({xuid})/devices/current/titles/current
访问游戏或游戏的用户的状态。 这些 Uri 的域是`userpresence.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 目标用户。| 
  
<a id="ID4EUB"></a>

 
## <a name="valid-methods"></a>有效的方法

[DELETE (/users/xuid({xuid})/devices/current/titles/current)](uri-usersxuiddevicescurrenttitlescurrentdelete.md)

&nbsp;&nbsp;删除关闭标题，而不是等待[presencerecord，他的](../../json/json-presencerecord.md)过期的状态。

[POST (/users/xuid({xuid})/devices/current/titles/current)](uri-usersxuiddevicescurrenttitlescurrentpost.md)

&nbsp;&nbsp;使用用户的状态更新游戏。
 
<a id="ID4EBC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EDC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[状态 URI](atoc-reference-presence.md)

   
---
title: /users/xuid({xuid})/devices/current/titles/current
assetID: f149c68b-9874-e348-4e1d-6acf5d305c49
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrent.html
description: " /users/xuid({xuid})/devices/current/titles/current"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0b5c17f1791d69ca8a4c902b6c37736c4da28a31
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2018
ms.locfileid: "8337643"
---
# <a name="usersxuidxuiddevicescurrenttitlescurrent"></a>/users/xuid({xuid})/devices/current/titles/current
访问游戏或游戏的用户的状态。 这些 Uri 的域是`userpresence.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 描述| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 目标用户。| 
  
<a id="ID4EUB"></a>

 
## <a name="valid-methods"></a>有效的方法

[DELETE (/users/xuid({xuid})/devices/current/titles/current)](uri-usersxuiddevicescurrenttitlescurrentdelete.md)

&nbsp;&nbsp;删除结束游戏，而不是等待[presencerecord，他的](../../json/json-presencerecord.md)过期的状态。

[POST (/users/xuid({xuid})/devices/current/titles/current)](uri-usersxuiddevicescurrenttitlescurrentpost.md)

&nbsp;&nbsp;更新游戏与用户的状态。
 
<a id="ID4EBC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EDC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[状态 URI](atoc-reference-presence.md)

   
---
title: /users/xuid({xuid})/groups/{moniker}/broadcasting
assetID: cf8319f6-46a2-b263-ea4c-f1ce403b571b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerbroadcasting.html
description: " /users/xuid({xuid})/groups/{moniker}/broadcasting"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 98eaa60204e3c98eb1b09a13372f7b0c084a6608
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651462"
---
# <a name="usersxuidxuidgroupsmonikerbroadcasting"></a>/users/xuid({xuid})/groups/{moniker}/broadcasting
访问由组名字对象指定的广播用户存在记录与在 URI 中将显示 XUID。 这些 Uri 的域是`userpresence.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| xuid| 字符串| Xbox 用户 ID (XUID) 与组中 XUIDs 相关的用户。| 
| 名字对象| 字符串| 定义用户组的字符串。 目前唯一接受的名字对象带有大写字母 P 是"People"。| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/users/xuid({xuid})/groups/{moniker}/broadcasting )](uri-usersxuidgroupsmonikerbroadcastingget.md)

&nbsp;&nbsp;检索指定组的名字对象的 URI 中将显示 XUID 与相关的广播用户存在记录。
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[存在 Uri](atoc-reference-presence.md)

   
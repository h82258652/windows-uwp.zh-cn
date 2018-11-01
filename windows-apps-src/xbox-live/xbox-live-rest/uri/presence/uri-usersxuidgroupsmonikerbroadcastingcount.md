---
title: /users/xuid({xuid})/groups/{moniker}/broadcasting/count
assetID: 535c8d46-7001-c31e-3e9d-82ad275095ae
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerbroadcastingcount.html
author: KevinAsgari
description: " /users/xuid({xuid})/groups/{moniker}/broadcasting/count"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dbfe207483f5814d2cd32ffcd5ac651c0ab5aa52
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5875909"
---
# <a name="usersxuidxuidgroupsmonikerbroadcastingcount"></a>/users/xuid({xuid})/groups/{moniker}/broadcasting/count
访问的组名字对象由指定的广播用户计数与在 URI 中出现的 XUID。 这些 Uri 的域是`userpresence.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| xuid| 字符串| Xbox 用户 ID (XUID) 相关的组中的 Xuid 的用户。| 
| 名字对象| 字符串| 定义的用户组的字符串。 目前仅接受名字对象以大写 P 是"People"。| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/users/xuid({xuid})/groups/{moniker}/broadcasting/count )](uri-usersxuidgroupsmonikerbroadcastingcountget.md)

&nbsp;&nbsp;检索与在 URI 中出现的 XUID 相关的组名字对象由指定的广播用户的计数。
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[状态 URI](atoc-reference-presence.md)

   
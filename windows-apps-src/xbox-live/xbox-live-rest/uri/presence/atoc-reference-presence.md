---
title: 状态 URI
assetID: 4ba44d9c-8615-cacc-2eee-7ff5e7c74383
permalink: en-us/docs/xboxlive/rest/atoc-reference-presence.html
author: KevinAsgari
description: " 状态 URI"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f4c2a34d47f894e2ac9aeaf6228c8ebd41348306
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2018
ms.locfileid: "4124483"
---
# <a name="presence-uris"></a>状态 URI
 
本部分提供了从 Xbox Live 服务的*状态*的详细信息的统一资源标识符 (URI) 地址和关联的超文本传输协议 (HTTP) 方法。
 
仅在 Xbox 360 上、 在 Windows Phone 设备上，或在 Windows 上运行的游戏可以使用此服务。
 
这些 Uri 的域是 userpresence.xboxlive.com。
 
你可以通过使用实时活动 (RTA) 服务订阅用户的状态更改。
 
<a id="ID4ERB"></a>

 
## <a name="in-this-section"></a>本部分内容

[/users/batch](uri-usersbatch.md)

&nbsp;&nbsp;一批用户的访问权限状态。

[/users/me](uri-usersme.md)

&nbsp;&nbsp;访问当前用户的状态。

[/users/me/groups/{moniker}](uri-usersmegroupsmoniker.md)

&nbsp;&nbsp;访问 presencerecord，他的有关我的用户组。

[/users/xuid({xuid})](uri-usersxuid.md)

&nbsp;&nbsp;访问其他用户或客户端存在。

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

&nbsp;&nbsp;访问游戏或游戏的用户的状态。

[/users/xuid({xuid})/groups/{moniker}](uri-usersxuidgroupsmoniker.md)

&nbsp;&nbsp;访问 presencerecord，他的一组。

[/users/xuid({xuid})/groups/{moniker}/broadcasting](uri-usersxuidgroupsmonikerbroadcasting.md)

&nbsp;&nbsp;访问组名字对象由指定的广播用户状态记录与显示在 URI 中的 XUID。

[/users/xuid({xuid})/groups/{moniker}/broadcasting/count](uri-usersxuidgroupsmonikerbroadcastingcount.md)

&nbsp;&nbsp;访问的组名字对象由指定的广播用户计数与显示在 URI 中的 XUID。
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[统一资源标识符 (URI) 参考](../atoc-xboxlivews-reference-uris.md)

   
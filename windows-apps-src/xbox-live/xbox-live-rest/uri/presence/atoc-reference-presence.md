---
title: 状态 URI
assetID: 4ba44d9c-8615-cacc-2eee-7ff5e7c74383
permalink: en-us/docs/xboxlive/rest/atoc-reference-presence.html
description: " 状态 URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a46ecd48c2b0bf523ab234a5f20cf9ed6669e75
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8928694"
---
# <a name="presence-uris"></a>状态 URI
 
本部分提供有关统一资源标识符 (URI) 地址和关联的超文本传输协议 (HTTP) 方法的详细信息从 Xbox Live 服务的*状态*。
 
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

&nbsp;&nbsp;访问组名字对象由指定的广播用户状态记录与在 URI 中出现的 XUID。

[/users/xuid({xuid})/groups/{moniker}/broadcasting/count](uri-usersxuidgroupsmonikerbroadcastingcount.md)

&nbsp;&nbsp;访问与在 URI 中出现的 XUID 相关的组名字对象由指定的广播用户计数。
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[统一资源标识符 (URI) 参考](../atoc-xboxlivews-reference-uris.md)

   
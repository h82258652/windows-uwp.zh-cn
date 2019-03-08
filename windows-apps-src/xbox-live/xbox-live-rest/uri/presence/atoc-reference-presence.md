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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632602"
---
# <a name="presence-uris"></a>状态 URI
 
本部分提供有关通用资源标识符 (URI) 地址和关联的超文本传输协议 (HTTP) 方法的详细信息从 Xbox Live 服务中针对*存在*。
 
仅运行在 Xbox 360 上、 在 Windows Phone 设备上，或在 Windows 上的游戏可以使用此服务。
 
这些 Uri 的域是 userpresence.xboxlive.com。
 
您可以通过使用实时活动 (RTA) 服务订阅用户的状态显示更改。
 
<a id="ID4ERB"></a>

 
## <a name="in-this-section"></a>本部分内容

[/users/batch](uri-usersbatch.md)

&nbsp;&nbsp;访问状态显示为一批用户的。

[/users/me](uri-usersme.md)

&nbsp;&nbsp;访问当前用户存在。

[/users/me/groups/{moniker}](uri-usersmegroupsmoniker.md)

&nbsp;&nbsp;访问我的小组 PresenceRecord。

[/users/xuid({xuid})](uri-usersxuid.md)

&nbsp;&nbsp;存在另一个用户或客户端的访问。

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

&nbsp;&nbsp;访问一个标题或标题的用户存在。

[/users/xuid({xuid})/groups/{moniker}](uri-usersxuidgroupsmoniker.md)

&nbsp;&nbsp;访问组 PresenceRecord。

[/users/xuid({xuid})/groups/{moniker}/broadcasting](uri-usersxuidgroupsmonikerbroadcasting.md)

&nbsp;&nbsp;访问由组名字对象指定的广播用户存在记录与在 URI 中将显示 XUID。

[/users/xuid({xuid})/groups/{moniker}/broadcasting/count](uri-usersxuidgroupsmonikerbroadcastingcount.md)

&nbsp;&nbsp;在 URI 中将显示 XUID 广播用户指定的组名字对象的计数与相关访问。
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[通用资源标识符 (URI) 引用](../atoc-xboxlivews-reference-uris.md)

   
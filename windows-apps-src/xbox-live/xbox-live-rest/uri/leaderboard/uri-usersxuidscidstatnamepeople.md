---
title: /users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}
assetID: 0983dad0-59b7-45b7-505d-603e341fe0cc
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidstatnamepeople.html
author: KevinAsgari
description: " /users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 161c7e96faf3ec217aeb188ccb3b5b1e354d217e
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2018
ms.locfileid: "4172747"
---
# <a name="usersxuidxuidscidsscidstatsstatnamepeopleallfavorite"></a>/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}
访问 （排名） 在社交排行榜。
这些 Uri 的域是`leaderboards.xboxlive.com`。

  * [URI 参数](#ID4EV)

<a id="ID4EV"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- |
| xuid| 字符串| 用户的标识符。|
| scid| 字符串| 服务配置，其中包含正在访问的资源的标识符。|
| statname| 字符串| 正在访问的用户统计数据资源的唯一标识符。|
| all\ | 最喜爱| 枚举| 是否要排名统计数据值 （分数） 为当前用户的所有已知的联系人或仅由该用户指定为常用联系人的联系人。|

<a id="ID4EOC"></a>


## <a name="valid-methods"></a>有效的方法

[GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all\|favorite})](uri-usersxuidscidstatnamepeopleget.md)

&nbsp;&nbsp;返回在社交排行榜的统计数据值 （分数） 为当前用户的任何一种所有已知的联系人或仅由该用户指定为常用联系人的联系人的排名。

<a id="ID4EYC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4E1C"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[排行榜 URI](atoc-reference-leaderboard.md)

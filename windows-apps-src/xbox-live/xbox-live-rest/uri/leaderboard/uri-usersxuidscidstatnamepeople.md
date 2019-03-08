---
title: /users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}
assetID: 0983dad0-59b7-45b7-505d-603e341fe0cc
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidstatnamepeople.html
description: " /users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 85a6470a64ceef3b154384d1ca859fb28733aad3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632572"
---
# <a name="usersxuidxuidscidsscidstatsstatnamepeopleallfavorite"></a>/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}
访问社交 （排名） 排行榜。
这些 Uri 的域是`leaderboards.xboxlive.com`。

  * [URI 参数](#ID4EV)

<a id="ID4EV"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| xuid| 字符串| 用户的标识符。|
| scid| 字符串| 服务配置，其中包含要访问的资源的标识符。|
| statname| 字符串| 要访问的用户统计信息资源的唯一标识符。|
| 所有\|收藏| 枚举| 排名统计信息是否为当前用户的所有已知的联系人或仅由该用户指定为最喜欢的人的联系人的值 （分数）。|

<a id="ID4EOC"></a>


## <a name="valid-methods"></a>有效的方法

[GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all\|favorite})](uri-usersxuidscidstatnamepeopleget.md)

&nbsp;&nbsp;通过排名统计信息值 （分数） 为当前用户是所有已知的联系人或仅由该用户指定为最喜欢的人的联系人返回社交排行榜。

<a id="ID4EYC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4E1C"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[排行榜 Uri](atoc-reference-leaderboard.md)

---
title: /users/xuid({xuid})/achievements/{scid}/{achievementid}
assetID: 656a6d63-1a11-b0a5-63d2-2b010abd62e7
permalink: en-us/docs/xboxlive/rest/uri-usersxuidachievementsscidachievementid.html
description: " /users/xuid({xuid})/achievements/{scid}/{achievementid}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 00c577f60b67f15f75c47b5e737ca12819695110
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599442"
---
# <a name="usersxuidxuidachievementsscidachievementid"></a>/users/xuid({xuid})/achievements/{scid}/{achievementid}
返回关于该成就，包括其配置元数据和特定于用户的数据的详细信息。 

> [!NOTE] 
> 仅受平台支持。 

 
这些 Uri 的域是`achievements.xboxlive.com`。
 
  * [URI 参数](#ID4E2)
 
<a id="ID4E2"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 其资源的访问的用户。 必须与匹配身份验证的用户的 XUID。| 
| scid| GUID| 正在访问其成就的服务配置的唯一标识符。| 
| achievementid| 32 位无符号的整数| 正在访问该成就的 （在指定 SCID) 唯一标识符。| 
  
<a id="ID4EMC"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})](uri-usersxuidachievementsscidachievementidget.md)

&nbsp;&nbsp;获取该成就的详细信息。
 
<a id="ID4EWC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EYC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[获得的成就 Uri](atoc-reference-achievementsv2.md)

   
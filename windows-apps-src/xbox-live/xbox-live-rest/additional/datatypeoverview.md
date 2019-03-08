---
title: 数据类型概述
assetID: c154a6fa-e7b2-4652-f6fc-f946f74480e9
permalink: en-us/docs/xboxlive/rest/datatypeoverview.html
description: " 数据类型概述"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 62932a921d51a988a5533d7ee08f4968bb67a29d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659652"
---
# <a name="data-type-overview"></a>数据类型概述
 
Xbox Live 服务使用各种与标识和身份验证相关的数据类型。 本主题概述了这些类型。
 
| 在任务栏的搜索框中键入| 描述| 
| --- | --- | 
| 玩家代号| 用户的唯一的用户可读的屏幕名称。| 
| 玩家| 用户的 XUID 和玩家代号、 以及播放机的索引中的会话 （或"座位"），播放机仍参与会话，以及自定义数据的小型 blob 是否包含一个 JSON 对象。| 
| profile| 有关用户访问通过配置文件的 URI 地址和 HTTP 方法，通常用户的用户设置，但还可能包括玩家卡、 玩家代号、 XUID，等的信息。| 
| 设置| 用户设置对象中的特定于标题的设置之一。| 
| UserClaims| 包含用户的 XUID 和玩家代号的简单 JSON 对象。| 
| UserSettings| 包含特定于标题的设置或当前经过身份验证用户的首选项的集合的 JSON 对象。 用户设置可以包含任意数据，可能与中的游戏活动相关。| 
| XUID| 用户的 Xbox 用户 ID，唯一的无符号长整数。 不应是用户可读。| 
 
<a id="ID4E6D"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EBE"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[其他参考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ENE"></a>

 
##### <a name="reference--player-jsonjsonjson-playermd"></a>引用[播放器 (JSON)](../json/json-player.md)

 [UserClaims (JSON)](../json/json-userclaims.md)

 [UserSettings (JSON)](../json/json-usersettings.md)

   
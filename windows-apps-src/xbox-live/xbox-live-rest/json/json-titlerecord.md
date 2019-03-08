---
title: TitleRecord (JSON)
assetID: 8e1bd699-e408-67c8-31da-2d968adfbc21
permalink: en-us/docs/xboxlive/rest/json-titlerecord.html
description: " TitleRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 89baf7e9a737428d492246f1647a561a4a8170cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603982"
---
# <a name="titlerecord-json"></a>TitleRecord (JSON)
有关标题，包括其名称和上次修改时间戳信息。 
<a id="ID4EN"></a>

 
## <a name="titlerecord"></a>TitleRecord
 
TitleRecord 必须包含 DeviceRecord 或 LastSeenRecord，但不是可以同时包含。
 
TitleRecord 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| id| 32 位无符号的整数| 职务的记录的 Id。| 
| name| 字符串| 标题的本地化的名称。| 
| 活动| [ActivityRecord](json-activityrecord.md)| 标题中的用户的活动。 仅返回深度为"全部"。| 
| lastModified| DateTime| UTC 时间戳记录上次更新的时间。| 
| 放置| 字符串| 用户界面中应用的位置。 可能性包括"fill"、"完整"、"对齐"或"后台"。 默认值为"完整"的设备而不会进行应用。| 
| 状态| 字符串| 标题的状态。 可以是"活动"或"非活动"（默认值）。 标题设置基于其自身条件的活动和非活动状态的状态。| 
  
<a id="ID4E6C"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

```json
{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    }
    
```

  
<a id="ID4EID"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EKD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EUD"></a>

 
##### <a name="reference"></a>参考 

[POST (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentpost.md)

   
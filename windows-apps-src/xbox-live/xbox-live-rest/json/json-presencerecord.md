---
title: PresenceRecord (JSON)
assetID: 414e6ef5-f7bd-70d0-7386-7aa1c3a56e21
permalink: en-us/docs/xboxlive/rest/json-presencerecord.html
description: " PresenceRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6d531352c4336e00c93a91e7c945602ab69695f2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622582"
---
# <a name="presencerecord-json"></a>PresenceRecord (JSON)
有关单个用户的联机状态的数据。
<a id="ID4EN"></a>


## <a name="presencerecord"></a>PresenceRecord

PresenceRecord 对象具有以下规范。

| 成员| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| xuid| 字符串| Xbox 用户 ID (XUID) 的目标用户。 为此用户提供的状态显示数据。|
| 设备| 数组[DeviceRecord](json-devicerecord.md)| 用户的设备记录的列表。|
| 状态| 字符串| Xbox LIVE 上的用户的活动。 可能值： <ul><li>联机：用户必须至少一个设备记录。</li><li>消失：用户是登录到 Xbox LIVE 但未在任何标题中处于活动状态。</li><li>脱机：用户已不存在任何设备上。</li></ul> | 
| lastSeen| [LastSeenRecord](json-lastseenrecord.md)| 在用户有无有效 DeviceRecords 时，上次看到的信息才可用。 如果该对象已从缓存中，删除其数据可能不会返回，因为不存在持久性存储。|

<a id="ID4E2C"></a>


## <a name="sample-json-syntax"></a>示例 JSON 语法


```json
{
  xuid:"0123456789",
  state:"online",
  devices:
  [{
    type:"D",
    titles:
    [{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    },
    {
      id:"12341235",
      name:"Contoso Waypoint",
      timestamp:"2012-09-17T07:15:23.4930000",
      placement:"snapped",
      state:"active",
      activity:
      {
        richPresence:"Using radar"
      }
    }]
  },
  {
    type:"W8",
    titles:
    [{
      id:"23452345",
      name:"Contoso Gamehelp",
      state:"active",
      placement:"full",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Nirvana page"
      }
    }]
  }]
}

```


<a id="ID4EED"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EGD"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)


<a id="ID4EQD"></a>


##### <a name="reference"></a>参考

[POST （/用户/批处理）](../uri/presence/uri-usersbatchpost.md)

 [获取 (/ 用户/我)](../uri/presence/uri-usersmeget.md)

 [DELETE (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentdelete.md)

 [GET (/users/xuid({xuid}))](../uri/presence/uri-usersxuidget.md)

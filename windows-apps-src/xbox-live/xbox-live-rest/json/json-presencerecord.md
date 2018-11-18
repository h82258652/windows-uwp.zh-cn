---
title: PresenceRecord (JSON)
assetID: 414e6ef5-f7bd-70d0-7386-7aa1c3a56e21
permalink: en-us/docs/xboxlive/rest/json-presencerecord.html
author: KevinAsgari
description: " PresenceRecord (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: febf5e377c73572e4e231f830d737b4e704d262e
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2018
ms.locfileid: "7160581"
---
# <a name="presencerecord-json"></a>PresenceRecord (JSON)
联机状态相关的单个用户的数据。
<a id="ID4EN"></a>


## <a name="presencerecord"></a>Presencerecord，他

Presencerecord，他的对象具有以下规范。

| 成员| 类型| 说明|
| --- | --- | --- |
| xuid| 字符串| Xbox 用户 ID (XUID) 目标用户。 为此用户提供的状态数据。|
| 设备| [DeviceRecord](json-devicerecord.md)的数组| 记录列表中用户的设备。|
| 状态| 字符串| Xbox LIVE 上的用户的活动。 可能值： <ul><li>联机： 用户有至少一台设备的记录。</li><li>离开： 用户已登录 Xbox LIVE 但不是活动任何作品中。</li><li>脱机： 用户不是任何设备上存在的。</li></ul> | 
| lastSeen| [LastSeenRecord](json-lastseenrecord.md)| 当用户在没有有效 DeviceRecords，最后看到的信息才可用。 如果已从缓存中，删除对象及其数据可能不会返回，因为没有任何持久存储。|

<a id="ID4E2C"></a>


## <a name="sample-json-syntax"></a>JSON 语法示例


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

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)


<a id="ID4EQD"></a>


##### <a name="reference"></a>参考

[POST (/users/batch)](../uri/presence/uri-usersbatchpost.md)

 [GET (/users/me)](../uri/presence/uri-usersmeget.md)

 [DELETE (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentdelete.md)

 [GET (/users/xuid({xuid}))](../uri/presence/uri-usersxuidget.md)

---
title: PresenceRecord (JSON)
assetID: 414e6ef5-f7bd-70d0-7386-7aa1c3a56e21
permalink: en-us/docs/xboxlive/rest/json-presencerecord.html
author: KevinAsgari
description: " PresenceRecord (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c365760f68aa7c87422e747606175ae9a12f0574
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4354439"
---
# <a name="presencerecord-json"></a>PresenceRecord (JSON)
联机状态相关的单个用户的数据。
<a id="ID4EN"></a>


## <a name="presencerecord"></a>PresenceRecord

Presencerecord，他的对象具有以下规范。

| 成员| 类型| 描述|
| --- | --- | --- |
| xuid| 字符串| Xbox 用户 ID (XUID) 目标用户。 为此用户提供的状态数据。|
| 设备| [DeviceRecord](json-devicerecord.md)的数组| 记录列表中用户的设备。|
| 状态| 字符串| Xbox LIVE 上的用户的活动。 可能值： <ul><li>联机： 用户有至少一台设备的记录。</li><li>离开： 用户已登录 Xbox LIVE 但不是活动任何作品中。</li><li>脱机： 用户不是任何设备上存在的。</li></ul> | 
| lastSeen| [LastSeenRecord](json-lastseenrecord.md)| 当用户在没有有效 DeviceRecords，最后看到的信息才可用。 如果已从缓存中，删除对象及其数据可能不会返回，因为没有任何持久存储区。|

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

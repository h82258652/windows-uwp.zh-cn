---
title: PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
assetID: e3e4f164-ac5e-cbd9-8c05-2e1ac00dc55e
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.html
author: KevinAsgari
description: " PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 92cf7ab408b14e74a8f231d6c81e3077a0a40be5
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2018
ms.locfileid: "4264905"
---
# <a name="put-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
创建、 更新或加入会话。

> [!IMPORTANT]
> 此 URI 方法需要 X Xbl 协定版本的标头元素： 104/105 或更高版本上每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4EYB)
  * [HTTP 状态代码](#ID4EFC)
  * [请求正文](#ID4EOC)
  * [响应正文](#ID4E4C)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法创建，加入，或更新会话，具体取决于发送相同的 JSON 请求正文模板的子集。 成功时，它将返回一个**MultiplayerSession**对象，包含从服务器返回的响应。 在它的属性可能不同于传入的**MultiplayerSession**对象中的属性。 此方法可以由**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.WriteSessionAsync**包装。

会话创建和更新操作有表示应用的更改应用程序/json 正文使用 PUT。 操作是幂等，相同的更改的多个应用程序，即有任何其他效果。

JSON 请求正文镜像会话数据结构。 所有字段和子字段都是可选的。

在 PUT 方法的会话创建或加入模式的线格式如下所示。

> [!NOTE]
> 请注意使用此模式。 Upates 盲目，无论该会话的当前状态。



```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
         Content-Type: application/json

```



在 PUT 方法的会话更新模式的线格式如下所示。

```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
         Content-Type: application/json

```



在 PUT 方法更新会话属性的线格式如下所示。 这是等效于对会话 URI 的 PUT 操作作为属性没有任何内容但下方的对象的正文。 区别是，此操作将返回错误代码 404 未找到如果会话不存在。 此操作支持 If-match 标头。

```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001/properties HTTP/1.1
         Content-Type: application/json

         { "system": { }, "custom": { } }

```



<a id="ID4EYB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 描述|
| --- | --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID)。 第 1 部分会话标识符。|
| sessionTemplateName| 字符串| 会话模板的当前实例的名称。 第 2 部分会话标识符。|
| 会话名| GUID| 会话的唯一 ID。 会话标识符的第 3 部分。|

<a id="ID4EFC"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
该服务返回 HTTP 状态代码，因为它适用于 MPSD。  
<a id="ID4EOC"></a>


## <a name="request-body"></a>请求正文

下面是示例请求正文用于创建或加入会话。 请求正文中的以下成员都是可选的。 其他所有可能的成员被禁止在请求中。

| 成员| 类型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- |
| 常量| object| 会话模板，以产生会话常量与合并的只读的设置。 |
| 属性 | object | 合并到的会话属性更改。|
| members.me | object| 常量和大量的属性，例如其顶级对应项。 任何 PUT 方法需要用户是会话的成员，并添加用户，如有必要。 "我"指定为 null，如果是从会话中删除发出请求的成员。 |
| 成员 | object| 表示用户添加到会话中，从零开始的索引键控其他对象。 在请求中的成员数开始时始终具有 0，即使会话已包含成员。 成员将添加到会话在请求中出现的顺序。 成员属性只能由用户属于其设置。 |
| 服务器 | object| 值，该值指示更新和添加内容为会话的关联的服务器参与者的设置。 服务器指定为 null，如果是从会话中删除该服务器条目。 |



```cpp
{
  "properties": {
    "custom": {"KANWE": "MGMSY"},
    "system": {}
  },
  "constants": {
    "custom": {},
    "system": {"visibility": "open"}
  },
  "members": {
    "reserve_0": {
    "constants": {
      "custom": {"type": "leader"},
      "system": {"xuid": "5500461"} }}
   }
}

```


<a id="ID4E4C"></a>


## <a name="response-body"></a>响应正文

示例创建或加入会话的响应正文：


```cpp
{
  "contractVersion": 104,
  "correlationId": "0FE81338-EE96-46E3-A3B5-2DBBD6C41C3B",
  "nextTimer": "2009-06-15T13:45:30.0900000Z",

  "initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
  },

  "hostCandidates": [ "ab90a362", "99582e67" ],

  "constants": {
    "system": {"visibility": "open"},
    "custom": {}
  },

  "properties": {
     "system": { "turn": [] },
     "custom": { "myProperty": "myValue" }
  },

  "members": {
      "1": {
        "properties": {
        "system": { },
        "custom": { }
      },

      "constants": {
        "system": { "xuid": "5500461" },
        "custom": { }
      }

      "gamertag": "stacy",
      "deviceToken": "9f4032ba7",
      "reserved": true,
      "activeTitleId": "8397267",
      "joinTime": "2009-06-15T13:45:30.0900000Z",
      "turn": true,
      "initializationFailure": "latency",
      "initializationEpisode": 1,
      "next": 4
    },
  },

  "membersInfo": {
      "first": 1,
      "next": 4,
      "count": 1,
      "accepted": 0
  },

  "servers": {
      "name": {
        "constants": { },
        "properties": { }
      }
  }
}

```


<a id="ID4EID"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EKD"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)

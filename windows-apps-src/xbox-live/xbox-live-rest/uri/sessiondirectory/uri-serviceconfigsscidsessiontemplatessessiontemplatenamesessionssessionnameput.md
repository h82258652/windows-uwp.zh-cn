---
title: PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
assetID: e3e4f164-ac5e-cbd9-8c05-2e1ac00dc55e
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.html
description: " PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d35b3f89f8b866a5236e8f5ac91eb37d9a82d306
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598552"
---
# <a name="put-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
创建、 更新或加入会话时。

> [!IMPORTANT]
> 此 URI 方法需要 X Xbl 约定版本标头元素：104/105 或更高版本上的每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4EYB)
  * [HTTP 状态代码](#ID4EFC)
  * [请求正文](#ID4EOC)
  * [响应正文](#ID4E4C)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法创建联接，或更新会话中，具体取决于发送何种相同的 JSON 请求正文模板子集。 如果成功，它将返回**MultiplayerSession**对象包含的响应从服务器返回。 在它的属性可能不同于在传入的属性**MultiplayerSession**对象。 此方法的两端可加**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.WriteSessionAsync**。

会话创建和更新操作使用 PUT 与一个应用程序/json 正文，其中，表示要应用的更改。 操作为幂等，这就是相同的更改的多个应用程序具有任何其他影响。

JSON 请求正文，镜像会话的数据结构。 所有字段和子字段都是可选的。

连网格式进行该 PUT 方法的会话创建或加入模式如下所示。

> [!NOTE]
> 请小心使用此模式。 Upates 盲目使用，无论该会话的当前状态。



```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
         Content-Type: application/json

```



PUT 方法的更新模式会话的传输格式如下所示。

```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
         Content-Type: application/json

```



PUT 方法来更新会话属性的传输格式如下所示。 它相当于会话 URI 的 PUT 操作，使用作为属性没有任何内容但下方的对象的正文。 不同之处是，此操作将返回错误代码 404 找不到如果会话不存在。 此操作支持 If-match 标头。

```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001/properties HTTP/1.1
         Content-Type: application/json

         { "system": { }, "custom": { } }

```



<a id="ID4EYB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID)。 会话标识符的第 1 部分中。|
| sessionTemplateName| 字符串| 会话模板的当前实例的名称。 会话标识符的第 2 部分中。|
| sessionName| GUID| 会话的唯一 ID。 会话标识符的第 3 部分。|

<a id="ID4EFC"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
同样适用于 MPSD，服务将返回 HTTP 状态代码。  
<a id="ID4EOC"></a>


## <a name="request-body"></a>请求正文

下面是用于创建或加入会话的示例请求正文。 以下成员的请求正文是可选的。 在请求中禁止使用所有其他可能的成员。

| 成员| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- |
| 常量| 对象| 与要为会话生成常量的会话模板合并的只读设置。 |
| 属性 | 对象 | 要合并到会话属性的更改。|
| members.me | 对象| 常量和大量的属性，如对应的顶级。 任何 PUT 方法要求用户属于会话，并添加用户，如有必要。 如果"me"被指定为 null，则会从会话中删除发出请求的成员。 |
| 成员 | 对象| 其他对象，表示用户添加到会话，并从零开始的索引进行键控。 在请求中的成员数始终 0 开始，即使会话已包含的成员。 成员将添加到请求中的显示的顺序中的会话。 成员属性仅可以设置到其所属的用户。 |
| 服务器 | 对象| 值，该值指示更新以及会话的新增功能的一组关联的服务器的参与者。 如果一台服务器指定为 null，是从会话中删除该服务器条目。 |



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

创建或加入会话的示例响应正文：


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

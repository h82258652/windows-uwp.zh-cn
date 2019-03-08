---
title: POST (/serviceconfigs/{scid}/hoppers/{hoppername})
assetID: 8cbf62aa-d639-e920-1e39-099133af17f8
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamepost.html
description: " POST (/serviceconfigs/{scid}/hoppers/{hoppername})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7682b92ec61c98679904825e360d73318e9fee90
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659832"
---
# <a name="post-serviceconfigsscidhoppershoppername"></a>POST (/serviceconfigs/{scid}/hoppers/{hoppername})

创建指定的匹配票证。

> [!IMPORTANT]
> 此方法旨在用于具有协定 103 或更高版本，并要求 X Xbl 约定版本标头元素：103 或更高版本上的每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4E5)
  * [Authorization](#ID4EJB)
  * [HTTP 状态代码](#ID4E3C)
  * [请求正文](#ID4EFD)
  * [响应正文](#ID4E3G)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法创建具有特定名称 ID (SCID) 级别的服务配置在 hopper 的匹配项票证。 此方法的两端可加**Microsoft.Xbox.Services.Matchmaking.MatchmakingService.CreateMatchTicketAsync**方法。  
<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服务配置 (SCID) 会话标识符。|
| hoppername | 字符串 | Hopper 的名称。 |

<a id="ID4EJB"></a>


## <a name="authorization"></a>授权

| 在任务栏的搜索框中键入| 必需| 描述| 如果缺少的响应|
| --- | --- | --- | --- | --- | --- | --- | --- |
| 权限和设备类型| 是| 当用户的设备类型设置为控制台时，只有具有在其声明中的多玩家特权的用户允许对匹配服务进行调用。 | 403|
| 设备类型| 是| 当用户的设备类型不存在或设置为非控制台，到匹配的标题不能仅限控制台的标题。 | 403|
| 标题 ID/采购/设备类型的概念验证| 是| 要匹配到标题必须允许指定的标题声明、 设备类型组合匹配。 | 403|

<a id="ID4E3C"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
同样适用于 MPSD，服务将返回 HTTP 状态代码。  
<a id="ID4EFD"></a>


## <a name="request-body"></a>请求正文

<a id="ID4ELD"></a>


### <a name="required-members"></a>所需的成员

| 成员| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| serviceConfig| GUID| 会话 SCID。|
| hopperName| 字符串| Hopper 的名称。|
| giveUpDuration| 32 位有符号的整数| 最长等待时间 （整秒数）。|
| preserveSession| 枚举| 一个值，该值指示是否在会话将重复使用作为要匹配到的会话。 可能的值为"始终"和"从不"。 |
| ticketSessionRef| MultiplayerSessionReference| 在其中的播放机或组当前正在播放的会话 MultiplayerSessionReference 对象。 |
| ticketAttributes| 对象的集合| 特性和有关的播放机组用户提供的值。|

<a id="ID4EXF"></a>


### <a name="prohibited-members"></a>禁止的成员

所有其他成员禁止在请求中使用。

<a id="ID4ECG"></a>


### <a name="sample-request"></a>示例请求

由引用会话**ticketSessionRef**可以创建匹配票证，并将该会话必须包含玩家匹配，以及其特定于播放机的属性之前，必须创建对象。 每个玩家必须创建或加入针对 MPSD，将相关的匹配属性添加到会话的会话。 匹配属性放置在名为 matchAttrs 上每个玩家的自定义属性字段。

创建或联接请求提交到**https://sessiondirectory.xboxlive.com/serviceconfigs/{scid}/sessiontemplates/{templatename}/sessions/{sessionname}** 和可能如下所示：


```cpp
{
   "members": {
     "me": {
       "constants": {
         "system": {
           "xuid": 2535285330879750
         }
      },
      "properties": {
         "custom": {
           "matchAttrs": {
             "skill": 5,
             "ageRange": "teenager"
           }
         }
      }
    }
  }
}

```


一旦创建会话后，标题可以调用的匹配服务要为该会话创建票证。


> [!NOTE] 
> 标题可以使用户能够重试此调用，但不是应重试它会自动在数据未能通过。  



```cpp
POST /serviceconfigs/{scid}/hoppers/{hoppername}

{
  "giveUpDuration":10,
  "preserveSession": "never",
  "ticketSessionRef": {
     "scid": "ABBACDDC-0000-0000-0000-000000000001",  
     "templateName": "TestTemplate",
     "name": "5E55104-0000-0000-0000-000000000001"
  },
  "ticketAttributes": {
    "desiredMap": "Test Map",
    "desiredGameType": "Test GameType"
  }
}

```


<a id="ID4E3G"></a>


## <a name="response-body"></a>响应正文

| 成员| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ticketId| GUID| 正在创建的票证 ID。|
| waitTime| 32 位有符号的整数| 平均等待时间 hopper （整秒数）。|


```cpp
{
  "ticketId":"0584338f-a2ff-4eb9-b167-c0e8ddecae72",
  "waitTime":60
}

```


<a id="ID4EHAAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EJAAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）  

[/serviceconfigs/{scid}/hoppers/{hoppername}](uri-serviceconfigsscidhoppershoppername.md)

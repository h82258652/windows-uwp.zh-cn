---
title: POST (/serviceconfigs/{scid}/hoppers/{hoppername})
assetID: 8cbf62aa-d639-e920-1e39-099133af17f8
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamepost.html
author: KevinAsgari
description: " POST (/serviceconfigs/{scid}/hoppers/{hoppername})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 089aba60b80e629a8068bcf39f009ac97fc6ad66
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "4014790"
---
# <a name="post-serviceconfigsscidhoppershoppername"></a>POST (/serviceconfigs/{scid}/hoppers/{hoppername})

创建指定的匹配票证。

> [!IMPORTANT]
> 此方法旨在用于与合同 103 或更高版本，并且需要 X Xbl 协定版本的标头元素： 103 或更高版本上的每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4E5)
  * [授权](#ID4EJB)
  * [HTTP 状态代码](#ID4E3C)
  * [请求正文](#ID4EFD)
  * [响应正文](#ID4E3G)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法创建具有特定名称在服务配置 ID (SCID) 级别的漏斗的匹配票证。 此方法可以包装**Microsoft.Xbox.Services.Matchmaking.MatchmakingService.CreateMatchTicketAsync**方法。  
<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID) 会话。|
| hoppername | 字符串 | 漏斗的名称。 |

<a id="ID4EJB"></a>


## <a name="authorization"></a>授权

| 类型| 必需| 描述| 如果缺少的响应|
| --- | --- | --- | --- | --- | --- | --- | --- |
| 特权和设备类型| 是| 当用户的 deviceType 设置为主机时，仅具有多人游戏中其声明特权的用户允许对匹配服务进行调用。 | 403|
| 设备类型| 是| 当用户的 deviceType 不存在或设置为非控制台，匹配到标题不能仅限控制台的标题。 | 403|
| 主题作品 ID/购买/设备类型的概念证明| 是| 正在匹配到游戏必须允许指定的标题声明，设备类型组合的匹配。 | 403|

<a id="ID4E3C"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
该服务返回 HTTP 状态代码，因为它适用于 MPSD。  
<a id="ID4EFD"></a>


## <a name="request-body"></a>请求正文

<a id="ID4ELD"></a>


### <a name="required-members"></a>所需的成员

| 成员| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| serviceConfig| GUID| 会话的 SCID。|
| hopperName| 字符串| 漏斗的名称。|
| giveUpDuration| 32 位有符号的整数| 最大的等待时间 （不可或缺的秒数）。|
| preserveSession| 枚举| 指示是否会话匹配到会话作为重复使用的值。 可能的值为"始终"，并且"从不"。 |
| ticketSessionRef| MultiplayerSessionReference| 在其中的玩家或组当前播放会话的 MultiplayerSessionReference 对象。 |
| ticketAttributes| 对象的集合| 属性和有关的玩家组用户提供的值。|

<a id="ID4EXF"></a>


### <a name="prohibited-members"></a>禁止的成员

在请求中禁止使用所有其他成员。

<a id="ID4ECG"></a>


### <a name="sample-request"></a>示例请求

可以创建匹配票证，并将该会话必须包含的玩家以进行匹配，以及其特定于玩家的属性之前，必须创建由**ticketSessionRef**对象引用会话。 每个玩家必须创建或加入针对 MPSD，将相关的匹配属性添加到会话的会话。 匹配属性均放置在一个名为每个玩家上 matchAttrs 的自定义属性字段。

创建或加入请求提交到**http://sessiondirectory.xboxlive.com/serviceconfigs/{scid}/sessiontemplates/{templatename}/sessions/{sessionname}** 并可能如下所示：


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


在会话已创建后，游戏可以调用匹配服务将为该会话创建票证。


> [!NOTE] 
> 游戏可以使用户重试此调用中，但应不自动重试它如果数据失败。  



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

| 成员| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 票证 Id| GUID| 正在创建票证的 ID。|
| waitTime| 32 位有符号的整数| 平均等待时间漏斗 （不可或缺的秒数）。|


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

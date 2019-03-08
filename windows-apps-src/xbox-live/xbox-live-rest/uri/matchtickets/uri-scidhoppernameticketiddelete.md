---
title: DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})
assetID: d9ff3f21-aa70-af41-afa1-9a9244fcdb95
permalink: en-us/docs/xboxlive/rest/uri-scidhoppernameticketiddelete.html
description: " DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fdd28cb94b31102d9af98aa95afde45424dadce9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589662"
---
# <a name="delete-serviceconfigsscidhoppershoppernameticketsticketid"></a>DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})

移除匹配票证。

> [!IMPORTANT]
> 此方法旨在用于具有协定 103 或更高版本，并要求 X Xbl 约定版本标头元素：103 或更高版本上的每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4E2)
  * [Authorization](#ID4EGB)
  * [HTTP 状态代码](#ID4EOC)
  * [请求正文](#ID4EXC)
  * [响应正文](#ID4ECD)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法从在服务配置 ID (SCID) 级别命名 hopper 中删除指定的票证 ID。 此方法的两端可加**Microsoft.Xbox.Services.Matchmaking.MatchmakingService.DeleteMatchTicketAsync**。  
<a id="ID4E2"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服务配置 (SCID) 会话标识符。|
| name| 字符串| Hopper 的名称。|
| ticketId| GUID| 票证 id。|

<a id="ID4EGB"></a>


## <a name="authorization"></a>授权

| 在任务栏的搜索框中键入| 必需| 描述| 如果缺少的响应|
| --- | --- | --- | --- | --- | --- | --- | --- |
| XUID (用户 ID)| 是| 发出请求的用户必须是引用该票证的票证会话的成员。| 403|
| 权限和设备类型| 是| 当用户的设备类型设置为控制台时，只有具有在其声明中的多玩家特权的用户允许对匹配服务进行调用。| 403|

<a id="ID4EOC"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

同样适用于 MPSD，服务将返回 HTTP 状态代码。  
<a id="ID4EXC"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送的任何对象。

<a id="ID4ECD"></a>


## <a name="response-body"></a>响应正文

响应正文中不发送任何对象。

<a id="ID4EPD"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4ERD"></a>


##### <a name="parent"></a>Parent 的子磁盘）  

[/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}](uri-scidhoppernameticketid.md)

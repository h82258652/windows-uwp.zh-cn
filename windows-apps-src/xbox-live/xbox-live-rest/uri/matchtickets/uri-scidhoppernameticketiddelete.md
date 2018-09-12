---
title: 删除 (/serviceconfigs/ {scid} {hoppername} /hoppers/ /tickets/ {票证 id})
assetID: d9ff3f21-aa70-af41-afa1-9a9244fcdb95
permalink: en-us/docs/xboxlive/rest/uri-scidhoppernameticketiddelete.html
author: KevinAsgari
description: " 删除 (/serviceconfigs/ {scid} {hoppername} /hoppers/ /tickets/ {票证 id})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3780fb9f69a97d4e2522aa17a806b1fb4917a9f7
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3881427"
---
# <a name="delete-serviceconfigsscidhoppershoppernameticketsticketid"></a>删除 (/serviceconfigs/ {scid} {hoppername} /hoppers/ /tickets/ {票证 id})

删除匹配票证。

> [!IMPORTANT]
> 此方法用于使用合约 103 或更高版本，并且需要 X Xbl 协定版本的标头元素： 103 或更高版本上的每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4E2)
  * [授权](#ID4EGB)
  * [HTTP 状态代码](#ID4EOC)
  * [请求正文](#ID4EXC)
  * [响应正文](#ID4ECD)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法从在服务配置 ID (SCID) 级别命名漏斗中删除指定的票证 ID。 此方法可以由**Microsoft.Xbox.Services.Matchmaking.MatchmakingService.DeleteMatchTicketAsync**包装。  
<a id="ID4E2"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID) 的会话。|
| name| 字符串| 漏斗的名称。|
| 票证 Id| GUID| 票证 id。|

<a id="ID4EGB"></a>


## <a name="authorization"></a>授权

| 类型| 必需| 描述| 如果缺少的响应|
| --- | --- | --- | --- | --- | --- | --- | --- |
| XUID (用户 ID)| 是| 发出请求的用户必须是指代票证的票证会话成员。| 403|
| 权限和设备类型| 是| 当用户的 deviceType 设置为主机时，仅具有多人游戏中其声明特权的用户允许对匹配服务进行调用。| 403|

<a id="ID4EOC"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

该服务返回 HTTP 状态代码应用于 MPSD。  
<a id="ID4EXC"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送任何对象。

<a id="ID4ECD"></a>


## <a name="response-body"></a>响应正文

该响应正文中不发送任何对象。

<a id="ID4EPD"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4ERD"></a>


##### <a name="parent"></a>Parent 的子磁盘）  

[/serviceconfigs/ {scid} {hoppername} /hoppers/ /tickets/ {票证 id}](uri-scidhoppernameticketid.md)

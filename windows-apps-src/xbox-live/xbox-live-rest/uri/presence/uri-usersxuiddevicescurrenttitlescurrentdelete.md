---
title: DELETE (/users/xuid({xuid})/devices/current/titles/current)
assetID: 3bf75247-0a2a-0e4c-afcc-9e7654a89648
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrentdelete.html
author: KevinAsgari
description: " DELETE (/users/xuid({xuid})/devices/current/titles/current)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 512cb5d65279a461937d91929284b2eb1921ec00
ms.sourcegitcommit: 5dda01da4702cbc49c799c750efe0e430b699502
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "4111756"
---
# <a name="delete-usersxuidxuiddevicescurrenttitlescurrent"></a>DELETE (/users/xuid({xuid})/devices/current/titles/current)
删除关闭标题，而不是等待[presencerecord，他的](../../json/json-presencerecord.md)过期的状态。 这些 Uri 的域是`userpresence.xboxlive.com`。
 
  * [URI 参数](#ID4EZ)
  * [授权](#ID4EEB)
  * [需的请求标头](#ID4ERD)
  * [可选的请求标头](#ID4EVF)
  * [请求正文](#ID4EVG)
  * [响应正文](#ID4EAH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 目标用户。| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>授权
 
| 类型| 必需| 描述| 如果缺少的响应| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| 是| 调用方的 Xbox 用户 ID (XUID)| 403 已禁止| 
| titleId| 是| 职务标题的 Id| 403 已禁止| 
| deviceId| 是所有 Windows 和 Web 除外| 调用方的 deviceid-| 403 已禁止| 
| deviceType| Web 除是| 调用方的 deviceType| 403 已禁止| 
  
<a id="ID4ERD"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值:"XBL3.0 x =&lt;userhash >;&lt;令牌 >"。| 
| x xbl 协定版本| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/数。 请求仅为路由到服务验证该标头，身份验证令牌中的声明的有效性后等。 示例值： 3，vnext。| 
| Content-Type| 字符串| 请求正文中的示例值的 mime 类型： 应用程序/json。| 
| Content-Length| 字符串| 请求正文的长度。 示例值： 312。| 
| Host| 字符串| 服务器的域名。 示例值： presencebeta.xboxlive.com。| 
  
<a id="ID4EVF"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion|  | 生成此请求应定向到 Xbox LIVE 的服务的名称/数。 请求仅为路由到服务验证该标头，身份验证令牌中的声明的有效性后等。 默认值： 1。| 
  
<a id="ID4EVG"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送任何对象。
  
<a id="ID4EAH"></a>

 
## <a name="response-body"></a>响应正文
 
如果成功，使用没有响应正文中返回 HTTP 状态代码。
 
如果错误 （HTTP 4xx 或 5xx），则在响应正文中返回相应的错误信息。
  
<a id="ID4ELH"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ENH"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

   
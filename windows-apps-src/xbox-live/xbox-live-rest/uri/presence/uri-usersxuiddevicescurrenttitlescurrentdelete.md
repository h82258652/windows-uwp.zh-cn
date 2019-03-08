---
title: DELETE (/users/xuid({xuid})/devices/current/titles/current)
assetID: 3bf75247-0a2a-0e4c-afcc-9e7654a89648
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrentdelete.html
description: " DELETE (/users/xuid({xuid})/devices/current/titles/current)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dd916fee5455276d45e4437e4ee90cacbde30bf9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604212"
---
# <a name="delete-usersxuidxuiddevicescurrenttitlescurrent"></a>DELETE (/users/xuid({xuid})/devices/current/titles/current)
删除右标题，而不是等待是否存在[PresenceRecord](../../json/json-presencerecord.md)过期。 这些 Uri 的域是`userpresence.xboxlive.com`。
 
  * [URI 参数](#ID4EZ)
  * [Authorization](#ID4EEB)
  * [所需的请求标头](#ID4ERD)
  * [可选的请求标头](#ID4EVF)
  * [请求正文](#ID4EVG)
  * [响应正文](#ID4EAH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 的目标用户。| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>授权
 
| 在任务栏的搜索框中键入| 必需| 描述| 如果缺少的响应| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| 是| 调用方的 Xbox 用户 ID (XUID)| 403 禁止访问| 
| titleId| 是| 标题的职务 Id| 403 禁止访问| 
| deviceId| 是为所有除 Windows 和 Web| 调用方的 deviceid-| 403 禁止访问| 
| deviceType| 是为所有除 Web| 调用方的设备类型| 403 禁止访问| 
  
<a id="ID4ERD"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值："XBL3.0 x =&lt;userhash >;&lt;令牌 >"。| 
| x-xbl-contract-version| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 请求将只路由到的服务验证标头身份验证令牌中声明的有效性后，依次类推。 示例值：3，vnext。| 
| 内容类型| 字符串| 请求示例值的正文的 mime 类型： application/json。| 
| 内容长度| 字符串| 请求正文的长度。 示例值：312.| 
| 主机| 字符串| 服务器的域名。 示例值： presencebeta.xboxlive.com。| 
  
<a id="ID4EVF"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 请求将只路由到的服务验证标头身份验证令牌中声明的有效性后，依次类推。 默认值：1.| 
  
<a id="ID4EVG"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送的任何对象。
  
<a id="ID4EAH"></a>

 
## <a name="response-body"></a>响应正文
 
如果成功，与任何响应正文中返回 HTTP 状态代码。
 
如果出现错误 （HTTP 4xx 或 5xx），响应正文中返回相应的错误的信息。
  
<a id="ID4ELH"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ENH"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

   
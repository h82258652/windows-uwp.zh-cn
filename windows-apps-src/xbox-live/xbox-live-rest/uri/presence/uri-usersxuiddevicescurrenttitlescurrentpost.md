---
title: POST (/users/xuid({xuid})/devices/current/titles/current)
assetID: d5e2d12d-ba75-2d04-2805-c69a4c143f73
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrentpost.html
author: KevinAsgari
description: " POST (/users/xuid({xuid})/devices/current/titles/current)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 31e04f1711d83679ac0b41c74c1c391b26bc7969
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5691856"
---
# <a name="post-usersxuidxuiddevicescurrenttitlescurrent"></a>POST (/users/xuid({xuid})/devices/current/titles/current)
使用用户的状态更新游戏。 这些 Uri 的域是`userpresence.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4EEB)
  * [授权](#ID4EPB)
  * [需的请求标头](#ID4ENE)
  * [可选的请求标头](#ID4ERG)
  * [请求正文](#ID4ERH)
  * [响应正文](#ID4EKAAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
此 URI 可由非控制台平台上的所有游戏添加和更新的状态、 完整状态和游戏的媒体状态数据。
 
**SandboxId**现在从 XToken 声明检索并强制执行。 如果**SandboxId**不存在，娱乐发现服务 (EDS) 将引发 400 错误请求错误。
  
<a id="ID4EEB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 目标用户。| 
  
<a id="ID4EPB"></a>

 
## <a name="authorization"></a>授权
 
| 类型| 必需| 描述| 如果缺少的响应| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| 是| 调用方的 Xbox 用户 ID (XUID)| 403 已禁止| 
| titleId| 是| 职务标题的 Id| 403 已禁止| 
| deviceId| 是针对 Windows 和 Web 除外| 调用方的 deviceid-| 403 已禁止| 
| deviceType| 是针对 Web 除外| 调用方的 deviceType| 403 已禁止| 
| sandboxId| 是，对于来自调用的 | 调用方的沙盒| 403 已禁止| 
  
<a id="ID4ENE"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值:"XBL3.0 x =&lt;userhash >;&lt;令牌 >"。| 
| x xbl 协定版本| 字符串| 名称/的内部版本号应指向此请求的 Xbox LIVE 的服务。 请求将仅可路由到的服务验证该标头，身份验证令牌中的声明的有效性后，依此类推。 示例值： 3，vnext。| 
| Content-Type| 字符串| 请求正文中的示例值的 mime 类型： application/json。| 
| Content-Length| 字符串| 请求正文的长度。 示例值： 312。| 
| Host| 字符串| 服务器的域名。 示例值： presencebeta.xboxlive.com。| 
  
<a id="ID4ERG"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion|  | 名称/的内部版本号应指向此请求的 Xbox LIVE 的服务。 请求将仅可路由到的服务验证该标头，身份验证令牌中的声明的有效性后，依此类推。 默认值： 1。| 
  
<a id="ID4ERH"></a>

 
## <a name="request-body"></a>请求正文
 
请求对象是[TitleRequest](../../json/json-titlerequest.md)。 实际存在的正文中的属性进行更新。 任何属性都不是正文的一部分但存在服务器上将不会修改。
 
<a id="ID4EAAAC"></a>

 
### <a name="sample-request"></a>示例请求
 

```cpp
{
  id:"12341234",
  placement:"snapped",
  state:"active"
}
      
```

   
<a id="ID4EKAAC"></a>

 
## <a name="response-body"></a>响应正文
 
如果成功，HTTP 状态代码的 200 或 201 创建将返回，根据需要。
 
如果出现错误 （HTTP 4xx 或 5xx），在响应正文中返回相应的错误的信息。
  
<a id="ID4EVAAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EXAAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

  
<a id="ID4EBBAC"></a>

 
##### <a name="further-information"></a>详细信息 

[市场 URI](../marketplace/atoc-reference-marketplace.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   
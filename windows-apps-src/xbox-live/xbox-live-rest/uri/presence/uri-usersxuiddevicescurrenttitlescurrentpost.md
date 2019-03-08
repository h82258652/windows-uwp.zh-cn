---
title: POST (/users/xuid({xuid})/devices/current/titles/current)
assetID: d5e2d12d-ba75-2d04-2805-c69a4c143f73
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrentpost.html
description: " POST (/users/xuid({xuid})/devices/current/titles/current)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b29a0bc796712d7b7c44a1fe8512f99bf09eb4fc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649522"
---
# <a name="post-usersxuidxuiddevicescurrenttitlescurrent"></a>POST (/users/xuid({xuid})/devices/current/titles/current)
更新用户的状态显示一个标题。 这些 Uri 的域是`userpresence.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4EEB)
  * [Authorization](#ID4EPB)
  * [所需的请求标头](#ID4ENE)
  * [可选的请求标头](#ID4ERG)
  * [请求正文](#ID4ERH)
  * [响应正文](#ID4EKAAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
在非控制台平台上的所有标题可以都使用此 URI 来添加和更新状态显示、 丰富的状态显示和标题的媒体状态显示数据。
 
**SandboxId**现在从声明中 XToken 检索，并强制执行。 如果**SandboxId**不存在，则娱乐发现服务 (EDS) 将会引发一个 400 错误请求错误。
  
<a id="ID4EEB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 的目标用户。| 
  
<a id="ID4EPB"></a>

 
## <a name="authorization"></a>授权
 
| 在任务栏的搜索框中键入| 必需| 描述| 如果缺少的响应| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| 是| 调用方的 Xbox 用户 ID (XUID)| 403 禁止访问| 
| titleId| 是| 标题的职务 Id| 403 禁止访问| 
| deviceId| 是为所有除 Windows 和 Web| 调用方的 deviceid-| 403 禁止访问| 
| deviceType| 是为所有除 Web| 调用方的设备类型| 403 禁止访问| 
| sandboxId| 是的调用来自 | 调用方的沙盒| 403 禁止访问| 
  
<a id="ID4ENE"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值："XBL3.0 x =&lt;userhash >;&lt;令牌 >"。| 
| x-xbl-contract-version| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 请求将只路由到的服务验证标头身份验证令牌中声明的有效性后，依次类推。 示例值：3，vnext。| 
| 内容类型| 字符串| 请求示例值的正文的 mime 类型： application/json。| 
| 内容长度| 字符串| 请求正文的长度。 示例值：312.| 
| 主机| 字符串| 服务器的域名。 示例值： presencebeta.xboxlive.com。| 
  
<a id="ID4ERG"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 请求将只路由到的服务验证标头身份验证令牌中声明的有效性后，依次类推。 默认值：1.| 
  
<a id="ID4ERH"></a>

 
## <a name="request-body"></a>请求正文
 
请求对象是[TitleRequest](../../json/json-titlerequest.md)。 更新正文中实际存在的属性。 任何属性是不是正文的一部分而存在的服务器上将不会修改。
 
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
 
如果成功，HTTP 状态代码 200 或 201 已创建的是返回，根据需要。
 
如果出现错误 （HTTP 4xx 或 5xx），响应正文中返回相应的错误的信息。
  
<a id="ID4EVAAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EXAAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

  
<a id="ID4EBBAC"></a>

 
##### <a name="further-information"></a>详细信息 

[Marketplace Uri](../marketplace/atoc-reference-marketplace.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   
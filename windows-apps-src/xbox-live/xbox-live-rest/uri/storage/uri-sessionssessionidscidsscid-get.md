---
title: GET (/sessions/{sessionId}/scids/{scid})
assetID: 1feaceed-ba0d-0b0c-e809-44ba41f2e4ea
permalink: en-us/docs/xboxlive/rest/uri-sessionssessionidscidsscid-get.html
author: KevinAsgari
description: " GET (/sessions/{sessionId}/scids/{scid})"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 47b88e62b722af30c61e94f78d796cd00352fe65
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5686414"
---
# <a name="get-sessionssessionidscidsscid"></a>GET (/sessions/{sessionId}/scids/{scid})
检索此存储类型的配额信息。 这些 Uri 的域是`titlestorage.xboxlive.com`。
 
  * [URI 参数](#ID4EX)
  * [授权](#ID4ECB)
  * [需的请求标头](#ID4ENB)
  * [请求正文](#ID4EWC)
  * [HTTP 状态代码](#ID4EBD)
  * [响应正文](#ID4E2H)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| sessionId| 字符串| 若要查找会话的 ID。| 
| scid| guid| 若要查找的服务配置 ID。| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>授权
 
请求必须包含有效的 Xbox LIVE 授权标头。 如果调用方不允许访问此资源，该服务将返回 403 禁止访问响应。 如果标头是无效或不存在，该服务将返回 401 未经授权的响应。 
  
<a id="ID4ENB"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标头| 值| 说明| 
| --- | --- | --- | --- | --- | --- | 
| x xbl 协定版本| 1| API 协定版本。| 
| 授权| XBL3.0 x = [哈希];[令牌]| STS 身份验证令牌。 STSTokenString 替换为由身份验证请求返回的令牌。 有关检索 STS 令牌和创建授权标头的其他信息，请参阅 Authenticating 和授权 Xbox LIVE 服务请求。| 
  
<a id="ID4EWC"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送任何对象。
  
<a id="ID4EBD"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
此部分中使用此方法对此资源所做的请求的响应，该服务返回其中一个状态代码。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| “确定”| 请求已成功。| 
| 201| 已创建| 创建实体。| 
| 400| 错误请求| 服务可能不理解格式不正确的请求。 通常无效参数。| 
| 401| 未授权| 请求要求用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 找不到| 找不到指定的资源。| 
| 406| 不允许| 不支持资源版本。| 
| 408| 请求超时| 请求所花的时间太长，才能完成。| 
| 500| 内部服务器错误| 服务器时遇到意外的情况，执行此请求将阻止它。| 
| 503| 服务不可用| 请求已被阻止，以秒为单位 （例如 5 秒更高版本） 的客户端重试值后重试请求。| 
  
<a id="ID4E2H"></a>

 
## <a name="response-body"></a>响应正文
 
如果调用成功，该服务将返回一个[quotaInfo (JSON)](../../json/json-quota.md)对象。 
 
<a id="ID4EKAAC"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
{
  "quotaInfo":
  {
    "usedBytes":619,
    "quotaBytes":16777216
  }
}
         
```

   
<a id="ID4EWAAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EYAAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/sessions/{sessionId}/scids/{scid}](uri-sessionssessionidscidsscid.md)

  
<a id="ID4ECBAC"></a>

 
##### <a name="reference"></a>参考 

[quotaInfo (JSON)](../../json/json-quota.md)

   
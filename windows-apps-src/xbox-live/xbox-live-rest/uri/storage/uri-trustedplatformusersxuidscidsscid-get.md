---
title: GET (/trustedplatform/users/xuid({xuid})/scids/{scid})
assetID: 29c8c12a-5d9f-89c1-a739-c600bad893c2
permalink: en-us/docs/xboxlive/rest/uri-trustedplatformusersxuidscidsscid-get.html
description: " GET (/trustedplatform/users/xuid({xuid})/scids/{scid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8425d5349b57cac209e177a9d9c013e28a433647
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618882"
---
# <a name="get-trustedplatformusersxuidxuidscidsscid"></a>GET (/trustedplatform/users/xuid({xuid})/scids/{scid})
检索此存储类型的配额信息。 这些 Uri 的域是`titlestorage.xboxlive.com`。
 
  * [URI 参数](#ID4EX)
  * [Authorization](#ID4ECB)
  * [所需的请求标头](#ID4ENB)
  * [请求正文](#ID4EWC)
  * [HTTP 状态代码](#ID4EBD)
  * [响应正文](#ID4EUAAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 播放机的用户发出请求。| 
| scid| GUID| 要查找服务配置的 ID。| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>授权
 
该请求必须包含有效的 Xbox LIVE 授权标头。 如果调用方不允许访问此资源，服务将返回 403 Forbidden 响应。 如果标头无效或缺失，则服务将返回 401 未授权的响应。 
  
<a id="ID4ENB"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| API 协定版本。| 
| 授权| XBL3.0 x=[hash];[token]| STS 身份验证令牌。 STSTokenString 将替换为返回身份验证请求的令牌。 有关检索的 STS 令牌和创建授权标头的其他信息，请参阅进行身份验证和授权 Xbox LIVE 服务请求。| 
  
<a id="ID4EWC"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送的任何对象。
  
<a id="ID4EBD"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码 
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 确定 | 请求已成功。| 
| 201| 创建时间 | 创建实体。| 
| 400| 无效的请求 | 服务无法理解请求格式不正确。 通常是一个无效的参数。| 
| 401| 未经授权 | 请求需要用户身份验证。| 
| 403| 已禁止 | 为用户或服务不允许该请求。| 
| 404| 未找到 | 找不到指定的资源。| 
| 406| 不可接受 | 不支持资源版本。| 
| 408| 请求超时 | 该请求花费了太长时间才能完成。| 
| 500| 内部服务器错误 | 服务器遇到意外的情况，致使无法履行请求。| 
| 503| 服务不可用 | 请求已达到限制，以秒为单位 （例如 5 秒更高版本） 的客户端-重试值后重试请求。| 
  
<a id="ID4EUAAC"></a>

 
## <a name="response-body"></a>响应正文
 
如果调用是成功，则服务会返回[quotaInfo](../../json/json-quota.md)对象。 
 
<a id="ID4ECBAC"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
{
  "quotaInfo" :
  {
    "usedBytes" : 619,
    "quotaBytes" : 16777216
  }
}
         
```

   
<a id="ID4EOBAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EQBAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/trustedplatform/users/xuid({xuid})/scids/{scid}](uri-trustedplatformusersxuidscidsscid.md)

  
<a id="ID4E1BAC"></a>

 
##### <a name="reference"></a>参考 

[quotaInfo (JSON)](../../json/json-quota.md)

   
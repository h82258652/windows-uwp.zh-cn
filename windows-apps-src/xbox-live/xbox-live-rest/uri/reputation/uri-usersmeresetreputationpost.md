---
title: POST (/users/me/resetreputation)
assetID: 1a4fed45-f608-dac2-3384-2ee493112f7b
permalink: en-us/docs/xboxlive/rest/uri-usersmeresetreputationpost.html
description: " POST (/users/me/resetreputation)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fc770673ac7e4034004f58032c7fa66cb28413e7
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2018
ms.locfileid: "8743683"
---
# <a name="post-usersmeresetreputation"></a>POST (/users/me/resetreputation)
使执行团队后 （例如） 帐户劫持将当前用户的信誉评分设置为某些任意值。 这些 Uri 的域是`reputation.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [授权](#ID4E5)
  * [需的请求标头](#ID4ETB)
  * [请求正文](#ID4END)
  * [HTTP 状态代码](#ID4EDE)
  * [响应正文](#ID4EFH)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
通过任何其他合作伙伴的所有除零售沙盒和 Retail，除非出于测试目的的所有沙盒中的用户，也可能会调用此方法。 请注意，此请求用于设置用户的"基本"信誉评分，他正面反馈权重将全部归零。在执行此调用后用户的实际信誉将这些基本评分以及他大使奖金他关注者奖金。
  
<a id="ID4E5"></a>

 
## <a name="authorization"></a>授权
 
从合作伙伴： 为零售沙盒， **PartnerClaim**来自强制执行团队;对于所有其他沙盒， **PartnerClaim**。
 
从用户： 以外的所有沙盒零售、 **XuidClaim**和**TitleClaim**除外。
  
<a id="ID4ETB"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
来自所有： **Content-type: application/json**。
 
从合作伙伴： **X Xbl 协定版本**（当前版本是 101）， **X Xbl 沙盒**。
 
从用户: （当前版本是 101） **X Xbl 协定版本**。
 
| 标头| 类型| 描述| 
| --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值:"XBL3.0 x =&lt;userhash >;&lt;令牌 >"。| 
| X RequestedServiceVersion|  | 名称/的内部版本号应指向此请求的 Xbox LIVE 的服务。 请求将仅可路由到的服务验证该标头，身份验证令牌中的声明的有效性后，依此类推。 默认值： 101。| 
  
<a id="ID4END"></a>

 
## <a name="request-body"></a>请求正文
 
<a id="ID4ETD"></a>

 
### <a name="sample-request"></a>示例请求
 
请求正文是一个简单的[ResetReputation (JSON)](../../json/json-resetreputation.md)文档。
 

```cpp
{
    "fairplayReputation": 75,
    "commsReputation": 75,
    "userContentReputation": 75
}
      
```

   
<a id="ID4EDE"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
此部分中使用此方法对此资源所做的请求的响应，该服务返回的状态代码之一。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 200| “确定”| 确定。| 
| 400| 错误请求| 服务可能不理解格式不正确的请求。 通常无效参数。| 
| 401| 未授权| 请求要求用户身份验证。| 
| 404| 找不到| 找不到指定的资源。| 
| 500| 内部服务器错误| 服务器时遇到意外的情况，执行此请求将阻止它。| 
| 503| 服务不可用| 请求已被阻止，以秒为单位 （例如 5 秒更高版本） 的客户端重试值后重试请求。| 
  
<a id="ID4EFH"></a>

 
## <a name="response-body"></a>响应正文
 
成功时，响应正文为空。 失败时，会返回一个[ServiceError (JSON)](../../json/json-serviceerror.md)文档。
 
<a id="ID4ERH"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
{
    "code": 4000,
    "source": "ReputationFD",
    "description": "No targetXuid field was supplied in the request"
}
         
```

   
<a id="ID4E2H"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E4H"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/me/resetreputation](uri-usersmeresetreputation.md)

   
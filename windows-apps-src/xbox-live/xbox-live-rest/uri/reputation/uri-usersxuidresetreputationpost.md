---
title: POST (/users/xuid({xuid})/resetreputation)
assetID: 3b76857f-b043-2c76-cf0c-c8f355fe3849
permalink: en-us/docs/xboxlive/rest/uri-usersxuidresetreputationpost.html
description: " POST (/users/xuid({xuid})/resetreputation)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2918249eaf359b383e89f24b8a37352bc3fe5132
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623522"
---
# <a name="post-usersxuidxuidresetreputation"></a>POST (/users/xuid({xuid})/resetreputation)
启用强制执行团队后 （例如） 帐户劫持为一些任意的值设置指定的用户的信誉分数。 这些 Uri 的域是`reputation.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4E5)
  * [Authorization](#ID4EJB)
  * [所需的请求标头](#ID4E5B)
  * [请求正文](#ID4EYD)
  * [HTTP 状态代码](#ID4EOE)
  * [响应正文](#ID4EQH)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
通过为所有沙箱零售，除任何其他合作伙伴和零售，除用于测试目的的所有沙盒中的用户，也可能会调用此方法。 请注意，此请求设置用户的"基本"信誉分数，他积极的反馈加权值将所有清。用户的实际信誉进行此调用后将这些基本分数加上他代表奖金再加上他跟踪块奖金。
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| xuid| 字符串| Xbox 用户 ID (XUID) 指定用户。| 
  
<a id="ID4EJB"></a>

 
## <a name="authorization"></a>授权
 
从合作伙伴：零售沙盒**PartnerClaim**强制实施团队; 对于所有其他沙盒**PartnerClaim**。
 
从用户：为所有除零售，沙箱**XuidClaim**并**TitleClaim**。
  
<a id="ID4E5B"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
从所有：**内容类型： application/json**。
 
从合作伙伴：**X Xbl 协定版本**（当前版本为 101）， **X Xbl 沙盒**。
 
从用户：**X Xbl 协定版本**（当前版本为 101）。
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值："XBL3.0 x =&lt;userhash >;&lt;令牌 >"。| 
| X-RequestedServiceVersion|  | 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 请求将只路由到的服务验证标头身份验证令牌中声明的有效性后，依次类推。 默认值：101.| 
  
<a id="ID4EYD"></a>

 
## <a name="request-body"></a>请求正文
 
<a id="ID4E5D"></a>

 
### <a name="sample-request"></a>示例请求
 
请求正文是一个简单[ResetReputation (JSON)](../../json/json-resetreputation.md)文档。
 

```cpp
{
    "fairplayReputation": 75,
    "commsReputation": 75,
    "userContentReputation": 75
}
      
```

   
<a id="ID4EOE"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 确定| 确定。| 
| 400| 无效的请求| 服务无法理解请求格式不正确。 通常是一个无效的参数。| 
| 401| 未经授权| 请求需要用户身份验证。| 
| 404| 未找到| 找不到指定的资源。| 
| 500| 内部服务器错误| 服务器遇到意外的情况，致使无法履行请求。| 
| 503| 服务不可用| 请求已达到限制，以秒为单位 （例如 5 秒更高版本） 的客户端-重试值后重试请求。| 
  
<a id="ID4EQH"></a>

 
## <a name="response-body"></a>响应正文
 
如果成功，响应正文为空。 在失败时，[服务错误 (JSON)](../../json/json-serviceerror.md)返回文档。
 
<a id="ID4E3H"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
{
    "code": 4000,
    "source": "ReputationFD",
    "description": "No targetXuid field was supplied in the request"
}
         
```

   
<a id="ID4EHAAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EJAAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/xuid({xuid})/resetreputation](uri-usersxuidresetreputation.md)

   
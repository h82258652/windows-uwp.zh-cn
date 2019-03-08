---
title: POST (/users/batchfeedback)
assetID: f94dcf19-a4e3-5bd0-5276-23e43bdcae52
permalink: en-us/docs/xboxlive/rest/uri-reputationusersbatchfeedbackpost.html
description: " POST (/users/batchfeedback)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0906d32a0e15b2eaaf9c33e7f658e9e9f0cd5124
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622722"
---
# <a name="post-usersbatchfeedback"></a>POST (/users/batchfeedback)
标题的服务用于标题的接口之外的批处理形式发送反馈。 这些 Uri 的域是`reputation.xboxlive.com`。
 
  * [请求正文](#ID4EX)
  * [必需的标头](#ID4E3E)
  * [HTTP 状态代码](#ID4EWG)
  * [响应正文](#ID4EDAAC)
 
<a id="ID4EX"></a>

 
## <a name="request-body"></a>请求正文 
 
调用方必须在其 web 请求对象的 ClientCertificates 部分中包含其声明证书。
 
<a id="ID4EBB"></a>

 
### <a name="required-members"></a>所需的成员 
 
请求应包含的数组**BatchFeedback**对象。 
  
<a id="ID4EPB"></a>

 
### <a name="prohibited-members"></a>禁止的成员 
 
所有其他成员禁止在请求中使用。
  
<a id="ID4E3B"></a>

 
### <a name="sample-request"></a>示例请求 
 

```cpp
{
    "items" :
    [
        {
            "targetXuid": "33445566778899",
            "titleId": "6487",
            "sessionRef":
            {
                "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
                "templateName": "CaptureFlag5",
                "name": "Halo556932",
            },
            "feedbackType": "FairPlayKillsTeammates",
            "textReason": "Killed 19 team members in a single session",
            "evidenceId": null
        },
        {
            "targetXuid": "33445566778899",
            "titleId": "6487",
            "sessionRef":
            {
                "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
                "templateName": "CaptureFlag5",
                "name": "Halo556932",
            },
            "feedbackType": "FairPlayQuitter",
            "textReason": "Quit 6 times from 9 sessions",
            "evidenceId": null
        }
    ]
}

      
```

 
| <b>字段</b>| <b>JSON 类型</b>| <b>描述</b>| 
| --- | --- | --- | 
| 项目| 数组| 反馈 JSON 文档的集合。| 
| targetXuid| 字符串| 目标用户的 XUID| 
| titleId| 字符串| 此反馈，已发送的标题或 NULL。| 
| sessionRef| 对象| 描述 MPSD 会话的对象此反馈相关，则为 NULL。| 
| feedbackType| 字符串| FeedbackType 枚举中的值的字符串版本。| 
| textReason| 字符串| 发件人可能会添加到提供有关已提交的反馈的详细信息的合作伙伴提供的文本。| 
| evidenceId| 字符串| 可以用作要提交的反馈的证据的资源 ID。 例如视频文件的 ID。| 
   
<a id="ID4E3E"></a>

 
## <a name="required-headers"></a>必需的标头
 
在 Xbox Live 服务请求时，以下标头是必需的。 

> [!NOTE] 
> 合作伙伴声明证书必须是请求，以便提交批处理的反馈发送。 


 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 101| API 协定版本。| 
| 内容类型| 应用程序/json| 正在提交的数据类型。| 
| 授权| "XBL3.0 x=&lt;userhash>;&lt;token>"| HTTP 身份验证的身份验证凭据。| 
| X-RequestedServiceVersion| 101| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 请求将只路由到的服务验证标头身份验证令牌中声明的有效性后，依次类推。| 
  
<a id="ID4EWG"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 400| 无效的请求| 服务无法理解请求格式不正确。 通常是一个无效的参数。| 
| 401| 未经授权| 请求需要用户身份验证。| 
| 404| 未找到| 找不到指定的资源。| 
| 500| 内部服务器错误| 服务器遇到意外的情况，致使无法履行请求。| 
| 503| 服务不可用| 请求已达到限制，以秒为单位 （例如 5 秒更高版本） 的客户端-重试值后重试请求。| 
  
<a id="ID4EDAAC"></a>

 
## <a name="response-body"></a>响应正文 
 
如果调用成功，此响应中不返回任何对象。 否则，该服务将返回[服务错误](../../json/json-serviceerror.md)对象。
  
<a id="ID4EXAAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EZAAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/batchfeedback](uri-reputationusersbatchfeedback.md)

  
<a id="ID4EFBAC"></a>

 
##### <a name="reference"></a>参考 

[Feedback (JSON)](../../json/json-feedback.md)

 [ServiceError (JSON)](../../json/json-serviceerror.md)

   
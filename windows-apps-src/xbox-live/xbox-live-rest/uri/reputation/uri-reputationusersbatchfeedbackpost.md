---
title: POST (/ 用户/batchfeedback)
assetID: f94dcf19-a4e3-5bd0-5276-23e43bdcae52
permalink: en-us/docs/xboxlive/rest/uri-reputationusersbatchfeedbackpost.html
author: KevinAsgari
description: " POST (/ 用户/batchfeedback)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d62e4f7106f7f0f2c324ca2c68ea8fe476bc7bfb
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3956536"
---
# <a name="post-usersbatchfeedback"></a>POST (/ 用户/batchfeedback)
使用你的游戏服务来在你的游戏界面之外的批处理形式发送反馈。 这些 Uri 的域是`reputation.xboxlive.com`。
 
  * [请求正文](#ID4EX)
  * [所需的标头](#ID4E3E)
  * [HTTP 状态代码](#ID4EWG)
  * [响应正文](#ID4EDAAC)
 
<a id="ID4EX"></a>

 
## <a name="request-body"></a>请求正文 
 
调用方必须在其 web 请求对象的 ClientCertificates 部分包括其声明证书。
 
<a id="ID4EBB"></a>

 
### <a name="required-members"></a>所需的成员 
 
请求应包含一个**BatchFeedback**对象数组。 
  
<a id="ID4EPB"></a>

 
### <a name="prohibited-members"></a>禁止的成员 
 
所有其他成员禁止在请求中。
  
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
| titleId| 字符串| 标题此反馈发送，则为 NULL。| 
| sessionRef| 对象| 描述在 MPSD 会话的对象此反馈与之，则为 NULL。| 
| feedbackType| 字符串| FeedbackType 枚举中的值字符串版本。| 
| textReason| 字符串| 发件人可能会添加以提供更多详细信息已提交的反馈的合作伙伴提供的文本。| 
| evidenceId| 字符串| 可用作所提交反馈的证据的资源的 ID。 例如视频文件的 ID。| 
   
<a id="ID4E3E"></a>

 
## <a name="required-headers"></a>所需的标头
 
在 Xbox Live 服务请求时，以下标头是必需的。 

> [!NOTE] 
> 合作伙伴声明证书必须与才能提交批量反馈请求发送。 


 
| 标头| 值| 说明| 
| --- | --- | --- | --- | --- | --- | --- | 
| x xbl 协定版本| 101| API 协定版本。| 
| 内容类型| 应用程序/json| 提交的数据的类型。| 
| 授权| "XBL3.0 x =&lt;userhash >;&lt;令牌 >"| HTTP 身份验证的身份验证凭据。| 
| X RequestedServiceVersion| 101| 生成此请求应定向到的 Xbox LIVE 的服务的名称/号码。 请求将仅可路由到的服务后验证标头，身份验证令牌中的声明的有效性，依此类推。| 
  
<a id="ID4EWG"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
该服务返回的状态代码之一此部分中使用此方法对此资源进行的请求的响应。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 400| 错误请求| 服务可能不理解格式不正确的请求。 通常是一个无效的参数。| 
| 401| 未授权| 请求要求用户身份验证。| 
| 404| 找不到| 找不到指定的资源。| 
| 500| 内部服务器错误| 服务器时遇到意外的情况，使其不能完成请求。| 
| 503| 服务不可用| 请求已被阻止，以秒为单位 （例如 5 秒更高版本） 的客户端重试值后重试请求。| 
  
<a id="ID4EDAAC"></a>

 
## <a name="response-body"></a>响应正文 
 
如果调用成功，此响应中不返回任何对象。 否则，该服务返回[ServiceError](../../json/json-serviceerror.md)对象。
  
<a id="ID4EXAAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EZAAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/ 用户/batchfeedback](uri-reputationusersbatchfeedback.md)

  
<a id="ID4EFBAC"></a>

 
##### <a name="reference"></a>参考 

[反馈 (JSON)](../../json/json-feedback.md)

 [ServiceError (JSON)](../../json/json-serviceerror.md)

   
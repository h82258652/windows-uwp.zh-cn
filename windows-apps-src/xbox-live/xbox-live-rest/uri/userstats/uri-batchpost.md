---
title: POST (/batch)
assetID: f5997c8e-82c7-0fba-9991-ce1116db4830
permalink: en-us/docs/xboxlive/rest/uri-batchpost.html
author: KevinAsgari
description: " POST (/batch)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: db832032a40b40d4b3a774a56487f7065d9cd8ff
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2018
ms.locfileid: "5169232"
---
# <a name="post-batch"></a>POST (/batch)
发布作为跨多个游戏的多个玩家统计数据的复杂的批处理请求的 GET 方法的方法。 这些 Uri 的域是`userstats.xboxlive.com`。
 
<a id="ID4ET"></a>

 
## <a name="remarks"></a>备注
 
游戏开发人员可以将统计数据标记为打开或 XDP 或开发人员中心使用限制。 排行榜是打开统计信息。 打开统计信息可以访问 Smartglass，以及 iOS、 Android、 Windows、 Windows Phone 和 web 应用程序，只要用户有权访问沙盒。 通过 XDP 或开发人员中心管理到沙盒的用户身份验证。
  
  * [备注](#ID4ET)
  * [备注](#ID4EFB)
  * [授权](#ID4EUB)
  * [需的请求标头](#ID4ETC)
  * [可选的请求标头](#ID4E3D)
  * [请求正文](#ID4EAF)
  * [HTTP 状态代码](#ID4EWF)
  * [响应正文](#ID4ENBAC)
 
<a id="ID4EFB"></a>

 
## <a name="remarks"></a>备注
 
调用方提供的用户、 服务配置 Id (Scid) 和每个要为其检索这些统计信息的 Scid 的统计数据名称的列表数组的邮件正文。
 
你可能会发现它的详细信息很有用，若要查看简单，单统计信息之前的[GET](uri-usersxuidscidsscidstatsget.md)方法读取本页更复杂，批模式。
  
<a id="ID4EUB"></a>

 
## <a name="authorization"></a>授权
 
没有针对内容隔离和访问控制方案实现的授权逻辑。
 
   * 假设调用方提交与请求有效的 XSTS 令牌，可以从任何平台上的客户端读取排行榜和用户统计信息。 写入都很明显限制为支持的客户端。
   * 游戏开发人员可以将统计数据标记为打开或 XDP 或开发人员中心使用限制。 排行榜是打开统计信息。 打开统计信息可以访问 Smartglass，以及 iOS、 Android、 Windows、 Windows Phone 和 web 应用程序，只要用户有权访问沙盒。 通过 XDP 或开发人员中心管理到沙盒的用户身份验证。
  
下面的示例是伪代码检查：
 

```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.
         
```

  
<a id="ID4ETC"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值:"XBL3.0 x =&lt;userhash >;&lt;令牌 >"。| 
  
<a id="ID4E3D"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion|  | 名称/的内部版本号此请求应定向到该服务。 请求将仅可路由到的服务验证该标头，身份验证令牌中的声明的有效性后，依此类推。 默认值： 1。| 
  
<a id="ID4EAF"></a>

 
## <a name="request-body"></a>请求正文
 
<a id="ID4EIF"></a>

 
### <a name="sample-request"></a>示例请求
 
以下文章正文通知服务从两个不同 Scid 为两个不同的用户所请求四个统计信息。
 

```cpp
{    
"requestedusers": [
                1234567890123460,
                1234567890123234
            ],
            "requestedscids": [
                {
                    "scid": "c402ff50-3e76-11e2-a25f-0800200c1212",
                    "requestedstats": [
                        "Test4FirefightKills",
                        "Test4FirefightHeadshots"
                    ]
                },
                {
                    "scid": "c402ff50-3e76-11e2-a25f-0800200c0343",
                    "requestedstats": [
                        "OverallTestKills",
                        "TestHeadshots"
                    ]
                }
            ] 
}
      
```

   
<a id="ID4EWF"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
此部分中使用此方法对此资源所做的请求的响应，该服务返回的状态代码之一。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| “确定”| 已成功检索会话。| 
| 304| 未修改| 资源不已修改自最后一次请求。| 
| 400| 错误请求| 服务可能不理解格式不正确的请求。 通常无效参数。| 
| 401| 未授权| 请求要求用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 找不到| 找不到指定的资源。| 
| 406| 不允许| 不支持资源版本。| 
| 408| 请求超时| 资源版本不受支持;应拒绝 MVC 层。| 
  
<a id="ID4ENBAC"></a>

 
## <a name="response-body"></a>响应正文
 
<a id="ID4EXBAC"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
{    
   "users":[          
       {    
      "xuid": "123456789"
        "gamertag": "WarriorSaint",
        "scids":[
          {
             "scid":"c402ff50-3e76-11e2-a25f-0800200c1212",
             "stats":  [
          {
                 "statname":"Test4FirefightKills",
                 "type":"Integer",
                 "value":7
             },
          {
                 "statname":"Test4FirefightHeadshots",
                 "type":"Integer",
                 "value":4
             }]
                        },
          {
             "scid":"c402ff50-3e76-11e2-a25f-0800200c0343",
             "stats":  [
          {
                 "statname":"OverallTestKills",
                 "type":"Integer",
                 "value":3434
             },
          {
                 "statname":"TestHeadshots",
                 "type":"Integer",
                 "value":41
             }]
          }],
                   },
    {    
                   "gamertag":"TigerShark",
                   "xuid":1234567890123234
        "scids":[
          {
             "scid":"123456789",
             "stats":  [
          {
                 "statname":"Test4FirefightKills",
                 "type":"Integer",
                 "value":63
             },
          {
                 "statname":"Test4FirefightHeadshots",
                 "type":"Integer",
                 "value":12
             }]
                        },
          {
"scid":"987654321",
             "stats":  [
          {
                 "statname":"OverallTestKills",
                 "type":"Integer",
                 "value":375
             },
          {
                 "statname":"TestHeadshots",
                 "type":"Integer",
                 "value":34
             }]
          }],
                   }]
}
         
```

   
<a id="ID4EDCAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EFCAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/batch](uri-batch.md)

   
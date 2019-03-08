---
title: GET (/users/xuid({xuid})/groups/{moniker} )
assetID: 63aa7e5d-0599-5850-756d-3079c0772238
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerget.html
description: " GET (/users/xuid({xuid})/groups/{moniker} )"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: df967677ce779fc128a8956f137a027a108d313d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623842"
---
# <a name="get-usersxuidxuidgroupsmoniker-"></a>GET (/users/xuid({xuid})/groups/{moniker} )
获取一组 PresenceRecord。 这些 Uri 的域是`userpresence.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4E5)
  * [查询字符串参数](#ID4EJB)
  * [Authorization](#ID4EKC)
  * [资源上的隐私设置的效果](#ID4EQD)
  * [所需的请求标头](#ID4EEH)
  * [可选的请求标头](#ID4EMBAC)
  * [请求正文](#ID4EMCAC)
  * [HTTP 状态代码](#ID4EXCAC)
  * [所需的响应标头](#ID4E3GAC)
  * [可选的响应标头](#ID4EMJAC)
  * [响应正文](#ID4E5KAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
检索与在 URI 中，用户的名字对象所指定的组中的用户，并返回为这些用户 PresenceRecord。 隐私或内容隔离管制的数据只是不会返回。
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| xuid| 字符串| Xbox 用户 ID (XUID) 与组中 XUIDs 相关的用户。| 
| 名字对象| 字符串| 定义用户组的字符串。 目前唯一接受的名字对象带有大写字母 P 是"People"。| 
  
<a id="ID4EJB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | 
| level| 字符串| 返回指定的此查询字符串的详细信息的级别。 有效选项包括"用户"、"设备"、"title"和"全部"。"用户"的级别是 PresenceRecord 对象而无需 DeviceRecord 嵌套对象。 "设备"的级别是 PresenceRecord 和 DeviceRecord 对象，而 TitleRecord 嵌套对象。 级别"title"是 PresenceRecord、 DeviceRecord 和 TitleRecord 对象，而 ActivityRecord 嵌套对象。 "全部"级别为整条记录，包括所有嵌套的对象。如果未提供此参数，该服务默认为标题级别 （即，它将返回状态显示为此用户到标题的详细信息）。| 
  
<a id="ID4EKC"></a>

 
## <a name="authorization"></a>授权
 
使用授权声明 | 声明| 在任务栏的搜索框中键入| 是否为必需？| 示例值| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>Xuid</b>| 64 位有符号的整数| 是| 1234567890| 
  
<a id="ID4EQD"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>资源上的隐私设置的效果
 
该服务将始终返回 200 OK 请求本身是否格式正确。 但是，它将筛选出响应中的信息时的隐私检查未通过。
 
资源上的隐私设置的效果 | 发出请求的用户| 面向用户的隐私设置| 行为| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 我| -| 如所述。| 
| 友元| 每个人| 如所述。| 
| 友元| 仅朋友| 如所述。| 
| 友元| 已阻止| 如所述的服务将筛选出数据。| 
| 非友元用户| 每个人| 如所述。| 
| 非友元用户| 仅朋友| 如所述的服务将筛选出数据。| 
| 非友元用户| 已阻止| 如所述的服务将筛选出数据。| 
| 第三方站点| 每个人| 如所述的服务将筛选出数据。| 
| 第三方站点| 仅朋友| 如所述的服务将筛选出数据。| 
| 第三方站点| 已阻止| 如所述的服务将筛选出数据。| 
  
<a id="ID4EEH"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值："XBL3.0 x =&lt;userhash >;&lt;令牌 >"。| 
| x-xbl-contract-version| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 请求将只路由到的服务验证标头身份验证令牌中声明的有效性后，依次类推。 示例值：3，vnext。| 
| 接受| 字符串| 内容类型可接受。 只有一个存在支持由<b>应用程序 /json</b>，但它仍必须指定标头中。| 
| Accept-Language| 字符串| 在响应中的字符串的可接受区域设置。 值示例： EN-US。| 
| 主机| 字符串| 服务器的域名。 示例值： userpresence.xboxlive.com。| 
  
<a id="ID4EMBAC"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 请求将只路由到的服务验证标头身份验证令牌中声明的有效性后，依次类推。 默认值：1.| 
  
<a id="ID4EMCAC"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送的任何对象。
  
<a id="ID4EXCAC"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 确定| 已成功检索该会话。| 
| 400| 无效的请求| 服务无法理解请求格式不正确。 通常是一个无效的参数。| 
| 401| 未经授权| 请求需要用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 未找到| 找不到指定的资源。| 
| 405| 不允许的方法| 在不受支持的内容类型上使用了 HTTP 方法。| 
| 406| 不可接受| 不支持资源版本。| 
| 500| 请求超时| 服务无法理解请求格式不正确。 通常是一个无效的参数。| 
| 503| 请求超时| 服务无法理解请求格式不正确。 通常是一个无效的参数。| 
  
<a id="ID4E3GAC"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 请求将只路由到的服务验证标头身份验证令牌中声明的有效性后，依次类推。 示例值：1，vnext。| 
| 内容类型| 字符串| 请求正文的 mime 类型。 示例值：<b>应用程序 /json</b>。| 
| Cache-Control| 字符串| 请求正常，以指定缓存行为。 示例值:"无缓存"。| 
| X-XblCorrelationId| 字符串| 要将服务器返回的内容和客户端接收到的内容相关联的服务生成的值。 示例值："4106d0bc-1cb3-47bd-83fd-57d041c6febe"。| 
| X-Content-Type-Option| 字符串| 返回符合 SDL 的值。 示例值:"nosniff"。| 
| 日期| 字符串| 发送消息的日期/时间。 示例值："周二，17 年 11 月 2012年 10:33:31 GMT"。| 
  
<a id="ID4EMJAC"></a>

 
## <a name="optional-response-headers"></a>可选的响应标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 重试间隔| 字符串| 返回 503 HTTP 错误。 可让客户端知道多长时间等待重试调用。 示例值："120".| 
| 内容长度| 字符串| 响应正文的长度。 示例值："527".| 
| Content-Encoding| 字符串| 编码的响应的类型。 示例值:"gzip"。| 
  
<a id="ID4E5KAC"></a>

 
## <a name="response-body"></a>响应正文
 
此 API 返回另一个用于每个请求中 XUIDs PresenceRecord 对象的数组。
 
<a id="ID4EGLAC"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
[
     {
         xuid:"0123456789",
         state:"online",
         devices:
         [
             {
                 type:"D",
                 titles:
                 [
                     {
                         id:"12341234",
                         name:"Contoso 5",
                         lastModified:"2012-09-17T07:15:23.4930000",
                         placement:"full",
                         state:"active",
                         activity:
                         {
                             richPresence:"Playing on Nirvana"
                         },
                     }
                 ]
             }
         ]
     },
     {
         xuid:"0123456780",
         state:"online",
         devices:
         [
             {
                 type:"D",
                 titles:
                 [
                     {
                         id:"12341235",
                         name:"Contoso Waypoint",
                         lastModified:"2012-09-17T07:15:23.4930000",
                         placement;"full",
                         state:"active",
                         activity:
                         {
                             richPresence:"Viewing stats"
                         },
                     }
                 ]
             }
         ]
     },
     {
         xuid:"0123456781",
         state:"online",
         devices:
         [
             {
                 type:"Win8",
                 titles:
                 [
                     {
                         id:"23452345",
                         name:"Contoso Gamehelp",
                         state:"active",
                         timestamp:"2012-09-17T07:15:23.4930000",
                         activity:
                         {
                             richPresence:"Viewing help"
                         }
                     }
                 ]
             }
         ]
     }
 ]
         
```

   
<a id="ID4EQLAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ESLAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/xuid({xuid})/groups/{moniker}](uri-usersxuidgroupsmoniker.md)

   
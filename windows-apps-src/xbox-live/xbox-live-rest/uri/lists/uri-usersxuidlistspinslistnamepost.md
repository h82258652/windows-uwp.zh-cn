---
title: POST (/users/xuid(xuid)/lists/PINS/{listname})
assetID: 813c0bd2-aca9-a1f6-9e81-a84a4c897b1e
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnamepost.html
author: KevinAsgari
description: " POST (/users/xuid(xuid)/lists/PINS/{listname})"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 743573b5fec1aa0f90f8d015d65ce49d02b34db0
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6204255"
---
# <a name="post-usersxuidxuidlistspinslistname"></a>POST (/users/xuid(xuid)/lists/PINS/{listname})
项目插入到列表中基于查询字符串参数**insertIndex**的索引。 这些 Uri 的域是`eplists.xboxlive.com`。
 
  * [备注](#ID4EY)
  * [URI 参数](#ID4ETB)
  * [查询字符串参数](#ID4E5B)
  * [授权](#ID4EZC)
  * [HTTP 状态代码](#ID4EGD)
  * [需的请求标头](#ID4EEAAC)
  * [示例请求](#ID4E1BAC)
  * [响应正文](#ID4EPCAC)
 
<a id="ID4EY"></a>

 
## <a name="remarks"></a>备注
 
此调用将插入到在基于查询字符串参数**insertIndex** （默认值为 0 或列表的开头） 的索引列表项。 请求正文中的所有项目将在该点都插入到列表中。 如果**insertIndex**大于现有列表中的项目数，将在结束时插入新项。
 
要插入的项必须具有必填的字段中的功能规范; 指示否则，将返回 HTTP 400。 同样，如果插入的结果将超过最大大小 （每个列表类型定义） 的列表将返回 HTTP 400 然后将插入任何操作。
 
如果该项是** 插入开头或结尾的列表中，则**如果-比赛： versionNumber**所需请求中包括的标头。 标头是可选的如果插入的开头或结尾。 如果存在，该标头将经过无论插入的位置。 在标头中**VersionNumber**是当前版本号的列表。 如果它不包含并是必需的或不匹配当前的列表版本编号，然后 HTTP 412 （前置条件失败） 将返回状态代码，并且该响应正文将包含最新的元数据的列表，其中包括的当前版本号。 这是为了防止从不同的客户端上彼此造成破坏的更新。
 
请注意，此调用不可幂等;重复的调用使用相同的数据可能会导致多个插入。 但是，由于没有列表目前支持重复，重复的调用将可能导致 HTTP 400 代码返回。
  
<a id="ID4ETB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| xuid| 字符串| Xbox 用户 ID (XUID)。| 
| listtype| 字符串| 列表 （如何使用和其工作原理） 的类型。 始终"固定"对于这些相关的方法。| 
| listname| 字符串| 列表的名称 （给定 listtype 执行哪些列表）。 始终为"XBLPins"中的 Pin 的项。| 
  
<a id="ID4E5B"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | 
| insertIndex| 字符串| 可选。 定义插入项目的位置。 受支持的值： 0，正整数和"结束"。 任何大于列表项目数的索引值将列表中，在底部添加新项，并且不会将"空白"插入列表中。 默认值： 0。| 
  
<a id="ID4EZC"></a>

 
## <a name="authorization"></a>授权
 
此调用在**授权**标头中预期的 XSTS SAML 令牌。 Xuid 声明必须存在于该 SAML 标记来标识调用方。 此值用于确定调用方是否有权访问问题的数据列表。 列表本身的 Xuid 也将标识，并将包含在列表的 URI。 使用此，我们将来可能支持共享访问列表，但的不是一项功能这一次。 目前，所有用户访问的列表都将自己并且没有共享的访问。 因此在 URI 中的 Xuid 必须匹配 SAML 声明令牌中的 Xuid。 

> [!NOTE] 
> 目前支持 XBL 身份验证 2.0 和 3.0 令牌。 


  
<a id="ID4EGD"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
此部分中使用此方法对此资源进行的请求的响应，该服务返回的状态代码之一。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| “确定”| 请求已成功完成。 响应正文应包含所请求的资源 （用于获取）。 POST 和 PUT 请求将接收最新列表元数据 （列表版本、 计数等）。| 
| 201| 已创建| 已创建了新的列表。 这是初始插入到列表返回内容。 该响应包括在列表上保持最新的元数据和位置标头包含列表的 URI。| 
| 304| 未修改| 返回上获取。 指示客户端列表的最新版本。 该服务的列表版本到<b>If-match</b>标头中的值进行比较。 如果相等，然后 304 获取不返回任何数据。| 
| 400| 错误请求| 请求格式不正确。 可能的值无效或 URI 或查询字符串参数类型。 数据值或请求指示该 API 的丢失或无效版本或也可能缺少所需的参数的结果。 请参阅<b>X XBL 合约版本</b>标头。| 
| 401| 未授权| 请求要求用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 找不到| 调用方没有访问资源。 这表示已创建了没有此类的列表。| 
| 412| 前置条件失败| 指示已发生更改的列表版本，并修改请求失败。 修改请求是插入、 更新或删除。 该服务将检查列表版本的<b>If-match</b>标头文件。 如果它不匹配当前版本的列表，然后操作将失败，这会返回当前列表元数据 （其中包括当前版本） 以及。| 
| 500| 内部服务器错误| 该服务拒绝由于服务器端错误的请求。| 
| 501| 未实现| 调用方请求尚未实现在服务器的 URI。 （当前仅时请求使用专为不是白名单列表名称。）| 
| 503| 服务不可用| 服务器拒绝请求，通常由于过多的负载。 延迟后 (请参阅<b>retry-after</b>标头)，可以重试请求。| 
  
<a id="ID4EEAAC"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标题| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 包含用于身份验证和授权请求的 STS 标记。 必须从 XSTS 服务访问令牌并包括为声明之一的 XUID。 | 
| X XBL 协定版本| 指定哪些 API 版本正在请求 （正整数）。 引脚支持版本 2。 如果此标头丢失或不受支持值，则服务将返回 400 – 错误请求与"不受支持或缺少合约版本标头"中的状态说明。| 
| Content-Type| 指定是否内容的请求/响应正文将包含在 json 或 xml。 受支持的值是"application/json"和"应用程序/xml"| 
| If-Match| 进行修改请求时，此标头必须包含列表的当前版本号。 修改请求使用 PUT、 POST，或删除动词命令。 如果"If-match"标头中的版本不匹配当前版本的列表，该请求将拒绝与 HTTP 412 – 前置条件失败返回代码。 （可选）可以也可用于获取，如果存在并传入的版本匹配当前的列表版本，然后无列表数据和 HTTP 304 – 将返回不修改代码。 | 
  
<a id="ID4E1BAC"></a>

 
## <a name="sample-request"></a>示例请求
 
**ContentType**、 **ItemId**或**ProviderId**、**提供商**和**区域设置**是必需的。
 

```cpp
{"Items":
  [{
    "ContentType": "Movie",
    "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
    "ProviderId": "",
    "Provider": "",
    "ImageUrl": "http://www.bing.com/thumb/get?bid=Gw%2fsjCGSS4kAAQ584x800&bn=SANGAM&fbid=7wIR63+Clmj+0A&fbn=CC", 
    "AltImageUrl": null, 
    "Title": "The Dark Knight", 
    "SubTitle": null, 
    "Locale": "en-us",
    "DeviceType": "XboxOne"
  }]}
      
```

  
<a id="ID4EPCAC"></a>

 
## <a name="response-body"></a>响应正文
 
<a id="ID4EVCAC"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
{
  "ListVersion":  1,
  "ListCount":  1,
  "MaxListSize": 200,
  "AllowDuplicates": "true",
  "AccessSetting":  "OwnerOnly"
}        
         
```

   
<a id="ID4E6CAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EBDAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

   
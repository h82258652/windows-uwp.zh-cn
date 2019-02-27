---
title: PUT (/users/xuid(xuid)/lists/PINS/{listname})
assetID: f7964d2e-a8c8-2caa-ca97-e0d76ef586f4
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameput.html
description: " PUT (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 35527df28c2ca482d0c5ae2fe25637b3bc29f6ca
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116072"
---
# <a name="put-usersxuidxuidlistspinslistname"></a>PUT (/users/xuid(xuid)/lists/PINS/{listname})
更新根据指定的请求正文中的每个项目的索引列表中的项。 这些 Uri 的域是`eplists.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4E1B)
  * [授权](#ID4EFC)
  * [HTTP 状态代码](#ID4ESC)
  * [需的请求标头](#ID4EPH)
  * [请求正文](#ID4EGBAC)
  * [响应正文](#ID4EWBAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
此调用将更新根据指定的请求正文中的每个项目的索引列表中的项。 此调用不会将项目插入到列表中，并且如果项不存在指定索引处，通过然后，调用将返回 400 错误请求状态。 可以在单个调用中，更新多个项目，但所有必须存在当前列表中。 也就是说，所有获取更新，或获取更新的任何。
 
此调用将允许该项目以进行更新，以便指定的**itemId**而不是**索引**。 若要执行此操作，只需发送给服务的**IndexedItems**结构中的索引中使用"-1"。 显然在此情况下， **itemId**不能更改一部分的更新，以便它仅适用于对其他元数据字段的更改。 **提供程序**/**providerId**组合可用于而不是**itemId**标识该项目。 在内部，该服务搜索这些项目和找出更新的正确索引的列表。 如果无法找到的项将返回 400 错误请求状态，并将更新任何项。
 
此调用需要**如果-匹配： versionNumber**标头以包括在请求中，如果使用的索引标识的项。 如果使用项目 Id，以标识项目 （并且该列表不允许重复项），然后**If-match**标头是可选的。 如果存在，将始终验证**If-match**标头。 在标头， **versionNumber**是列表的当前版本号。 如果它不包含 （和所需），或执行不匹配将返回当前的列表版本编号，然后 HTTP 412 前置条件失败状态代码和响应正文将包含最新的元数据的列表，包括当前版本号。 这是为了防止从不同的客户端上彼此造成破坏的更新。
  
<a id="ID4E1B"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| xuid| 字符串| Xbox 用户 ID (XUID)。| 
| listtype| 字符串| 列表 （如何使用和其工作原理） 的类型。 始终"固定"对于这些相关的方法。| 
| listname| 字符串| 列表的名称 （给定 listtype 执行哪些列表）。 始终为"XBLPins"中的 Pin 的项目。| 
  
<a id="ID4EFC"></a>

 
## <a name="authorization"></a>授权
 
此调用在**授权**标头中预期的 XSTS SAML 令牌。 Xuid 声明必须存在于该 SAML 令牌来标识调用方。 使用此值来确定调用方是否有问题的列表数据的访问权限。 列表本身的 Xuid 也将标识和将包含在列表的 URI。 使用此，我们将来可能支持共享访问列表，但的不是一项功能这一次。 目前，所有用户访问的列表都将自己并且没有共享的访问。 因此在 URI 中的 Xuid 必须匹配 SAML 声明令牌中的 Xuid。 

> [!NOTE] 
> 目前支持 XBL 身份验证 2.0 和 3.0 令牌。 


  
<a id="ID4ESC"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
该服务返回的状态代码之一此部分中使用此方法对此资源进行的请求的响应。 使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 说明| 
| --- | --- | --- | --- | --- | --- | --- | 
| 200| “确定”| 请求已成功完成。 响应正文应包含所请求的资源 （用于获取）。 POST 和 PUT 请求将接收最新列表元数据 （列表版本、 计数等）。| 
| 201| 已创建| 已创建了新的列表。 这被返回上初始插入到列表。 该响应包括在列表上保持最新的元数据和位置标头包含列表的 URI。| 
| 304| 未修改| 返回上获取。 指示客户端列表的最新版本。 该服务的列表版本到<b>If-match</b>标头中的值进行比较。 如果它们相等，然后 304 获取不返回任何数据。| 
| 400| 错误请求| 请求格式不正确。 可能的值无效或 URI 或查询字符串参数类型。 也可以是缺少所需的参数的结果或数据值或请求指出 API 丢失或无效版本。 请参阅<b>X XBL 合约版本</b>标头。| 
| 401| 未授权| 请求要求用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 未找到| 调用方没有访问资源。 这表示已创建了没有此类列表。| 
| 412| 前置条件失败| 指示已更改的列表版本，并修改请求失败。 修改请求是插入、 更新或删除。 该服务将检查列表版本的<b>If-match</b>标头文件。 如果不匹配当前版本的列表，然后操作将失败，这会返回当前列表元数据 （其中包括当前版本） 以及。| 
| 500| 内部服务器错误| 该服务拒绝由于服务器端错误请求。| 
| 501| 未实现| 调用方请求尚未实现在服务器的 URI。 （当前仅时请求使用专为不是列入白名单列表名称。）| 
| 503| 服务不可用| 服务器拒绝请求，通常由于过多的负载。 延迟后 (请参阅<b>retry-after</b>标头)，可以重试请求。| 
  
<a id="ID4EPH"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标题| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 包含用于身份验证和授权请求的 STS 标记。 必须从 XSTS 服务访问令牌并作为一个声明包含 XUID。 | 
| X XBL 协定版本| 指定哪些 API 版本被请求 （正整数）。 引脚支持版本 2。 如果此标头丢失或不受支持值，则服务将返回 400 – 错误请求与"不受支持或缺少合约版本标头"中的状态说明。| 
| Content-Type| 指定内容的请求/响应正文将包含在 json 或 xml。 受支持的值是"application/json"和"应用程序/xml"| 
| If-Match| 进行修改请求时，此标头必须包含列表的当前版本号。 修改请求使用 PUT、 POST，或删除动词命令。 如果"If-match"标头中的版本不匹配当前版本的列表，将拒绝请求，并 HTTP 412 – 前置条件失败返回代码。 （可选）此外可为获取，如果存在和传入的版本匹配当前的列表版本则没有列表数据和 HTTP 304 – 将返回不修改代码。 | 
  
<a id="ID4EGBAC"></a>

 
## <a name="request-body"></a>请求正文
 
<a id="ID4EMBAC"></a>

 
### <a name="sample-request"></a>示例请求
 

```cpp
{"IndexedItems":
 [{ "Index": 0, 
     "Item": 
     {
    "ContentType": "Movie",
    "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
    "ProviderId": null,
    "Provider": null,
    "ImageUrl": "https://www.bing.com/thumb/...",
    "Title": "The Dark Knight",
    "SubTitle":null, 
    "Locale": "en-us",
    "DeviceType": "XboxOne"
}
}]}      
      
```

   
<a id="ID4EWBAC"></a>

 
## <a name="response-body"></a>响应正文
 
<a id="ID4E3BAC"></a>

 
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

   
<a id="ID4EGCAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EICAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

   
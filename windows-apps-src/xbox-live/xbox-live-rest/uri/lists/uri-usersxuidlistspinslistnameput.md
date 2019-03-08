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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622922"
---
# <a name="put-usersxuidxuidlistspinslistname"></a>PUT (/users/xuid(xuid)/lists/PINS/{listname})
更新列表根据指定的请求正文中的每个项的索引中的项。 这些 Uri 的域是`eplists.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4E1B)
  * [Authorization](#ID4EFC)
  * [HTTP 状态代码](#ID4ESC)
  * [所需的请求标头](#ID4EPH)
  * [请求正文](#ID4EGBAC)
  * [响应正文](#ID4EWBAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
此调用将更新列表根据指定的请求正文中的每个项的索引中的项。 此调用会将项插入列表中，并且如果项不存在的指定索引处，通过然后调用将返回 400 错误的请求状态。 可以在单个调用中，更新多个项，但所有必须存在于当前列表。 也就是说，所有更新或无更新。
 
此调用将允许通过指定要更新的项**itemId**而不是**索引**。 若要执行此操作，只需使用"-1"中索引**IndexedItems**发送到服务的结构。 在此情况下，显然**itemId**无法更改作为更新的一部分，因此，它将仅适用于对其他元数据字段的更改。 **提供程序**/**providerId**而不是可以使用组合**itemId**来标识的项。 在内部，这些项和找出正确的索引，以更新列表中搜索服务。 如果无法找到的项将返回 400 错误的请求状态，并将更新任何项。
 
此调用需要**如果-匹配： versionNumber**标头要包含在请求中，如果使用索引来标识的项。 如果使用项 Id 标识的项目 （和该列表不允许重复项），则**If-match**是可选的标头。 如果存在，请**If-match**标头将始终进行验证。 在标题中， **versionNumber**是列表的当前版本号。 如果它不是包含 （以及所需），或者不匹配项将返回当前列表版本编号，然后将 HTTP 412 不满足前提条件失败状态代码和响应正文将包含列表的最新的元数据，包括当前版本号。 这是为了防止出现更新来自不同客户端上另一个中造成破坏。
  
<a id="ID4E1B"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| xuid| 字符串| Xbox 用户 ID (XUID)。| 
| listtype| 字符串| 列表 （如何使用和处理方式） 的类型。 始终"固定"这些相关方法。| 
| listname| 字符串| 列表的名称 （若要对给定 listtype 的列表）。 始终为"XBLPins"Pin 中的项。| 
  
<a id="ID4EFC"></a>

 
## <a name="authorization"></a>授权
 
此调用需要在 XSTS SAML 令牌**授权**标头。 Xuid 声明必须存在于该 SAML 令牌，以确定调用方。 此值用于确定调用方是否有问题的列表数据的访问权限。 列表本身 Xuid 也将标识，将包含在列表的 URI。 使用此，我们在将来可能支持共享访问列表，但的不是一项功能这一次。 目前，在用户访问的所有列表将都为其自身并没有共享访问。 因此在 URI 中 Xuid 必须匹配 Xuid SAML 声明令牌中。 

> [!NOTE] 
> 目前支持 XBL 身份验证 2.0 和 3.0 令牌。 


  
<a id="ID4ESC"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | 
| 200| 确定| 请求已成功完成。 响应正文应包含请求的资源 （针对 GET)。 POST 和 PUT 请求会收到最新列表元数据 （列表版本、 计数等）。| 
| 201| 创建时间| 已创建一个新列表。 返回该信息是初始插入到列表。 响应包括列表中最新的元数据和 location 标头包含列表的 URI。| 
| 304| 未修改| 返回上获取。 指示客户端具有最新版本的列表。 该服务中的值进行比较<b>If-match</b>列表版本标头。 如果它们相等，则不包含数据，获取返回 304。| 
| 400| 无效的请求| 请求的格式不正确。 可能是无效的值或 URI 或查询字符串参数类型。 也可以是缺少所需的参数的结果或数据值或请求指示缺失或无效的 API 版本。 请参阅<b>X XBL 协定版本</b>标头。| 
| 401| 未经授权| 请求需要用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 未找到| 调用方没有对资源的访问。 这指示已创建了任何此类列表。| 
| 412| 前置条件失败| 指示列表的版本已更改的修改请求失败。 修改请求是插入、 更新或删除。 服务检查<b>If-match</b>列表版本标头。 如果它不匹配当前版本的列表，则操作将失败，这将与当前列表元数据 （其中包括最新版本） 一起返回。| 
| 500| 内部服务器错误| 该服务将拒绝请求，因为服务器端错误。| 
| 501| 未实现| 调用方请求未在服务器实现的 URI。 （当前仅在请求时使用专为不是已列入允许列表的列表名称。）| 
| 503| 服务不可用| 服务器将拒绝该请求，通常因负载过大。 在延迟后 (请参阅<b>稍后重试</b>标头)，可以重试请求。| 
  
<a id="ID4EPH"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 包含用于进行身份验证和授权请求的 STS 令牌。 必须是从 XSTS 服务的令牌，并包含 XUID 作为声明之一。 | 
| X-XBL-Contract-Version| 指定的 API 版本正在请求 （正整数）。 Pin 支持第 2 版。 如果此标头缺失或值不支持该服务将返回 400 – 错误的请求"不受支持或缺少协定版本标头"中的状态说明。| 
| 内容类型| 指定请求/响应正文的内容将是 json 或 xml 中。 支持的值为"application/json"和"application/xml"| 
| If-Match| 在修改请求时，此标头必须包含列表的当前版本号。 修改请求使用 PUT，POST，或删除谓词。 如果"If-match"标头中的版本与当前版本的列表不匹配，将拒绝该请求，并出现 HTTP 412 – 前置条件失败返回代码。 （可选）此外可用于获取，如果存在且传入的版本与当前列表版本然后任何列表数据，并且 HTTP 304 相匹配，将返回未修改代码。 | 
  
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

   
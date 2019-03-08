---
title: POST /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
assetID: df61be42-c229-7408-5e4c-dbf4ae95b52b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameindexpost.html
description: " POST /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7711beee6551c40afe1afcb031278484a3dc5820
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627462"
---
# <a name="post-usersxuidxuidlistspinslistnameindexindexinsertindexinsertindex"></a>POST /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
将在列表中的项移至列表中的其他位置。 这些 Uri 的域是`eplists.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4EEB)
  * [查询字符串参数](#ID4EWC)
  * [请求正文](#ID4EVD)
  * [HTTP 状态代码](#ID4EEE)
  * [所需的请求标头](#ID4E1BAC)
  * [响应正文](#ID4EQDAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注 
 
此调用，以便轻松地将项移动到不同的索引中的单个操作的列表中的客户端。 只有一项可能一次移动。 如果要移动的项的索引不存在，则将返回 HTTP 400。 插入点的索引遵循标准插入相同的规则。 它将默认为 0 – 列表的开头和任意数量的列表中的项目数大于重新插入列表的末尾处的项。 同样可以通过将传递为 insertIndex"end"指示列表的末尾。 
 
此调用还可标识要由 itemId （或提供程序/providerId 组合） 进行移动的项。 在这种情况下，必须在请求正文中传递 itemId，并且它必须匹配列表中的现有项。 如果未显示，返回 HTTP 400 错误将返回与 IndexOutOfRange 说明; 中有关调用此特殊版本，使用"-1"的项的索引作为要移动。 
 
此调用需要"如果-匹配： versionNumber"标头以包含在请求中如果指定项的索引。 如果使用 itemId 以指示要移动的项，然后"If-match"标头是可选的。 如果存在，将始终验证"If-match"标头。 在"If-match"标头，versionNumber 是列表的当前版本号。 如果它不是包含 （以及所需），或不匹配当前将返回列表版本号，则 HTTP 412 – 前置条件失败的状态代码和响应正文将包含最新的元数据的列表，其中包括当前的版本号. 这样做是为了防止出现更新来自不同客户端相互中造成破坏。 
  
<a id="ID4EEB"></a>

 
## <a name="uri-parameters"></a>URI 参数 
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| XUID| 字符串| 用户的 XUID。| 
| listname| 字符串| 要操作的列表的名称。| 
| index| 字符串| 指定要移动的项的当前索引。 如果索引值为零个或一个正整数，这是指当前项的索引，并且在调用的请求正文应为空。 但是，如果索引值为"-1"，必须由 ItemId 或提供程序/ProviderID 调用的请求正文中指定要移动的项。| 
  
<a id="ID4EWC"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数 
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | 
| insertIndex| 字符串| 指定要插入项的列表位置。 允许的值为零的正整数和"end"。 "end"将项放置在当前列表的末尾。 如果指定的值不在列表的末尾，在列表末尾插入项。 | 
  
<a id="ID4EVD"></a>

 
## <a name="request-body"></a>请求正文 
 
指定项时才发送请求正文，移动 itemId 或提供程序/ProviderId。
 
<a id="ID4E6D"></a>

  
示例请求 

```cpp
{
  "Item":
  {
    "ItemId":  "ed591a0c-dde3-563f-99af-530278a3c402",
    "ProviderId":  null,
    "Provider": null
  }
}
    
```

  
<a id="ID4EEE"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码 
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 确定 | 请求已成功完成。 响应正文应包含请求的资源 （针对 GET)。 POST 和 PUT 请求会收到最新列表元数据 （列表版本、 计数等）。| 
| 201| 创建时间 | 已创建一个新列表。 返回该信息是初始插入到列表。 响应包括列表中最新的元数据和 location 标头包含列表的 URI。| 
| 304| 未修改| 返回上获取。 指示客户端具有最新版本的列表。 该服务中的值进行比较<b>If-match</b>列表版本标头。 如果它们相等，则不包含数据，获取返回 304。 | 
| 400| 无效的请求 | 请求的格式不正确。 可能是无效的值或 URI 或查询字符串参数类型。 也可以是缺少所需的参数的结果或数据值或请求指示缺失或无效的 API 版本。 请参阅<b>X XBL 协定版本</b>标头。 | 
| 401| 未经授权 | 请求需要用户身份验证。| 
| 403| 已禁止 | 为用户或服务不允许该请求。| 
| 404| 未找到 | 调用方没有对资源的访问。 这指示已创建了任何此类列表。| 
| 412| 前置条件失败 | 指示列表的版本已更改的修改请求失败。 修改请求是插入、 更新或删除。 服务检查<b>If-match</b>列表版本标头。 如果它不匹配当前版本的列表，则操作将失败，这将与当前列表元数据 （其中包括最新版本） 一起返回。 | 
| 500| 内部服务器错误 | 该服务将拒绝请求，因为服务器端错误。| 
| 501| 未实现 | 调用方请求未在服务器实现的 URI。 （当前仅在请求时使用专为不是已列入允许列表的列表名称。）| 
| 503| 服务不可用 | 服务器将拒绝该请求，通常因负载过大。 在延迟后 (请参阅<b>稍后重试</b>标头)，可以重试请求。 | 
  
<a id="ID4E1BAC"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 包含用于进行身份验证和授权请求的 STS 令牌。 必须是从 XSTS 服务的令牌，并包含 XUID 作为声明之一。 | 
| X-XBL-Contract-Version| 指定的 API 版本正在请求 （正整数）。 Pin 支持第 2 版。 如果此标头缺失或值不支持该服务将返回 400 – 错误的请求"不受支持或缺少协定版本标头"中的状态说明。| 
| 内容类型| 指定请求/响应正文的内容将是 json 或 xml 中。 支持的值为"application/json"和"application/xml"| 
| If-Match| 在修改请求时，此标头必须包含列表的当前版本号。 修改请求使用 PUT，POST，或删除谓词。 如果"If-match"标头中的版本与当前版本的列表不匹配，将拒绝该请求，并出现 HTTP 412 – 前置条件失败返回代码。 （可选）此外可用于获取，如果存在且传入的版本与当前列表版本然后任何列表数据，并且 HTTP 304 相匹配，将返回未修改代码。 | 
  
<a id="ID4EQDAC"></a>

 
## <a name="response-body"></a>响应正文 
 
如果调用成功，则该服务返回的最新的元数据的列表。 
 
<a id="ID4E1DAC"></a>

 
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

   
<a id="ID4EIEAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EKEAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}](uri-usersxuidlistspinslistnameindex.md)

   
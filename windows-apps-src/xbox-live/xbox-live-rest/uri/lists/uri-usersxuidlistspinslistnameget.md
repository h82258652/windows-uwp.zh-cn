---
title: GET (/users/xuid(xuid)/lists/PINS/{listname})
assetID: a63f595a-61dd-5885-c405-9833230abb94
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameget.html
description: " GET (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a31e6a6b541276d3191ba5d40767a1929c70168
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641622"
---
# <a name="get-usersxuidxuidlistspinslistname"></a>GET (/users/xuid(xuid)/lists/PINS/{listname})
返回列表的内容。 这些 Uri 的域是`eplists.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4EIB)
  * [查询字符串参数](#ID4ETB)
  * [Authorization](#ID4ESD)
  * [所需的请求标头](#ID4E6D)
  * [请求正文](#ID4EVF)
  * [HTTP 状态代码](#ID4EAG)
  * [响应正文](#ID4E5CAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
**ListCount**字段中返回的数据指示在维护服务-这种情况下的总列表中有多少项，所以可以用来确定其中是列表的末尾，，这可能是从多少不同数量请求未返回特定项。
 
如果列表尚不存在，则结果将不包含任何列表项， **listCount**将为零并**listVersion**将为零。
  
<a id="ID4EIB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| xuid| 字符串| Xbox 用户 ID (XUID)。| 
| listtype| 字符串| 列表 （如何使用和处理方式） 的类型。 始终"固定"这些相关方法。| 
| listname| 字符串| 列表的名称 （若要对给定 listtype 的列表）。 始终为"XBLPins"Pin 中的项。| 
  
<a id="ID4ETB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | 
| skipItems| 32 位有符号的整数| 可选。 若要在返回结果之前枚举中跳过的项目数。 默认值：0.| 
| maxItems| 32 位有符号的整数| 可选。 要返回的项数的最大数目。 如果在请求中指定无最大值，默认值为 25 个项目。 该服务将不会施加最多此值;如果值大于列表中的项的数目，则会出现任何错误返回的所有项。| 
| filterItemId| 字符串| 可选。 指定要在列表中找到的项。 返回列表中项的所有实例。 允许客户端以快速确定以及其中某项是在列表中。 方便的大型列表以确定而无需循环访问整个列表的项的所有实例。 默认值： null。| 
| filterContentType| 字符串| 可选。 指定要返回的内容类型的逗号分隔列表 （不会返回类型不在列表中）。 用于从列表中仅获取某些内容类型。 以逗号分隔列表的内容类型用于此筛选器。 （多个内容类型可以查询一次调用。）支持内容类型包括所有定义娱乐发现服务 (EDS) 的媒体类型。 默认值： null （所有内容类型）。| 
| filterDeviceType| 字符串| 可选。 指定要返回的设备类型的逗号分隔列表 （不会返回类型不在列表中）。 筛选返回的集，以仅返回已插入从一组特定的设备类型的项。 设备类型的逗号分隔列表用于此筛选器 （一次调用，就可以查询多个设备类型）。 可能值：XboxOne、 MCapensis、 WindowsPhone、 WindowsPhone7、 Web、 PC、 MoLive。 默认值： null （所有内容类型）。| 
  
<a id="ID4ESD"></a>

 
## <a name="authorization"></a>授权
 
此调用需要在 XSTS SAML 令牌**授权**标头。 Xuid 声明必须存在于该 SAML 令牌，以确定调用方。 此值用于确定调用方是否有问题的列表数据的访问权限。 列表本身 Xuid 也将标识，将包含在列表的 URI。 使用此，我们在将来可能支持共享访问列表，但的不是一项功能这一次。 目前，在用户访问的所有列表将都为其自身并没有共享访问。 因此在 URI 中 Xuid 必须匹配 Xuid SAML 声明令牌中。 

> [!NOTE] 
> 目前支持 XBL 身份验证 2.0 和 3.0 令牌。 


  
<a id="ID4E6D"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 包含用于进行身份验证和授权请求的 STS 令牌。 必须是从 XSTS 服务的令牌，并包含 XUID 作为声明之一。 | 
| X-XBL-Contract-Version| 指定的 API 版本正在请求 （正整数）。 Pin 支持第 2 版。 如果此标头缺失或值不支持该服务将返回 400 – 错误的请求"不受支持或缺少协定版本标头"中的状态说明。| 
| 内容类型| 指定请求/响应正文的内容将是 json 或 xml 中。 支持的值为"application/json"和"application/xml"| 
| If-Match| 在修改请求时，此标头必须包含列表的当前版本号。 修改请求使用 PUT，POST，或删除谓词。 如果"If-match"标头中的版本与当前版本的列表不匹配，将拒绝该请求，并出现 HTTP 412 – 前置条件失败返回代码。 （可选）此外可用于获取，如果存在且传入的版本与当前列表版本然后任何列表数据，并且 HTTP 304 相匹配，将返回未修改代码。 | 
  
<a id="ID4EVF"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送的任何对象。
  
<a id="ID4EAG"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4E5CAC"></a>

 
## <a name="response-body"></a>响应正文
 
<a id="ID4EEDAC"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
{ 
"ListMetadata":
  {"ListVersion": 12,
   "ListCount": 6,
   "MaxListSize": 200,
   "AccessSetting": "OwnerOnly",
   "AllowDuplicates": true
  },
"ListItems":
  [{ 
   "Index": 0,
   "DateAdded": "\/Date(1198908717056)/",
   "DateModified": "\/Date(1198908717056)/",
   "HydrationResult": "Indeterminate",
   "HydratedItem": null

   "Item":
   {
     "ContentType": "Movie",
     "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
     "ProviderId": null,
     "Provider": null,
     "ImageUrl": "https://www.bing.com/thumb/get?bid=Gw%2fsjCGSS4kAAQ584x800&bn=SANGAM&fbid=7wIR63+Clmj+0A&fbn=CC",
     "Title": "The Dark Knight",
     "SubTitle": null,
     "Locale": "en-us",
     "AltImageUrl": null,
     "DeviceType": "XboxOne"
    }
  }]
}
         
```

   
<a id="ID4EODAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EQDAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

  
<a id="ID4E1DAC"></a>

 
##### <a name="further-information"></a>详细信息 

[Marketplace Uri](../marketplace/atoc-reference-marketplace.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   
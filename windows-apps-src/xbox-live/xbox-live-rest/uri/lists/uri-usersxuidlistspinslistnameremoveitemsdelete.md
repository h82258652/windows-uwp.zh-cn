---
title: DELETE /users/xuid(xuid)/lists/PINS/{listname}/RemoveItems
assetID: ad049340-f752-e05e-8b34-62adb8e4771f
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameremoveitemsdelete.html
author: KevinAsgari
description: " DELETE /users/xuid(xuid)/lists/PINS/{listname}/RemoveItems"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: baa9cb22c89ec749d93978e115cd84f96fef9f91
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4314196"
---
# <a name="delete-usersxuidxuidlistspinslistnameremoveitems"></a>DELETE /users/xuid(xuid)/lists/PINS/{listname}/RemoveItems
按索引从列表中删除项目。 这些 Uri 的域是`eplists.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4ECB)
  * [查询字符串参数](#ID4ELC)
  * [请求正文](#ID4END)
  * [HTTP 状态代码](#ID4EYD)
  * [需的请求标头](#ID4EOBAC)
  * [响应正文](#ID4EEDAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注 
 
从列表中删除项目。 要删除项由其列表中的索引和中查询字符串参数"索引"标识。 索引列表必须用逗号分隔，并且可以将 100 个索引传递单次调用。 但是，如果没有索引传递 （空查询字符串参数） 则内容整个列表将被删除 （列表将保持但为空，版本号将继续增加）。 一旦项都被删除，列表"压缩"以便没有漏洞剩顺序的索引。 
 
此调用需要"如果-比赛： versionNumber"标头包含在请求中，versionNumber 所在的文件的当前版本号。 如果它不是包含在内，或者不匹配当前将返回号，则 HTTP 412 – 前置条件失败的状态代码的列表版本，并且该响应正文将包含最新的元数据的列表，其中包括当前版本号。 这是为了防止从不同的客户端上彼此造成破坏的更新。 
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI 参数 
 
| 参数| 类型| 描述| 
| --- | --- | --- | 
| XUID| 字符串| 用户的 XUID。| 
| listname| 字符串| 列表来操作的名称。| 
  
<a id="ID4ELC"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数 
 
| 参数| 类型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 索引| 字符串| 零或正整数。 数字不需要是连续的。 例如，查询参数索引 = 1，8 将删除该项目的索引 1 和 8。 <b>如果未指定索引，将删除整个列表。</b>| 
  
<a id="ID4END"></a>

 
## <a name="request-body"></a>请求正文 
 
请求正文必须为空进行此调用。
  
<a id="ID4EYD"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码 
 
该服务返回的状态代码之一此部分中使用此方法对此资源所做的请求的响应。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| “确定” | 已成功完成请求。 响应正文应包含所请求的资源 （GET)。 POST 和 PUT 请求将接收最新列表元数据 （列表版本、 计数等）。| 
| 201| 已创建 | 已创建了新的列表。 这被返回初始插入到列表。 该响应包括在列表上保持最新的元数据和位置标头包含列表的 URI。| 
| 304| 未修改| 返回上获取。 指示客户端列表的最新版本。 该服务的列表版本到<b>If-match</b>标头中的值进行比较。 如果它们是否相等，然后 304 获取不返回任何数据。 | 
| 400| 错误请求 | 请求格式不正确。 可能是无效的值或 URI 或查询字符串参数类型。 数据值或请求指示丢失或无效的 API 版本或也可能缺少所需的参数的结果。 请参阅<b>X XBL 合约版本</b>标头。 | 
| 401| 未授权 | 请求要求用户身份验证。| 
| 403| 已禁止 | 为用户或服务不允许该请求。| 
| 404| 找不到 | 调用方没有访问资源。 这表示已创建了没有此类列表。| 
| 412| 前置条件失败 | 指示已更改的列表版本，并且修改请求失败。 修改请求是的 insert、 更新或删除。 该服务将检查列表版本的<b>If-match</b>标头文件。 如果它不匹配当前版本的列表，然后操作将失败，这会返回以及当前列表元数据 （其中包括当前版本）。 | 
| 500| 内部服务器错误 | 该服务拒绝由于服务器端错误请求。| 
| 501| 未实现 | 调用方请求尚未实现在服务器的 URI。 （当前仅时请求使用专为不是列入白名单列表名称。）| 
| 503| 服务不可用 | 服务器拒绝请求，通常由于过多的负载。 延迟后 (请参阅<b>retry-after</b>标头)，可以重试请求。 | 
  
<a id="ID4EOBAC"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标题| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 包含用于身份验证和授权请求的 STS 标记。 必须从 XSTS 服务访问令牌并包括为声明之一的 XUID。 | 
| X XBL 协定版本| 指定哪些 API 版本正在请求 （正整数）。 引脚支持版本 2。 如果此标头丢失或不受支持值，则服务将返回 400 – 错误请求与"不受支持或丢失合约版本标头"中的状态说明。 | 
| Content-Type| 指定的请求/响应正文的内容将位于 json 或 xml。 受支持的值为"应用程序/json"和"应用程序/xml"| 
| If-Match| 进行修改请求时，此标头必须包含列表的当前版本号。 修改请求使用 PUT、 POST，或删除动词命令。 如果"If-match"标头中的版本不匹配当前版本的列表，将拒绝请求，并 HTTP 412 – 前置条件失败返回代码。 （可选）此外可为获取，如果存在并传入的版本匹配当前的列表版本则没有列表数据和 HTTP 304 – 将返回不修改代码。 | 
  
<a id="ID4EEDAC"></a>

 
## <a name="response-body"></a>响应正文 
 
如果在调用成功，该服务返回的最新的元数据的列表。 
 
<a id="ID4EODAC"></a>

 
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

   
<a id="ID4E1DAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E3DAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[统一资源标识符 (URI) 参考](../atoc-xboxlivews-reference-uris.md)

   
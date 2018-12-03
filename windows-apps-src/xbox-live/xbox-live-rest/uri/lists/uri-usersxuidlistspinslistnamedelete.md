---
title: DELETE (/users/xuid(xuid)/lists/PINS/{listname})
assetID: b43e3faa-7791-8bcb-3aec-7bdad8ffbebf
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnamedelete.html
description: " DELETE (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: eed1d73917be450038fdf098b802d0d7c1c44a7b
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8341399"
---
# <a name="delete-usersxuidxuidlistspinslistname"></a>DELETE (/users/xuid(xuid)/lists/PINS/{listname})
从列表中删除项目。 这些 Uri 的域是`eplists.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4EIB)
  * [查询字符串参数](#ID4ETB)
  * [授权](#ID4ETC)
  * [需的请求标头](#ID4EAD)
  * [请求正文](#ID4EWE)
  * [HTTP 状态代码](#ID4EBF)
  * [响应正文](#ID4E6BAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
若要删除的项由其列表中的索引和中查询字符串参数**的索引**标识。 索引列表必须以逗号分隔，并且可以将 100 个索引传递单次调用。 但是，如果没有索引传递 （空的查询字符串参数） 则整个列表的内容将被删除 （空列表将保留，和版本号将继续增加）。 一旦项都被删除，列表"压缩"，以便任何洞将一直处于排序的索引。 因此，此调用不是幂等。
 
此调用需要**如果-比赛： versionNumber**标头包含在请求中， **versionNumber**所在的文件的当前版本号。 如果它不是包含在内，或者不匹配当前的列表版本编号，然后 HTTP 412 （前置条件失败） 将返回状态代码，并且该响应正文将包含最新的元数据的列表，其中包括当前版本号。 这是为了防止从不同的客户端上彼此造成破坏的更新。
  
<a id="ID4EIB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 描述| 
| --- | --- | --- | 
| xuid| 字符串| Xbox 用户 ID (XUID)。| 
| listtype| 字符串| 列表 （它如何使用和其工作原理） 的类型。 始终"固定"对于这些相关的方法。| 
| listname| 字符串| 列表的名称 （给定 listtype 执行哪些列表）。 始终为"XBLPins"中的 Pin 的项。| 
  
<a id="ID4ETB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 类型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 索引| 字符串| 可选。 指定要删除项目的位置。 受支持的值： 0，正整数和"结束"。 默认值： 0。| 
 
示例：**索引 = 1，8**删除项目在索引 1 和 8。 索引必须是唯一的。 如果提供了没有索引，将清除整个列表。
  
<a id="ID4ETC"></a>

 
## <a name="authorization"></a>授权
 
此调用在**授权**标头中预期的 XSTS SAML 令牌。 Xuid 声明必须存在于该 SAML 令牌来标识调用方。 此值用于确定调用方是否有权访问问题的数据列表。 列表本身的 Xuid 也将被标识和将包含在列表的 URI。 使用这种情况，我们将来可能支持共享访问列表，但这不是一项功能这一次。 目前，所有用户访问的列表都将自己，并且没有共享的访问。 因此在 URI 中的 Xuid 必须匹配 SAML 声明令牌中的 Xuid。 

> [!NOTE] 
> 目前支持 XBL 身份验证 2.0 和 3.0 令牌。 


  
<a id="ID4EAD"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标题| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 包含用于身份验证和授权请求的 STS 标记。 必须从服务 XSTS 令牌并包括为一个声明的 XUID。 | 
| X XBL 协定版本| 指定哪些 API 版本被请求 （正整数）。 引脚支持版本 2。 如果此标头丢失或不受支持值，则服务将返回 400 – 错误请求与"不受支持或丢失合约版本标头"中的状态说明。| 
| Content-Type| 指定内容的请求/响应正文将位于 json 或 xml。 受支持的值为"application/json"和"应用程序/xml"| 
| If-Match| 进行修改请求时，此标头必须包含列表的当前版本号。 修改请求使用 PUT、 POST，或删除动词命令。 如果"If-match"标头中的版本不匹配当前版本的列表，将拒绝请求，并 HTTP 412 – 前置条件失败，返回代码。 （可选）此外可为获取，如果存在并传入的版本匹配当前的列表版本则没有列表数据和 HTTP 304 – 将返回不修改代码。 | 
  
<a id="ID4EWE"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送任何对象。
  
<a id="ID4EBF"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
此部分中使用此方法对此资源所做的请求的响应，该服务返回一个状态代码。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| “确定”| 已成功完成请求。 响应正文应包含所请求的资源 （GET)。 POST 和 PUT 请求将接收最新列表元数据 （列表版本、 计数等）。| 
| 201| 已创建| 已创建了新的列表。 这被返回初始插入到列表。 该响应包括在列表上保持最新的元数据和位置标头包含列表的 URI。| 
| 304| 未修改| 返回上获取。 指示客户端列表的最新版本。 该服务将列表版本<b>If-match</b>标头中的值进行比较。 如果它们是否相等，然后 304 获取不返回任何数据。| 
| 400| 错误请求| 请求格式不正确。 可能的值无效或 URI 或查询字符串参数的类型。 数据值或请求指示 API 丢失或无效版本或也可能缺少所需的参数的结果。 请参阅<b>X XBL 合约版本</b>标头。| 
| 401| 未授权| 请求要求用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 找不到| 调用方没有访问资源。 这表示已创建了任何此类的列表。| 
| 412| 前置条件失败| 指示已更改的列表版本，并修改请求失败。 修改请求是插入、 更新或删除。 该服务将检查列表版本<b>If-match</b>标头。 如果它不匹配当前版本的列表，然后操作将失败，这会返回当前列表元数据 （其中包括当前版本） 以及。| 
| 500| 内部服务器错误| 该服务拒绝由于服务器端错误请求。| 
| 501| 未实现| 调用方请求尚未实现在服务器的 URI。 （当前仅时请求使用专为不是列入白名单列表名称。）| 
| 503| 服务不可用| 服务器拒绝请求，通常是由于过多的负载。 延迟后 (请参阅<b>retry-after</b>标头)，可以重试请求。| 
  
<a id="ID4E6BAC"></a>

 
## <a name="response-body"></a>响应正文
 
<a id="ID4EFCAC"></a>

 
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

   
<a id="ID4EPCAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ERCAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

   
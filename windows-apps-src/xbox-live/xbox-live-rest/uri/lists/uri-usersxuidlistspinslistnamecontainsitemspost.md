---
title: 发布/用户/xuid (xuid) / 列出了/PIN / {listname} / ContainsItems
assetID: 86ee6d1a-fb1f-b918-f605-a9b494c0e787
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnamecontainsitemspost.html
author: KevinAsgari
description: " 发布/用户/xuid (xuid) / 列出了/PIN / {listname} / ContainsItems"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d1bacb3cf6a97cf72b03df10096f3ebf779b045b
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3881110"
---
# <a name="post-usersxuidxuidlistspinslistnamecontainsitems"></a>发布/用户/xuid (xuid) / 列出了/PIN / {listname} / ContainsItems
确定是否列表而不检索整个列表包含一组项 （由 itemId 指定）。 这些 Uri 的域是`eplists.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4EAB)
  * [查询字符串参数](#ID4EJC)
  * [请求正文](#ID4EUC)
  * [HTTP 状态代码](#ID4E6C)
  * [需的请求标头](#ID4EVAAC)
  * [响应正文](#ID4ELCAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注 
 
此终结点允许调用方确定是否一个或多个项目存在于指定的列表中未实际获取列表和搜索本身。 项目集由 itemId （或提供程序/providerId 组合），返回的数据列表是否包含每个项目使用布尔值 true 或 false ndicating 传递的数据。 
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>URI 参数 
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| XUID| 字符串| 用户的 XUID。| 
| listname| 字符串| 列表来操作的名称。| 
  
<a id="ID4EJC"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数 
 
不支持查询参数。
  
<a id="ID4EUC"></a>

 
## <a name="request-body"></a>请求正文 
 

```cpp
{
  "Items":
  [{"ItemId":  "ed591a0c-dde3-563f-99af-530278a3c402",
    "ProviderId":  null,
    "Provider": null
  }]
}


    
```

  
<a id="ID4E6C"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码 
 
该服务将返回一个状态代码此部分中使用此方法对此资源进行的请求的响应。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 说明| 
| --- | --- | --- | --- | --- | --- | 
| 200| “确定” | 请求已成功完成。 响应正文应包含所请求的资源 （GET)。 POST 和 PUT 请求将接收最新列表元数据 （列表版本、 计数等）。| 
| 201| 已创建 | 已创建新的列表。 这被返回初始插入到列表。 该响应包括在列表上保持最新的元数据和位置标头包含列表的 URI。| 
| 304| 未修改| 返回上获取。 指示客户端具有最新版本的列表。 该服务将列表版本<b>If-match</b>标头中的值进行比较。 如果相等，然后 304 获取不返回任何数据。 | 
| 400| 错误请求 | 请求已格式不正确。 可能的值无效或 URI 或查询字符串参数类型。 也可以是缺少所需的参数的结果或数据值或请求 API 的丢失或无效的版本。 请参阅<b>X XBL 协定版本</b>标头。 | 
| 401| 未授权 | 请求要求用户身份验证。| 
| 403| 已禁止 | 为用户或服务不允许该请求。| 
| 404| 找不到 | 调用方没有访问资源。 这指示已创建了任何此类列表。| 
| 412| 前置条件失败 | 指示已更改的列表版本和修改请求失败。 修改请求是插入、 更新或删除。 服务会检查列表版本的<b>If-match</b>标头文件。 如果它不匹配当前版本的列表，然后操作将失败，这会返回以及当前列表元数据 （其中包括当前版本）。 | 
| 500| 内部服务器错误 | 该服务拒绝由于服务器端错误请求。| 
| 501| 未实现 | 调用方请求尚未实现在服务器的 URI。 （当前仅时请求使用由列表名称不是列入白名单。）| 
| 503| 服务不可用 | 服务器拒绝请求，通常由于过多的负载。 延迟后 (请参阅<b>retry-after</b>标头)，可以重试请求。 | 
  
<a id="ID4EVAAC"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标题| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 包含用于身份验证和授权请求的 STS 标记。 必须从 XSTS 服务令牌并包括为一个声明的 XUID。 | 
| X XBL 协定版本| 指定哪些 API 版本正在请求 （正整数）。 引脚支持版本 2。 如果此标头丢失或不受支持值，则服务将返回 400 – 错误请求与"不受支持或丢失合约版本标头"状态描述中。 | 
| Content-Type| 指定内容的请求/响应正文将包含在 json 或 xml。 受支持的值为"应用程序/json"和"应用程序/xml"| 
| If-Match| 进行修改请求时，此标头必须包含列表的当前版本号。 修改请求使用 PUT、 POST，或删除动词命令。 如果"If-match"标头中的版本不匹配当前版本的列表，将拒绝请求，并 HTTP 412 – 前置条件失败返回代码。 （可选）此外可为获取，如果存在并传入的版本匹配当前的列表版本，然后没有列表数据和 HTTP 304 – 将返回不修改代码。 | 
  
<a id="ID4ELCAC"></a>

 
## <a name="response-body"></a>响应正文 
 
如果在调用成功，将返回的项目列表，以及每个项目指定该项目是否在列表中或不布尔值。 
 
<a id="ID4EVCAC"></a>

 
### <a name="sample-response"></a>示例响应 
 

```cpp
{
  "ContainedItems":
  [{"Contained": "true",
    "Item":
   {"ItemId":  "ed591a0c-dde3-563f-99af-530278a3c402",
    "ProviderId": null,
    "Provider": null
   }
  }]
}


      
```

   
<a id="ID4EBDAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EDDAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[统一资源标识符 (URI) 引用](../atoc-xboxlivews-reference-uris.md)

   
---
title: GET (/sessions/{sessionId}/scids/{scid}/data/{path})
assetID: 007821b8-16f0-2fe1-5196-890743d77775
permalink: en-us/docs/xboxlive/rest/uri-sessionssessionidscidssciddatapath-get.html
description: " GET (/sessions/{sessionId}/scids/{scid}/data/{path})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 28a279347fa5a463c0d482a624af831c0cdb0fba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623712"
---
# <a name="get-sessionssessionidscidssciddatapath"></a>GET (/sessions/{sessionId}/scids/{scid}/data/{path})
列出指定路径处的文件信息。 这些 Uri 的域是`titlestorage.xboxlive.com`。
 
  * [URI 参数](#ID4EX)
  * [可选查询字符串参数](#ID4ECB)
  * [Authorization](#ID4EWC)
  * [所需的请求标头](#ID4EDD)
  * [请求正文](#ID4EME)
  * [HTTP 状态代码](#ID4EZE)
  * [响应正文](#ID4EUBAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| sessionId| 字符串| 若要查找的会话 ID。| 
| scid| GUID| 要查找服务配置的 ID。| 
| path| 字符串| 若要返回的数据项目的路径。 返回所有匹配的目录和子目录。 有效字符包括大写字母 (A-Z)、 小写字母 (a-z)、 数字 (0-9)、 下划线 (_) 和正斜杠 （/）。 可能为空。 最大长度为 256。| 
  
<a id="ID4ECB"></a>

 
## <a name="optional-query-string-parameters"></a>可选查询字符串参数 
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | 
| skipItems| int| 返回在集合中，例如，N + 1 开始的项跳过 N 项。| 
| ContinuationToken| 字符串| 返回从给定的继续标记处开始的项。 如果二者均提供继续标记参数优先于 skipItems。 换而言之，如果继续标记参数存在，则忽略 skipItems 参数。| 
| maxItems| int| 要从集合，可以结合 skipItems 和 continuationToken 要返回的项的范围中返回的项的最大数目。 如果 maxItems 不存在，并且可能会返回少于 maxItems，即使尚未返回最后一个结果页该服务可能会提供默认值。 | 
  
<a id="ID4EWC"></a>

 
## <a name="authorization"></a>授权 
 
该请求必须包含有效的 Xbox LIVE 授权标头。 如果调用方不允许访问此资源，服务将返回 403 Forbidden 响应。 如果标头无效或缺失，则服务将返回 401 未授权的响应。 
  
<a id="ID4EDD"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| API 协定版本。| 
| 授权| XBL3.0 x=[hash];[token]| STS 身份验证令牌。 STSTokenString 将替换为返回身份验证请求的令牌。 有关检索的 STS 令牌和创建授权标头的其他信息，请参阅进行身份验证和授权 Xbox LIVE 服务请求。| 
  
<a id="ID4EME"></a>

 
## <a name="request-body"></a>请求正文 
 
此请求的正文中不发送的任何对象。
  
<a id="ID4EZE"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 确定| 请求已成功。| 
| 201| 创建时间| 创建实体。| 
| 400| 无效的请求| 服务无法理解请求格式不正确。 通常是一个无效的参数。| 
| 401| 未经授权| 请求需要用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 未找到| 找不到指定的资源。| 
| 406| 不可接受| 不支持资源版本。| 
| 408| 请求超时| 该请求花费了太长时间才能完成。| 
| 500| 内部服务器错误| 服务器遇到意外的情况，致使无法履行请求。| 
| 503| 服务不可用| 请求已达到限制，以秒为单位 （例如 5 秒更高版本） 的客户端-重试值后重试请求。| 
  
<a id="ID4EUBAC"></a>

 
## <a name="response-body"></a>响应正文
 
如果调用成功，服务将返回的数组[TitleBlob](../../json/json-titleblob.md)对象。 
 
<a id="ID4ECCAC"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
{
"blobs":
[
    {
        "fileName":"foo\bar\blob.txt,binary",
        "clientFileTime":"2012-01-01T01:02:03.1234567Z",
        "displayName":"Friendly Name",
        "size":12,
        "etag":"0x8CEB3E4F8F3A5BF"
    },
    {
        "fileName":"foo\bar\blob2.txt,binary",
        "displayName":"Blob 2",
        "size":4,
        "etag":"0x8CEB3FE57F1A142"
    },
    {
        "fileName":"foo\jsonblob.txt,json",
        "size":15,
        "etag":"0x8CEB40152B4A6F8"
    }
],
"pagingInfo":
    {
        "continuationToken":"54",
    }
}
         
```

   
<a id="ID4EOCAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EQCAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[/sessions/{sessionId}/scids/{scid}/data/{path}](uri-sessionssessionidscidssciddatapath.md)

  
<a id="ID4E3CAC"></a>

 
##### <a name="reference--titleblob-jsonjsonjson-titleblobmd"></a>引用[TitleBlob (JSON)](../../json/json-titleblob.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

   
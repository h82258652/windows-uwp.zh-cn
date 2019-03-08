---
title: GET (/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type})
assetID: eef3c530-2f56-442a-fa47-f459a77f5798
permalink: en-us/docs/xboxlive/rest/uri-sessionssessionidscidssciddatapathandfilenametype-get.html
description: " GET (/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5e97a193e4f821b9fcd31b26d3d023929f1dfda8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661922"
---
# <a name="get-sessionssessionidscidssciddatapathandfilenametype"></a>GET (/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type})
将文件下载。 这些 Uri 的域是`titlestorage.xboxlive.com`。
 
  * [URI 参数](#ID4EX)
  * [Authorization](#ID4ECB)
  * [可选查询字符串参数](#ID4EPB)
  * [所需的请求标头](#ID4EQC)
  * [可选的请求标头](#ID4EZD)
  * [请求正文](#ID4EDF)
  * [HTTP 状态代码](#ID4EQF)
  * [响应标头](#ID4EDDAC)
  * [响应正文](#ID4EGEAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| sessionId| 字符串| 若要查找的会话 ID。| 
| scid| GUID| 要查找服务配置的 ID。| 
| pathAndFileName| 字符串| 要访问的项的路径和文件名称。 有效的字符 （直至并包括最后的正斜杠） 的路径部分包括大写字母 (A-Z)、 小写字母 (a-z)、 数字 (0-9)、 下划线 (_)，和正斜杠 （/）。路径部分可能为空。有效的字符 （最后一个正斜杠之后的所有内容） 的文件部分包括大写字母 (A-Z)、 小写字母 (a-z)、 数字 (0-9)、 下划线 (_)、 句点 （.） 和连字符 （-）。 文件名称不为空、 以句点结尾或包含两个连续句点。| 
| type| 字符串| 数据的格式。 可能的值为二进制或 json。| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>授权 
 
该请求必须包含有效的 Xbox LIVE 授权标头。 如果调用方不允许访问此资源，服务将返回 403 Forbidden 响应。 如果标头无效或缺失，则服务将返回 401 未授权的响应。 
  
<a id="ID4EPB"></a>

 
## <a name="optional-query-string-parameters"></a>可选查询字符串参数 
 
因 blob 类型而异。 二进制 blob 不支持查询参数。
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 选择| 字符串| 仅当类型为 json 时才可用。 指定响应应仅包含特定属性/值的 json，与通过此参数确定。 使用"dot"（.） 来指定子属性和方括号 ([和]) 来指定数组索引。 例如，"array1 [4].prop2"指定"array1"数组索引 4"prop2"属性。| 
  
<a id="ID4EQC"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| API 协定版本。| 
| 授权| XBL3.0 x=[hash];[token]| STS 身份验证令牌。 STSTokenString 将替换为返回身份验证请求的令牌。 有关检索的 STS 令牌和创建授权标头的其他信息，请参阅进行身份验证和授权 Xbox LIVE 服务请求。| 
  
<a id="ID4EZD"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| 指定必须与匹配的现有项完成此操作。| 
| If-None-Match| 指定用于与任何现有项完成此操作不匹配的 ETag。| 
| 范围| 指定要下载字节的范围。 遵循标准的 HTTP 范围标头格式。| 
  
<a id="ID4EDF"></a>

 
## <a name="request-body"></a>请求正文 
 
此请求的正文中不发送的任何对象。
  
<a id="ID4EQF"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码 
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 确定 | 请求已成功。| 
| 201| 创建时间 | 创建实体。| 
| 400| 无效的请求 | 服务无法理解请求格式不正确。 通常是一个无效的参数。| 
| 401| 未经授权 | 请求需要用户身份验证。| 
| 403| 已禁止 | 为用户或服务不允许该请求。| 
| 404| 未找到 | 找不到指定的资源。| 
| 406| 不可接受 | 不支持资源版本。| 
| 408| 请求超时 | 该请求花费了太长时间才能完成。| 
| 500| 内部服务器错误 | 服务器遇到意外的情况，致使无法履行请求。| 
| 503| 服务不可用 | 请求已达到限制，以秒为单位 （例如 5 秒更高版本） 的客户端-重试值后重试请求。| 
  
<a id="ID4EDDAC"></a>

 
## <a name="response-headers"></a>响应头
 
| 标头| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| ETag 是资源的不透明的标识符分配到特定版本的 URL 中找到的 web 服务器。 如果在该 URL 处的资源内容发生变化，分配新的和不同的 ETag。| 
| Content-Range| 如果这是部分下载，此标头指定下载的字节范围。| 
  
<a id="ID4EGEAC"></a>

 
## <a name="response-body"></a>响应正文
 
如果调用成功，服务将返回文件的内容。
  
<a id="ID4EREAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ETEAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type}](uri-sessionssessionidscidssciddatapathandfilenametype.md)

  
<a id="ID4E6EAC"></a>

 
##### <a name="reference--titleblob-jsonjsonjson-titleblobmd"></a>引用[TitleBlob (JSON)](../../json/json-titleblob.md)

   
---
title: POST (/trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type})
assetID: 0c89b845-c40f-b28e-f102-d2a96f58dcf9
permalink: en-us/docs/xboxlive/rest/uri-trustedplatformusersbatchscidssciddatapathandfilenametype-post.html
description: " POST (/trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 504a73534b9ee536970caec1b5fd1ea6ce731b31
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650542"
---
# <a name="post-trustedplatformusersbatchscidssciddatapathandfilenametype"></a>POST (/trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type})
下载多个文件从多个用户具有相同的文件名。 这些 Uri 的域是`titlestorage.xboxlive.com`。
 
  * [URI 参数](#ID4EX)
  * [Authorization](#ID4ECB)
  * [请求正文](#ID4EPB)
  * [HTTP 状态代码](#ID4E3C)
  * [所需的响应标头](#ID4EPAAC)
  * [可选的响应标头](#ID4ESBAC)
  * [响应正文](#ID4E3CAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| scid| GUID| 要查找服务配置的 ID。| 
| pathAndFileName| 字符串| 要访问的项的路径和文件名称。 有效的字符 （直至并包括最后的正斜杠） 的路径部分包括大写字母 (A-Z)、 小写字母 (a-z)、 数字 (0-9)、 下划线 (_)，和正斜杠 （/）。路径部分可能为空。有效的字符 （最后一个正斜杠之后的所有内容） 的文件部分包括大写字母 (A-Z)、 小写字母 (a-z)、 数字 (0-9)、 下划线 (_)、 句点 （.） 和连字符 （-）。 文件名称不为空、 以句点结尾或包含两个连续句点。| 
| type| 字符串| 数据的格式。 可能的值为二进制或 json。| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>授权 
 
该请求必须包含有效的 Xbox LIVE 授权标头。 如果调用方不允许访问此资源，服务将返回 403 Forbidden 响应。 如果标头无效或缺失，则服务将返回 401 未授权的响应。 
  
<a id="ID4EPB"></a>

 
## <a name="request-body"></a>请求正文
 
| 属性| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | 
| xuids| 64 位无符号整数的数组| XUIDs 要为其下载文件的列表。| 
 
<a id="ID4EQC"></a>

 
### <a name="sample-request"></a>示例请求
 

```cpp
{
    "xuids" : 
    [
      12345,
      45678,
      78901
    ]
}
      
```

   
<a id="ID4E3C"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码 
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EPAAC"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
| 标头| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 内容处置| 描述部件的内容。 标头的"name"和"filename"部分所指向此文件所属的用户的 XUID。| 
| HttpStatusCode| 检索此特定文件与相关的 HTTP 状态代码。| 
  
<a id="ID4ESBAC"></a>

 
## <a name="optional-response-headers"></a>可选的响应标头
 
| 标头| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| ETag 是资源的不透明的标识符分配到特定版本的 URL 中找到的 web 服务器。 如果在该 URL 处的资源内容发生变化，分配新的和不同的 ETag。| 
| 内容类型| 如果成功检索该文件，这是文件的内容类型。| 
| Content-Range| 如果该文件已成功检索，并且被部分下载，这是在响应中包含的文件的字节范围。 | 
  
<a id="ID4E3CAC"></a>

 
## <a name="response-body"></a>响应正文
 
如果调用成功，服务将多个部分组成的响应中返回所需的文件的内容。
 
<a id="ID4EGDAC"></a>

 
### <a name="sample-response"></a>示例响应 
 

```cpp
HTTP/1.1 200 OK
Transfer-Encoding: chunked
Content-Type: multipart/form-data; boundary=c0a9fd75-d036-4294-8b7b-85fea15a31bb

228
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="123"; filename="123"
HttpStatusCode: 200
ETag: 0x8CF327717411C31
Content-Type: application/octet-stream

asdf123
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="456"; filename="456"
HttpStatusCode: 200
ETag: 0x8CF32771E954BB8
Content-Type: application/octet-stream

asdf456
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="789"; filename="789"
HttpStatusCode: 404


--c0a9fd75-d036-4294-8b7b-85fea15a31bb--

0

```

   
<a id="ID4EUDAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EWDAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}](uri-trustedplatformusersbatchscidssciddatapathandfilenametype.md)

   
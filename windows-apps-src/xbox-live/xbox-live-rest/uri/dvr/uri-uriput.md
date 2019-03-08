---
title: PUT (/{uri})
assetID: 24a24c93-f43b-017e-91e0-29e190fec8a8
permalink: en-us/docs/xboxlive/rest/uri-uriput.html
description: " PUT (/{uri})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 862b956e29222bb9d28f98510f13d42fd1a51b6f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633112"
---
# <a name="put-uri"></a>PUT (/{uri})
游戏剪辑数据上传。
这些 Uri 的域是`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，取决于 URI 相关的函数。

  * [备注](#ID4EX)
  * [URI 参数](#ID4EQB)
  * [查询字符串参数](#ID4ERC)
  * [所需的请求标头](#ID4EBE)
  * [可选的请求标头](#ID4ENG)
  * [请求正文](#ID4EWH)
  * [HTTP 状态代码](#ID4ECAAC)
  * [所需的响应标头](#ID4EYEAC)
  * [可选的响应标头](#ID4ELHAC)
  * [响应正文](#ID4ELIAC)

<a id="ID4EX"></a>


## <a name="remarks"></a>备注

之后**InitialUploadResponse**会返回，通过执行上传**uploadUri**该对象中返回。 客户端应拆分到的文件**expectedBlocks**连续的块，不超过 2 MB。 它们可以并行上传。

如果要上传的文件块中，服务器将返回每个请求已接受 (202) 的 HTTP 状态代码，直到它已收到所有预期的块，在这种情况下它作为一个文件，返回已创建 (201) 提交所有块。 在这些情况下，响应不包含一个对象，并在服务器可能会安排其他处理。 出现错误时， **ServiceErrorResponse**对象可能会与相应的 HTTP 状态代码一起返回。

可恢复的错误代码，客户端应使用标准的回退重试机制重试。

> [!NOTE] 
> 即使成功完成上传，进一步处理将发生，可以的拒绝原因的剪辑不相关上传或元数据对进行了补充过程。


<a id="ID4EQB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- |
| <b>uri</b>| 字符串| <b>UploadUri</b>字段中<b>InitialUploadResponse</b>对象。|

<a id="ID4ERC"></a>


## <a name="query-string-parameters"></a>查询字符串参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- |
| <b>blockNum</b>| 32 位无符号的整数| 需要<b>expectedBlocks</b>设置。 零开始编制索引块号确定文件块中的顺序。 例如，如果<b>expectedBlocks</b>为 7，则<b>blockNum</b>可以介于 0 到 6。 |
| <b>uploadId</b>| 字符串| 必需。 中的不透明 ID <b>GameClipsServiceUploadResponse</b>对象。|

<a id="ID4EBE"></a>


## <a name="required-request-headers"></a>所需的请求标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值：<b>Xauth=&lt;authtoken></b>|
| X-RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。示例：1，vnext。|
| 内容类型| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序 /json</b>。|
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序 /json</b>。|
| Cache-Control| 字符串| 请求正常，以指定缓存行为。|

<a id="ID4ENG"></a>


## <a name="optional-request-headers"></a>可选的请求标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Accept-Encoding| 字符串| 可接受的压缩编码。 示例值： gzip、 deflate，标识。|
| ETag| 字符串| 用于缓存优化。 示例值："686897696a7c876b7e"。|

<a id="ID4EWH"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送的任何对象。

<a id="ID4ECAAC"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 原因短语| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 确定| 已成功检索该会话。|
| 301| 已永久移动| 服务已移动到另一个 URI。|
| 307| 临时重定向| 服务已移动到另一个 URI。|
| 400| 无效的请求| 服务无法理解请求格式不正确。 通常是一个无效的参数。|
| 401| 未经授权| 请求需要用户身份验证。|
| 403| 已禁止| 为用户或服务不允许该请求。|
| 404| 未找到| 找不到指定的资源。|
| 406| 不可接受| 不支持资源版本。|
| 408| 请求超时| 该请求花费了太长时间才能完成。|
| 410| 消失| 请求的资源不再可用。|

<a id="ID4EYEAC"></a>


## <a name="required-response-headers"></a>所需的响应标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。示例：1，vnext。|
| 内容类型| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序 /json</b>。|
| Cache-Control| 字符串| 请求正常，以指定缓存行为。|
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序 /json</b>。|
| 重试间隔| 字符串| 指示客户端在不可用的服务器的情况下请稍后再试。|
| 改变| 字符串| 指示下游代理如何缓存响应。|

<a id="ID4ELHAC"></a>


## <a name="optional-response-headers"></a>可选的响应标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Etag| 字符串| 用于缓存优化。 示例："686897696a7c876b7e"。|

<a id="ID4ELIAC"></a>


## <a name="response-body"></a>响应正文

响应正文中不发送任何对象。

<a id="ID4EWIAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EYIAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/{uri}](uri-uri.md)

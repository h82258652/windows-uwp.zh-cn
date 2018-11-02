---
title: PUT (/{uri})
assetID: 24a24c93-f43b-017e-91e0-29e190fec8a8
permalink: en-us/docs/xboxlive/rest/uri-uriput.html
author: KevinAsgari
description: " PUT (/{uri})"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 56a5150e354568f3ea7ec772953bd1e1e8efa579
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5940203"
---
# <a name="put-uri"></a>PUT (/{uri})
上传游戏剪辑数据。
这些 Uri 的域是`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，根据问题的 URI 的函数。

  * [备注](#ID4EX)
  * [URI 参数](#ID4EQB)
  * [查询字符串参数](#ID4ERC)
  * [需的请求标头](#ID4EBE)
  * [可选的请求标头](#ID4ENG)
  * [请求正文](#ID4EWH)
  * [HTTP 状态代码](#ID4ECAAC)
  * [所需的响应标头](#ID4EYEAC)
  * [可选的响应标头](#ID4ELHAC)
  * [响应正文](#ID4ELIAC)

<a id="ID4EX"></a>


## <a name="remarks"></a>备注

**InitialUploadResponse**返回后，通过该对象中返回**uploadUri**执行上传。 客户端应将文件拆分为**expectedBlocks**连续块中，不能大于 2 MB。 它们可以并行上传。

如果你要上载块中的文件，服务器将返回 HTTP 状态代码的接受 (202) 为每个请求，直到它已经收到所有预期的块中，在这种情况下它作为一个文件，返回创建 (201) 提交的所有块。 在这些情况下，响应不包含一个对象，并服务器可能计划额外的处理。 在发生错误， **ServiceErrorResponse**对象可能会返回以及相应的 HTTP 状态代码。

在恢复的错误代码中，客户端应使用标准后关闭重试机制重试。

> [!NOTE] 
> 即使上传操作成功完成，则进一步处理将发生的可能原因剪辑不相关的上传或元数据的拒绝补充过程。


<a id="ID4EQB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- | --- |
| <b>uri</b>| 字符串| <b>InitialUploadResponse</b>对象内<b>uploadUri</b>字段。|

<a id="ID4ERC"></a>


## <a name="query-string-parameters"></a>查询字符串参数

| 参数| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- |
| <b>blockNum</b>| 32 位无符号的整数| 如果设置<b>expectedBlocks</b>必需。 确定排序的文件中的块的零开始编制的块数量。 例如，如果<b>expectedBlocks</b> 7，然后<b>blockNum</b>可以是从 0 到 6。 |
| <b>uploadId</b>| 字符串| 必需。 <b>GameClipsServiceUploadResponse</b>对象中的不透明 ID。|

<a id="ID4EBE"></a>


## <a name="required-request-headers"></a>需的请求标头

| 标头| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值： <b>Xauth =&lt;authtoken ></b>|
| X RequestedServiceVersion| 字符串| 名称/的内部版本号应指向此请求的 Xbox LIVE 的服务。 请求将仅路由到该服务后验证标头、 身份验证令牌等中的声明的有效性。示例： 1，vnext。|
| Content-Type| 字符串| 响应正文的 MIME 类型。 示例： <b>application/json</b>。|
| 接受| 字符串| 内容类型的可接受的值。 示例： <b>application/json</b>。|
| 缓存控制| 字符串| 若要指定缓存行为的礼貌用语请求。|

<a id="ID4ENG"></a>


## <a name="optional-request-headers"></a>可选的请求标头

| 标头| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Accept-Encoding| 字符串| 可接受的压缩编码。 示例值： gzip，桥，标识。|
| ETag| 字符串| 用于缓存优化。 示例值:"686897696a7c876b7e"。|

<a id="ID4EWH"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送任何对象。

<a id="ID4ECAAC"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

本部分中使用此方法对此资源区域设置发出请求的响应，该服务返回的状态代码之一。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 原因短语| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| “确定”| 已成功检索会话。|
| 301| 已永久移动| 该服务已移动到不同的 URI。|
| 307| 临时重定向| 该服务已移动到不同的 URI。|
| 400| 错误请求| 服务可能不理解的格式不正确的请求。 通常参数无效。|
| 401| 未授权| 请求要求用户身份验证。|
| 403| 已禁止| 为用户或服务不允许该请求。|
| 404| 找不到| 找不到指定的资源。|
| 406| 不允许| 资源版本不受支持。|
| 408| 请求超时| 请求所花的时间太长，才能完成。|
| 410| 前面| 所请求的资源不再可用。|

<a id="ID4EYEAC"></a>


## <a name="required-response-headers"></a>所需的响应标头

| 标头| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X RequestedServiceVersion| 字符串| 名称/的内部版本号应指向此请求的 Xbox LIVE 的服务。 请求将仅路由到该服务后验证标头、 身份验证令牌等中的声明的有效性。示例： 1，vnext。|
| Content-Type| 字符串| 响应正文的 MIME 类型。 示例： <b>application/json</b>。|
| 缓存控制| 字符串| 若要指定缓存行为的礼貌用语请求。|
| 接受| 字符串| 内容类型的可接受的值。 示例： <b>application/json</b>。|
| 重试后| 字符串| 指示客户端在不可用的服务器的情况下稍后重试。|
| 有所不同| 字符串| 指示下游代理如何缓存响应。|

<a id="ID4ELHAC"></a>


## <a name="optional-response-headers"></a>可选的响应标头

| 标头| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Etag| 字符串| 用于缓存优化。 示例:"686897696a7c876b7e"。|

<a id="ID4ELIAC"></a>


## <a name="response-body"></a>响应正文

该响应正文中不发送任何对象。

<a id="ID4EWIAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EYIAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/{uri}](uri-uri.md)

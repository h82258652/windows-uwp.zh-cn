---
title: 获取 (/ {uri})
assetID: a67a3288-88f9-c504-5fa8-8fd06055d079
permalink: en-us/docs/xboxlive/rest/uri-uriget.html
author: KevinAsgari
description: " 获取 (/ {uri})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b70c87b848cec5f9bbe3ad4a4b3fdf224c84c1dc
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3960372"
---
# <a name="get-uri"></a>获取 (/ {uri})
下载游戏剪辑。 这些 Uri 的域是`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，则根据问题的 URI 的函数。
 
  * [备注](#ID4EX)
  * [URI 参数](#ID4EDB)
  * [需的请求标头](#ID4EEC)
  * [可选的请求标头](#ID4EQE)
  * [请求正文](#ID4EZF)
  * [所需的响应标头](#ID4EEG)
  * [HTTP 状态代码](#ID4EYAAC)
  * [可选的响应标头](#ID4EOFAC)
  * [响应正文](#ID4EOGAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>备注
 
客户端可以下载任何剪辑或已到达的已发布状态并且可下载的类型，指定**GameClipUri**对象中的缩略图。 检索用户或公共剪辑列表时，响应正文中包含请求文件的 URI。
  
<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| <b>uri</b>| 字符串| <b>GameClipUri</b>对象内<b>uri</b>字段中。| 
  
<a id="ID4EEC"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值： <b>Xauth =&lt;authtoken ></b>| 
| X RequestedServiceVersion| 字符串| 生成此请求应定向到的 Xbox LIVE 的服务的名称/号码。 验证标头、 身份验证令牌等中的声明的有效性后仅为请求路由到该服务。示例： 1，vnext。| 
| Content-Type| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序/json</b>。| 
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序/json</b>。| 
| 缓存控制| 字符串| 若要指定缓存行为的礼貌请求。| 
  
<a id="ID4EQE"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 字符串| 可接受的压缩编码。 示例值： gzip，桥，标识。| 
| ETag| 字符串| 用于缓存优化。 示例值:"686897696a7c876b7e"。| 
  
<a id="ID4EZF"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送任何对象。
  
<a id="ID4EEG"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion| 字符串| 生成此请求应定向到的 Xbox LIVE 的服务的名称/号码。 验证标头、 身份验证令牌等中的声明的有效性后仅为请求路由到该服务。示例： 1，vnext。| 
| Content-Type| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序/json</b>。| 
| 缓存控制| 字符串| 若要指定缓存行为的礼貌请求。| 
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序/json</b>。| 
| 重试后| 字符串| 指示客户端在不可用的服务器的情况下我们深表歉意。| 
| 有所不同| 字符串| 指示下游代理如何缓存响应。| 
  
<a id="ID4EYAAC"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
该服务返回的状态代码之一此部分中使用此方法对此资源进行的请求的响应。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| “确定”| 已成功检索会话。| 
| 301| 已永久移动| 该服务已移动到不同的 URI。| 
| 307| 临时重定向| 该服务已移动到不同的 URI。| 
| 400| 错误请求| 服务可能不理解格式不正确的请求。 通常是一个无效的参数。| 
| 401| 未授权| 请求要求用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 找不到| 找不到指定的资源。| 
| 406| 不允许| 不支持资源版本。| 
| 408| 请求超时| 请求时间太长，才能完成。| 
| 410| 前面| 所请求的资源不再可用。| 
  
<a id="ID4EOFAC"></a>

 
## <a name="optional-response-headers"></a>可选的响应标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Etag| 字符串| 用于缓存优化。 示例:"686897696a7c876b7e"。| 
  
<a id="ID4EOGAC"></a>

 
## <a name="response-body"></a>响应正文
 
<a id="ID4EUGAC"></a>

  
 
如果成功，服务器将返回的视频剪辑，可能会截断根据范围请求标头。 对于被截断剪裁，响应将部分内容 (206)。 如果服务器返回整个文件，它将响应确定 (200)。 在发生错误， **GameClipsServiceErrorResponse**对象可能会返回以及相应的 HTTP 状态代码 (例如，416，请求范围无法满足)。
   
<a id="ID4E4GAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E6GAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/ {uri}](uri-uri.md)

   
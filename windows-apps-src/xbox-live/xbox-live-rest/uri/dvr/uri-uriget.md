---
title: GET (/{uri})
assetID: a67a3288-88f9-c504-5fa8-8fd06055d079
permalink: en-us/docs/xboxlive/rest/uri-uriget.html
description: " GET (/{uri})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 757d84c9ad5a005e042b42d699ada08504dc57ff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650612"
---
# <a name="get-uri"></a>GET (/{uri})
下载游戏剪辑。 这些 Uri 的域是`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，取决于 URI 相关的函数。
 
  * [备注](#ID4EX)
  * [URI 参数](#ID4EDB)
  * [所需的请求标头](#ID4EEC)
  * [可选的请求标头](#ID4EQE)
  * [请求正文](#ID4EZF)
  * [所需的响应标头](#ID4EEG)
  * [HTTP 状态代码](#ID4EYAAC)
  * [可选的响应标头](#ID4EOFAC)
  * [响应正文](#ID4EOGAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>备注
 
客户端可以下载任何剪辑或已达到已发布状态且是可下载的类型中, 指定的缩略图**GameClipUri**对象。 检索的用户或公共剪辑列表时，响应正文中包含请求文件的 URI。
  
<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| <b>uri</b>| 字符串| <b>Uri</b>字段中<b>GameClipUri</b>对象。| 
  
<a id="ID4EEC"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值：<b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。示例：1，vnext。| 
| 内容类型| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序 /json</b>。| 
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序 /json</b>。| 
| Cache-Control| 字符串| 请求正常，以指定缓存行为。| 
  
<a id="ID4EQE"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 字符串| 可接受的压缩编码。 示例值： gzip、 deflate，标识。| 
| ETag| 字符串| 用于缓存优化。 示例值："686897696a7c876b7e"。| 
  
<a id="ID4EZF"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送的任何对象。
  
<a id="ID4EEG"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。示例：1，vnext。| 
| 内容类型| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序 /json</b>。| 
| Cache-Control| 字符串| 请求正常，以指定缓存行为。| 
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序 /json</b>。| 
| 重试间隔| 字符串| 指示客户端在不可用的服务器的情况下请稍后再试。| 
| 改变| 字符串| 指示下游代理如何缓存响应。| 
  
<a id="ID4EYAAC"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EOFAC"></a>

 
## <a name="optional-response-headers"></a>可选的响应标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Etag| 字符串| 用于缓存优化。 示例："686897696a7c876b7e"。| 
  
<a id="ID4EOGAC"></a>

 
## <a name="response-body"></a>响应正文
 
<a id="ID4EUGAC"></a>

  
 
如果成功，服务器将返回可能截断根据范围请求标头的视频剪辑。 对于被截断的剪辑，响应将部分内容 (拨打 206)。 如果服务器返回整个文件，它将响应 OK (200)。 出现错误时， **GameClipsServiceErrorResponse**以及相应的 HTTP 状态代码 (例如，416，范围无法满足请求） 可能返回对象。
   
<a id="ID4E4GAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E6GAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/{uri}](uri-uri.md)

   
---
title: 标准 HTTP 状态代码
assetID: 7a19de56-7acd-ad2b-e8e6-53126991093b
permalink: en-us/docs/xboxlive/rest/httpstatuscodes.html
description: " 标准 HTTP 状态代码"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c77972e88dbf4950f716594ee61220d1734a7fef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635552"
---
# <a name="standard-http-status-codes"></a>标准 HTTP 状态代码
 
超文本传输协议 (HTTP) 标准描述了多个返回的响应客户端请求中的服务器的状态代码。 Xbox Live 服务方法返回 HTTP 协议符合状态代码来描述请求的状态。
 
下面是返回的 Xbox Live 服务，以及其典型含义状态代码的列表。
 
<a id="ID4EAB"></a>

 
## <a name="xbox-live-services-status-codes"></a>Xbox Live 服务状态代码
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | 
| 200| 确定| 请求已成功。| 
| 201| 创建时间| 创建实体。| 
| 202| 接受| 已接受请求，但尚未完成。| 
| 204| 无内容| 请求已完成，但目前没有要返回的内容。| 
| 301| 已永久移动| 服务已移动到另一个 URI。| 
| 302| 找到| 请求的资源已临时驻留在其他 URI 下。| 
| 307| 临时重定向| 已暂时更改此资源的 URI。| 
| 400| 无效的请求| 服务无法理解请求格式不正确。 通常是一个无效的参数。| 
| 401| 未经授权| 请求需要用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 未找到| 找不到指定的资源。| 
| 406| 不可接受| 不支持资源版本。| 
| 408| 请求超时| 该请求花费了太长时间才能完成。| 
| 409| 冲突| 由于与资源的当前状态冲突而未完成请求。| 
| 410| 消失| 请求的资源不再可用。| 
| 412| 前置条件失败| 服务器不满足请求方放在请求的前提条件之一。| 
| 416| 请求的范围不符合条件| 所请求的范围不可用。| 
| 500| 内部服务器错误| 服务器遇到意外的情况，致使无法履行请求。| 
| 501| 未实现| 服务器不支持完成请求所需的功能。| 
| 503| 服务不可用| 请求已达到限制，以秒为单位 （例如 5 秒更高版本） 的客户端-重试值后重试请求。| 
 

> [!NOTE] 
> 某些资源和方法提供该资源或方法的上下文中的特定状态代码的含义的特定信息。 有关更多详细信息，请参阅资源或正在使用的方法的文档。 

  
<a id="ID4E3BAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E5BAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[其他参考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4EKCAC"></a>

 
##### <a name="reference--universal-resource-identifier-uri-referenceuriatoc-xboxlivews-reference-urismd"></a>引用[通用资源标识符 (URI) 引用](../uri/atoc-xboxlivews-reference-uris.md)

 [其他参考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4EZCAC"></a>

 
##### <a name="external-links--w3org-http11-status-code-definitionshttpswwww3orgprotocolsrfc2616rfc2616-sec10htmlsec10"></a>外部链接[W3.org:HTTP/1.1 状态代码定义](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10)

   
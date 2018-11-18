---
title: 标准 HTTP 状态代码
assetID: 7a19de56-7acd-ad2b-e8e6-53126991093b
permalink: en-us/docs/xboxlive/rest/httpstatuscodes.html
author: KevinAsgari
description: " 标准 HTTP 状态代码"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5540339d61c81b08b9843f7352ac816d93fcf12e
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2018
ms.locfileid: "7155519"
---
# <a name="standard-http-status-codes"></a>标准 HTTP 状态代码
 
超文本传输协议 (HTTP) 标准描述中向客户端请求的响应由服务器返回的状态代码的数量。 Xbox Live 服务方法将返回 HTTP 协议符合状态代码来描述请求的状态。
 
下面是返回的 Xbox Live 服务，以及它们的典型含义状态代码的列表。
 
<a id="ID4EAB"></a>

 
## <a name="xbox-live-services-status-codes"></a>Xbox Live 服务状态代码
 
| 代码| 原因短语| 说明| 
| --- | --- | --- | 
| 200| “确定”| 请求已成功。| 
| 201| 已创建| 创建实体。| 
| 202| 已接受| 已接受请求，但尚未完成。| 
| 204| 任何内容| 请求已完成，但没有要返回的内容。| 
| 301| 已永久移动| 该服务已移动到不同的 URI。| 
| 302| 找到| 所请求的资源暂时在不同的 URI。| 
| 307| 临时重定向| 此资源的 URI 暂时已发生更改。| 
| 400| 错误请求| 服务可能不理解格式不正确的请求。 通常参数无效。| 
| 401| 未授权| 请求要求用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 找不到| 找不到指定的资源。| 
| 406| 不允许| 不支持资源版本。| 
| 408| 请求超时| 请求所花的时间太长，才能完成。| 
| 409| 冲突| 由于与资源的当前状态的冲突未完成请求。| 
| 410| 前面| 所请求的资源不再可用。| 
| 412| 前置条件失败| 服务器不满足，则请求者将放在请求前置条件之一。| 
| 416| 请求的范围无法满足| 所请求的范围不可用。| 
| 500| 内部服务器错误| 服务器时遇到意外的情况，执行此请求将阻止它。| 
| 501| 未实现| 服务器不支持所需满足该请求的功能。| 
| 503| 服务不可用| 请求已被阻止，以秒为单位 （例如 5 秒更高版本） 的客户端重试值后重试请求。| 
 

> [!NOTE] 
> 某些资源和方法提供的含义的资源或方法的上下文中的特定状态代码的特定信息。 有关更多详细信息，请参阅文档中针对资源或你所使用的方法。 

  
<a id="ID4E3BAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E5BAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[其他参考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4EKCAC"></a>

 
##### <a name="reference--universal-resource-identifier-uri-referenceuriatoc-xboxlivews-reference-urismd"></a>引用[统一资源标识符 (URI) 参考](../uri/atoc-xboxlivews-reference-uris.md)

 [其他参考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4EZCAC"></a>

 
##### <a name="external-links--w3org-http11-status-code-definitionshttpwwww3orgprotocolsrfc2616rfc2616-sec10htmlsec10"></a>外部链接[W3.org: HTTP/1.1 状态代码定义](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10)

   
---
title: DELETE (/users/me/scids/{scid}/clips/{gameClipId})
assetID: 486fac60-6884-2e3f-9ef8-8de5da0ad8af
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipiddelete.html
description: " DELETE (/users/me/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 391e4d79a389c358dea83509b52782d086201ffc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622832"
---
# <a name="delete-usersmescidsscidclipsgameclipid"></a>DELETE (/users/me/scids/{scid}/clips/{gameClipId})
这些 Uri 的域是删除游戏剪辑`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，取决于 URI 相关的函数。
 
  * [备注](#ID4EX)
  * [URI 参数](#ID4ECB)
  * [Authorization](#ID4ENB)
  * [所需的请求标头](#ID4EYB)
  * [可选的请求标头](#ID4EEE)
  * [请求正文](#ID4ENF)
  * [HTTP 状态代码](#ID4EYF)
  * [所需的响应标头](#ID4EIAAC)
  * [可选的响应标头](#ID4E2CAC)
  * [响应正文](#ID4E2DAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>备注
 
提供了一种机制用于从 GameClips 服务中删除用户自己的视频。 根据删除，从系统删除所有元数据和实际的视频资产 （生成和原始）。 这是一项永久性操作。 

> [!NOTE] 
> 指定的所有者 ID 必须匹配在授权令牌中删除请求才能成功调用方。 


  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | 
| scid| 字符串| 正在访问的资源的服务的配置 ID。 必须与匹配身份验证的用户的 SCID。| 
| gameClipId| 字符串| GameClip 正在访问的资源 ID。| 
  
<a id="ID4ENB"></a>

 
## <a name="authorization"></a>授权
 
需要为此方法仅 Xuid 声明。
  
<a id="ID4EYB"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值：<b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。示例：1，vnext。| 
| 内容类型| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序 /json</b>。| 
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序 /json</b>。| 
| Cache-Control| 字符串| 请求正常，以指定缓存行为。| 
  
<a id="ID4EEE"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 字符串| 可接受的压缩编码。 示例值： gzip、 deflate，标识。| 
| ETag| 字符串| 用于缓存优化。 示例值："686897696a7c876b7e"。| 
  
<a id="ID4ENF"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送的任何对象。
  
<a id="ID4EYF"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 204| 确定| 成功删除的剪辑。| 
| 401| 未经授权| 没有在请求中的身份验证令牌格式的问题。| 
| 403| 已禁止| 缺少一些必需的声明。| 
| 404| 未找到| 在 URL 中指定的剪辑时不存在 （或它已被删除第二次）。| 
| 503| 不可接受| 此服务或某些下游的依赖关系都已关闭。 使用标准的回退行为重试。| 
  
<a id="ID4EIAAC"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。示例：1，vnext。| 
| 内容类型| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序 /json</b>。| 
| Cache-Control| 字符串| 请求正常，以指定缓存行为。| 
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序 /json</b>。| 
| 重试间隔| 字符串| 指示客户端在不可用的服务器的情况下请稍后再试。| 
| 改变| 字符串| 指示下游代理如何缓存响应。| 
  
<a id="ID4E2CAC"></a>

 
## <a name="optional-response-headers"></a>可选的响应标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Etag| 字符串| 用于缓存优化。 示例："686897696a7c876b7e"。| 
  
<a id="ID4E2DAC"></a>

 
## <a name="response-body"></a>响应正文
 
该服务将使用 HTTP 状态代码 204 （无内容） 成功后的响应。 尝试删除同一个对象，或不存在的对象将返回 404。
 
如果出错， **ServiceErrorResponse**将返回对象。
  
<a id="ID4EJEAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ELEAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

   
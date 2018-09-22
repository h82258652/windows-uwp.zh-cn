---
title: POST (/users/me/scids/{scid}/clips/{gameClipId})
assetID: 410aecad-57f9-c3dc-f35f-19c4d8dfb704
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipidpost.html
author: KevinAsgari
description: " POST (/users/me/scids/{scid}/clips/{gameClipId})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 28c8b9e20e990c51c6b3d7e56e72f4d5d6551b39
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2018
ms.locfileid: "4123367"
---
# <a name="post-usersmescidsscidclipsgameclipid"></a>POST (/users/me/scids/{scid}/clips/{gameClipId})
更新用户自己的数据的游戏剪辑元数据。 有关这些 Uri 域是`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，则根据问题的 URI 的函数。
 
  * [备注](#ID4EX)
  * [URI 参数](#ID4EAB)
  * [需的请求标头](#ID4ELB)
  * [可选的请求标头](#ID4EXD)
  * [请求正文](#ID4EAF)
  * [所需的响应标头](#ID4EVF)
  * [可选的响应标头](#ID4EJAAC)
  * [响应正文](#ID4EJBAC)
  * [相关的 Uri](#ID4EWBAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>备注
 
用于更新游戏剪辑元数据的 API 在更新元数据的自己的游戏剪辑辅助功能和标题，如和更新的公共属性 （如应用进行评分或递增的视图计数） 分为两类的任何其他游戏剪辑。 如果对 URI 中的 XUID 不匹配的 XUID 声明中，可以编辑只将公共数据，并将拒绝任何要编辑的任何其他数据的请求。 在这种情况中多个字段尝试进行编辑和至少其中一个无效请求，整个请求将会失败。
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| scid| 字符串| 正在访问的资源的服务配置 ID。 必须匹配的身份验证的用户的 SCID。| 
| gameClipId| 字符串| 正在访问的资源的 GameClip ID。| 
  
<a id="ID4ELB"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值： <b>Xauth =&lt;authtoken ></b>| 
| X RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/数。 验证在标头、 身份验证令牌等中的声明的有效性后仅为请求路由到该服务。示例： 1，vnext。| 
| Content-Type| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序/json</b>。| 
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序/json</b>。| 
| 缓存控制| 字符串| 若要指定缓存行为的礼貌请求。| 
  
<a id="ID4EXD"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 字符串| 可接受的压缩编码。 示例值： gzip，桥，标识。| 
| ETag| 字符串| 用于缓存优化。 示例值:"686897696a7c876b7e"。| 
  
<a id="ID4EAF"></a>

 
## <a name="request-body"></a>请求正文
 
请求的正文应采用 JSON 格式的[UpdateMetadataRequest](../../json/json-updatemetadatarequest.md)对象。 示例：
 
更改用户剪辑名称和可见性：
 

```cpp
{
  "userCaption": "I've changed this 100 Times!",
  "visibility": "Owner"
}

```

 
更改只是标题属性 （这只是一个示例，由于此字段的架构是取决于调用方）：
 

```cpp
{
  "titleData": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }"
}

```

  
<a id="ID4EVF"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/数。 验证在标头、 身份验证令牌等中的声明的有效性后仅为请求路由到该服务。示例： 1，vnext。| 
| Content-Type| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序/json</b>。| 
| 缓存控制| 字符串| 若要指定缓存行为的礼貌请求。| 
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序/json</b>。| 
| 重试后| 字符串| 指示客户端在不可用的服务器的情况下稍后重试。| 
| 不同| 字符串| 指示下游代理如何缓存响应。| 
  
<a id="ID4EJAAC"></a>

 
## <a name="optional-response-headers"></a>可选的响应标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Etag| 字符串| 用于缓存优化。 示例:"686897696a7c876b7e"。| 
  
<a id="ID4EJBAC"></a>

 
## <a name="response-body"></a>响应正文
 
将返回的元数据的 HTTP 状态代码为 200 的成功更新时。
 
否则将相应的 HTTP 状态代码返回 JSON 格式的 ServiceErrorResponse 对象。
  
<a id="ID4EWBAC"></a>

 
## <a name="related-uris"></a>相关的 Uri
 
以下 Uri 更新元数据中的公共字段。 没有任何所需的这些请求的正文。 将返回的元数据的 HTTP 状态代码为 200 的成功更新时。 否则将相应的 HTTP 状态代码返回 JSON 格式的 ServiceErrorResponse 对象。
 
   * **POST /users/ {ownerId} {scid} /scids/ /clips/ {gameClipId} /ratings/ {评分值}** -适用于指定剪辑指定的评分。 评分值应为 1 到 5 之间的整数。
   * **发布 /users/ {ownerId} {scid} /scids/ /clips/ {gameClipId} / 标志**-标志来包含可能有问题的内容要检查通过强制执行该剪辑。
   * **POST /users/ {ownerId} {scid} /scids/ /clips/ {gameClipId} / 查看**-增加视图计数指定游戏剪辑。 建议，这称为不正确播放启动时，但当 75%的 80%的播放完毕。
   
<a id="ID4EMCAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EOCAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

   
---
title: POST (/users/me/scids/{scid}/clips/{gameClipId})
assetID: 410aecad-57f9-c3dc-f35f-19c4d8dfb704
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipidpost.html
description: " POST (/users/me/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 89f3b53631f5570ab6d0d0619f6678fc3e3c2dd2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645822"
---
# <a name="post-usersmescidsscidclipsgameclipid"></a>POST (/users/me/scids/{scid}/clips/{gameClipId})
更新游戏剪辑用户自己的数据的元数据。 这些 Uri 的域是`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，取决于 URI 相关的函数。
 
  * [备注](#ID4EX)
  * [URI 参数](#ID4EAB)
  * [所需的请求标头](#ID4ELB)
  * [可选的请求标头](#ID4EXD)
  * [请求正文](#ID4EAF)
  * [所需的响应标头](#ID4EVF)
  * [可选的响应标头](#ID4EJAAC)
  * [响应正文](#ID4EJBAC)
  * [相关的 Uri](#ID4EWBAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>备注
 
更新游戏剪辑元数据 API 操作划分为两个类别，更新自己的游戏的剪辑的可访问性和标题，如元数据和更新的公共属性 （例如应用评级或递增视图计数） 的任何其他游戏剪辑。 如果在 URI 中的 XUID 与声明中的 XUID 不匹配，仅将公共数据可以进行编辑，若要编辑的任何其他数据的任何请求将被拒绝。 在这种情况中尝试多个字段进行编辑和其中一个无效的请求，整个请求会失败。
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| scid| 字符串| 正在访问的资源的服务的配置 ID。 必须与匹配身份验证的用户的 SCID。| 
| gameClipId| 字符串| GameClip 正在访问的资源 ID。| 
  
<a id="ID4ELB"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值：<b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。示例：1，vnext。| 
| 内容类型| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序 /json</b>。| 
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序 /json</b>。| 
| Cache-Control| 字符串| 请求正常，以指定缓存行为。| 
  
<a id="ID4EXD"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 字符串| 可接受的压缩编码。 示例值： gzip、 deflate，标识。| 
| ETag| 字符串| 用于缓存优化。 示例值："686897696a7c876b7e"。| 
  
<a id="ID4EAF"></a>

 
## <a name="request-body"></a>请求正文
 
请求的正文应是[UpdateMetadataRequest](../../json/json-updatemetadatarequest.md)以 JSON 格式的对象。 示例：
 
更改用户名剪辑和可见性：
 

```cpp
{
  "userCaption": "I've changed this 100 Times!",
  "visibility": "Owner"
}

```

 
更改只需标题属性 （因为此字段的架构是由调用方，这是只是一个示例）：
 

```cpp
{
  "titleData": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }"
}

```

  
<a id="ID4EVF"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。示例：1，vnext。| 
| 内容类型| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序 /json</b>。| 
| Cache-Control| 字符串| 请求正常，以指定缓存行为。| 
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序 /json</b>。| 
| 重试间隔| 字符串| 指示客户端在不可用的服务器的情况下请稍后再试。| 
| 改变| 字符串| 指示下游代理如何缓存响应。| 
  
<a id="ID4EJAAC"></a>

 
## <a name="optional-response-headers"></a>可选的响应标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Etag| 字符串| 用于缓存优化。 示例："686897696a7c876b7e"。| 
  
<a id="ID4EJBAC"></a>

 
## <a name="response-body"></a>响应正文
 
将返回在成功的 HTTP 状态代码 200 的元数据更新时。
 
否则将使用相应的 HTTP 状态代码返回 JSON 格式的 ServiceErrorResponse 对象。
  
<a id="ID4EWBAC"></a>

 
## <a name="related-uris"></a>相关的 Uri
 
下面的 Uri 更新元数据中的公共字段。 没有所需的这些请求正文。 将返回在成功的 HTTP 状态代码 200 的元数据更新时。 否则将使用相应的 HTTP 状态代码返回 JSON 格式的 ServiceErrorResponse 对象。
 
   * **开机自检 /users/ {ownerId} /scids/ {scid} /clips/ {gameClipId} /ratings/ {评级值}** -适用于指定的剪辑指定的分级。 评级值应介于 1 和 5 之间的整数。
   * **发布 /users/ {ownerId} /scids/ {scid} /clips/ {gameClipId} / 标志**-标记以包含存在安全隐患的内容要检查的强制实施的剪辑。
   * **开机自检 /users/ {ownerId} /scids/ {scid} /clips/ {gameClipId} / 视图**-递增指定游戏剪辑的视图计数。 建议，这称为不正确时播放已启动，但完成后 75%-80%的播放。
   
<a id="ID4EMCAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EOCAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

   
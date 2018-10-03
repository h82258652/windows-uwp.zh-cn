---
title: POST (/users/me/scids/{scid}/clips)
assetID: 44535926-9fb8-5498-b1c8-514c0763e6c9
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipspost.html
author: KevinAsgari
description: " POST (/users/me/scids/{scid}/clips)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2e6cee1adbe9e9401bec2ce578ab0d04da921170
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2018
ms.locfileid: "4267271"
---
# <a name="post-usersmescidsscidclips"></a>POST (/users/me/scids/{scid}/clips)
发出初始上载请求。 这些 Uri 的域是`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，则根据问题的 URI 的函数。
 
  * [备注](#ID4EX)
  * [URI 参数](#ID4EFB)
  * [授权](#ID4EQB)
  * [需的请求标头](#ID4EKC)
  * [可选的请求标头](#ID4ENE)
  * [请求正文](#ID4ENF)
  * [示例请求](#ID4E1F)
  * [HTTP 状态代码](#ID4EDG)
  * [响应正文](#ID4EVAAC)
  * [示例响应](#ID4EFBAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>备注
 
这是 GameClip 上载过程的第一部分。 后的视频捕获，建议调用 GameClips 服务立即获取以 ID 和 URI 供上传的位，即使上传未计划立即开始。 用户配额检查和内容隔离，隐私，依此类推，若要查看是否视频应甚至计划用于上传客户端通过其他检查，将执行此调用。 从此调用肯定响应指示服务愿意接受视频剪辑的上传。 上传的所有剪辑必须与特定标题 （通过 SCID) 都关联，以接受系统中。
 
此调用不是幂等;后续调用将导致不同 Id 和要颁发的 Uri。 重试失败应遵循标准客户端后关闭行为。
  
<a id="ID4EFB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 描述| 
| --- | --- | --- | 
| scid| 字符串| 正在访问的资源的服务配置 ID。 必须匹配的身份验证的用户的 SCID。| 
  
<a id="ID4EQB"></a>

 
## <a name="authorization"></a>授权
 
以下声明所需的此方法：
 
   * Xuid
   * DeviceType-必须上传的设备
   * DeviceId
   * TitleId
   * TitleSandboxId
   
<a id="ID4EKC"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标头| 类型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值： <b>Xauth =&lt;authtoken ></b>| 
| X RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/数。 验证该标头、 身份验证令牌等中的声明的有效性后仅为请求路由到该服务。示例： 1，vnext。| 
| Content-Type| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序/json</b>。| 
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序/json</b>。| 
  
<a id="ID4ENE"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 类型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 字符串| 可接受的压缩编码。 示例值： gzip，桥，标识。| 
  
<a id="ID4ENF"></a>

 
## <a name="request-body"></a>请求正文
 
请求的正文应采用 JSON 格式的[InitialUploadRequest](../../json/json-initialuploadrequest.md)对象。
  
<a id="ID4E1F"></a>

 
## <a name="sample-request"></a>示例请求
 

```cpp
{
   "clipNameStringId": "1193045",
   "userCaption": "OMG Look at this!",
   "sessionRef": "4587552a-a5ad-4c4c-a787-5bc5af70e4c9",
   "dateRecorded": "2012-12-23T11:08:08Z",
   "durationInSeconds": 27,
   "expectedBlocks": 7,
   "fileSize": 1234567,
   "type": "MagicMoment, Achievement",
   "source": "Console",
   "visibility": "Default",
   "titleData": "{ 'Boss': 'The Invincible' }",
   "systemProperties": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }",
   "thumbnailSource": "Offset",
   "thumbnailOffsetMillseconds": 20000,
   "savedByUser": false
 }
      
```

  
<a id="ID4EDG"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
该服务返回的状态代码之一此部分中使用此方法对此资源所做的请求的响应。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| “确定”| 成功检索会话。| 
| 400| 错误请求| 在请求正文中，没有错误或用户通过他们的配额。| 
| 401| 未授权| 没有在请求中的身份验证令牌格式问题。| 
| 403| 已禁止| 声明已丢失，或者 DeviceType 不需要一些。| 
| 503| 不允许| 该服务或一些下游的依赖项都已关闭。 使用标准后关闭行为重试。| 
  
<a id="ID4EVAAC"></a>

 
## <a name="response-body"></a>响应正文
 
[InitialUploadResponse](../../json/json-initialuploadresponse.md)对象或 JSON 格式的[ServiceErrorResponse](../../json/json-serviceerrorresponse.md)对象可以响应。
  
<a id="ID4EFBAC"></a>

 
## <a name="sample-response"></a>示例响应
 

```cpp
{
   "clipName": "ClipName",
   "gameClipId": "6b364924-5650-480f-86a7-fc002a1ee752",  
   "titleName": "TitleName",
   "uploadUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container",
   "largeThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/large",
   "smallThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/small"
 }
         
```

  
<a id="ID4EOBAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EQBAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/me/scids/{scid}/clips](uri-usersmescidclips.md)

   
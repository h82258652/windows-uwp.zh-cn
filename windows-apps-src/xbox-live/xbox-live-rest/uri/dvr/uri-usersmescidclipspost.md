---
title: POST (/users/me/scids/{scid}/clips)
assetID: 44535926-9fb8-5498-b1c8-514c0763e6c9
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipspost.html
description: " POST (/users/me/scids/{scid}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7a8973390ccbf5dd9980410f60f03a7edd78c134
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608782"
---
# <a name="post-usersmescidsscidclips"></a>POST (/users/me/scids/{scid}/clips)
发出初始上传请求。 这些 Uri 的域是`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，取决于 URI 相关的函数。
 
  * [备注](#ID4EX)
  * [URI 参数](#ID4EFB)
  * [Authorization](#ID4EQB)
  * [所需的请求标头](#ID4EKC)
  * [可选的请求标头](#ID4ENE)
  * [请求正文](#ID4ENF)
  * [示例请求](#ID4E1F)
  * [HTTP 状态代码](#ID4EDG)
  * [响应正文](#ID4EVAAC)
  * [示例响应](#ID4EFBAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>备注
 
这是 GameClip 上传过程的第一部分。 在捕获的视频，建议调用 GameClips 服务立即获取的 ID 和 URI 的位数上, 传，即使在上传不计划立即启动。 此调用将执行用户配额检查和其他检查，通过内容隔离、 隐私性，依此类推，若要查看是否视频应甚至计划进行上传客户端。 此调用的积极响应指示服务是愿意接受上传的视频剪辑。 上传的所有剪辑必须都关联具有特定标题 （通过 SCID) 接受系统中。
 
此调用不是幂等;后续调用将导致不同 Id 和要颁发的 Uri。 重试次数失败时应遵循标准的客户端回退行为。
  
<a id="ID4EFB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| scid| 字符串| 正在访问的资源的服务的配置 ID。 必须与匹配身份验证的用户的 SCID。| 
  
<a id="ID4EQB"></a>

 
## <a name="authorization"></a>授权
 
此方法需要有以下声明：
 
   * xuid
   * 设备类型-必须是要上传的设备
   * DeviceId
   * TitleId
   * TitleSandboxId
   
<a id="ID4EKC"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值：<b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。示例：1，vnext。| 
| 内容类型| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序 /json</b>。| 
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序 /json</b>。| 
  
<a id="ID4ENE"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 字符串| 可接受的压缩编码。 示例值： gzip、 deflate，标识。| 
  
<a id="ID4ENF"></a>

 
## <a name="request-body"></a>请求正文
 
请求的正文应是[InitialUploadRequest](../../json/json-initialuploadrequest.md)以 JSON 格式的对象。
  
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
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 确定| 已成功检索该会话。| 
| 400| 无效的请求| 请求正文中出现错误或用户通过其配额。| 
| 401| 未经授权| 没有在请求中的身份验证令牌格式的问题。| 
| 403| 已禁止| 一些必需的声明丢失，或者不是设备类型。| 
| 503| 不可接受| 此服务或某些下游的依赖关系都已关闭。 使用标准的回退行为重试。| 
  
<a id="ID4EVAAC"></a>

 
## <a name="response-body"></a>响应正文
 
响应可进行[InitialUploadResponse](../../json/json-initialuploadresponse.md)对象或[ServiceErrorResponse](../../json/json-serviceerrorresponse.md)以 JSON 格式的对象。
  
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

   
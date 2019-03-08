---
title: GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})
assetID: dbd60c93-9d8e-609b-0ae3-b3f7ee26ba2d
permalink: en-us/docs/xboxlive/rest/uri-usersowneridscidclipsgameclipidget.html
description: " GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5d2858b11bf8fb902ea07a016c8f41db375013f9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640952"
---
# <a name="get-usersowneridscidsscidclipsgameclipid"></a>GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})
如果已知的所有 Id，以找到它从系统获取单个游戏剪辑。 这些 Uri 的域是`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，取决于 URI 相关的函数。
 
  * [备注](#ID4EX)
  * [URI 参数](#ID4EVB)
  * [Authorization](#ID4EAC)
  * [所需的请求标头](#ID4EUH)
  * [可选的请求标头](#ID4EBCAC)
  * [请求正文](#ID4ETDAC)
  * [HTTP 状态代码](#ID4E5DAC)
  * [所需的响应标头](#ID4EQIAC)
  * [可选的响应标头](#ID4EJLAC)
  * [响应正文](#ID4EJMAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>备注
 
所有数据进行查询剪辑现已推出**GameClip**对象从任何元数据查询中返回。 **XUID**， **ServiceConfigId**， **GameClipId**并**SandboxId**在请求的声明，才能获取通过此 API 的单个游戏剪辑。 如果游戏的剪辑已被标记为强制执行，或内容隔离或隐私检查确定用户没有权限，以获取特定游戏剪辑，API 将返回 HTTP 状态代码为 404 （未找到）。
 
**SandboxId**现在从声明中 XToken 检索，并强制执行。 如果**SandboxId**不存在，则娱乐发现服务 (EDS) 将会引发一个 400 错误请求错误。
 
此 API 还必须用于刷新过期的 Uri。 查询完成后，游戏剪辑任何过期的 Uri 将进行相应地刷新。 

> [!NOTE] 
> URI 刷新可以最多需要 30 到 40 秒后完成此请求。 如果 URI 已过期，尝试将其立即用于流式处理操作将获取从 IIS 平滑流式处理服务器的 HTTP 500 状态代码。 我们正在研究如何缩短此操作，并将随着该工作的进行相应地更新本说明。 


  
<a id="ID4EVB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | 
| ownerId| 字符串| 其资源的访问的用户的用户标识。 支持的格式:"me"或"xuid(123456789)"。 最大长度：16.| 
| scid| 字符串| 正在访问的资源的服务的配置 ID。 必须与匹配身份验证的用户的 SCID。| 
| gameClipId| 字符串| GameClip 正在访问的资源 ID。| 
  
<a id="ID4EAC"></a>

 
## <a name="authorization"></a>授权
 
使用授权声明 | 声明| 在任务栏的搜索框中键入| 是否为必需？| 示例值| 备注| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| xuid| 64 位有符号的整数| 是| 1234567890|  | 
| TitleId| 64 位有符号的整数| 是| 1234567890| 用于<b>内容隔离</b>检查。| 
| SandboxId| 十六进制的二进制文件| 是|  | 将定向到正确的区域进行查找，系统和用于<b>内容隔离</b>检查。| 
  
资源上的隐私设置的效果 | 发出请求的用户| 面向用户的隐私设置| 行为| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 我| -| 如所述。| 
| 友元| 每个人| 禁止访问。| 
| 友元| 仅朋友| 禁止访问。| 
| 友元| 已阻止| 禁止访问。| 
| 非友元用户| 每个人| 禁止访问。| 
| 非友元用户| 仅朋友| 禁止访问。| 
| 非友元用户| 已阻止| 禁止访问。| 
| 第三方站点| 每个人| 禁止访问。| 
| 第三方站点| 仅朋友| 禁止访问。| 
| 第三方站点| 已阻止| 禁止访问。| 
 
<a id="ID4EUH"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值：<b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。示例：1，vnext。| 
| 内容类型| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序 /json</b>。| 
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序 /json</b>。| 
| Cache-Control| 字符串| 请求正常，以指定缓存行为。| 
  
<a id="ID4EBCAC"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 字符串| 可接受的压缩编码。 示例值： gzip、 deflate，标识。| 
| ETag| 字符串| 用于缓存优化。 示例值："686897696a7c876b7e"。| 
| 范围| 字符串|  | 
  
<a id="ID4ETDAC"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送的任何对象。
  
<a id="ID4E5DAC"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 确定| 已成功检索该会话。| 
| 301| 已永久移动|  | 
| 307| 临时重定向|  | 
| 400| 无效的请求| 服务无法理解请求格式不正确。 通常是一个无效的参数。| 
| 401| 未经授权| 请求需要用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 未找到| 找不到指定的资源。| 
| 406| 不可接受| 不支持资源版本。| 
| 408| 请求超时| 该请求花费了太长时间才能完成。| 
| 410| 消失| 请求的资源不再可用。| 
  
<a id="ID4EQIAC"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。示例：1，vnext。| 
| 内容类型| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序 /json</b>。| 
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序 /json</b>。| 
| Cache-Control| 字符串| 请求正常，以指定缓存行为。| 
| 重试间隔| 字符串| 指示客户端在不可用的服务器的情况下请稍后再试。 示例：<b>应用程序 /json</b>。| 
| 改变| 字符串| 指示下游代理如何缓存响应。 示例：<b>应用程序 /json</b>。| 
  
<a id="ID4EJLAC"></a>

 
## <a name="optional-response-headers"></a>可选的响应标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| 字符串| 用于缓存优化。 示例值："686897696a7c876b7e"。| 
  
<a id="ID4EJMAC"></a>

 
## <a name="response-body"></a>响应正文
 
<a id="ID4EPMAC"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
{
 "gameClip": {
   "xuid": "2716903703773872",
   "clipName": "1234567890",
   "titleName": "",
   "gameClipId": "cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000",
   "state": "Published",
   "dateRecorded": "2013-05-08T21:32:17.4201279Z",
   "lastModified": "2013-05-08T21:34:48.8117829Z",
   "userCaption": "",
   "type": "DeveloperInitiated",
   "source": "Console",
   "visibility": "Public",
   "durationInSeconds": 30,
   "scid": "00000000-0000-0012-0023-000000000070",
   "titleId": 0,
   "rating": 0,
   "ratingCount": 0,
   "views": 0,
   "titleData": "",
   "systemProperties": "",
   "savedByUser": false,
   "thumbnails": [
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000\/thumbnails\/large",
       "fileSize": 0,
       "thumbnailType": "Large"
     },
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000\/thumbnails\/small",
       "fileSize": 0,
       "thumbnailType": "Small"
     }
   ],
   "gameClipUris": [
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000",
       "fileSize": 9332015,
       "uriType": "Download",
       "expiration": "9999-12-31T23:59:59.9999999"
     }
   ]
 }
}
         
```

   
<a id="ID4EZMAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E2MAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/{ownerId}/scids/{scid}/clips/{gameClipId}](uri-usersowneridscidclipsgameclipid.md)

  
<a id="ID4EFNAC"></a>

 
##### <a name="further-information"></a>详细信息 

[Marketplace Uri](../marketplace/atoc-reference-marketplace.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   
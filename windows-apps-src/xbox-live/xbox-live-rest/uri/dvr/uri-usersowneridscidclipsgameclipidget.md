---
title: GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})
assetID: dbd60c93-9d8e-609b-0ae3-b3f7ee26ba2d
permalink: en-us/docs/xboxlive/rest/uri-usersowneridscidclipsgameclipidget.html
author: KevinAsgari
description: " GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 13b96b0d2f1f674533dd2c070bd1a10884bb7370
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2018
ms.locfileid: "4174105"
---
# <a name="get-usersowneridscidsscidclipsgameclipid"></a>GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})
如果已知的所有 Id，以找到它，请从系统获取单个游戏剪辑。 这些 Uri 的域是`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，具体问题的 URI 的函数取决于。
 
  * [备注](#ID4EX)
  * [URI 参数](#ID4EVB)
  * [授权](#ID4EAC)
  * [需的请求标头](#ID4EUH)
  * [可选的请求标头](#ID4EBCAC)
  * [请求正文](#ID4ETDAC)
  * [HTTP 状态代码](#ID4E5DAC)
  * [所需的响应标头](#ID4EQIAC)
  * [可选的响应标头](#ID4EJLAC)
  * [响应正文](#ID4EJMAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>备注
 
查询该剪辑的所有数据都在从任何元数据查询返回的**GameClip**对象中可用。 **XUID**、 **ServiceConfigId**、 **GameClipId**和**SandboxId**请求的声明中需要获取此 API 通过单个游戏剪辑。 如果游戏剪辑被标记为强制执行，或内容隔离或隐私检查确定用户没有权限，以获取特定游戏剪辑，该 API 将返回 HTTP 状态代码的 404 （未找到）。
 
现在，从 XToken 声明检索并强制执行**SandboxId** 。 如果不存在**SandboxId** ，则娱乐发现服务 (EDS) 将引发 400 错误请求错误。
 
此外必须使用此 API 以刷新过期的 Uri。 完成查询时，该游戏的剪辑任何过期的 Uri 将被相应地刷新。 

> [!NOTE] 
> URI 刷新可能需要最多 30 40 秒后完成此请求。 如果 URI 已过期，尝试使用它立即进行流式处理的操作将获得从 IIS 平滑流式处理服务器的 HTTP 500 状态代码。 我们正在运行的方法来缩短，，并将该工作随着相应地更新此注意。 


  
<a id="ID4EVB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | --- | 
| ownerId| 字符串| 用户的用户身份的正在访问其资源。 支持的格式:"me"或"xuid(123456789)"。 最大长度： 16。| 
| scid| 字符串| 正在访问的资源的服务配置 ID。 必须匹配的身份验证的用户的 SCID。| 
| gameClipId| 字符串| GameClip 所访问的资源的 ID。| 
  
<a id="ID4EAC"></a>

 
## <a name="authorization"></a>授权
 
使用授权声明 | 声明| 类型| 是否必需？| 示例值| 备注| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Xuid| 64 位有符号整数| 是| 1234567890|  | 
| TitleId| 64 位有符号整数| 是| 1234567890| 用于<b>内容隔离</b>检查。| 
| SandboxId| 十六进制的二进制文件| 是|  | 可将定向到正确的区域的查找，系统和用于<b>内容隔离</b>检查。| 
  
在资源的隐私设置的效果 | 请求的用户| 目标用户的隐私设置| 行为| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 我| -| 所述。| 
| 好友| 每个人都| 禁止访问。| 
| 好友| 仅好友| 禁止访问。| 
| 好友| 阻止| 禁止访问。| 
| 非好友用户| 每个人都| 禁止访问。| 
| 非好友用户| 仅好友| 禁止访问。| 
| 非好友用户| 阻止| 禁止访问。| 
| 第三方网站| 每个人都| 禁止访问。| 
| 第三方网站| 仅好友| 禁止访问。| 
| 第三方网站| 阻止| 禁止访问。| 
 
<a id="ID4EUH"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值： <b>Xauth =&lt;authtoken ></b>| 
| X RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/数。 验证在标头、 身份验证令牌等中的声明的有效性后仅为请求路由到该服务。示例： 1，vnext。| 
| Content-Type| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序/json</b>。| 
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序/json</b>。| 
| 缓存控制| 字符串| 若要指定缓存行为的礼貌请求。| 
  
<a id="ID4EBCAC"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 字符串| 可接受的压缩编码。 示例值： gzip，桥，标识。| 
| ETag| 字符串| 用于缓存优化。 示例值:"686897696a7c876b7e"。| 
| 范围| 字符串|  | 
  
<a id="ID4ETDAC"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送任何对象。
  
<a id="ID4E5DAC"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
此部分中使用此方法对此资源所做的请求的响应，该服务返回的状态代码之一。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| “确定”| 已成功检索会话。| 
| 301| 已永久移动|  | 
| 307| 临时重定向|  | 
| 400| 错误请求| 服务可能不理解格式不正确的请求。 通常无效参数。| 
| 401| 未授权| 请求要求用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 找不到| 找不到指定的资源。| 
| 406| 不允许| 不支持资源版本。| 
| 408| 请求超时| 请求所花的时间太长，才能完成。| 
| 410| 前面| 所请求的资源不再可用。| 
  
<a id="ID4EQIAC"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/数。 验证在标头、 身份验证令牌等中的声明的有效性后仅为请求路由到该服务。示例： 1，vnext。| 
| Content-Type| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序/json</b>。| 
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序/json</b>。| 
| 缓存控制| 字符串| 若要指定缓存行为的礼貌请求。| 
| 重试后| 字符串| 指示客户端在不可用的服务器的情况下稍后重试。 示例：<b>应用程序/json</b>。| 
| 不同| 字符串| 指示下游代理如何缓存响应。 示例：<b>应用程序/json</b>。| 
  
<a id="ID4EJLAC"></a>

 
## <a name="optional-response-headers"></a>可选的响应标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| 字符串| 用于缓存优化。 示例值:"686897696a7c876b7e"。| 
  
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

[市场 URI](../marketplace/atoc-reference-marketplace.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   
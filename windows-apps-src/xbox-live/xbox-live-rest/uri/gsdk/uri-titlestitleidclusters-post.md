---
title: POST (/titles/{titleId}/clusters)
assetID: 0977b0b0-872d-f7ad-9ba0-30d56cff4912
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidclusters-post.html
author: KevinAsgari
description: " POST (/titles/{titleId}/clusters)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c87af8cb76ce452c067edddb55c382d8c604ca25
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5879409"
---
# <a name="post-titlestitleidclusters"></a>POST (/titles/{titleId}/clusters)
允许客户端创建 Xbox Live 计算服务器实例的 URI。 这些 Uri 的域是`gameserverms.xboxlive.com`。
 
  * [URI 参数](#ID4EX)
  * [需的请求标头](#ID4EGB)
  * [授权](#ID4ELD)
  * [请求正文](#ID4EWD)
  * [所需的响应标头](#ID4EZE)
  * [响应正文](#ID4E5G)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 描述| 
| --- | --- | 
| titleId| 游戏应在其中操作该请求 ID。| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>主机名

gameserverms.xboxlive.com
 
<a id="ID4EGB"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
当发出请求下, 表中所示的标头是必需的。
 
| 标头| 值| 说明| 
| --- | --- | --- | --- | --- | 
| 用户代理|  | 有关用户代理发出请求的信息。| 
| 内容类型| 应用程序/json| 提交的数据的类型。| 
| Host| gameserverms.xboxlive.com|  | 
| Content-Length|  | 请求对象的长度。| 
| x xbl 协定版本| 1| API 协定版本。| 
| 授权| XBL3.0 x = [哈希];[令牌]| 身份验证令牌。| 
  
<a id="ID4ELD"></a>

 
## <a name="authorization"></a>授权
 
请求必须包含有效的 Xbox Live 授权标头。 如果调用方不允许访问此资源，该服务将在响应中返回 403 禁止访问。 如果在标头是无效或不存在，该服务在响应中返回 401 未经授权。
  
<a id="ID4EWD"></a>

 
## <a name="request-body"></a>请求正文
 
请求必须包含一个具有以下成员的 JSON 对象。
 
| 成员| 说明| 
| --- | --- | --- | --- | --- | --- | --- | 
| sessionId| 从 MPSD 会话标识符。| 
| abortIfQueued| 可选参数，该设置为 true 告知 GSMS 不进行排队此会话的资源，如果它可以不立即完成。 如果请求已中止，因为此值为 true，将包含响应对象<code>"fulfillmentState" : "Aborted"</code>。 | 
 
<a id="ID4ERE"></a>

 
### <a name="sample-request"></a>示例请求
 

```cpp
{
  "sessionId" : "/serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/session/scott1",
  "abortIfQueued" : "true"
}

      
```

   
<a id="ID4EZE"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
响应将始终会包括下表中所示的标头。
 
| 标头| 值| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 缓存控制|  | 必须通过沿请求/响应链的所有缓存机制的话应遵循的指令。| 
| 内容类型| 应用程序/json| 在响应中的数据的类型。| 
| Content-Length|  | 响应正文的长度。| 
| X 内容类型选项|  |  | 
| X XblCorrelationId|  | 响应正文的 mime 类型。| 
| 日期|  |  | 
  
<a id="ID4E5G"></a>

 
## <a name="response-body"></a>响应正文
 
如果在调用成功，该服务将返回一个具有以下成员的 JSON 对象。
 
| 成员| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| pollIntervalMilliseconds| 建议毫秒才能完成轮询间隔。 请注意，这不是估计，当群集将准备好，但而对调用方轮询给定当前池的请求和实施情况率和订阅状态更新的频率的建议。| 
| fulfillmentState| 指示是否提供的会话被立即分配一个资源，"完成"，添加到队列的未来的资源、 可用性"队列"，或终止，"中止"，因为无法满足请求时立即请求为"true"的指定的 abortIfQueued。 | 
 
<a id="ID4EWH"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
{
  "pollIntervalMilliseconds" : "1000",
  "fulfillmentState" : "Fulfilled" | "Queued" | "Aborted"
}
      
```

   
<a id="remarks"></a>

 
## <a name="remarks"></a>备注
 
收到以下响应代码时，游戏应仅重试对服务调用：
 
   * 408-服务器超时
   * 429： 请求过多
   * 500-服务器错误
   * 502-错误的网关
   * 503-服务不可用
   * 504-网关超时
   
<a id="ID4EFBAC"></a>

 
## <a name="see-also"></a>另请参阅
 [/titles/{titleId}/clusters](uri-titlestitleidclusters.md)

  
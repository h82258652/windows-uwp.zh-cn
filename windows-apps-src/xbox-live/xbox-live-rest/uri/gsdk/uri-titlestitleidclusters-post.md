---
title: POST (/titles/{titleId}/clusters)
assetID: 0977b0b0-872d-f7ad-9ba0-30d56cff4912
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidclusters-post.html
description: " POST (/titles/{titleId}/clusters)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 91d7c49628914f887c5d2243942e10e47d47b095
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608902"
---
# <a name="post-titlestitleidclusters"></a>POST (/titles/{titleId}/clusters)
允许客户端创建 Xbox Live 计算服务器实例的 URI。 这些 Uri 的域是`gameserverms.xboxlive.com`。
 
  * [URI 参数](#ID4EX)
  * [所需的请求标头](#ID4EGB)
  * [Authorization](#ID4ELD)
  * [请求正文](#ID4EWD)
  * [所需的响应标头](#ID4EZE)
  * [响应正文](#ID4E5G)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 描述| 
| --- | --- | 
| titleId| 请求应作用于的标题的 ID。| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>主机名

gameserverms.xboxlive.com
 
<a id="ID4EGB"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
当发出请求，如下表中所示的标头是必需的。
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | 
| 用户代理|  | 有关发出请求的用户代理信息。| 
| 内容类型| 应用程序/json| 正在提交的数据类型。| 
| 主机| gameserverms.xboxlive.com|  | 
| 内容长度|  | 请求对象的长度。| 
| x-xbl-contract-version| 1| API 协定版本。| 
| 授权| XBL3.0 x=[hash];[token]| 身份验证令牌。| 
  
<a id="ID4ELD"></a>

 
## <a name="authorization"></a>授权
 
该请求必须包含有效的 Xbox Live 授权标头。 如果调用方不允许访问此资源，服务在响应中返回 403 禁止访问。 如果标头无效或缺失，服务在响应中返回 401 未授权。
  
<a id="ID4EWD"></a>

 
## <a name="request-body"></a>请求正文
 
请求必须包含具有以下成员的 JSON 对象。
 
| 成员| 描述| 
| --- | --- | --- | --- | --- | --- | --- | 
| sessionId| 从 MPSD 会话标识符。| 
| abortIfQueued| 可选参数，此操作时设置为 true 指示 GSMS 不进行排队此会话的资源，如果它可以不立即实现。 如果请求被中止，因为此值为 true，则响应对象将包含<code>"fulfillmentState" : "Aborted"</code>。 | 
 
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
 
响应将始终包括下表中所示的标头。
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Cache-Control|  | 必须通过所有缓存机制请求/响应链上服从的指令。| 
| 内容类型| 应用程序/json| 在响应中的数据类型。| 
| 内容长度|  | 响应正文的长度。| 
| X-Content-Type-Options|  |  | 
| X-XblCorrelationId|  | 响应正文的 mime 类型。| 
| 日期|  |  | 
  
<a id="ID4E5G"></a>

 
## <a name="response-body"></a>响应正文
 
如果调用成功，服务将返回具有以下成员的 JSON 对象。
 
| 成员| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| pollIntervalMilliseconds| 建议以毫秒为单位来轮询完成状态的时间间隔。 请注意，这不是时群集将准备就绪后，估计但而不是对调用方轮询给定的订阅和请求和执行率的当前池的状态更新的频率的建议。| 
| fulfillmentState| 指示是否立即提供的会话已分配资源，"完成"，添加到队列以备将来资源的可用性"已排队"，或已中止，"中止"，因为无法完成请求后，将立即请求为"true"的指定的 abortIfQueued。 | 
 
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
 
接收到以下的响应代码时，标题应仅重试对服务调用：
 
   * 408 — 服务器超时
   * 429 — 请求过多
   * 500-服务器错误
   * 502-错误的网关
   * 503-服务不可用
   * 504-网关超时
   
<a id="ID4EFBAC"></a>

 
## <a name="see-also"></a>另请参阅
 [/titles/{titleId}/clusters](uri-titlestitleidclusters.md)

  
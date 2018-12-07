---
title: DELETE (/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type})
assetID: b4ddc3d2-890d-f677-0109-45d318c3128d
permalink: en-us/docs/xboxlive/rest/uri-sessionssessionidscidssciddatapathandfilenametype-delete.html
description: " DELETE (/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8864892392e1cfddba7f510fc0f9e7efdb2dd036
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8784269"
---
# <a name="delete-sessionssessionidscidssciddatapathandfilenametype"></a>DELETE (/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type})
删除文件。 这些 Uri 的域是`titlestorage.xboxlive.com`。
 
  * [URI 参数](#ID4EX)
  * [授权](#ID4EEB)
  * [需的请求标头](#ID4ERB)
  * [可选的请求标头](#ID4E1C)
  * [请求正文](#ID4EWD)
  * [HTTP 状态代码](#ID4EDE)
  * [响应正文](#ID4EUBAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数 
 
| 参数| 类型| 描述| 
| --- | --- | --- | 
| sessionId| 字符串| 若要查找会话的 ID。| 
| scid| guid| 若要查找的服务配置 ID。| 
| pathAndFileName| 字符串| 要访问的项的路径和文件名。 有效的字符 （至当前阶段并包括最终正斜杠） 的路径部分包括 (A-Z) 的大写字母、 小写字母 (a-z)、 数字 (0-9) 下划线 (_)，并且反斜杠 （/）。路径部分可能为空。有效的字符的文件名称部分 （最终正斜杠后面的所有内容） 包含大写字母 (A-Z)、 小写字母 (a-z)、 数字 (0-9)、 下划线 (_)，句点 （.） 和连字符 （-）。 文件名称不能为空，以句号结尾或包含两个连续句点。| 
| type| 字符串| 数据的格式。 可能的值为二进制文件或 json。| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>授权 
 
请求必须包含有效的 Xbox LIVE 授权标头。 如果调用方不允许访问此资源，该服务将返回 403 禁止访问响应。 如果标头是无效或不存在，该服务将返回 401 未经授权的响应。 
  
<a id="ID4ERB"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | --- | 
| x xbl 协定版本| 1| API 协定版本。| 
| 授权| XBL3.0 x = [哈希];[令牌]| STS 身份验证令牌。 STSTokenString 替换为由身份验证请求返回的令牌。 有关检索 STS 令牌和创建授权标头的其他信息，请参阅 Authenticating 和授权 Xbox LIVE 服务请求。| 
  
<a id="ID4E1C"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标题| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| 指定必须匹配要完成此操作的现有项目 ETag。| 
  
<a id="ID4EWD"></a>

 
## <a name="request-body"></a>请求正文 
 
此请求的正文中不发送任何对象。
  
<a id="ID4EDE"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码 
 
此部分中使用此方法对此资源所做的请求的响应，该服务返回的状态代码之一。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| “确定” | 该文件已成功，删除，或不存在。| 
| 201| 已创建 | 已创建实体。| 
| 400| 错误请求 | 服务可能不理解格式不正确的请求。 通常无效参数。| 
| 401| 未授权 | 请求要求用户身份验证。| 
| 403| 已禁止 | 为用户或服务不允许该请求。| 
| 404| 找不到 | 找不到指定的资源。| 
| 406| 不允许 | 资源版本不受支持。| 
| 408| 请求超时 | 请求所花的时间太长，才能完成。| 
| 500| 内部服务器错误 | 服务器时遇到意外的情况，执行此请求将阻止它。| 
| 503| 服务不可用 | 请求已被阻止，以秒为单位 （例如 5 秒更高版本） 的客户端重试值后重试请求。| 
  
<a id="ID4EUBAC"></a>

 
## <a name="response-body"></a>响应正文 
 
该响应正文中不发送任何对象。
  
<a id="ID4EDCAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EFCAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type}](uri-sessionssessionidscidssciddatapathandfilenametype.md)

   
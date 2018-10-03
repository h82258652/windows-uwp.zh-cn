---
title: GET (/global/scids/{scid}/data/{pathAndFileName},{type})
assetID: 5ca8e0dd-3c45-1b7b-022e-d5d61414fd7d
permalink: en-us/docs/xboxlive/rest/uri-globalscidssciddatapathandfilenametype-get.html
author: KevinAsgari
description: " GET (/global/scids/{scid}/data/{pathAndFileName},{type})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 03da8b482dcfb8a4972fee69c0e3995d792cb87a
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2018
ms.locfileid: "4268269"
---
# <a name="get-globalscidssciddatapathandfilenametype"></a>GET (/global/scids/{scid}/data/{pathAndFileName},{type})
下载文件。 这些 Uri 的域是`titlestorage.xboxlive.com`。
 
  * [URI 参数](#ID4EX)
  * [授权](#ID4ECB)
  * [可选的查询字符串参数](#ID4EPB)
  * [需的请求标头](#ID4EZC)
  * [可选的请求标头](#ID4ECE)
  * [请求正文](#ID4EMF)
  * [HTTP 状态代码](#ID4EZF)
  * [响应标头](#ID4EMDAC)
  * [响应正文](#ID4EPEAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 描述| 
| --- | --- | --- | 
| scid| guid| 若要查找的服务配置 ID。| 
| pathAndFileName| 字符串| 若要访问该项目的路径和文件名称。 有效的字符 （达且包括最终正斜杠） 的路径部分包括 (A-Z) 的大写字母、 小写字母 (a-z)、 数字 (0-9)，下划线 (_) 和正斜杠 （/）。路径部分可能为空。有效的字符的文件名称部分 （最终正斜杠后面的所有内容） 包含大写字母 (A-Z)、 小写字母 (a-z)、 数字 (0-9)，下划线 (_)，句点 （.） 和连字符 （-）。 文件名称不能为空，以句号结尾或包含两个连续句点。| 
| type| 字符串| 数据的格式。 可能的值为： 二进制文件、 配置或 json。| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>授权 
 
请求必须包含有效的 Xbox LIVE 授权标头。 如果调用方不允许访问此资源，该服务将返回 403 禁止访问响应。 如果在标头丢失或无效，该服务将返回 401 未经授权的响应。 
  
<a id="ID4EPB"></a>

 
## <a name="optional-query-string-parameters"></a>可选的查询字符串参数 
 
因 blob 类型。 二进制文件 blob 不支持查询参数。
 
| 参数| 类型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 选择| 字符串| 仅 json 类型时才可用。 指定响应只应包含某个属性/值的 JSON，通过此参数确定。 使用一个"点"（.） 来指定子属性和放在方括号 ([和]) 来指定数组索引。 例如，"array1 [4].prop2"指定"array1"数组索引 4"prop2"属性。| 
| customSelector| 字符串| 用于配置类型文件。 指示哪些自定义虚拟节点包含。 配置类型文件的详细信息，请参阅标题存储。| 
  
<a id="ID4EZC"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x xbl 协定版本| 1| API 协定版本。| 
| 授权| XBL3.0 x = [哈希];[令牌]| STS 身份验证令牌。 STSTokenString 替换为由身份验证请求返回的令牌。 有关检索 STS 令牌和创建授权标头的其他信息，请参阅 Authenticating 和授权 Xbox LIVE 服务请求。| 
  
<a id="ID4ECE"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标题| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| 指定必须匹配要完成此操作的现有项目 ETag。| 
| If-None-Match| 指定 ETag，不匹配任何现有项目，以完成此操作。| 
| 范围| 指定要下载字节的范围。 遵循标准的 HTTP 范围标头格式。| 
  
<a id="ID4EMF"></a>

 
## <a name="request-body"></a>请求正文 
 
此请求的正文中不发送任何对象。
  
<a id="ID4EZF"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码 
 
该服务返回的状态代码之一此部分中使用此方法对此资源所做的请求的响应。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| “确定” | 请求已成功。| 
| 201| 已创建 | 创建实体。| 
| 400| 错误请求 | 服务可能不理解格式不正确的请求。 通常无效参数。| 
| 401| 未授权 | 请求要求用户身份验证。| 
| 403| 已禁止 | 为用户或服务不允许该请求。| 
| 404| 找不到 | 找不到指定的资源。| 
| 406| 不允许 | 不支持资源版本。| 
| 408| 请求超时 | 请求所花的时间太长，才能完成。| 
| 500| 内部服务器错误 | 服务器时遇到意外的情况，执行此请求将阻止它。| 
| 503| 服务不可用 | 请求已被阻止，以秒为单位 （例如 5 秒更高版本） 的客户端重试值后重试请求。| 
  
<a id="ID4EMDAC"></a>

 
## <a name="response-headers"></a>响应标头
 
| 标题| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| ETag 是由对某一 URL 找到资源的特定版本的 web 服务器分配一个不透明标识符。 如果在该 URL 处的资源内容发生改变，被分配新功能和不同的 ETag。| 
| 内容区域| 如果这部分下载，此标头指定下载的字节范围。| 
  
<a id="ID4EPEAC"></a>

 
## <a name="response-body"></a>响应正文
如果在调用成功，该服务将返回文件的内容。  
<a id="ID4EYEAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E1EAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[/global/scids/{scid}/data/{pathAndFileName},{type}](uri-globalscidssciddatapathandfilenametype.md)

  
<a id="ID4EGFAC"></a>

 
##### <a name="reference--titleblob-jsonjsonjson-titleblobmd"></a>引用[TitleBlob (JSON)](../../json/json-titleblob.md)

   
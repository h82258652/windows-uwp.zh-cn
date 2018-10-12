---
title: POST (/titles/{titleId}/variants)
assetID: 84303448-5a11-d96f-907d-77f57f859741
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidvariants-post.html
author: KevinAsgari
description: " POST (/titles/{titleId}/variants)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 246427b772403ca07adac2a4b1b07ec159142049
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2018
ms.locfileid: "4537182"
---
# <a name="post-titlestitleidvariants"></a>POST (/titles/{titleId}/variants)
URI 由客户端检索列表的游戏的变体，为指定的游戏 id。这些 Uri 的域是`gameserverds.xboxlive.com`和`gameserverms.xboxlive.com`。
 
  * [URI 参数](#ID4EZ)
  * [需的请求标头](#ID4EIB)
  * [可选的请求标头](#ID4EED)
  * [授权](#ID4E3D)
  * [请求正文](#ID4EEE)
  * [所需的响应标头](#ID4ELF)
  * [可选的响应标头](#ID4EMG)
  * [响应正文](#ID4EEH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 描述| 
| --- | --- | 
| titleid| 游戏应在其中操作该请求 ID。| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>主机名

gameserverds.xboxlive.com
 
<a id="ID4EIB"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
当发出请求下, 表中所示的标头是必需的。
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | 
| 内容类型| 应用程序/json| 提交的数据的类型。| 
| Host| gameserverds.xboxlive.com|  | 
| Content-Length|  | 请求对象的长度。| 
| x xbl 协定版本| 1| API 协定版本。| 
| 授权| XBL3.0 x = [哈希];[令牌]| 身份验证令牌。| 
  
<a id="ID4EED"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
当发出请求下, 表中所示的标头是可选的。
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| X XblCorrelationId|  | 请求正文的 mime 类型。| 
  
<a id="ID4E3D"></a>

 
## <a name="authorization"></a>授权

请求必须包含有效的 Xbox Live 授权标头。 如果调用方不允许访问此资源，该服务将在响应中返回 403 禁止访问。 如果标头是无效或不存在，该服务将在响应中返回 401 未经授权。
 
<a id="ID4EEE"></a>

 
## <a name="request-body"></a>请求正文
 
请求必须包含一个具有以下成员的 JSON 对象。
 
| 成员| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 区域设置| 若要返回的变体本地。| 
| maxVariants| 变体，要返回的最大数量。| 
| publisherOnly|  | 
| 限制|  | 
 
<a id="ID4EDF"></a>

 
### <a name="sample-request"></a>示例请求
 

```cpp
{
  "locale": "en-us",
  "maxVariants": "100",
  "publisherOnly": "false",
  "restriction": null
}

```

   
<a id="ID4ELF"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
响应将始终会包括下表中所示的标头。
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 内容类型| 应用程序/json| 响应正文中的数据的类型。| 
| Content-Length|  | 响应正文的长度。| 
  
<a id="ID4EMG"></a>

 
## <a name="optional-response-headers"></a>可选的响应标头
 
响应可能包括的标头，如下所示。
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X XblCorrelationId|  | 响应正文的 mime 类型。| 
  
<a id="ID4EEH"></a>

 
## <a name="response-body"></a>响应正文
 
如果在调用成功，该服务将返回一个具有以下成员的 JSON 对象。
 
| 成员| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 变体| 变体的数组。| 
| variantId| 变体的 Id。| 
| name| 变体的名称。| 
| isPublisher|  | 
| 排名|  | 
| gameVariantSchemaId|  | 
| variantSchemas| 数组的变体的架构。| 
| variantSchemaId| 架构的 Id。| 
| schemaContent| 架构内容| 
| name| 架构的名称| 
| gsiSets| GSI 集的数组。| 
| minRequiredPlayers| 最小的变体的玩家人数。| 
| maxAllowedPlayers| 最大为变体的玩家数。| 
| gsiSetId| GSI 集的 Id。| 
| gsiSetName| GSI 集的名称。| 
| selectionOrder|  | 
| variantSchemaId| 设置 varaint 架构 GSI 中使用的 id。| 
 
<a id="ID4EYBAC"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
{
 "variants": [
     { 
       "variantId": "8B6EF8A0-7807-42C4-9CB0-1D9B8B8CE742", 
       "name": "tankWarsV2.0",
       "isPublisher": "true",
       "rank": "1",
       "gameVariantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB"
     }],
  "variantSchemas": [
     {
        "variantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB",
        "schemaContent": "&lt;?xml version=\"1.0\" encoding=\"UTF-8\" ?>&lt;xs:schema xmlns:xs=\"http://www.w3.org/2001/XMLSchema\">&lt;xs:element name=\"root\">&lt;/xs:element>&lt;/xs:schema>"
        "name": "tanksSchema"
     }],
     "gsiSets":
     [{ 
          "minRequiredPlayers": "5", 
          "maxAllowedPlayers": "10", 
          "gsiSetId": "B28047F5-B52F-477E-97C2-4C1C39E31D42",
          "gsiSetName": "TanksGSISet",
          "selectionOrder": "1",
          "variantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB"
     }]
 }

  

```

   
<a id="ID4ERCAC"></a>

 
## <a name="see-also"></a>另请参阅
 [/titles/{titleId}/variants](uri-titlestitleidvariants.md)

  
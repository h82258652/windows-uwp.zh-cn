---
title: POST (/titles/{titleId}/variants)
assetID: 84303448-5a11-d96f-907d-77f57f859741
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidvariants-post.html
description: " POST (/titles/{titleId}/variants)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 17974ddf7dec26abac18ccee9fda5249bc9d656f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618492"
---
# <a name="post-titlestitleidvariants"></a>POST (/titles/{titleId}/variants)
检索一组 id。 指定的标题的游戏不同版本的客户端调用的 URI这些 Uri 的域是`gameserverds.xboxlive.com`和`gameserverms.xboxlive.com`。
 
  * [URI 参数](#ID4EZ)
  * [所需的请求标头](#ID4EIB)
  * [可选的请求标头](#ID4EED)
  * [Authorization](#ID4E3D)
  * [请求正文](#ID4EEE)
  * [所需的响应标头](#ID4ELF)
  * [可选的响应标头](#ID4EMG)
  * [响应正文](#ID4EEH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 描述| 
| --- | --- | 
| titleid| 请求应作用于的标题的 ID。| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>主机名

gameserverds.xboxlive.com
 
<a id="ID4EIB"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
当发出请求，如下表中所示的标头是必需的。
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | 
| 内容类型| 应用程序/json| 正在提交的数据类型。| 
| 主机| gameserverds.xboxlive.com|  | 
| 内容长度|  | 请求对象的长度。| 
| x-xbl-contract-version| 1| API 协定版本。| 
| 授权| XBL3.0 x=[hash];[token]| 身份验证令牌。| 
  
<a id="ID4EED"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
当发出请求，如下表中所示的标头是可选的。
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| X-XblCorrelationId|  | 请求正文的 mime 类型。| 
  
<a id="ID4E3D"></a>

 
## <a name="authorization"></a>授权

该请求必须包含有效的 Xbox Live 授权标头。 如果调用方不允许访问此资源，服务在响应中返回 403 禁止访问。 如果标头无效或缺失，服务在响应中返回 401 未授权。
 
<a id="ID4EEE"></a>

 
## <a name="request-body"></a>请求正文
 
请求必须包含具有以下成员的 JSON 对象。
 
| 成员| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| locale| 若要返回的变体本地。| 
| maxVariants| 返回的变体的最大数目。| 
| publisherOnly|  | 
| 限制|  | 
 
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
 
响应将始终包括下表中所示的标头。
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 内容类型| 应用程序/json| 响应正文中的数据类型。| 
| 内容长度|  | 响应正文的长度。| 
  
<a id="ID4EMG"></a>

 
## <a name="optional-response-headers"></a>可选的响应标头
 
响应可能包括如下所示的标头。
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-XblCorrelationId|  | 响应正文的 mime 类型。| 
  
<a id="ID4EEH"></a>

 
## <a name="response-body"></a>响应正文
 
如果调用成功，服务将返回具有以下成员的 JSON 对象。
 
| 成员| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 变体| 变体的数组。| 
| variantId| 变体的 Id。| 
| name| 变体的名称。| 
| isPublisher|  | 
| 排名|  | 
| gameVariantSchemaId|  | 
| variantSchemas| 变体的架构的数组。| 
| variantSchemaId| 架构的 Id。| 
| schemaContent| 架构内容| 
| name| 架构的名称| 
| gsiSets| GSI 集的数组。| 
| minRequiredPlayers| 该变体的播放机中最小的数。| 
| maxAllowedPlayers| 变体的玩家数目上限。| 
| gsiSetId| GSI 集的 Id。| 
| gsiSetName| GSI 集的名称。| 
| selectionOrder|  | 
| variantSchemaId| 设置 GSI 中使用的 varaint 架构的 id。| 
 
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

  
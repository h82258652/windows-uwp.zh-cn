---
title: ServiceError (JSON)
assetID: 81c43f6e-bfff-c4b5-d25c-eace22649f01
permalink: en-us/docs/xboxlive/rest/json-serviceerror.html
description: " ServiceError (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: da3d682a1b66d25a12f21a93e9596d13afae7f90
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2018
ms.locfileid: "8751272"
---
# <a name="serviceerror-json"></a>ServiceError (JSON)
包含有关错误对服务调用失败时返回的信息。 
<a id="ID4EN"></a>

 
## <a name="serviceerror"></a>ServiceError
 
ServiceError 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| code| 32 位有符号整数 | 错误类型。 请参阅下表了解可能的值。 | 
| 源| 字符串 | 引发了错误的服务的名称。 例如，值为<code>ReputationFD</code>指示错误是在信誉服务。 | 
| description| 字符串| 错误的描述。 | 
 
<a id="ID4EBC"></a>

 
### <a name="error-codes"></a>错误代码
 
| 值| 描述| 
| --- | --- | --- | --- | --- | 
| 0| 成功无错误| 
| 4000| 提交与 POST 请求失败验证的请求正文的 JSON 文档无效。 请参阅描述字段的详细信息。 | 
| 4100| 用户不会不存在 XUID 请求 URI 中包含不表示 XBOX Live 上的有效用户。| 
| 4500| 授权错误调用方无权执行请求的操作。| 
| 5000| 服务错误没有内部服务错误| 
| 5300| 服务不可用服务不可用。| 
   
<a id="ID4EQE"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
    "code": 4000,
    "source": "ReputationFD",
    "description": "No targetXuid field was supplied in the request"
}
    
```

  
<a id="ID4EZE"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E2E"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   
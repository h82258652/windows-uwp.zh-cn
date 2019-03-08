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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631702"
---
# <a name="serviceerror-json"></a>ServiceError (JSON)
包含有关对服务调用失败时返回错误的信息。 
<a id="ID4EN"></a>

 
## <a name="serviceerror"></a>ServiceError
 
服务错误对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| code| 32 位有符号的整数 | 错误的类型。 请参阅下表中的可能的值。 | 
| 源| 字符串 | 引发了错误的服务的名称。 例如，值为<code>ReputationFD</code>指示的错误为信誉服务中。 | 
| description| 字符串| 错误的说明。 | 
 
<a id="ID4EBC"></a>

 
### <a name="error-codes"></a>错误代码
 
| 值| 描述| 
| --- | --- | --- | --- | --- | 
| 0| 成功无错误| 
| 4000| 使用 POST 请求失败验证提交的请求正文的 JSON 文档无效。 请参阅说明字段的详细信息。 | 
| 4100| 用户不会不存在 XUID 请求 URI 中包含不表示 XBOX Live 上的有效用户。| 
| 4500| 授权错误调用方无权执行请求的操作。| 
| 5000| 服务错误出现在服务内部错误| 
| 5300| 服务不可用服务将不可用。| 
   
<a id="ID4EQE"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   
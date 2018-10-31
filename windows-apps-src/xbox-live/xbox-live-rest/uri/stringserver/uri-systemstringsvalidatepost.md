---
title: POST (/system/strings/validate)
assetID: 6a59bc0b-8edd-87bf-efaf-f16efa3bedf7
permalink: en-us/docs/xboxlive/rest/uri-systemstringsvalidatepost.html
author: KevinAsgari
description: " POST (/system/strings/validate)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1c3364d3020627aa0d0826a390bf5c4f0b633af8
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5818360"
---
# <a name="post-systemstringsvalidate"></a>POST (/system/strings/validate)
接受用于验证字符串的数组，并返回数组大小相同的结果。 这些 Uri 的域是`client-strings.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [需的请求标头](#ID4EIB)
  * [请求正文](#ID4ELC)
  * [HTTP 状态代码](#ID4E4C)
  * [响应正文](#ID4ETF)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
每个结果指示对应的字符串是否在 Xbox LIVE，可接受以及是否包含有问题的字符串，如果适用。
 
相同的字符串将始终提供相同的结果。 如果你收到未成功结果，分析结果并相应地修改的字符串。
 
 

> [!NOTE] 
> 生成<b>VerifyStringResult</b>将仅报告第一个字符串中的有问题单词。 可能有其他冲突字符串中的字词。 如果你打算替换为有问题的字词，才能使用该字符串，你应替换为有问题的字词或子字符串，然后重新检查要查找其他有问题的子字符串的字符串。  

 
  
<a id="ID4EIB"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标题| 说明| 
| --- | --- | --- | 
| 授权| 身份验证令牌。 示例： XBL3.0 x = [哈希];[令牌]。| 
| x xbl 协定版本| 整数 API 协定版本。 必须对此 API 为 1 或 2。| 
  
<a id="ID4ELC"></a>

 
## <a name="request-body"></a>请求正文
 
请求正文是一个字符串，数组的大小，没有限制和 512 个字符，每个字符串数组。
 
<a id="ID4ETC"></a>

 
### <a name="sample-request"></a>示例请求
 

```cpp
{
    "stringstoVerify":
    [
        "Poppycock",
        "The quick brown fox jumped over the lazy dog.",
        "Hey, want to hang out?",
        "oh noes"
    ]
}
      
```

   
<a id="ID4E4C"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
此部分中使用此方法对此资源所做的请求的响应，该服务返回其中一个状态代码。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 说明| 
| --- | --- | --- | --- | --- | --- | 
| 200| “确定”| 所有字符串都已成功都处理。 这并不一定意味着所有字符串都必须正面的 Hresult。| 
| 401| 未授权| 请求要求用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 406| 不允许| 缺少<b>content-type: application/json</b>标头。| 
| 408| 请求超时| 服务可能不理解格式不正确的请求。 通常无效参数。| 
  
<a id="ID4ETF"></a>

 
## <a name="response-body"></a>响应正文
 
返回[VerifyStringResult (JSON)](../../json/json-verifystringresult.md)，与请求数组大小相同的数组。
  
<a id="ID4EAG"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ECG"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/system/strings/validate](uri-systemstringsvalidate.md)

   
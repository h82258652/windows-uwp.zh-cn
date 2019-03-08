---
title: POST (/system/strings/validate)
assetID: 6a59bc0b-8edd-87bf-efaf-f16efa3bedf7
permalink: en-us/docs/xboxlive/rest/uri-systemstringsvalidatepost.html
description: " POST (/system/strings/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 70e86567f449674c7a046e072437d9ee715dc6d6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595502"
---
# <a name="post-systemstringsvalidate"></a>POST (/system/strings/validate)
接受一个用于验证字符串数组并返回数组的大小相等的结果。 这些 Uri 的域是`client-strings.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [所需的请求标头](#ID4EIB)
  * [请求正文](#ID4ELC)
  * [HTTP 状态代码](#ID4E4C)
  * [响应正文](#ID4ETF)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
每个结果指示相应字符串是否可以对 Xbox LIVE 以及是否包含出现问题的字符串，如果适用。
 
相同字符串将始终提供相同的结果。 如果你收到一个非成功的结果，分析结果，并相应地修改的字符串。
 
 

> [!NOTE] 
> 得到<b>VerifyStringResult</b>将仅报告字符串中第一个有问题的单词。 可能有其他有问题的字符串中的单词。 如果你打算替换为有问题的单词可使字符串可用，应替换为有问题的单词或子字符串，然后重新验证要查找其他有问题的子字符串的字符串。  

 
  
<a id="ID4EIB"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 描述| 
| --- | --- | --- | 
| 授权| 身份验证令牌。 示例：XBL3.0 x=[hash];[token].| 
| x-xbl-contract-version| 整数 API 协定版本。 此 api，必须为 1 或 2。| 
  
<a id="ID4ELC"></a>

 
## <a name="request-body"></a>请求正文
 
请求正文是一个字符串，该数组的大小没有限制和每个字符串的 512 个字符数组。
 
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
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 200| 确定| 已成功处理的所有字符串。 这不一定意味着所有字符串都具有正 Hresult。| 
| 401| 未经授权| 请求需要用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 406| 不可接受| 缺少<b>内容类型： 应用程序 /json</b>标头。| 
| 408| 请求超时| 服务无法理解请求格式不正确。 通常是一个无效的参数。| 
  
<a id="ID4ETF"></a>

 
## <a name="response-body"></a>响应正文
 
返回的数组[VerifyStringResult (JSON)](../../json/json-verifystringresult.md)，请求数组的大小相同。
  
<a id="ID4EAG"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ECG"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/system/strings/validate](uri-systemstringsvalidate.md)

   
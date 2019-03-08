---
title: 标准 HTTP 请求和响应标头
assetID: a5f8fd96-9393-5234-04ad-837e5c117c92
permalink: en-us/docs/xboxlive/rest/httpstandardheaders.html
description: " 标准 HTTP 请求和响应标头"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ca97e82365eab40266b3ffdd84924f71289eede6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636512"
---
# <a name="standard-http-request-and-response-headers"></a>标准 HTTP 请求和响应标头
 
<a id="ID4ES"></a>

 
## <a name="request-headers"></a>请求头
 
下表列出了在 Xbox Live 服务请求时使用的标准 HTTP 标头。
 
| 标头| 值| 描述| 
| --- | --- | --- | 
| x-xbl-contract-version| 1| API 协定版本。 需要对 Xbox Live 服务的所有请求。| 
| 授权| STSTokenString| STS 身份验证令牌。 此标头的值检索从<b>GetTokenAndSignatureResult.Token</b>属性。 | 
| 内容类型| application/xml, application/json, multipart/form-data or application/x-www-form-urlencoded| 指定正在使用请求提交的内容类型。| 
| 内容长度| 整数值| 指定要在 POST 请求中提交的数据的长度。| 
| Accept-Language | 字符串| 指定如何对返回的字符串进行本地化。 请参阅<a href="https://msdn.microsoft.com/en-us/library/bb975829.aspx">高级 Xbox 360 编程</a>为有效的语言/区域组合的列表。| 
  
<a id="ID4E6C"></a>

 
## <a name="response-headers"></a>响应头
 
下表列出了在 Xbox Live 服务响应中使用的标准 HTTP 标头。
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 内容类型| 应用程序/xml、 application/json| 指定要返回的内容类型。| 
| 内容长度| 整数值| 指定要返回的数据的长度。| 
  
<a id="ID4EEE"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EGE"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[其他参考](atoc-xboxlivews-reference-additional.md)

   
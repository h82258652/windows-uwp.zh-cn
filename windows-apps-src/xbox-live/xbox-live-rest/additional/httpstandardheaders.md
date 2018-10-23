---
title: 标准 HTTP 请求和响应标头
assetID: a5f8fd96-9393-5234-04ad-837e5c117c92
permalink: en-us/docs/xboxlive/rest/httpstandardheaders.html
author: KevinAsgari
description: " 标准 HTTP 请求和响应标头"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 31499f8d6fa41d888afd84bea64f7f9de0585b96
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2018
ms.locfileid: "5405077"
---
# <a name="standard-http-request-and-response-headers"></a>标准 HTTP 请求和响应标头
 
<a id="ID4ES"></a>

 
## <a name="request-headers"></a>请求标头
 
下表列出了在 Xbox Live 服务请求时使用的标准 HTTP 标头。
 
| 标头| 值| 说明| 
| --- | --- | --- | 
| x xbl 协定版本| 1| API 协定版本。 所需的所有 Xbox Live 服务请求。| 
| 授权| STSTokenString| STS 身份验证令牌。 此标头的值被从<b>GetTokenAndSignatureResult.Token</b>属性。 | 
| Content-Type| 应用程序/xml、 application/json、 multipart/表单数据或应用程序/x-www-窗体-urlencoded| 指定提交的请求的内容的类型。| 
| Content-Length| 整数值| 指定提交 POST 请求中的数据的长度。| 
| 接受的语言 | String| 指定如何本地化返回的任何字符串。 有关有效的语言/区域设置组合的列表，请参阅<a href="http://msdn.microsoft.com/en-us/library/bb975829.aspx">高级 Xbox 360 编程</a>。| 
  
<a id="ID4E6C"></a>

 
## <a name="response-headers"></a>响应标头
 
下表列出了在 Xbox Live 服务响应中使用标准的 HTTP 标头。
 
| 标头| 值| 说明| 
| --- | --- | --- | --- | --- | --- | 
| Content-Type| 应用程序/xml，application/json| 指定要返回的内容的类型。| 
| Content-Length| 整数值| 指定所返回的数据的长度。| 
  
<a id="ID4EEE"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EGE"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[其他参考](atoc-xboxlivews-reference-additional.md)

   
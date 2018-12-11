---
title: 标准 HTTP 请求和响应标头
assetID: a5f8fd96-9393-5234-04ad-837e5c117c92
permalink: en-us/docs/xboxlive/rest/httpstandardheaders.html
description: " 标准 HTTP 请求和响应标头"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5dea70403b2a6a7c61be2761d1f5d1ff3d75ccb6
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8887353"
---
# <a name="standard-http-request-and-response-headers"></a>标准 HTTP 请求和响应标头
 
<a id="ID4ES"></a>

 
## <a name="request-headers"></a>请求标头
 
下表列出了在 Xbox Live 服务请求时使用的标准 HTTP 标头。
 
| 标头| 值| 描述| 
| --- | --- | --- | 
| x xbl 协定版本| 1| API 协定版本。 所需的所有 Xbox Live 服务请求。| 
| 授权| STSTokenString| STS 身份验证令牌。 此标头的值被从<b>GetTokenAndSignatureResult.Token</b>属性。 | 
| Content-Type| 应用程序/xml、 application/json、 multipart/表单数据或应用程序/x-www-窗体-urlencoded| 指定提交与请求的内容的类型。| 
| Content-Length| 整数值| 指定在 POST 请求正在提交的数据的时长。| 
| 接受的语言 | String| 指定如何本地化返回的任何字符串。 有效的语言组合的列表，请参阅<a href="http://msdn.microsoft.com/en-us/library/bb975829.aspx">高级 Xbox 360 编程</a>。| 
  
<a id="ID4E6C"></a>

 
## <a name="response-headers"></a>响应标头
 
下表列出了在 Xbox Live 服务响应中使用标准的 HTTP 标头。
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | --- | 
| Content-Type| 应用程序/xml，application/json| 指定要返回的内容的类型。| 
| Content-Length| 整数值| 指定所返回的数据的时长。| 
  
<a id="ID4EEE"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EGE"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[其他参考](atoc-xboxlivews-reference-additional.md)

   
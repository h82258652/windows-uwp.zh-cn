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
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2019
ms.locfileid: "9045452"
---
# <a name="standard-http-request-and-response-headers"></a>标准 HTTP 请求和响应标头
 
<a id="ID4ES"></a>

 
## <a name="request-headers"></a>请求标头
 
下表列出了进行 Xbox Live 服务请求时使用的标准 HTTP 标头。
 
| 标头| 值| 说明| 
| --- | --- | --- | 
| x xbl 协定版本| 1| API 协定版本。 所需的所有 Xbox Live 服务请求。| 
| 授权| STSTokenString| STS 身份验证令牌。 从<b>GetTokenAndSignatureResult.Token</b>属性检索此标头的值。 | 
| Content-Type| 应用程序/xml、 application/json、 multipart/表单数据或应用程序/x-www 的窗体的 urlencoded| 指定的提交的请求的内容的类型。| 
| Content-Length| 整数值| 指定提交 POST 请求中的数据的长度。| 
| 接受的语言 | String| 指定如何本地化返回任何字符串。 有效的语言或区域设置组合列表，请参阅<a href="https://msdn.microsoft.com/en-us/library/bb975829.aspx">高级 Xbox 360 编程</a>。| 
  
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

   
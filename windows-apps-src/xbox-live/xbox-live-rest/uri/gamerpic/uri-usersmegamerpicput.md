---
title: PUT (/users/me/gamerpic)
assetID: ddf71c62-197d-a81d-35a7-47c6dc9e1b0c
permalink: en-us/docs/xboxlive/rest/uri-usersmegamerpicput.html
description: " PUT (/users/me/gamerpic)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7aedc7cbd8366c9cb8d3a60e2cb1f5e843b24a8a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661652"
---
# <a name="put-usersmegamerpic"></a>PUT (/users/me/gamerpic)
将上传 1080 x 1080 gamerpic。 
  * [请求正文](#ID4EQ)
  * [HTTP 状态代码](#ID4EZ)
  * [响应正文](#ID4EXC)
 
<a id="ID4EQ"></a>

 
## <a name="request-body"></a>请求正文
 
请求正文是 gamerpic （1080 x 1080 PNG 文件）。
  
<a id="ID4EZ"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | 
| 200| 确定| 成功获取。| 
| 201| 创建。| 上传成功。| 
| 403| 已禁止| 权限被吊销。| 
| 500| 错误| 出现错误。| 
  
<a id="ID4EXC"></a>

 
## <a name="response-body"></a>响应正文
 
响应正文中不发送任何对象。
  
<a id="ID4ECD"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EED"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/me/gamerpic](uri-usersmegamerpic.md)

   
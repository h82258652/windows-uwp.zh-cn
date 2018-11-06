---
title: PUT (/users/me/gamerpic)
assetID: ddf71c62-197d-a81d-35a7-47c6dc9e1b0c
permalink: en-us/docs/xboxlive/rest/uri-usersmegamerpicput.html
author: KevinAsgari
description: " PUT (/users/me/gamerpic)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0d9346303e82dea2dbbd60b542c4ee207dd40901
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2018
ms.locfileid: "6025661"
---
# <a name="put-usersmegamerpic"></a>PUT (/users/me/gamerpic)
用于上传 1080 x 1080 玩家图片。 
  * [请求正文](#ID4EQ)
  * [HTTP 状态代码](#ID4EZ)
  * [响应正文](#ID4EXC)
 
<a id="ID4EQ"></a>

 
## <a name="request-body"></a>请求正文
 
请求正文是玩家头像 （1080 x 1080 PNG 文件）。
  
<a id="ID4EZ"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
本部分中使用此方法对此资源区域设置发出请求的响应，该服务返回的状态代码之一。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 说明| 
| --- | --- | --- | 
| 200| “确定”| 成功获取。| 
| 201| 创建。| 上传已成功。| 
| 403| 已禁止| 权限被吊销。| 
| 500| 错误| 出现错误。| 
  
<a id="ID4EXC"></a>

 
## <a name="response-body"></a>响应正文
 
该响应正文中不发送任何对象。
  
<a id="ID4ECD"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EED"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/me/gamerpic](uri-usersmegamerpic.md)

   
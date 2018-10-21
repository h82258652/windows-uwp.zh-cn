---
title: PUT (/users/me/gamerpic)
assetID: ddf71c62-197d-a81d-35a7-47c6dc9e1b0c
permalink: en-us/docs/xboxlive/rest/uri-usersmegamerpicput.html
author: KevinAsgari
description: " PUT (/users/me/gamerpic)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8c8c8f4297bb671f8e90c233ccf98dc2cf0730ad
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2018
ms.locfileid: "5156757"
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
 
此部分中使用此方法对此资源所做的请求的响应，该服务返回的状态代码之一。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
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

   
---
title: GET (/users/xuid({xuid})/history/titles)
assetID: c0a6cb3b-bab6-03b8-a79e-87defaaa6421
permalink: en-us/docs/xboxlive/rest/uri-titlehistoryusersxuidhistorytitlesgetv2.html
author: KevinAsgari
description: " GET (/users/xuid({xuid})/history/titles)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: bf858750d04e6be95e75d7c6c74d09cadde6789f
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2018
ms.locfileid: "6029926"
---
# <a name="get-usersxuidxuidhistorytitles"></a>GET (/users/xuid({xuid})/history/titles)
获取一份标题为其用户已解锁或对其成就的进度。 此 API 不会返回游戏播放或启动的用户的完整历史记录。 这些 Uri 的域是`achievements.xboxlive.com`。
 
  * [URI 参数](#ID4EY)
  * [查询字符串参数](#ID4EDB)
  * [授权](#ID4EFD)
  * [可选的请求标头](#ID4EGE)
  * [请求正文](#ID4ERF)
 
<a id="ID4EY"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 所访问其游戏历史记录的用户。| 
  
<a id="ID4EDB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 必需| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | 
| skipItems| 否| 32 位有符号整数| 返回在给定的项目数之后开始的项。 例如， <b>skipItems ="3"</b>将检索项目开头的第四项检索。 | 
| ContinuationToken| 否| 字符串| 返回在给定的延续令牌启动的项。 | 
| maxItems| 否| 32 位有符号整数| 要从该集合，这可以与<b>skipItems</b>和<b>continuationToken</b>返回项目的范围结合使用返回的项数的最大数量。 如果<b>maxItems</b>不存在，并且可能会返回少于<b>maxItems</b>，即使尚未返回结果的最后一页服务可能会提供一个默认值。 | 
  
<a id="ID4EFD"></a>

 
## <a name="authorization"></a>授权
 
| 声明| 是否为必需？| 说明| 如果缺少的行为| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 用户| 调用方是授权的 Xbox LIVE 用户。| 调用方必须是 Xbox LIVE 上的有效用户。| 403 已禁止| 
  
<a id="ID4EGE"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>X RequestedServiceVersion</b>| 字符串| 名称/的内部版本号应指向此请求的 Xbox LIVE 的服务。 请求将仅路由到该服务后验证标头、 身份验证令牌等中的声明的有效性。| 
| <b>x xbl 协定版本</b>| 32 位无符号的整数| 如果存在，并且设置为 2，就将使用此 API 的 V2 版本。 否则为 V1。| 
  
<a id="ID4ERF"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送任何对象。
  
<a id="ID4EDG"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EFG"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/xuid({xuid})/history/titles](uri-titlehistoryusersxuidhistorytitlesv2.md)

  
<a id="ID4EPG"></a>

 
##### <a name="reference"></a>参考 

[UserTitle (JSON)](../../json/json-usertitlev2.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

 [分页参数](../../additional/pagingparameters.md)

   
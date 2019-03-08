---
title: GET (/users/xuid({xuid})/history/titles)
assetID: c0a6cb3b-bab6-03b8-a79e-87defaaa6421
permalink: en-us/docs/xboxlive/rest/uri-titlehistoryusersxuidhistorytitlesgetv2.html
description: " GET (/users/xuid({xuid})/history/titles)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cadbf38385bbc321ef5bf23eb93c3fbc5c1a2417
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655582"
---
# <a name="get-usersxuidxuidhistorytitles"></a>GET (/users/xuid({xuid})/history/titles)
获取用户具有对其解除锁定或进度对其成就标题的列表。 此 API 不返回用户的完整历史记录的标题播放或启动。 这些 Uri 的域是`achievements.xboxlive.com`。
 
  * [URI 参数](#ID4EY)
  * [查询字符串参数](#ID4EDB)
  * [Authorization](#ID4EFD)
  * [可选的请求标头](#ID4EGE)
  * [请求正文](#ID4ERF)
 
<a id="ID4EY"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 的用户正在访问其标题历史记录。| 
  
<a id="ID4EDB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 必需| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | 
| skipItems| 否| 32 位有符号的整数| 返回从给定数目的项后开始的项。 例如， <b>skipItems ="3"</b>将检索项的第四项开头检索。 | 
| ContinuationToken| 否| 字符串| 返回从给定的继续标记处开始的项。 | 
| maxItems| 否| 32 位有符号的整数| 要从集合，可以结合返回的项数的最大数目<b>skipItems</b>并<b>continuationToken</b>要返回的项的范围。 该服务可能会提供默认值，如果<b>maxItems</b>不存在，并且可能返回少于<b>maxItems</b>，即使尚未返回结果的最后一页。 | 
  
<a id="ID4EFD"></a>

 
## <a name="authorization"></a>授权
 
| 声明| 是否为必需？| 描述| 如果缺少的行为| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 用户| 调用方是 Xbox LIVE 的授权的用户。| 调用方必须是 Xbox LIVE 上的有效用户。| 403 禁止访问| 
  
<a id="ID4EGE"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>X-RequestedServiceVersion</b>| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。| 
| <b>x-xbl-contract-version</b>| 32 位无符号的整数| 如果存在，并且设置为 2，就将使用此 API 的 V2 版本。 否则为 V1。| 
  
<a id="ID4ERF"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送的任何对象。
  
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

   
---
title: 分页参数
assetID: f8d059fd-30e7-be60-0a46-c9a833c400c6
permalink: en-us/docs/xboxlive/rest/pagingparameters.html
author: KevinAsgari
description: " 分页参数"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e1ed654e4dc1c0f1233ecdedf5d4af66da868bff
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2018
ms.locfileid: "4083033"
---
# <a name="paging-parameters"></a>分页参数
 
某些 Xbox Live 服务 Uri 返回 JavaScript 对象表示法 (JSON) 对象的集合。 通过将分页参数指定为查询字符串附加到 URI 的一部分，可以通过分页这些集合。 遵循分页参数的完整列表。 允许分页参数的所有 Uri 都链接到此页面底部。
 
<a id="ID4E2"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数 
 
| 参数| 必需| 类型| 说明| 
| --- | --- | --- | --- | 
| ContinuationToken| 否| 字符串| 返回在给定的延续令牌启动的项目。 | 
| maxItems| 否| 32 位有符号的整数| 要从该集合，这可以与<b>skipItems</b>和<b>continuationToken</b>返回项目的范围结合使用返回的项目的最大数量。 如果<b>maxItems</b>不存在，并且可能会返回少于<b>maxItems</b>，即使尚未返回结果的最后一页服务可能会提供一个默认值。 | 
| skipItems| 否| 32 位有符号的整数| 返回在给定的项目数之后开始的项目。 例如， <b>skipItems ="3"</b>将检索项目开头的第四项检索。 | 
  
<a id="ID4EDD"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EFD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[其他参考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ERD"></a>

 
##### <a name="reference--get-usersxuidxuidachievementsuriachievementsuri-achievementsusersxuidachievementsgetv2md"></a>引用[获取 (/users/xuid({xuid})/achievements)](../uri/achievements/uri-achievementsusersxuidachievementsgetv2.md)

   
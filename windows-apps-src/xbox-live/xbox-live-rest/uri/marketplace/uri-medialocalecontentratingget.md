---
title: GET (/media/{marketplaceId}/contentRating)
assetID: e677acdb-d905-3bea-b0ca-6d8becd663c0
permalink: en-us/docs/xboxlive/rest/uri-medialocalecontentratingget.html
description: " GET (/media/{marketplaceId}/contentRating)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8d1cb9d09de8671d4cd3d61e96a8335412237e5c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641582"
---
# <a name="get-mediamarketplaceidcontentrating"></a>GET (/media/{marketplaceId}/contentRating)
获取内容分级令牌。 这些 Uri 的域是`eds.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4ELB)
  * [查询字符串参数](#ID4EWB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
强制实施对允许儿童若要查看的内容的家长控制是一项复杂的任务。 不仅每个媒体项类型具有其自己的评级系统，而且这些评级系统而异的国家/地区到国家/地区。 这意味着需要指定正确筛选所有项的数据的多个不同部分。
 
而不是所有 API 调用中指定的所有参数，此 API 将生成值将传递到**combinedContentRating**其他 Api 中的参数和仍进行通信的相同信息。 此操作旨在使 Api 更易于使用和维护，如多个参数传递到此 API 处于折叠状态到单个可重用的值对于其他 Api。
 
尽管此 API 返回的确切值可能会最终更改，它们应极少更改 （例如版本娱乐发现服务 (EDS) 之间） 并因此无法针对长时间的缓存。 任何 API 接受**combinedContentRating**参数将提供有意义的错误消息中传递的值是无效的如果它是的指示调用方只需调用此 API，再次以获取更新后的值。 如果 API 接受**combinedContentRating**参数，但一个也不提供，没有筛选的内容会进行基于家长控制。 

> [!NOTE] 
> 这并不意味着，返回唯一的"安全"内容-这意味着，返回的所有内容，包括可能的成人内容。 


  
<a id="ID4ELB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | 
| marketplaceId| 字符串| 必需。 从获取值的字符串<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>。| 
  
<a id="ID4EWB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | 
| filterExplicit| 布尔值| 可选。 筛选器显式音乐。| 
| filterFamilyOnlyApps| 布尔值| 可选。 筛选器未标记为系列友好的应用。| 
| filterUnrated| 布尔值| 可选。 确定是未分级的内容是否应在响应中或不包含。| 
| maxGameRating| 32 位有符号的整数| 可选。 筛选的游戏。| 
| maxMovieRating| 32 位有符号的整数| 可选。 筛选特定级别以上的电影。| 
| maxTVRating| 32 位有符号的整数| 可选。 筛选电视。| 
  
<a id="ID4E5D"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EAE"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/media/{marketplaceId}/contentRating](uri-medialocalecontentrating.md)

  
<a id="ID4EKE"></a>

 
##### <a name="further-information"></a>详细信息 

[EDS 常见标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询精简将](../../additional/edsqueryrefiners.md)

 [Marketplace Uri](atoc-reference-marketplace.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   
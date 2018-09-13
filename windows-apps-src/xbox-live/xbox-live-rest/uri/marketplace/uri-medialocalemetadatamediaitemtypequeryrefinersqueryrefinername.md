---
title: /media/ {marketplaceId} / 元数据/mediaItemTypes / {mediaitemtype} /queryrefiners/ {queryrefinername}
assetID: 1e2f5fd3-3d14-9a97-ffbf-ab2a18ff4f15
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinername.html
author: KevinAsgari
description: " /media/ {marketplaceId} / 元数据/mediaItemTypes / {mediaitemtype} /queryrefiners/ {queryrefinername}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: af1090dd5dc33d3070ddafc9c79105d09c400a3c
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3962698"
---
# <a name="mediamarketplaceidmetadatamediaitemtypesmediaitemtypequeryrefinersqueryrefinername"></a>/media/ {marketplaceId} / 元数据/mediaItemTypes / {mediaitemtype} /queryrefiners/ {queryrefinername}
访问可接受的值为给定的查询精选名称和给定的媒体项类型。 这些 Uri 的域是`eds.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| marketplaceId| 字符串| 必需。 字符串从<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>获得的值。| 
| mediaitemtype| 字符串| 必需。 从值之一[获取 (/media/ {marketplaceId} / 元数据/mediaGroups / {mediagroup} / mediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md)。| 
| queryrefinername| 字符串| 必需。 名称查询精选的所需的值，如"流派"或"十年期"。 请参阅 QueryRefiners。| 
  
<a id="ID4EKC"></a>

 
## <a name="valid-methods"></a>有效的方法

[获取 (/media/ {marketplaceId} / 元数据/mediaItemTypes / {mediaitemtype} /queryrefiners/ {queryrefinername})](uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinernameget.md)

&nbsp;&nbsp;列出了可接受的值为给定的查询精选名称和给定的媒体项类型。
 
<a id="ID4EUC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EWC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[市场 Uri](atoc-reference-marketplace.md)

  
<a id="ID4EAD"></a>

 
##### <a name="further-information"></a>详细信息 

[EDS 公共标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询精简将](../../additional/edsqueryrefiners.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   
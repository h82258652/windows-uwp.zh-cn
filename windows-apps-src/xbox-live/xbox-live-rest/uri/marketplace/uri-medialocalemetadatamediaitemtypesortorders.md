---
title: /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders
assetID: 221c440d-c448-11e9-f455-6d0f95fc16ef
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediaitemtypesortorders.html
author: KevinAsgari
description: " /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c453dc1800770547623fc4920f6d2c964a4b2f88
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2018
ms.locfileid: "4207212"
---
# <a name="mediamarketplaceidmetadatamediaitemtypesmediaitemtypesortorders"></a>/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders
可用的访问排序顺序为给定的 mediaitem 类型和 EDS 的给定的版本。 这些 Uri 的域是`eds.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| marketplaceId| 字符串| 必需。 字符串从<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>获得的值。| 
| mediaitemtype| 字符串| 必需。 从值之一[GET (/media/ {marketplaceId} / /metadata/mediagroups/ {mediagroup} / mediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md)。| 
  
<a id="ID4EBC"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders)](uri-medialocalemetadatamediaitemtypesortordersget.md)

&nbsp;&nbsp;列出了可用的排序订单的给定的 mediaitem 类型和 EDS 给定的版本。
 
<a id="ID4ELC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ENC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[市场 URI](atoc-reference-marketplace.md)

  
<a id="ID4EXC"></a>

 
##### <a name="further-information"></a>详细信息 

[EDS 通用标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询优化器](../../additional/edsqueryrefiners.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   
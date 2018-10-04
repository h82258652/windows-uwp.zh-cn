---
title: 市场 URI
assetID: 27b6035f-84b9-67a8-6a12-85c450d18a58
permalink: en-us/docs/xboxlive/rest/atoc-reference-marketplace.html
author: KevinAsgari
description: " 市场 URI"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4be83e2d4301a708a705a8bec0a1d975b6435bc5
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4316305"
---
# <a name="marketplace-uris"></a>市场 URI

本部分提供了从*应用商店*服务，也称为娱乐发现服务 (EDS) 的 Xbox Live 服务的详细信息通用资源标识符 (URI) 地址和关联的超文本传输协议 (HTTP) 方法。

仅运行的 Xbox 360 上、 在 Windows Phone 设备上，或在 Xbox.com 上的游戏均可使用此服务。

这些 Uri 的域是 eds.xboxlive.com 和 inventory.xboxlive.com。

<a id="ID4EPB"></a>

 
## <a name="in-this-section"></a>本部分内容

[/users/me/inventory](uri-inventory.md)

&nbsp;&nbsp;访问当前与提供的用户相关联的清单的集。

[/users/me/consumables/{itemID}](uri-inventoryconsumablesitemurl.md)

&nbsp;&nbsp;访问完整的一组特定的易耗型库存项目的详细信息。

[/inventory/{itemID}](uri-inventoryitemurl.md)

&nbsp;&nbsp;访问完整的一组特定的库存项目的详细信息。

[/media/{marketplaceId}/crossMediaGroupSearch](uri-localecrossmediagroupsearch.md)

&nbsp;&nbsp;访问多个不同的媒体组中的项。

[/media/{marketplaceId}/browse](uri-medialocalebrowse.md)

&nbsp;&nbsp;允许浏览的单个媒体组中的项。

[/media/{marketplaceId}/contentRating](uri-medialocalecontentrating.md)

&nbsp;&nbsp;访问内容分级令牌。

[/media/{marketplaceId}/details](uri-medialocaledetails.md)

&nbsp;&nbsp;详细信息和元数据，产品/服务返回有关的一个或多个项目。

[/media/{marketplaceId}/fields](uri-medialocalefields.md)

&nbsp;&nbsp;访问字段令牌。

[/media/{marketplaceId}/metadata/mediaGroups](uri-medialocalemetadatamediagroups.md)

&nbsp;&nbsp;列出所有受支持的 mediaGroups 给定 EDS 版本。

[/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes](uri-medialocalemetadatamediagroupsmediaitemtypes.md)

&nbsp;&nbsp;访问每个媒体组 EDS 的给定版本可用 mediaItemTypes。

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaItemType}/fields](uri-medialocalemetadatamediaitemtypefields.md)

&nbsp;&nbsp;访问从其中一个期待看到的数据，为给定的 mediaitemtype 和 EDS 的给定的版本字段。

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners](uri-medialocalemetadatamediaitemtypequeryrefiners.md)

&nbsp;&nbsp;对于给定的媒体项类型，请访问查询优化器。

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}](uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinername.md)

&nbsp;&nbsp;访问可接受的值为给定的查询精选名称和给定的媒体项类型。

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues](uri-medialocalemediaitemtypequeryrefinersubqueryrefinervalues.md)

&nbsp;&nbsp;对于给定的查询精选值 (例如，"subgenres 在给定流派") 的访问权限的子值列表。

[/media/{marketplaceId}/metadata/mediaItemTypes](uri-medialocalemetadatamediaitemtypes.md)

&nbsp;&nbsp;访问所有受支持的 mediaItemTypes 给定 EDS 版本。

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders](uri-medialocalemetadatamediaitemtypesortorders.md)

&nbsp;&nbsp;可用的访问排序顺序为给定的 mediaitem 类型和 EDS 的给定的版本。

[/media/{marketplaceId}/singleMediaGroupSearch](uri-medialocalesinglemediagroupsearch.md)

&nbsp;&nbsp;允许搜索单个媒体组中的项。

<a id="ID4EFD"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EHD"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[统一资源标识符 (URI) 参考](../atoc-xboxlivews-reference-uris.md)


<a id="ID4ERD"></a>


##### <a name="further-information"></a>详细信息

[其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

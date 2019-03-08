---
title: 市场 URI
assetID: 27b6035f-84b9-67a8-6a12-85c450d18a58
permalink: en-us/docs/xboxlive/rest/atoc-reference-marketplace.html
description: " 市场 URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9fd8112c6e16b3e9d9fb70c34381e88ba5aa6273
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593872"
---
# <a name="marketplace-uris"></a>市场 URI

本部分提供有关通用资源标识符 (URI) 地址和关联的超文本传输协议 (HTTP) 方法的详细信息从 Xbox Live 服务中针对*marketplace*服务，也称为娱乐发现服务 (EDS)。

仅在 Xbox 360 上、 在 Windows Phone 设备上，或在 Xbox.com 上运行的游戏可以使用此服务。

这些 Uri 的域是 eds.xboxlive.com 和 inventory.xboxlive.com。

<a id="ID4EPB"></a>

 
## <a name="in-this-section"></a>本部分内容

[/users/me/inventory](uri-inventory.md)

&nbsp;&nbsp;访问当前与提供的用户相关联的清单的集。

[/users/me/consumables/{itemID}](uri-inventoryconsumablesitemurl.md)

&nbsp;&nbsp;访问完整集的特定可使用清单项的详细信息。

[/inventory/{itemID}](uri-inventoryitemurl.md)

&nbsp;&nbsp;访问完整集的特定清单项的详细信息。

[/media/{marketplaceId}/crossMediaGroupSearch](uri-localecrossmediagroupsearch.md)

&nbsp;&nbsp;访问多个不同的媒体组中的项。

[/media/{marketplaceId}/browse](uri-medialocalebrowse.md)

&nbsp;&nbsp;允许浏览单个媒体组中的项。

[/media/{marketplaceId}/contentRating](uri-medialocalecontentrating.md)

&nbsp;&nbsp;访问内容分级令牌。

[/media/{marketplaceId}/details](uri-medialocaledetails.md)

&nbsp;&nbsp;返回提议详细信息和元数据有关的一个或多个项。

[/media/{marketplaceId}/fields](uri-medialocalefields.md)

&nbsp;&nbsp;访问字段令牌。

[/media/{marketplaceId}/metadata/mediaGroups](uri-medialocalemetadatamediagroups.md)

&nbsp;&nbsp;列出所有受支持的 mediaGroups 给定 EDS 版本。

[/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes](uri-medialocalemetadatamediagroupsmediaitemtypes.md)

&nbsp;&nbsp;访问每个媒体 EDS 的给定版本的组可用 mediaItemTypes。

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaItemType}/fields](uri-medialocalemetadatamediaitemtypefields.md)

&nbsp;&nbsp;访问一个可以从其预期给定的 mediaitemtype 和 EDS 的给定的版本的数据字段。

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners](uri-medialocalemetadatamediaitemtypequeryrefiners.md)

&nbsp;&nbsp;访问查询精简将为给定的媒体项类型。

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}](uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinername.md)

&nbsp;&nbsp;访问给定的查询精选器名称和给定媒体项类型的可接受值。

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues](uri-medialocalemediaitemtypequeryrefinersubqueryrefinervalues.md)

&nbsp;&nbsp;给定的查询精选值 (例如，"subgenres 中给定流派") 的访问的子值的列表。

[/media/{marketplaceId}/metadata/mediaItemTypes](uri-medialocalemetadatamediaitemtypes.md)

&nbsp;&nbsp;访问所有受支持的 mediaItemTypes 给定 EDS 版本。

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders](uri-medialocalemetadatamediaitemtypesortorders.md)

&nbsp;&nbsp;可用的访问对给定的 mediaitem 类型和 EDS 的给定的版本的订单进行排序。

[/media/{marketplaceId}/singleMediaGroupSearch](uri-medialocalesinglemediagroupsearch.md)

&nbsp;&nbsp;允许单个媒体组内的项的搜索。

<a id="ID4EFD"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EHD"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[通用资源标识符 (URI) 引用](../atoc-xboxlivews-reference-uris.md)


<a id="ID4ERD"></a>


##### <a name="further-information"></a>详细信息

[其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

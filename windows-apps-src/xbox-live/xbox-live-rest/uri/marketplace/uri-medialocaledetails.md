---
title: /media/{marketplaceId}/details
assetID: bc8758ed-2f90-b501-5c3f-6f253f02d754
permalink: en-us/docs/xboxlive/rest/uri-medialocaledetails.html
description: " /media/{marketplaceId}/details"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f58e5247c3fd52e84a3a9bab28c6926f74e864e3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655702"
---
# <a name="mediamarketplaceiddetails"></a>/media/{marketplaceId}/details
返回提议详细信息和元数据有关的一个或多个项。 这些 Uri 的域是`eds.xboxlive.com`。
 
API 与相关的 API 和浏览 API 的详细信息 (时 passin ID 中的) 作为这些 API 返回显式或隐式地 fiven id 关联的其他项有关的信息，而详细信息 API 返回的其他信息有关同一项。
 
不同的媒体项类型的多个 Id 可以传递到一个调用 （只要它们不是类型 ProviderContentID-请参阅下文的），但它们都必须属于相同的媒体组。 但是，有几种： 调用方不知道的媒体组的客户端方案。 该 API 支持这通过允许在以下情况下的"未知"媒体组 sepcial 值：
 
   * idType = XboxHexTitle，其会生成 AppType 或 GameType 项
   * idType = ProviderContentId，其会生成 MovieType 或 TVType 项
  
下图总结的哪个 ID 可以与哪些媒体组提供类型的整个映射：
 
| ID 类型| AppType| GameType| MovieType| MusicArtistType| MusicType| TVType| WebVideoType| Unknown| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 规范| Y| Y| Y| Y| Y| Y| Y| N| 
| ZuneCatalog| N| N| Y| Y| Y| Y| N| N| 
| ZuneMediaInstance| N| N| Y| N| Y| Y| N| N| 
| 产品/服务| Y| Y| Y| N| Y| Y| N| N| 
| AMG| N| N| N| N| Y| N| N| N| 
| MediaNet| N| N| N| N| Y| N| N| N| 
| XboxHexTitle| Y| Y| N| N| N| N| N| Y| 
| ProviderContentId| N| N| Y| N| N| Y| N| Y| 
 
  * [参数说明](#ID4EEH)
  * [URI 参数](#ID4EUH)
 
<a id="ID4EEH"></a>

 
## <a name="parameter-notes"></a>参数说明
 
<a id="ID4EIH"></a>

 
### <a name="providercontentid"></a>ProviderContentId
 
此操作用于查找提供程序特定的 id。例如 Netflix Id 或 Hulu id。
 
ProviderContentId idType 时，接受单个值。 这是因为 ProviderContentIds 是唯一的 ID 可以包含类型。 字符。 由于 '。 字符也是我们使用 Id 之间的分隔符，什么是 Id 之间 delimieter 之间不存在二义性和新增功能与 ID 本身的一部分。 API 的 rest 方式是相同的 ProviderContentIds，大容量查找功能除外。
   
<a id="ID4EUH"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| marketplaceId| 字符串| 必需。 从获取值的字符串<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>。| 
  
<a id="ID4EWAAC"></a>

 
## <a name="valid-methods"></a>有效的方法

[获取 (/media/ {marketplaceId} / 详细信息)](uri-medialocaledetailsget.md)

&nbsp;&nbsp;返回提议详细信息和元数据有关的一个或多个项。 
 
<a id="ID4EABAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ECBAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[Marketplace Uri](atoc-reference-marketplace.md)

  
<a id="ID4EMBAC"></a>

 
##### <a name="further-information"></a>详细信息 

[EDS 常见标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询精简将](../../additional/edsqueryrefiners.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   
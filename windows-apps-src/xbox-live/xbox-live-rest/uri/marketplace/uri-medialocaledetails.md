---
title: /media/{marketplaceId}/details
assetID: bc8758ed-2f90-b501-5c3f-6f253f02d754
permalink: en-us/docs/xboxlive/rest/uri-medialocaledetails.html
author: KevinAsgari
description: " /media/{marketplaceId}/details"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7b8dcea7c0987a2bc783adae0398c9579ded2fe8
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "4020730"
---
# <a name="mediamarketplaceiddetails"></a>/media/{marketplaceId}/details
详细信息和元数据，产品/服务返回有关的一个或多个项目。 这些 Uri 的域是`eds.xboxlive.com`。
 
API 不同于相关的 API 和浏览 API 的详细信息 (当 passin ID 中的) 为这些 API 返回显式或隐式方法，与 fiven ID 相关联的其他项目的信息，而详细信息 API 返回的其他信息有关相同的项。
 
可以将多个 Id 的不同的媒体项类型传递到单个调用 （只要它们不类型 ProviderContentID-请参阅下面的），但它们都必须属于相同的媒体组。 但是，有几个调用方不知道媒体组在其中的客户端方案。 API 支持这通过允许 sepcial 值为"未知"媒体组在以下情况：
 
   * idType = XboxHexTitle，这将产生 AppType 或 GameType 项
   * idType = ProviderContentId，这将产生 MovieType 或 TVType 项
  
下表总结了整个映射的哪个 ID 可与哪些媒体组提供类型：
 
| ID 类型| AppType| GameType| MovieType| MusicArtistType| MusicType| TVType| WebVideoType| Unknown| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 规范| Y| Y| Y| Y| Y| Y| Y| N| 
| ZuneCatalog| N| N| Y| Y| Y| Y| N| N| 
| ZuneMediaInstance| N| N| Y| N| Y| Y| N| N| 
| 优惠| Y| Y| Y| N| Y| Y| N| N| 
| AMG| N| N| N| N| Y| N| N| N| 
| MediaNet| N| N| N| N| Y| N| N| N| 
| XboxHexTitle| Y| Y| N| N| N| N| N| Y| 
| ProviderContentId| N| N| Y| N| N| Y| N| Y| 
 
  * [参数的说明](#ID4EEH)
  * [URI 参数](#ID4EUH)
 
<a id="ID4EEH"></a>

 
## <a name="parameter-notes"></a>参数的说明
 
<a id="ID4EIH"></a>

 
### <a name="providercontentid"></a>ProviderContentId
 
这是用于查找提供程序例如特定 id。 Netflix Id 或 Hulu id。
 
ProviderContentId idType 时，将接受仅单个值。 这是因为 ProviderContentIds 是唯一的 ID，可以包含类型 '。 字符。 由于 '。 字符也是我们使用 Id 之间的分隔符，什么是 Id 之间 delimieter 之间没有多义性和内容是 ID 本身的一部分。 该 API 的其余部分适用于 ProviderContentIds，进行相同的方式，除了批量查找功能。
   
<a id="ID4EUH"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| marketplaceId| 字符串| 必需。 字符串从<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>获得的值。| 
  
<a id="ID4EWAAC"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/media/{marketplaceId}/details)](uri-medialocaledetailsget.md)

&nbsp;&nbsp;详细信息和元数据，产品/服务返回有关的一个或多个项目。 
 
<a id="ID4EABAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ECBAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[市场 URI](atoc-reference-marketplace.md)

  
<a id="ID4EMBAC"></a>

 
##### <a name="further-information"></a>详细信息 

[EDS 通用标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询优化器](../../additional/edsqueryrefiners.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   
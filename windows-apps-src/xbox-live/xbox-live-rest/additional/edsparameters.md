---
title: EDS 参数
assetID: 9475b427-53bc-697b-6d24-1787320260b7
permalink: en-us/docs/xboxlive/rest/edsparameters.html
author: KevinAsgari
description: " EDS 参数"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6d3ab2880bee2e6a6f5cf7a5350244e786e5e615
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/05/2018
ms.locfileid: "4384719"
---
# <a name="eds-parameters"></a>EDS 参数

<a id="ID4EO"></a>


## <a name="query-string-parameters"></a>查询字符串参数

这些查询参数不一定接受所有[娱乐发现服务 (EDS) Api](../uri/marketplace/atoc-reference-marketplace.md)，但所有都接受由多个 API。

| 参数| 类型| 描述|
| --- | --- | --- |
| combinedContentRating| 字符串| 可选。 请参阅[GET (/media/ {marketplaceId} / contentRating)](../uri/marketplace/uri-medialocalecontentratingget.md)。|
| ContinuationToken| 字符串| 可选。 延续令牌是包含该服务需要分页某些应用场景中的信息的不透明 blob。 如果省略值，第一页结果返回 （其中页面大小由 maxItems 参数），以及可用于获取结果的第二页延续令牌。 第二个页面将包含结果的第三页延续令牌，依此类推。|
| 所需| 字符串| 可选。 请参阅组合的字段名称 API。|
| desiredMediaItemTypes| 字符串的数组| 可选。 此参数确定要在响应中返回的项目类型。|
| 域| 字符串| 可选。 域参数确定游戏和应用市场上下文中的客户端调用的。 默认情况下域是"现代"，以指示客户端可以仅寻求 Xbox One 的内容。 如果客户端想要切换到 surface Xbox 360 内容的域，它必须为"Xbox360"指定域。 目前，跨域结果不支持。 可能的值为： <ul><li>Xbox360</li><li>现代</li></ul> SingleMediaGroupSearch 仅支持"Xbox360"值。 支持浏览和详细信息。 crossMediaGroupSearch 不受支持，并将返回 400 错误。|
| 字段| 字符串| 可选。 请参阅[GET (/media/ {marketplaceId} / 字段)](../uri/marketplace/uri-medialocalefieldsget.md)。|
| firstPartyOnly| 布尔值| 可选的筛选参数。 确定是否返回仅第一方内容或这两个第一方和第三方内容从查询返回是否。 |
| freeOnly| 布尔值| 可选的筛选参数。 限制结果以仅免费内容。|
| GroupBy| TK| GroupBy 参数用于帮助归类到组的结果集，而不是单个结果集。 指定此参数将修改的结果集返回多个项目的列表，每个存储桶中的项目数由 maxItems 参数。 <ul><li>MediaGroup-MediaGroup 结果进行分组。</li></ul> |
| hasTrailer| 布尔值| 可选的筛选参数。 确定是否返回的项目必须包含预告片，或如果遇到预告片是可选的。 如果值为 true，所有项目必须都具有预告片。|
| id| 字符串| 可选。 如果提供，限制结果以只能在子元素的项与给定的 id。 如果未提供此参数，还必须指定 MediaItemType。 |
| id| 字符串的数组| 必需。 所有将为其返回详细信息 （最多 10) 的 Id。 注意，任何 ID 包含字符非法放入中的 URL （ProviderContentId 类型 Id 是正常情况下完整 Url 本身，从而包含非法字符） 必须使用 URL 编码，以便正确发送到 EDS。|
| idType| 字符串| 可选。 Id 它在传递给 id 参数的类型。 有效的值包括： <ul><li>规范 （必应/市场）</li><li>XboxHexTitle （在控制台上播放的应用）</li></ul>  所提供的所有 Id 必须共享同一个 idType。 如果省略此值，则会假设所有 Id 为 Canonical。|
| latestOnly| 布尔值| 可选的筛选参数。 限制为仅使用最新的发布日期的结果。|
| maxItems| 32 位有符号整数| 可选。 确定应从调用返回的项目的最大数量。 有效值介于 1 和 25，非独占之间的数字。 如果省略，该参数将默认为 25。|
| mediaGroup| 字符串| 可选。 媒体组的 Id。 所提供的所有 Id 必须共享相同的媒体组。|
| MediaItemType| 字符串| 可选。 其 ID 已 id 参数中指定的项目类型。 如果提供 id 参数，则还必须指定此参数。|
| orderBy| 字符串| 必需。 OrderBy 参数确定要返回的项目的排序方式。 此字段的常见值此处列出，但某些 Api 可能会支持其他值。<ul><li>playCountDaily-按的次数计数播放的媒体，最近一天。</li><li>freeAndPaidCountDaily-通过免费和付费购买，最近一天的计数。</li><li>paidCountAllTime-通过唯一付费购买，所有时间的计数。</li><li>paidCountDaily-通过付费的购买，最近一天的计数。</li><li>digitalReleaseDate-截止日期可用的下载。</li><li>releaseDate-通过存储中可用的日期回退到数字发布日期 （如果可用）。</li><li>userRatings-通过平均用户分级。</li></ul> |
| preferredProvider| 字符串| 可选。 如果用户首选内容提供商，如 Comcast Xfinity 或 Verizon FIOS 可以传入该提供程序的 ID。 在项目的实际顺序将不会更改，对于每个项，指定提供程序的信息将显示在提供程序列表的顶部 （如果首选的内容提供商有可用的项）。|
| q| 字符串| 必需。 查询搜索中使用的术语。|
| queryRefiners| 字符串的数组| 可选。 请参阅[EDS 查询优化器](edsqueryrefiners.md)的列表。|
| 关系| 字符串| 可选。 使用 ID 参数作为基本搜索其他产品的指定的关系类型匹配的筛选器： <ul><li>bundledWith-查找捆绑包产品 ID 参数是这些捆绑包的一部分。</li><li>bundledProducts-查找包含在捆绑包 ID 参数指定的产品。</li></ul>  将此参数返回唯一产品 （可以浏览调用中返回） 的市场中可见的。 捆绑包是否隐藏的产品，它仍然是捆绑包的一部分，但不是会返回这些结果中。|
  | ScopeId | 字符串 | 在反向查找方案中使用此参数为视频媒体。 |
  | ScopeIdType | 字符串 | 在反向查找方案中使用此参数为视频媒体。 可能的值： 标题。 |
  | skipItems | 32 位有符号整数 | 可选。 用于非跨组方案中的分页、 skipItems 参数用于确定已检测到多少项 （并因此应显示哪些项首先的结果中设置）。 该值是基于 0，因此 skipItems = 0 （或只需不提供 skipItems） 开始检索在开始菜单的列表。 skipItems = 3 将跳过列表中的前三个项目，并开始与第四个项的检索。 |
  | subscriptionLevel | 字符串的数组 | 可选的筛选参数。 SubscriptionLevel 参数确定订阅用户的类型有 （如用户是否有付费的订阅或免费订阅）。 可能的值如下所示。 <ul><li>金牌： 用户有付费的订阅</li><li>银牌： 用户有免费订阅。</li></ul>  |
| 目标| 字符串| EDS 提供灵活地筛选产品/服务的目标设备。 产品/服务 （ProviderContent 或可用性） 返回项目可以限制到目标设备。|
| topRatedOnly| 布尔值| 可选的筛选参数。 限制为仅最佳分级的内容的结果。|

<a id="ID4EGEAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EIEAC"></a>


### <a name="parent"></a>Parent 的子磁盘）

[其他参考](atoc-xboxlivews-reference-additional.md)


<a id="ID4EUEAC"></a>


### <a name="further-information"></a>详细信息

[市场 URI](../uri/marketplace/atoc-reference-marketplace.md)

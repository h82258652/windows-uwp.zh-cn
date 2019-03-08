---
title: EDS 参数
assetID: 9475b427-53bc-697b-6d24-1787320260b7
permalink: en-us/docs/xboxlive/rest/edsparameters.html
description: " EDS 参数"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5412bcebc73dfd25d81b2073527e64e81de1bba4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655412"
---
# <a name="eds-parameters"></a>EDS 参数

<a id="ID4EO"></a>


## <a name="query-string-parameters"></a>查询字符串参数

这些查询参数不一定是接受的所有[娱乐发现服务 (EDS) Api](../uri/marketplace/atoc-reference-marketplace.md)，但所有接受由多个 API。

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| combinedContentRating| 字符串| 可选。 请参阅[获取 (/media/ {marketplaceId} / contentRating)](../uri/marketplace/uri-medialocalecontentratingget.md)。|
| ContinuationToken| 字符串| 可选。 继续标记是包含该服务所需的某些方案中的分页信息的不透明 blob。 如果省略此值，第一页结果被返回 （其中页大小由 maxItems 参数），以及可用于获取结果的第二页的继续标记。 第二页将包含结果的第三页的继续标记，依此类推。|
| 所需的| 字符串| 可选。 请参阅组合的字段名称 API。|
| desiredMediaItemTypes| 字符串数组| 可选。 此参数确定要在响应中返回的项的类型。|
| 域| 字符串| 可选。 域参数确定游戏和应用 marketplace 上下文在哪种客户端调用的。 默认情况下的域是"新式"，以指示客户端可以只向请求 Xbox One 的内容。 如果客户端想要切换到图面上的 Xbox 360 内容的域，它必须为"Xbox360"指定域。 目前，跨域不支持结果。 可能的值为： <ul><li>Xbox360</li><li>现代</li></ul> SingleMediaGroupSearch 仅支持"Xbox360"值。 支持浏览和详细信息。 crossMediaGroupSearch 不受支持，将返回 400 错误。|
| 字段| 字符串| 可选。 请参阅[获取 (/media/ {marketplaceId} / 字段)](../uri/marketplace/uri-medialocalefieldsget.md)。|
| firstPartyOnly| 布尔值| 可选的筛选器参数。 确定是否返回仅第一方的内容，或者这两个第一方和第三方内容从查询返回是否。 |
| freeOnly| 布尔值| 可选的筛选器参数。 限制结果仅免费内容。|
| GroupBy| TK| GroupBy 参数用于帮助将分类到组中，在结果集中，而不是单个结果集。 指定此参数将修改结果集返回多个列表的项，其中每个存储桶中的项的数目确定由 maxItems 参数。 <ul><li>MediaGroup-按 MediaGroup 结果进行分组。</li></ul> |
| hasTrailer| 布尔值| 可选的筛选器参数。 确定是否返回的项必须包含尾部，或如果具有尾部是可选的。 如果值为 true，所有项必须都具有尾部。|
| id| 字符串| 可选。 如果提供，限制仅为具有给定 ID 的项的子级的结果 如果提供此参数，则还必须指定 MediaItemType。 |
| id| 字符串数组| 必需。 所有要返回其详细信息 （最多 10 个） 的 Id。 请注意，任何 ID 包含非法将放入中的字符 （ProviderContentId 类型 Id 是通常情况下完整的 Url 本身，因此包含非法字符） 的 URL 必须采用 URL 编码，正确发送到 EDS。|
| idType| 字符串| 可选。 Id 中传递给 id 参数的类型。 有效值包括： <ul><li>规范 （必应/应用商店）</li><li>XboxHexTitle （应用程序在控制台上播放）</li></ul>  所有提供的 Id 必须共享相同 idType。 如果省略此值，则会假定所有 Id Canonical。|
| latestOnly| 布尔值| 可选的筛选器参数。 将结果限制到仅具有最新的发布日期。|
| maxItems| 32 位有符号的整数| 可选。 确定应从调用返回的最大项数。 有效值为 1 到 25，非独占之间的数字。 如果省略，则该参数默认为 25。|
| mediaGroup| 字符串| 可选。 媒体组的 Id。 所有提供的 Id 必须共享同一介质组。|
| MediaItemType| 字符串| 可选。 Id 参数中未指定其 ID 的项的类型。 如果提供的 id 参数，则还必须指定此参数。|
| orderBy| 字符串| 必需。 OrderBy 参数确定返回的项的排序方式。 这里列出了此字段的公共值，但某些 Api 可能支持其他值。<ul><li>playCountDaily-按计数的时间播放媒体，最新一天。</li><li>freeAndPaidCountDaily-按计数的免费和付费购买，最新一天。</li><li>paidCountAllTime-按计数仅付费购买，所有时间。</li><li>paidCountDaily-按计数仅付费购买，最新一天。</li><li>digitalReleaseDate-按日期可供下载。</li><li>releaseDate-按商店中可用的日期回退到数字发布日期 （如果可用）。</li><li>userRatings-按平均用户分级。</li></ul> |
| preferredProvider| 字符串| 可选。 如果用户具有首选内容提供程序，如 Comcast Xfinity 或 Verizon FIOS 可传入该提供程序的 ID。 在实际项目的顺序不会更改，为每个项，指定提供程序的信息将显示在提供程序的列表的顶部 （如果首选的内容提供商具有可用的项）。|
| q| 字符串| 必需。 查询在搜索中使用的术语。|
| queryRefiners| 字符串数组| 可选。 请参阅的列表[EDS 查询精简将](edsqueryrefiners.md)。|
| 关系| 字符串| 可选。 使用 ID 参数作为基础来搜索为指定的关系类型匹配其他产品的筛选器： <ul><li>bundledWith-查找捆绑产品的 ID 参数是作为这些捆绑包的一部分。</li><li>bundledProducts-查找 ID 参数指定的绑定中包含的产品。</li></ul>  使用此参数，将返回唯一产品 （可以在浏览调用中返回） 在 marketplace 中可见的。 如果绑定具有一种隐藏的产品，它仍是捆绑包的一部分，但不是会返回这些结果中。|
  | ScopeId | 字符串 | 有关视频媒体在反向查找方案中使用此参数。 |
  | ScopeIdType | 字符串 | 有关视频媒体在反向查找方案中使用此参数。 可能值：标题。 |
  | skipItems | 32 位有符号的整数 | 可选。 在非跨组方案中的分页，skipItems 参数用于确定多少个项都出现 （并因此应显示哪些项首先在结果集中）。 值是基于 0 的因此 skipItems = 0 （或只需不提供 skipItems） 开始检索开始处的列表。 skipItems = 3 将跳过列表中的前三个项并开始使用第四个项的检索。 |
  | subscriptionLevel | 字符串数组 | 可选的筛选器参数。 SubscriptionLevel 参数确定类型的订阅用户有 （如用户是否拥有付费的订阅或免费订阅）。 可能的值如下所示。 <ul><li>金级：用户具有付费的订阅</li><li>silver:用户可以免费订阅。</li></ul>  |
| TargetDevices| 字符串| EDS 提供灵活地进行筛选提供了适用于目标设备。 产品/服务 （ProviderContent 或可用性） 返回的项可以限制到的目标设备。|
| topRatedOnly| 布尔值| 可选的筛选器参数。 将结果限制为仅最佳分级的内容。|

<a id="ID4EGEAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EIEAC"></a>


### <a name="parent"></a>Parent 的子磁盘）

[其他参考](atoc-xboxlivews-reference-additional.md)


<a id="ID4EUEAC"></a>


### <a name="further-information"></a>详细信息

[Marketplace Uri](../uri/marketplace/atoc-reference-marketplace.md)

---
title: 辅助 EDS API
assetID: 5729ab80-e88d-0190-fb61-bd0cc4f134f6
permalink: en-us/docs/xboxlive/rest/eds-apis.html
description: " 辅助 EDS API"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3f2f729359f52b09879e7227ede71e238fe63801
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603232"
---
# <a name="auxiliary-eds-apis"></a>辅助 EDS API

有几个娱乐发现服务 (EDS) Api 不直接提供信息的内容，但提供了有关如何使用服务或帮助驱动器常见 UI 模型的常规信息。

<a id="ID4EQ"></a>


## <a name="auxiliary-apis"></a>辅助 Api

| API| URI| 描述|
| --- | --- | --- |
| API 参数值| /{locale}/metadata| 可以对该服务的调用中使用的参数的可能值的枚举|
| 组合内容分级生成器| /{locale}/contentRating| 在其他 Api 中为筛选出可能令人反感或显式内容创建一个值，可以使用该值。 有关详细信息，请参阅下文。|
| 组合的字段名称生成器| /{locale}/fields| 在控制返回哪些字段的详细信息 Api 中创建一个值，可以使用该值。 有关详细信息，请参阅下文。|

<a id="ID4EBC"></a>


### <a name="api-parameter-values"></a>API 参数值

此 API 介绍了可以与服务一起使用的参数。 返回的信息是可供客户端 UI，因为本地化的文本通常会显示每个参数。

没有任何以下 Api 接受任何查询参数。

| API| URI| 描述|
| --- | --- | --- | --- | --- | --- |
| 类型| /{locale}/metadata/mediaGroups| 媒体组的完整列表|
| 媒体项每个媒体组类型| /{locale}/metadata/mediaGroups/{mediaItemTypeGroup}/mediaItemTypes| 媒体的列表项给定的介质组中包含的类型。|
| 所有媒体项类型| /{locale}/metadata/mediaItemTypes| 媒体项类型的完整列表|
| 每个媒体项类型的字段| /{locale}/metadata/mediaItemTypes/{mediaItemType}/fields| 单个媒体项类型中的字段列表|
| 查询精简将| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners| 查询精简将支持给定的媒体项类型的列表|
| 所有查询精选值| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners/{queryRefiner}| 给定介质指定的查询精选的值项类型|
| 所有查询精选子值| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners/{queryRefiner}/subQueryRefinerValues| 对于给定的查询精选值 (例如"subgenres 中给定流派") 的子值的列表。 查询精选值作为名为"queryRefinerValue"，这样做是为了允许查询精选值与字符禁止 URI 词干中传递的查询字符串参数传入。|
| 排序| /{locale}/metadata/mediaItemTypes/{mediaItemType}/sortOrders| 提供的媒体项类型的排序顺序的列表|

<a id="ID4EEF"></a>


### <a name="combined-content-rating-generator"></a>组合内容分级生成器

强制实施对允许儿童若要查看的内容的家长控制是一项复杂的任务。 不仅每个媒体项类型具有其自己的评级系统，而且这些评级系统而异的国家/地区到国家/地区。 这意味着需要指定正确筛选所有项的数据的多个不同部分。

而不是所有 API 调用中指定的所有参数，此 API 生成值将传递到其他 Api 中的 combinedContentRating 参数并仍进行通信的相同信息。 此操作旨在使 Api 更易于使用和维护，如多个参数传递到此 API 处于折叠状态到单个可重用的值对于其他 Api。

尽管此 API 返回的确切值可能会最终更改，它们应极少更改 （例如之间的 EDS 版本） 并因此无法针对长时间的缓存。 接受 combinedContentRating 参数将提供的有意义的错误消息中传递的值是无效的如果这是一个指示，指明调用方只是任何 API 需要调用此 API 在再次获取更新后的值。 如果 API 接受 combinedContentRating 参数，但没有提供，没有筛选的内容将根据家长控制

> [!NOTE]
> 这并不意味着，返回唯一的"安全"内容-这意味着，返回的所有内容，包括可能的成人内容）。



<a id="ID4EWF"></a>


### <a name="combined-field-name"></a>组合的字段名称

EDS Api 中，默认情况下，返回一组很少最小的每个项的字段：

   * 媒体项类型
   * 媒体组
   * ID
   * 名称

若要获取详细信息，Api 接受用于指定应返回数据的其他部分的"字段"参数。 由于有许多可能的字段，每个 API 调用的完全指定其名称将极大地膨胀，请求。 相反，名称可传递到此 API，这会导致生成可以传递到其他 Api 的更小的值。

对于任何接受此参数的 API，提供的值必须是指定的介质的所有项类型中的所有字段的超集。 不能指定不同的字段的不同的媒体项类型集。 但是，如果字段应用于一个媒体项类型，但不是另一个，它才会出现在媒体项类型存在数据 (例如如果结合使用字段名称 API 调用中包含"AvatarBodyType"，则仅 AvatarItems 将包含该字段)。

此 API 返回的值是高度可缓存-实际上，它们不应更改除 EDS 部署之间。 建议，如果需要缓存，则缓存持续时间不超过用户会话。

除了接受的实际字段名称，此 API 接受"all"作为有效的值。 这将生成一个包含可以指定每个字段的值。 "全部"的值很可能仅用于开发、 调试和测试目的。

<a id="ID4ERG"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4ETG"></a>


##### <a name="parent"></a>Parent 的子磁盘）  

[其他参考](atoc-xboxlivews-reference-additional.md)


<a id="ID4E6G"></a>


##### <a name="further-information"></a>详细信息

[Marketplace Uri](../uri/marketplace/atoc-reference-marketplace.md)

---
title: GET (/users/me/inventory)
assetID: 7b74dd08-2854-319d-3ed0-ddee75d922b9
permalink: en-us/docs/xboxlive/rest/uri-inventoryget.html
author: KevinAsgari
description: " GET (/users/me/inventory)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 270f36d354ee4561d12f78644436ad51e286768c
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2018
ms.locfileid: "4125237"
---
# <a name="get-usersmeinventory"></a>GET (/users/me/inventory)
提供的一套当前与返回到调用方提供的用户相关联的清单。
这些 Uri 的域是`inventory.xboxlive.com`。

  * [备注](#ID4EV)
  * [查询字符串参数](#ID4EHB)
  * [示例请求](#ID4EDE)
  * [响应正文](#ID4ERE)

<a id="ID4EV"></a>


## <a name="remarks"></a>备注

没有策略检查，强制执行，否则筛选会作为此调用的一部分。 调用方可以选择将查询参数传递以缩小返回的结果的范围。

调用方可以使用一个延续令牌的结果翻页，通过以前响应所述： **/users/me/inventory?continuationToken = continuationTokenString**。

调用方可能会使若要查看特定项的信息的详细信息与特定项 URL 的 API 调用。

<a id="ID4EHB"></a>


## <a name="query-string-parameters"></a>查询字符串参数

| 参数| 类型| 说明|
| --- | --- | --- |
| 可用性| 字符串| 要返回的项数当前可用。 默认值为"可用"，然后返回项目的当前日期之间的开始日期和结束日期范围。 其他值包括"全部"，返回所有项和"不可用"当前日期为其返回项都位于之外的开始日期和结束日期范围和它因此当前不可用。 |
| 容器| 字符串| 可选。 如果你将值设置为游戏的产品 ID，然后从库存结果仅包括与该游戏相关的项目。 从你的服务器以筛选到特定游戏的产品的结果调用库存时，这是特别有用。|
| expandSatisfyingEntitlements| 字符串| 一个标志，指示是否响应包括用户已在结果中返回的所有令人满意的权利。 默认值为"false"。 此参数使用值为"true"，通过满足 Xbox 360 购买迁移到 Xbox One，订阅权益，如捆绑的权利项，向用户授予的任何产品时等被添加到结果。 如果此值为"false"然后仅父项目，如捆绑包的 ProductID 返回结果，并且不包含单个项。 **注意：** 如果对 URI 中不包含项类型参数仅支持使用此参数值为"true"，否则你将收到了 HTTP 400 错误。 |  
  | productIds | 字符串 |  你想要从用户的库存，特别是检索的 ProductIds 集合分隔，。  如果用户在其清单结果中没有提供的产品 Id，该项目将不会出现在结果中的 API 调用。 如果为 true 传入以及 expandSatisfyingEntitlements 参数集捆绑包的 productID，（无论，你会在你的查询字符串中指定其 productIds） 中调用结果返回捆绑包中包含的所有项。   |
  | 状态 | 字符串 | 要返回的项的状态。 默认值为"全部"，这将返回所有项目。 其他值"启用"，指示该唯一 itemsthat 启用应返回，"暂停"，这指示仅在暂停的项应返回，"过期"，指示应返回的已过期的项目，"取消"，这表示应返回取消的项目，"Renewed"，这表示应返回的已更新的项目。  |

除了这些资源支持标准分页机制。

<a id="ID4EDE"></a>


## <a name="sample-request"></a>示例请求

此 URI 方法的完全限定域名 `https://inventory.xboxlive.com/users/me/inventory.
         `

> [!NOTE] 
> 哪些用户被视为取决于令牌提供，其中可能包括多个用户。 如果你希望单个用户的库存，还必须为特定用户想要以独占方式，请考虑提供用户哈希。

.

<a id="ID4ERE"></a>


## <a name="response-body"></a>响应正文

如果在调用成功，该服务返回的数组清单项。 请参阅[inventoryItem (JSON)](../../json/json-inventoryitem.md)。

<a id="ID4E4E"></a>


### <a name="sample-response"></a>示例响应


```cpp
{
  "pagingInfo": {
    "continuationToken": string,
    "totalItems": int
  },
  "items":
  {
    "url": string,
    "itemType": "Music",
    "titleId": string,
    "containers": string,
    "obtained": DateTime,
    "startDate": DateTime,
    "endDate": DateTime,
    "state": "Enabled"  
}

```


<a id="ID4EHF"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EJF"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/users/me/inventory](uri-inventory.md)


<a id="ID4ETF"></a>


##### <a name="further-information"></a>详细信息

[EDS 通用标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询优化器](../../additional/edsqueryrefiners.md)

 [市场 URI](atoc-reference-marketplace.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

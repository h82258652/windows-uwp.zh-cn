---
title: GET (/users/me/inventory)
assetID: 7b74dd08-2854-319d-3ed0-ddee75d922b9
permalink: en-us/docs/xboxlive/rest/uri-inventoryget.html
description: " GET (/users/me/inventory)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 31c787108fad84f06b02ded3958f9d2f99727cbe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632202"
---
# <a name="get-usersmeinventory"></a>GET (/users/me/inventory)
提供一组的当前与返回给调用方提供的用户相关联的清单。
这些 Uri 的域是`inventory.xboxlive.com`。

  * [备注](#ID4EV)
  * [查询字符串参数](#ID4EHB)
  * [示例请求](#ID4EDE)
  * [响应正文](#ID4ERE)

<a id="ID4EV"></a>


## <a name="remarks"></a>备注

无策略检查，强制执行，或筛选将发生此调用的一部分。 调用方具有的传递查询参数，以便缩小返回的结果的范围的选项。

调用方可以通过使用继续标记的结果页，如前一响应通过提供： **/users/me/inventory？ continuationToken = continuationTokenString**。

调用方可能会使用特定项的 URL 的 API 的详细信息调用若要查看特定项目的信息。

<a id="ID4EHB"></a>


## <a name="query-string-parameters"></a>查询字符串参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| 可用性| 字符串| 要返回的项数当前可用。 默认值为"可用"为其当前的日期是开始日期和结束之间的返回项的日期范围。 其他值包括"全部"，它将返回所有项和"不可用"的当前日期为其返回的项都范围之外的开始日期和结束日期范围并因此当前不可用。 |
| 容器| 字符串| 可选。 如果将值设置为一个游戏的产品 ID，然后从清单结果只包括与该游戏相关的项。 从你的服务器来筛选到特定游戏的产品的结果调用清单时，这是特别有用。|
| expandSatisfyingEntitlements| 字符串| 一个标志，指示是否响应包含用户在结果中返回的所有令人满意的权利。 默认值为"false"。 如果值为"true"，通过满足如捆绑在一起的权利项 Xbox 360 购买迁移到 Xbox One，订阅权益，向用户授予任何产品使用此参数等添加到结果。 当此值为"false"，则仅父项，如捆绑的 ProductID 返回结果而不是单个包含的项目中。 **注意：** 如果在 URI 中不包含 itemType 参数仅支持使用此参数，值为"true"，否则将收到 HTTP 400 错误。 |  
  | productIds | 字符串 |  一系列你想要专门从用户的清单中检索的 Productid 分隔 '，'。  如果用户在其清单结果中没有提供产品 id，该项将不出现在结果中从 API 调用。 如果为 true 传入 expandSatisfyingEntitlements 参数集以及一个捆绑包的 productID，（无论在查询字符串中指定其 Productid） 调用结果中返回数据包中包含的所有项。   |
  | 状态 | 字符串 | 要返回的项的状态。 默认值为"全部"，这将返回所有项。 其他值处于"已启用"，指示启用该唯一 itemsthat 应返回，"挂起"，指示已挂起的项应返回的"已过期"指示应返回其已过期的项，"已取消"，指示应返回已取消的项，并且指示应返回已续订的项"Renewed"。  |

除此之外，该资源支持标准的分页机制。

<a id="ID4EDE"></a>


## <a name="sample-request"></a>示例请求

为此 URI 方法的完全限定域名 `https://inventory.xboxlive.com/users/me/inventory.
         `

> [!NOTE] 
> 哪些用户被视为取决于令牌提供，其中可能包括多个用户。 如果你想单个用户的清单，您还必须为想要以独占方式考虑的特定用户提供用户哈希。

.

<a id="ID4ERE"></a>


## <a name="response-body"></a>响应正文

如果调用成功，该服务返回的清单项的数组。 请参阅[inventoryItem (JSON)](../../json/json-inventoryitem.md)。

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

[EDS 常见标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询精简将](../../additional/edsqueryrefiners.md)

 [Marketplace Uri](atoc-reference-marketplace.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

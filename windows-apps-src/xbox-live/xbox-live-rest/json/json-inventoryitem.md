---
title: inventoryItem (JSON)
assetID: 446cca28-b2d3-1b84-f973-94065519b391
permalink: en-us/docs/xboxlive/rest/json-inventoryitem.html
description: " inventoryItem (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 240e527258923cff146009810c190e401e0574d0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628492"
---
# <a name="inventoryitem-json"></a>inventoryItem (JSON)
核心清单项表示了授权可以授予标准项。
<a id="ID4EN"></a>


## <a name="inventoryitem"></a>inventoryItem

InventoryItem 对象具有以下规范。

| 成员| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| url| 字符串| 此特定库存物料的唯一标识符。|
| itemType| 字符串| 项的类型。 当前值 <ul><li><b>Unknown</b></li><li><b>游戏</b></li><li><b>电影</b></li><li> <b>TVShow</b></li><li><b>MusicVideo</b></li><li><b>GameTrial</b></li><li><b>ViralVideo</b></li><li><b>TVEpisode</b></li><li><b>TVSeason</b></li><li><b>TVSeries</b></li><li><b>VideoPreview</b></li><li><b>海报</b></li><li><b>播客</b></li><li><b>映像</b></li><li><b>BoxArt</b></li><li><b>ArtistPicture</b></li><li><b>GameContent</b></li><li><b>GameDemo</b></li><li><b>主题</b></li><li><b>XboxOriginalGame</b></li><li><b>GamerTile</b></li><li><b>ArcadeGame</b></li><li><b>GameConsumable</b></li><li><b>唱片集</b></li><li><b>AlbumDisc</b></li><li><b>AlbumArt</b></li><li><b>GameVideo</b></li><li><b>BackgroundArt</b></li><li><b>TVTrailer</b></li><li><b>GameTrailer</b></li><li><b>VideoShort</b></li><li><b>Bundle</b></li><li><b>XnaCommunityGame</b></li><li><b>Promotional</b></li><li><b>MovieTrailer</b></li><li><b>SlideshowPreviewImage</b></li><li><b>ServerBackedGames</b></li><li><b>Marketplace</b></li><li><b>AvatarItem</b></li><li><b>LiveApp</b></li><li><b>WebGame</b></li><li><b>MobileGame</b></li><li><b>MobilePdlc</b></li><li><b>MobileConsumable</b></li><li><b>App</b></li><li><b>MetroGame</b></li><li><b>MetroGameContent</b></li><li><b>MetroGameConsumable</b></li><li><b>GameLayer</b></li><li><b>GameActivity</b></li><li><b>GameV2</b></li><li><b>SubscriptionV2</b></li><li><b>订阅</b><br/><br/> **注意：** 指定游戏**GameV2**，消费品都**GameConsumable**，并持久 DLC 是**GameContent**。 |
  | 容器 | 字符串 | 这是包含此项的"容器"的组。 可以查询属于特定容器的项的用户的清单。 通过购买项添加到清单时确定这些容器。 |
  | 获取 | DateTime | 日期和时间项添加到用户的清单。 |
  | startDate | DateTime | 日期和时间项或者将成为可供使用。 |
  | endDate | DateTime | 日期和时间项或者将变为不可用。 |
  | 状态 | 字符串 | 项的状态。 允许的值为**已启用**， **Suspended**，**已过期**，**已取消**， **Renewed**。  |
  | trial | 布尔值 | 必需。 如果此权利是试用; 则为 true否则为 false。 如果购买了授权的试用版，然后购买完整版，您将收到两者。 |
  | trialTimeRemaining | TimeSpan | 可以为 null。 试用版，以分钟为单位上剩余多少时间。 |
  | 可使用 | 数组 | 如果项为可使用，则将包含的内联表示形式可使用清单项，以及其当前数量的唯一标识符 （链接）。 |

<a id="ID4EMAAC"></a>


## <a name="sample-json-syntax"></a>示例 JSON 语法


```json
inventoryItem {
  "url": string,
  "itemType": "Music" | "Video" | "Game" | "AvatarItem" | "Subscription" | "DLC" | "Consumable" | ...,
  "obtained": DateTime,
  "beginDate": DateTime,
  "endDate": DateTime,
  "state": "Unavailable" | "Available" | "Suspended" | "Expired",
  "trial": true,
  "trialTimeRemaining":"23:12:14",
  ("consumable": {"url": string, "quantity": int})
}

```


<a id="ID4EVAAC"></a>


## <a name="consumable-inventory-item"></a>可使用清单项

可使用的实体提供了可使用项的属性的最小集。

| 成员| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| url| 字符串| 特定的可使用清单项的的唯一标识符。|
| quantity| 32 位有符号的整数| 此清单项的当前数量。|


```json
consumableInventoryItem {
  "url": string,
  "quantity": int
}

```


<a id="ID4E4BAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4E6BAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)


<a id="ID4EJCAC"></a>


##### <a name="reference"></a>参考

[/users/me/inventory](../uri/marketplace/uri-inventory.md)

 [/inventory/consumables/{itemID}](../uri/marketplace/uri-inventoryconsumablesitemurl.md)

 [/inventory/{itemID}](../uri/marketplace/uri-inventoryitemurl.md)

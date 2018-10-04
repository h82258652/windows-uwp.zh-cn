---
title: inventoryItem (JSON)
assetID: 446cca28-b2d3-1b84-f973-94065519b391
permalink: en-us/docs/xboxlive/rest/json-inventoryitem.html
author: KevinAsgari
description: " inventoryItem (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0a7521de7da4ebd31f0a1d8c59bb7c0134eddc08
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4353221"
---
# <a name="inventoryitem-json"></a>inventoryItem (JSON)
核心清单项表示可被授予权利的标准项。
<a id="ID4EN"></a>


## <a name="inventoryitem"></a>inventoryItem

InventoryItem 对象具有以下规范。

| 成员| 类型| 描述|
| --- | --- | --- |
| url| 字符串| 此特定库存项目的唯一标识符。|
| 项类型| 字符串| 项类型。 当前值 <ul><li><b>Unknown</b></li><li><b>游戏</b></li><li><b>电影</b></li><li> <b>TVShow</b></li><li><b>MusicVideo</b></li><li><b>GameTrial</b></li><li><b>ViralVideo</b></li><li><b>TVEpisode</b></li><li><b>TVSeason</b></li><li><b>TVSeries</b></li><li><b>VideoPreview</b></li><li><b>海报</b></li><li><b>播客</b></li><li><b>图像</b></li><li><b>BoxArt</b></li><li><b>ArtistPicture</b></li><li><b>GameContent</b></li><li><b>GameDemo</b></li><li><b>主题</b></li><li><b>XboxOriginalGame</b></li><li><b>GamerTile</b></li><li><b>ArcadeGame</b></li><li><b>GameConsumable</b></li><li><b>唱片集</b></li><li><b>AlbumDisc</b></li><li><b>AlbumArt</b></li><li><b>GameVideo</b></li><li><b>BackgroundArt</b></li><li><b>TVTrailer</b></li><li><b>GameTrailer</b></li><li><b>VideoShort</b></li><li><b>捆绑包</b></li><li><b>XnaCommunityGame</b></li><li><b>促销</b></li><li><b>MovieTrailer</b></li><li><b>SlideshowPreviewImage</b></li><li><b>ServerBackedGames</b></li><li><b>市场</b></li><li><b>AvatarItem</b></li><li><b>LiveApp</b></li><li><b>WebGame</b></li><li><b>MobileGame</b></li><li><b>MobilePdlc</b></li><li><b>MobileConsumable</b></li><li><b>应用</b></li><li><b>MetroGame</b></li><li><b>MetroGameContent</b></li><li><b>MetroGameConsumable</b></li><li><b>GameLayer</b></li><li><b>GameActivity</b></li><li><b>GameV2</b></li><li><b>SubscriptionV2</b></li><li><b>订阅</b><br/><br/> **注意：** 游戏指定的**GameV2**、 易耗品**GameConsumable**，并持久型 DLC 是**GameContent**。 |
  | 容器 | 字符串 | 这是一套"容器"包含此项。 可为属于特定容器的项目中查询用户的清单。 这些容器的确定时由购买，该项将添加到清单。 |
  | 获取 | DateTime | 日期和时间项添加到用户的清单。 |
  | startDate | DateTime | 日期和时间的项目就会变得或将变为可供使用。 |
  | endDate | DateTime | 日期和时间的项目就会变得或将变为不可用。 |
  | 状态 | 字符串 | 项的状态。 允许值为**启用**、**暂停**、**到期**、**取消**，**续订**。  |
  | trial | 布尔值 | 必需。 如果此权利是试用版;否则为则返回 false。 如果你购买权利的试用版和购买完整版，你将收到两者。 |
  | trialTimeRemaining | 时间跨度 | 可以为 null。 试用版，以分钟为单位上剩余多少时间。 |
  | 消费品 | array | 如果项目是易耗型，它包含的内联表示形式的易耗型清单项，以及其当前的数量的唯一标识符 （链接）。 |

<a id="ID4EMAAC"></a>


## <a name="sample-json-syntax"></a>JSON 语法示例


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


## <a name="consumable-inventory-item"></a>易耗型库存项目

易耗型实体显示最小的易耗型项的属性集。

| 成员| 类型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| url| 字符串| 对于特定的易耗型库存项目的唯一标识符。|
| quantity| 32 位有符号整数| 当前此库存项目的数量。|


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

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)


<a id="ID4EJCAC"></a>


##### <a name="reference"></a>参考

[/users/me/inventory](../uri/marketplace/uri-inventory.md)

 [/ 清查/易耗品 / {itemID}](../uri/marketplace/uri-inventoryconsumablesitemurl.md)

 [/inventory/{itemID}](../uri/marketplace/uri-inventoryitemurl.md)

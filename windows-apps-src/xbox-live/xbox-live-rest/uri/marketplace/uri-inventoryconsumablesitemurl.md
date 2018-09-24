---
title: /users/me/consumables/{itemID}
assetID: 45724827-5e35-326f-3f17-f49e606d9e08
permalink: en-us/docs/xboxlive/rest/uri-inventoryconsumablesitemurl.html
author: KevinAsgari
description: 用户的 Xbox 易耗品 rESTful 终结点。
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7ed278542fa538a1297069b0f7d67d413e180f30
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "4152249"
---
# <a name="usersmeconsumablesitemid"></a>/users/me/consumables/{itemID}
访问完整的一组特定的易耗型库存项目的详细信息。
这些 Uri 的域是`inventory.xboxlive.com`。

  * [URI 参数](#ID4EV)

<a id="ID4EV"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- |
| itemID| 字符串| 为每个用户单数库存项目的唯一 ID|

<a id="ID4ERB"></a>


## <a name="valid-methods"></a>有效的方法

[POST ({itemID})](uri-inventoryconsumablesitemurlpost.md)

&nbsp;&nbsp;指示，已使用所有或易耗型库存项目的部分和递减请求的距离该消耗品的数量。

<a id="ID4E4B"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4E6B"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[市场 URI](atoc-reference-marketplace.md)


<a id="ID4EJC"></a>


##### <a name="further-information"></a>详细信息

[EDS 通用标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询优化器](../../additional/edsqueryrefiners.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

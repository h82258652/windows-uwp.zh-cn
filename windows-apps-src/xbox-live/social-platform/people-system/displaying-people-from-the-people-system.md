---
title: 显示来自人脉系统的联系人
author: KevinAsgari
description: 了解使用 Xbox Live 人脉系统显示人脉的代码流。
ms.assetid: c97b699f-ebc2-4f65-8043-e99cca8cbe0c
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fc6fd23d9113c4d0e3dbac6d3cf0c880fc71d6a9
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5477557"
---
# <a name="display-people-from-the-people-system"></a>显示来自人脉系统的联系人

Xbox Live 上的服务仅返回该服务所有的数据，并且仅返回对用户的 XUID 引用；例如，人脉服务仅拥有并返回用户联系人列表上的 XUID，以及有关每个 XUID 的一些非常基本的信息（如最喜爱的状态）。 状态服务拥有有关 XUID 的在线状态信息的数据。 排行榜服务拥有 XUID 列表上的排名信息。 显示名称和玩家代号信息永远不会从除个人资料服务以外的任何其他服务返回，因此，调用多个服务对于在体验中呈现联系人列表是必要的。

服务 API 的一般调用模式是进行一次单程调用，来首先从可以最好地筛选或排序列表的服务获取 XUID 列表，然后对获取每个 XIUD 所需的任何其他元数据所需要的其他服务进行同时的往返调用。 就图像而言，可能需要进行第三次往返调用来从图像 URL 获取图像。

为了减少为获取用户联系人列表相关数据所需的往返调用的次数，在相关服务中引入了人脉*名字对象*。 此新功能允许呼叫方向主要服务抽象地表达该服务应从人脉服务获取用户的联系人列表，然后使用该 XUID 集确定返回范围。

以下是一些调用流程场景示例，说明了标题如何从与人脉相关的服务获取数据：

-   游戏中的当前用户列表
-   当前用户处于联机状态的联系人列表
-   包含随机用户的全球排行榜
-   用户联系人的排行榜


## <a name="list-of-users-currently-in-game"></a>游戏中的当前用户列表

| 标题包含  | 目标  | 要呈现的字段  | 调用流程
|-------------------------------------------------|----------------------------------------------------|--------------------|--------------------------------------|
| 游戏中其他用户的随机 XUID 列表 | 为每位其他用户呈现最低限度的信息 | GameDisplayName \[个人资料\] | 使用 XUID 列表调用个人资料。 |


## <a name="list-of-the-current-users-people-who-are-online"></a>当前用户处于联机状态的联系人列表

## <a name="title-has"></a>标题包含：
当前用户的 XUID

## <a name="goal"></a>目标
呈现当前用户的联系人列表中联机用户的完整列表

## <a name="field-to-render-owning-service"></a>要呈现的字段\[拥有的服务\]
* 最喜爱的指示器[人脉]
* 显示图片[个人资料]
* GameDisplayName [个人资料]
* 基本联机状态（绿球）[状态]

## <a name="call-flow"></a>调用流程
1. 调用状态，传入人脉名字对象以获取每个用户联系人的 XUID 和联机状态。
1. 并行：
 1. 调用个人资料，传入整个 XUID 列表以获取每个联系人的显示名称和图片 URL。
 1. 调用联系人，传入 XUID 列表以找出是否有用户最喜爱的 XUID。
1. 调用个人资料后：
 1. 获取每个图片 URL 的图像

## <a name="global-leaderboard-containing-random-users"></a>包含随机用户的全球排行榜

## <a name="title-has"></a>标题包含：
排行榜的 ID/名称。

## <a name="goal"></a>目标
呈现排行榜上每个用户的基本信息

## <a name="field-to-render-owning-service"></a>要呈现的字段[拥有的服务]
* 最喜爱的指示器[人脉]
* GameDisplayName [个人资料]
* 排名[排行榜]
* 分数[排行榜]

## <a name="call-flow"></a>调用流程
1. 调用排行榜以获取特定排行榜的 XUID、排名和分数。
1. 并行：
 1. 调用个人资料，传入 XUID 列表以获取每个联系人的显示名称和图片 URL。
 1. 调用联系人，传入 XUID 列表以找出是否有用户最喜爱的 XUID。

## <a name="leaderboard-of-users-people"></a>用户联系人的排行榜

## <a name="title-has"></a>标题包含：
* 排行榜的 ID/名称。
* 当前用户的 XUID

## <a name="goal"></a>目标
呈现排行榜上每个用户的基本信息

## <a name="field-to-render-owning-service"></a>要呈现的字段[拥有的服务]
* 最喜爱的指示器[人脉]
* GameDisplayName [个人资料]
* 排名[排行榜]
* 分数[排行榜]

## <a name="call-flow"></a>调用流程
1. 调用排行榜，传入人脉名字对象以获取仅限于用户联系人列表的特定排行榜的 XUID、排名和分数。
1. 并行：
 1. 调用个人资料，传入 XUID 列表以获取每个联系人的显示名称和图片 URL。
 1. 调用联系人，传入 XUID 列表以找出是否有用户最喜爱的 XUID。

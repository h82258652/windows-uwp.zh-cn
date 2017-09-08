---
title: Achievements 2017
author: StaceyHaffner
description: "介绍如何在 UDC 中配置成就以提供奖励。"
ms.assetid: 
ms.author: sthaff
ms.date: 07-11-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "xbox live, xbox, 游戏, uwp, windows 10, xbox one, udc, 通用开发人员中心"
ms.openlocfilehash: eef41656b76db57cf68b3247285ac7b875370778
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2017
---
# <a name="configure-achievements-2017-on-dev-center"></a>在开发人员中心上配置 Achievements 2017

> [!IMPORTANT]
> 成就仅适用于 ID@Xbox 或托管合作伙伴。 参加 Xbox Live 创意者计划的游戏不受支持。

你可以使用通用开发人员中心 (UDC) 来配置与你的游戏关联的 [Achievements 2017](simplified-achievements.md)。 通过以下操作添加新成就：

1. 导航至游戏中的“成就”部分，位于 `Services` -> `Xbox Live` -> `Achievements` 下。
2. 单击 `New Achievement` 按钮并填写表格。  完成后，单击 `Save`。

![](../images/udc/achievements_1.png)

## <a name="description"></a>描述
你可以在描述部分输入成就的基本信息，如名称和锁定/解锁描述。 你可以通过访问 UDC 中的 `Localized Strings` 服务配置部分为成就添加本地化支持。

![](../images/udc/achievements_2.png)

`Achievement Name` 字段为面向公众的成就名称。

`Locked Description` 字段为玩家在尚未解锁成就时将看到的描述。 此字段的长度限制是 100 个字符。

`Unlocked Description` 字段为玩家在解锁成就后将看到的描述。 此字段的长度限制是 100 个字符。

## <a name="details"></a>详细信息
详细信息部分用于关联重要信息，如图像、成就类型、玩家分数奖励（如有）以及在解锁之前是否应隐藏成就。

![](../images/udc/achievements_3.png)

`Image Icon` 字段为显示在成就旁边的图像。 它必须为 1920 x 1080 png。

`Base Achievements` 发布初始游戏时对玩家可用。 `Non-Base Achievements` 发布初始游戏（如发布新的 DLC 内容）后可用。

`Gamerscore` 字段为解锁成就时将会赢取的玩家分数积分值。 每个成就可获得 0-200 积分奖励。  

`Public` 无论是否已解锁成就，所有玩家均可看到成就。 `Secret` 在解锁之前，成就将被隐藏。

`Achievement deep link` 是一种从成就取回参数的方式，你可以通过该方式链接至游戏中可赢取成就的位置。 在 GET API 响应中，将会返回深层链接。 指定的 URL 必须包含`ms-xbl-{titleID}://`前缀。

> [!TIP]
> 成就深层链接需要使用游戏的十六进制 TitleId。 你可以在 UDC 的 `Xbox Live Setup`屏幕上找到它。 

## <a name="additional-rewards"></a>其他奖励
在某些情况下，在玩家解锁成就时，你可能想要提供游戏内奖励或插图。 你可以在 `Additional Rewards` 部分定义与成就关联的奖励（如有）。 成就可包含两个其他奖励 - 每种奖励类型之一。 你可以阅读[成就奖励](achievement-rewards.md)文章，以了解更多信息。

若要创建新的奖励，请单击 `Additional Rewards` 部分的 `Add Reward` 按钮并填写表格。

![](../images/udc/achievements_4.png)

### <a name="reward-details"></a>奖励详细信息
填写奖励详细信息以关联新奖励。 完成后，单击 `Add`。

![](../images/udc/achievements_5.png)

可以创建两种类型的成就奖励。 它们是： 

1. 如果你想要为玩家提供诸如高分辨率的概念艺术作品、早期的设计绘图、专门开发的艺术资源以及其他数字艺术资源之类的奖励，则可使用 `Art` 类型。 艺术资源可以在 Xbox One 仪表板中显示并且也可在配套体验中显示，只需查询成就服务即可。
2. 如果你想要在不更新游戏的前提下为玩家提供自定义的游戏内奖励，则可使用 `In-Game` 类型。 在某些潜在场景下为游戏内货币/积分或特殊字符、武器或地图的访问权限。

`Display Name` 字段是玩家将看到的奖励名称。 此字段的长度限制是 57 个字符。

`Description` 字段为玩家将看到的奖励描述，必须包含任何兑换说明。 此字段的长度限制是 90 个字符。

`Image` 或 `Art` 字段为与奖励关联的图像。 如果类型已设为艺术作品，则它将是为其奖励的插图。 否则，它表示玩家将收到的游戏内奖励。 此图像必须为 1920 x 1080 png。

仅当选择 `In-game` 奖励类型时才会显示 `In-game value` 字段。 它用于指定传递至游戏代码（用于解锁游戏内奖励）的值。
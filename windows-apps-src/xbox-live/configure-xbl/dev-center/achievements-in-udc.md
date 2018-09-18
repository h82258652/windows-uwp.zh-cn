---
title: Achievements 2017
author: PhillipLucas
description: 介绍如何在 Microsoft 开发人员中心配置成就以提供奖励。
ms.assetid: ''
ms.author: kevinasg
ms.date: 11/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, udc, 通用开发人员中心
ms.openlocfilehash: 108d022909afc9a118cc1f011acf100972576841
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "4015490"
---
# <a name="configure-achievements-2017-on-dev-center"></a>在开发人员中心上配置 Achievements 2017

> [!IMPORTANT]
> 成就仅适用于 ID@Xbox 或托管合作伙伴。 参加 Xbox Live 创意者计划的游戏不受支持。

可以使用 [Microsoft 开发人员中心](https://developer.microsoft.com/dashboard)来配置与游戏关联的 [Achievements 2017](../../achievements-2017/simplified-achievements.md)。 通过以下操作添加新成就：

1. 导航至游戏中的“成就”部分，位于**服务** > **Xbox Live** > **成就**下。
2. 单击**新成就**按钮并填写表格。  完成后，单击**保存**。

![在 Microsoft 开发人员中心创建新成就的屏幕截图](../../images/dev-center/achievements-1.png)

## <a name="description"></a>描述
可以在描述部分输入成就的基本信息，如名称和锁定/解锁描述。 可以通过访问 [Microsoft 开发人员中心](https://developer.microsoft.com/dashboard)的**本地化字符串**服务配置部分为成就添加本地化支持。

![在 Microsoft 开发人员中心配置新成就时的描述字段的屏幕截图](../../images/dev-center/achievements-2.png)

**成就名称**字段为面向公众的成就名称。

**未解锁成就**字段为玩家在尚未解锁成就时看到的描述。 此字段的长度限制是 100 个字符。

**已解锁成就**字段为玩家在解锁成就后看到的描述。 此字段的长度限制是 100 个字符。

## <a name="details"></a>详细信息
详细信息部分用于关联重要信息，如图像、成就类型、玩家分数奖励（如有）以及在解锁之前是否应隐藏成就。

![在 Microsoft 开发人员中心配置新成就时的详细信息字段的屏幕截图](../../images/dev-center/achievements-3.png)

**图像图标**字段为显示在成就旁边的图像。 它必须为 1920 x 1080 png。

**基础成就**在发布初始游戏时对玩家可用。 **非基础成就**在发布初始游戏（如发布新的 DLC 内容）后可用。

**玩家分数**字段为解锁成就时将会赢取的玩家分数积分值。 每个成就可获得 0-200 积分奖励。  

**公开**成就对所有玩家可见，无论他们是否已解锁成就。 **秘密**成就在解锁之前处于隐藏状态。

**成就深层链接**是一种从成就取回参数的方式，可以通过该方式链接至游戏中可赢取成就的位置。 在 GET API 响应中，将会返回深层链接。 指定的 URL 必须包含`ms-xbl-{titleID}://`前缀。

> [!TIP]
> 成就深层链接需要使用游戏的十六进制 TitleId。 在 [Microsoft 开发人员中心](https://developer.microsoft.com/dashboard)的 [Xbox Live 设置](xbox-live-setup.md)上可以找到它。

## <a name="additional-rewards"></a>其他奖励
在某些情况下，在玩家解锁成就时，可能需要提供游戏内奖励或插图。 可以在**额外奖励**部分定义与成就关联的奖励（如果有）。 成就可包含两个其他奖励 - 每种奖励类型之一。 你可以阅读[成就奖励](../../achievements-2017/achievement-rewards.md)文章，以了解更多信息。

要创建新奖励，请单击**额外奖励**部分的**添加奖励**按钮并填写表格。

![在 Microsoft 开发人员中心为成就添加奖励的屏幕截图](../../images/dev-center/achievements-4.png)

### <a name="reward-details"></a>奖励详细信息
填写奖励详细信息以关联新奖励。 完成后，单击**添加**。

![在 Microsoft 开发人员中心为成就配置奖励详细信息的屏幕截图](../../images/dev-center/achievements-5.png)

可以创建两种类型的成就奖励。 它们是：

1. 如果要为玩家提供诸如高分辨率的概念艺术作品、早期的设计绘图、专门开发的艺术资源以及其他数字艺术资源之类的奖励，可使用**艺术作品**类型。 通过查询成就服务，艺术资源会显示在 Xbox One 仪表板中，也可以显示在配套体验中。
2. 如果要在不更新游戏的前提下为玩家提供自定义的游戏内奖励，可使用**游戏内**类型。 在某些潜在场景下为游戏内货币/积分或特殊字符、武器或地图的访问权限。

**显示名称**字段是玩家将看到的奖励名称。 此字段的长度限制是 57 个字符。

**描述**字段为玩家将看到的奖励描述，必须包含任何兑换说明。 此字段的长度限制是 90 个字符。

**图像**或**艺术作品**字段为与奖励关联的图像。 如果类型设为艺术作品，则它将是为其奖励的插图。 否则，它表示玩家将收到的游戏内奖励。 此图像必须为 1920 x 1080 png。

仅当选择**游戏内**奖励类型时才会显示**游戏内值**字段。 它用于指定传递至游戏代码（用于解锁游戏内奖励）的值。

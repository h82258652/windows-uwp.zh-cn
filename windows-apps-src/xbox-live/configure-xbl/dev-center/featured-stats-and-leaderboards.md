---
title: 2017 特别推荐的统计数据和排行榜
author: shrutimundra
description: 了解如何在 Windows 开发人员中心上配置 Xbox Live 2017 特别推荐的统计数据和排行榜
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.author: kevinasg
ms.date: 10/30/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, 游戏, uwp, Windows 10, Xbox one, 特别推荐的统计数据和排行榜, 排行榜, 统计数据 2017, Windows 开发人员中心
ms.openlocfilehash: 7db8efe28c6dc85c129823ec8f6b0ddac69b49f4
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5819028"
---
# <a name="configuring-featured-stats-and-leaderboards-2017-on-windows-dev-center"></a>在 Windows 开发人员中心上配置 2017 特别推荐的统计数据和排行榜

若要让游戏与统计数据服务交互，需要在 [Windows 开发人员中心](https://developer.microsoft.com/dashboard)上定义统计数据。 所有特别推荐的统计数据都将显示在游戏中心内，因而它可以自动充当排行榜。 我们将存储原始值，但是游戏具备可确定是否应提供新值的逻辑。

![游戏中心成就页面的屏幕截图](../../images/dev-center/featured-stats-and-leaderboards/featured-stats-and-leaderboards-2.png)上图显示特别推荐的统计数据在标题 GameHub 中的效果。 特别推荐的统计数据带红色方框显示。

使用 Data Platform 2017，你只需要配置用于精选到玩家游戏中心页面的社交排行榜的统计数据。

可以使用 Windows 开发人员中心配置与你的游戏关联的特别推荐的统计数据和排行榜。 通过执行以下操作添加配置：

1. 导航到针对你的作品的**特别推荐的统计数据和排行榜**部分（位于**服务** > **Xbox Live** > **特别推荐的统计数据和排行榜**下）。
2. 单击**新建**按钮会打开一个强制回应表单。 填写完成后，单击**保存**。

![新特别推荐的统计数据和排行榜对话框的图像](../../images/dev-center/featured-stats-and-leaderboards/featured-stats.png)

**显示名称**字段是用户将在游戏中心里看到的字段。 该字符串可以在 Xbox Live 服务配置的**本地化字符串**中进行本地化。

**ID** 字段是统计数据名称，也是你通过代码更新统计数据时对其引用的方式。 有关更多详细信息，请参阅[更新统计数据](../../leaderboards-and-stats-2017/player-stats-updating.md)。

**格式**是统计数据的数据格式。选项包括整数、小数、百分比、短时间跨度、长时间跨度和字符串。

每个**格式**选项选中后将在下拉列表中提供有关接受值或缩进格式的信息。

* 整数统计数据接受整数，如 1、2 或 100。
* 小数统计数据接受小数点后面两位的分数，如 1.05 或 12.00
* 百分比统计数据接受 0 到 100 之间的整数。 后跟“%”。 （例如 0%、100%）
* 短时间跨度统计数据遵循 HH:MM:SS 格式（如 02:10:30），并要求你提供统计数据的时间单位。可用的时间单位有毫秒、秒、分钟、小时和天。
* 长时间跨度统计数据遵循 Xd Xh Xm 格式（如 1d 2h 10m），还要求提供统计数据的时间单位。

**排序**字段让你可以将排行榜的排序顺序更改为升序或降序。

在配置特别推荐的统计数据和排行榜时，请注意以下要求：

| 开发人员类型 | 要求 | 限制 |
|----------------|-------------|-------|
| Xbox Live 创意者计划 | 不要求将任何统计数据指定为特别推荐的统计数据 | 20 |
| ID@Xbox 和 Microsoft 合作伙伴 | 你必须至少指定 3 个特别推荐的统计数据 | 20 |

## <a name="next-steps"></a>后续步骤

接下来，你将需要通过代码更新统计数据。  有关更多详细信息，请参阅[更新统计数据](../../leaderboards-and-stats-2017/player-stats-updating.md)。

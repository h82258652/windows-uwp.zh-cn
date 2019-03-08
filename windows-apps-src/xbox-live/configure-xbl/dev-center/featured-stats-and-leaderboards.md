---
title: 2017 特别推荐的统计数据和排行榜
description: 了解如何配置在合作伙伴中心中的 Xbox Live 特色统计信息和排行榜 2017
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 10/30/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live、 Xbox、 游戏、 uwp、 windows 10、 一个 Xbox、 特色统计信息和排行榜、 排行榜、 统计信息 2017 中，合作伙伴中心
ms.openlocfilehash: e152ea8aa745536f0b7843f4f7d372a79dc3223f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655102"
---
# <a name="configuring-featured-stats-and-leaderboards-2017-in-partner-center"></a>在合作伙伴中心中配置功能完备的统计信息和排行榜 2017

要进行与统计信息服务进行交互的游戏，stat 需要中定义[合作伙伴中心](https://partner.microsoft.com/dashboard)。 所有特别推荐的统计数据都将显示在游戏中心内，因而它可以自动充当排行榜。 我们将存储原始值，但是游戏具备可确定是否应提供新值的逻辑。

![在游戏中心获得的成就页面的屏幕截图](../../images/dev-center/featured-stats-and-leaderboards/featured-stats-and-leaderboards-2.png)上图显示在标题的 GameHub 外观特色统计信息。 特别推荐的统计数据带红色方框显示。

使用数据平台 2017，只需配置状态用于特色的播放器 GameHub 页的社交排行榜。

合作伙伴中心可用于配置功能完备的统计信息和程序与您的游戏排行榜。 通过执行以下操作添加配置：

1. 导航到针对你的作品的**特别推荐的统计数据和排行榜**部分（位于**服务** > **Xbox Live** > **特别推荐的统计数据和排行榜**下）。
2. 单击**新建**按钮会打开一个强制回应表单。 填写完成后，单击**保存**。

![新特别推荐的统计数据和排行榜对话框的图像](../../images/dev-center/featured-stats-and-leaderboards/featured-stats.png)

**显示名称**字段是用户将在游戏中心里看到的字段。 该字符串可以在 Xbox Live 服务配置的**本地化字符串**中进行本地化。

**ID** 字段是统计数据名称，也是你通过代码更新统计数据时对其引用的方式。 有关更多详细信息，请参阅[更新统计数据](../../leaderboards-and-stats-2017/player-stats-updating.md)。

**格式**是统计信息的数据格式。选项包括整数、 小数、 百分比、 短的时间跨度，长时间跨度和字符串。

每个**格式**选项选中后将在下拉列表中提供有关接受值或缩进格式的信息。

* 整数统计数据接受整数，如 1、2 或 100。
* 小数统计数据接受小数点后面两位的分数，如 1.05 或 12.00
* 百分比统计数据接受 0 到 100 之间的整数。 后跟“%”。 （例如 0%、100%）
* 短时间跨度统计信息按照 hh: mm： 格式，如 02:10:30，并将要求您提供的统计信息的时间单位。 可用的时间单位为毫秒、 秒、 分钟、 小时和天。
* 长时间跨度统计数据遵循 Xd Xh Xm 格式（如 1d 2h 10m），还要求提供统计数据的时间单位。

**排序**字段让你可以将排行榜的排序顺序更改为升序或降序。

在配置特别推荐的统计数据和排行榜时，请注意以下要求：

| 开发人员类型 | 要求 | 限制 |
|----------------|-------------|-------|
| Xbox Live 创意者计划 | 不要求将任何统计数据指定为特别推荐的统计数据 | 20 |
| ID@Xbox 和 Microsoft 合作伙伴 | 你必须至少指定 3 个特别推荐的统计数据 | 20 |

## <a name="next-steps"></a>后续步骤

接下来，你将需要通过代码更新统计数据。  有关更多详细信息，请参阅[更新统计数据](../../leaderboards-and-stats-2017/player-stats-updating.md)。

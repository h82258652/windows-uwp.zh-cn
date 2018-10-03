---
title: 玩家统计数据
author: aablackm
description: 简介 Stats 2017
ms.author: aablackm
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live，xbox，游戏，uwp，windows 10，xbox one，玩家统计数据、 排行榜，stats 2017
ms.localizationpriority: medium
ms.openlocfilehash: 24e953dcca8f57b689cdee879551b188aa01f42f
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2018
ms.locfileid: "4260163"
---
# <a name="stats-2017"></a>Stats 2017

Stats 2017 允许开发人员为表示进度和游戏中的实力的单个玩家配置的统计数据。 这些统计数据是将允许玩家与好友和较大的游戏社区，以及展示的一些功能和挑战你的游戏必须提供更具竞争力的社交工具。 如果你有使用特别推荐的统计信息，如最长偏移和最佳挂起时间的赛车游戏，你可以传达赛车游戏玩家可以预期的类型。 查看如何针对他们的好友和社区的玩家堆栈将为他们提供更多奖励购买和玩你的作品。 玩家将看到游戏的 GameHub 特别推荐的统计信息。 特别推荐的统计数据可能还会定期出现在用户可能会添加到其主视图的固定内容块。

通过接受统计信息值作为键/值对从你的游戏玩家并存储该统计数据值，以便它可以显示在以后运行 stats 2017。 Stats 2017 旨在通过将保存有关单个玩家的统计数据，以便他们可以相比并在排行榜上排名更高版本支持 Xbox Live 排行榜方案。 Stats 2017 接受发送给它几乎任何验证的情况，因此它是由你的作品来处理所有可确定正确的统计数据值的逻辑的值。

## <a name="configured-stats-and-featured-leaderboards"></a>配置统计数据和特别推荐的排行榜

在[Windows 开发人员中心仪表板](https://developer.microsoft.com/en-us/dashboard/windows/overview)上配置的统计数据。 若要配置的统计数据必须已配置主题作品。 如果你没有配置主题作品可以了解如何执行此操作通过阅读[创建和测试新创意者主题作品](../get-started-with-creators/create-and-test-a-new-creators-title.md)。  在 Windows 开发人员中心配置的统计信息将显示在标题 GameHub 中的如下图所示：

在下图中的黄色突出显示*功能统计数据*。
![官方俱乐部页面社交排行榜](../images/omega/gamehub_featuredstats.png)


在 Xbox One，用户可以直接 pin 游戏到其主屏幕快速发现相关信息、 社区驱动内容和开发人员文章。 你配置的统计数据也可能显示为固定主页内容的一部分。 统计数据都将显示稍高排名好友，鼓励他们玩自己向上排行榜的方式以及玩家。 排行榜中固定的内容将显示在下图中突出显示。

![光环 5 固定排行榜](../images/stats/Halo_5_Pinned_Leaderboard.png)

特别推荐的统计数据中游戏进度，具体取决于配置统计信息与其他还玩游戏的好友进行比较。 好友竞争高分或只需通过共享体验绑定时，这会鼓励你的游戏的多个播放时间。 特别推荐的统计数据排行榜都将重置每个月的第一天的每月排行榜。

开发人员限制为拥有为他们的游戏，不会超过 20 个特别推荐的统计数据但不要求适用于开发人员要包含在其游戏中的特别推荐的统计数据。

## <a name="further-reading"></a>进一步阅读
了解如何使用[更新 Stats 2017](player-stats-updating.md)更新统计数据值[配置特别推荐的统计数据和排行榜 2017年](../configure-xbl/dev-center/featured-stats-and-leaderboards.md)了解与配置的统计数据
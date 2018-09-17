---
title: Xbox Live SDK 的新增功能 - 2017 年 3 月
author: KevinAsgari
description: Xbox Live SDK 的新增功能 - 2017 年 3 月
ms.assetid: 03180585-6f87-4929-acfc-750bd78988a0
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cd57ea928db2a04dd4117af439c42666a5d3734e
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2018
ms.locfileid: "3988984"
---
# <a name="whats-new-for-the-xbox-live-sdk---march-2017"></a>Xbox Live SDK 的新增功能 - 2017 年 3 月

请参阅[新增功能 - 2016 年 12 月](1612-whats-new.md)文章了解 2016 年 12 月版本中增加了哪些功能。

## <a name="xbox-services-api"></a>Xbox 服务 API

### <a name="data-platform-2017"></a>数据平台 2017

我们引入了简化的统计 API。  传统上来讲，你必须发送与 XDP 或开发人员中心定义的统计规则对应的事件，并且这会更新云中的统计信息值。  我们将此模型称为 Stats 2013。

使用 Stats 2017，你的标题现在可以控制统计信息值。  你只需调用包含最新统计信息值的 API，它们即可被直接发送到服务，无需发送事件。  这使用新的 `StatsManager` API，你可以在[玩家统计信息](../leaderboards-and-stats-2017/player-stats.md)中了解更多信息

### <a name="github"></a>GitHub

Xbox Live API (XSAPI) 现已在 GitHub 上提供，位置在 [https://github.com/Microsoft/xbox-live-api](https://github.com/Microsoft/xbox-live-api)。  仍建议使用 XDK 附带的二进制文件或将其用作 NuGet 程序包，不过，欢迎你使用源，我们乐于接收有关源代码的反馈。  

## <a name="xbox-live-creators-program"></a>Xbox Live 创意者计划

Xbox Live 创意者计划是一项开发人员计划，为更广大的开发人员受众提供一部分 Xbox Live 功能。  如果你已经参与了 ID@Xbox 计划，这不会对你产生任何影响。

你可以在[开发人员计划概述](../developer-program-overview.md)中了解有关此计划的详细信息。

## <a name="documentation"></a>文档

推出了以下新文章

| 文章 | 描述 |
|---------|-------------|
|[Xbox Live 服务配置](../xbox-live-service-configuration.md) | 更新了为 Xbox Live 游戏进行服务配置的相关信息
| [配置 Unity 中的 Xbox Live](../get-started-with-creators/configure-xbox-live-in-unity.md) | 针对 Xbox Live 创意者计划开发人员的 Unity 设置的新信息 |
| [Xbox Live 沙盒](../xbox-live-sandboxes.md) | Xbox Live 沙盒和内容隔离的简化指南 |
| [Xbox Live 测试帐户](../xbox-live-test-accounts.md) | 有关测试帐户如何工作，以及如何在 Windows 开发人员中心创建帐户的信息 |

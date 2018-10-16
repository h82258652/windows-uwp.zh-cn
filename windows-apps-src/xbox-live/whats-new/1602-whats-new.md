---
title: Xbox Live SDK 的新增功能 - 2016 年 2 月
author: KevinAsgari
description: Xbox Live SDK 的新增功能 - 2016 年 2 月
ms.assetid: 7ff34ea4-f07d-4584-98e4-13977994ccca
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9180642d324146667425d6031143430dc499b5b9
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2018
ms.locfileid: "4687092"
---
# <a name="whats-new-for-the-xbox-live-sdk---february-2016"></a>Xbox Live SDK 的新增功能 - 2016 年 2 月

请参阅[新增功能 - 2015 年 10 月](1510-whats-new.md)文章了解 1510 中增加了哪些功能

## <a name="os-and-tool-support"></a>操作系统和工具支持
Xbox Live SDK 支持 Windows 10 RTM [版本 10.0.10240] 和 Visual Studio 2015 RTM [版本 14.0.23107.0]。

## <a name="throttling"></a>限制
- 细化限制很快将推向大多数 Xbox Live 服务。  Xbox 服务 API (XSAPI) 将自动处理重试，并通知你在开发期间被限制的调用。  更多详细信息可以在“文档”的[有关调用 Xbox Live 的最佳实践](../using-xbox-live/best-practices/best-practices-for-calling-xbox-live.md)一文中找到。

## <a name="leaderboards"></a>排行榜
- 多列排行榜现在可以使用 GetLeaderboard API 访问。 如果你提供其他列的名称矢量，如果存在这些列，结果中列的矢量将被填充。

## <a name="documentation"></a>文档
- [Application Insights](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/application-insights) 文档在此处。  你可以利用免费的 Azure 帐户使用 Application Insights 来近似实时地查看数据平台事件。  此功能目前仅面向在桌面的 Windows 10 上运行的 UWP 应用程序提供。
- 更新了讨论如何为发送数据平台事件生成包装器的有关面向 UWP 开发人员的 Xbox 常见事件工具的文档。  请注意，这是可选的，如果你愿意，你可以继续使用 WriteInGameEvent API。
- 使用 Fiddler 调试数据平台事件，并确保它们已正确发送。  这仅适用于 UWP 事件。
- 提供了有关如何收集 Live 跟踪分析器工具日志的信息。  请参阅[分析对 Xbox Live 服务的调用](../tools/analyze-service-calls.md)一文。

---
author: jnHs
Description: The Xbox analytics report in the Windows Dev Center dashboard shows you statistics about how your customers are engaging with the Xbox features in your product.
title: Xbox 分析报告
ms.author: wdg-dev-content
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, xbox 分析, xbox 实时分析, xbox 统计数据
ms.localizationpriority: medium
ms.openlocfilehash: 9e69c41ec2ae6dface93b9f3148e699e448faa18
ms.sourcegitcommit: 914b38559852aaefe7e9468f6f53a7465bf36e30
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2018
ms.locfileid: "3398579"
---
# <a name="xbox-analytics-report"></a>Xbox 分析报告

Windows 开发人员中心仪表板中的 **Xbox 分析报告**向你显示了有关客户使用游戏中的 Xbox 功能的情况统计数据。 它还提供服务运行状况信息，以帮助你解决客户端错误。

> [!IMPORTANT]
> 只有当你要发布 Xbox 游戏或要发布一款使用 Xbox Live 服务的游戏时，才能查看此报告。 若要执行此操作，你必须先完成[概念审批过程](../gaming/concept-approval.md)，其中包括通过[Microsoft 合作伙伴](../xbox-live/developer-program-overview.md#microsoft-partners)发布的游戏和游戏通过提交[ID@Xbox计划](../xbox-live/developer-program-overview.md#id)。 通过[Xbox Live 创意者计划](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)发布的游戏不可当前在此报告中可见。

通过展开**分析**并选择 **Xbox 分析**，你可以从游戏的左侧导航菜单中查看 **Xbox 分析**报告。  可以在仪表板中查看此数据，或[下载报告](download-analytic-reports.md)以供脱机查看。


## <a name="overview-tab"></a>“概述”选项卡

**概述**选项卡上的部分显示了有关你的玩家是谁以及他们的 Xbox Live 功能使用情况的信息。

对于其中许多统计数据，我们还会显示 **Xbox 平均值**，以便你可以轻松地对比着普通 Xbox 客户来查看你的客户与 Xbox 交互的情况。

> [!NOTE]
> 这些统计数据来自连接到 Xbox Live 的客户，而不是来自所有 Xbox 客户。


### <a name="concurrent-usage"></a>并发使用

此部分显示关于每分钟或每小时玩游戏的平均客户数量的近似实时使用情况数据（延迟时间为 5-15 分钟）。 你可以通过选择此部分右上角中的筛选器图标来选择时间范围（从**过去一小时**直到**过去 7 天**）。


### <a name="gamerscore-distribution"></a>玩家分数分布情况

此部分显示有关客户玩家分数的信息。 你可以选择 **All games** 以查看客户的总玩家分数的分布情况，或者选择 **This game** 以查看仅通过你的游戏获得的玩家分数分布情况。


### <a name="achievement-unlocks"></a>成就解锁

此部分显示指定时间范围内解锁每项成就的客户总数。 你可以通过选择此部分右上角中的筛选器图标来选择时间范围（**前一天**、**过去 30 天**或**生存期**）。


### <a name="game-statistics"></a>游戏统计信息

此部分包含一些选项卡，你可以选择这些选项卡以显示游戏客户的不同数据。 请注意，此部分中的统计信息指的是总体功能使用情况，而不是你的特定产品内的功能使用情况。

- **Social usage** 选项卡显示与你的客户社交情况相关的数据。
   - **游戏邀请**显示（针对任何游戏）发出邀请的客户的百分比。
   - **群聊天**显示（针对任何游戏）使用群聊天的客户的百分比。
   - **文本消息**显示（针对任何游戏）通过 Xbox shell 发送消息的客户的百分比。
- **Streaming usage** 选项卡显示在 Twitch 和 YouTube 上观看或流式传输（任何游戏的）游戏玩法的游戏客户的百分比。
- **Game DVR usage** 选项卡显示与客户如何录制和查看游戏玩法相关的数据。 你可以查看已（针对任何游戏）观看和上传游戏剪辑和游戏玩法屏幕截图的客户的百分比。


### <a name="friends-and-followers"></a>好友和关注者

此部分显示玩你游戏的客户的 **Median number of friends** 和 **Median number of followers**。


### <a name="accessory-usage"></a>外部设备使用情况

此图表显示使用外部硬盘驱动器的游戏客户和（在 Xbox 上）使用 Xbox Elite 无线控制器的游戏客户的百分比。

此数据并不意味着这些客户玩游戏时在外部硬盘驱动器上安装了你的产品或者使用了 Elite 控制器。 通常，它指的是使用这些功能的产品客户数。


### <a name="connection-type"></a>连接类型

此图表显示（在 Xbox 上）使用**有线** Internet 连接与使用**无线**连接的产品客户的百分比。


## <a name="xbox-live-service-health-tab"></a>“Xbox Live 服务健康”选项卡

借助 **Xbox Live 服务健康选项卡**上的部分，你可以了解任何 Xbox Live 客户端错误（包括速率限制）的影响。 它还允许利用终结点和状态代码进行深入调查，以便获取信息来帮助你解决这些问题，并使你了解有关特定于产品请求的 Xbox Live 服务可用性的信息。

> [!NOTE]
> 在查看此信息和解决问题时，我们建议优先处理速率限制问题，因为这些错误通常对客户造成的影响最大。


### <a name="apply-filters"></a>应用筛选器

在此选项卡顶部附近，可以选择希望显示数据的时间段。 默认选择为 **30D**（30 天），但你可以选择显示 **7D**（7 天）或指定自定义日期范围（不超过 30 天）内的数据。 对于自定义日期范围，请注意，所有图表都会将图表范围删减为你所输入日期范围内提供的数据的第一天和最后一天。

还可以展开**筛选器**，以按照程序包版本、设备类型和/或沙盒筛选该页面上的所有数据。
- **程序包版本**：默认筛选器为**所有版本**，但你可以将服务运行状况数据局限于特定的程序包版本。
- **设备类型**：默认设置为**所有设备**，但你可以将服务运行状况数据局限于特定的设备类型。
- **沙盒**：默认设置为**零售**，但你可以将服务运行状况数据局限于特定的沙盒。

下面列出的所有图表中的信息将反映所选的日期范围和任何筛选器。 某些部分还允许应用其他筛选器。


### <a name="client-errors-by-service"></a>各项服务的客户端错误

**各项服务的客户端错误**图表显示选定时间段内每项 Xbox Live 服务的每日客户端 (4xx) 错误数。

你还可以通过选择**速率限制**来仅查看速率限制错误。 这将显示选定时间段内每项 Xbox Live 服务的每日速率限制 (429) 和速率限制免除 (429E) 错误的数量。

> [!NOTE]
> 429E 状态代码实际上已成功返回为 200 状态代码，但是，如果服务当时遇到大量请求，那么它的速率会受到限制，所以我们建议你将其视为与强制执行的情况完全相同 (429)。

默认情况下，此图表按错误计数显示前六项服务。 你可以选择此部分右上角中的筛选器图标来选择其他服务。 你最多可以同时查看六项服务的错误。

> [!NOTE]
> 图例只显示每项服务的区别性前缀（例如，**presence**，而不是 **presence.xboxlive.com**）。 你可以在 **Xbox Live 服务健康**选项卡下方的**各终结点的客户端错误**表中找到完整服务地址。


### <a name="service-availability"></a>服务可用性

**服务可用性**图表显示选定时间段内每个 Xbox Live 服务的每日可用性。 此项的计算公式为 *1-（总服务器 (5xx) 错误数/总响应数）*，并且它特定于产品，而非特定于整个 Xbox Live。

默认情况下，此图表显示可用性最低的六项服务。 你可以选择此部分右上角中的筛选器图标来选择其他服务。 你最多可以同时查看六项服务的可用性。

> [!NOTE]
> 图例只显示每项服务的区别性前缀（例如，**presence**，而不是 **presence.xboxlive.com**）。 你可以在 **Xbox Live 服务健康**选项卡下方的**各终结点的客户端错误**表中找到完整服务地址。


### <a name="client-errors-by-endpoint"></a>各终结点的客户端错误

**各终结点的客户端错误**表显示选定时间段内按每个 Xbox Live 服务、终结点和状态代码细分的每日客户端 (4xx) 错误数。 默认情况下，该表按服务响应总数降序排列，但你可以通过单击任何列标题来更改排序顺序。

你还可以通过选择**速率限制**来仅查看速率限制错误。 这将显示选定时间段内每个 Xbox Live 服务、终结点和状态代码的每日速率限制 (429) 和速率限制免除 (429E) 错误的数量。

> [!NOTE]
> 429E 状态代码实际上已成功返回为 200 状态代码，但是，如果服务当时遇到大量请求，那么它的速率会受到限制，所以我们建议你将其视为与强制执行的情况完全相同 (429)。










 

 

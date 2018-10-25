---
author: jnHs
Description: The Feedback report in the Windows Dev Center dashboard lets you see the problems, suggestions, and upvotes that your Windows 10 customers have submitted through Feedback Hub.
title: 反馈报告
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.author: wdg-dev-content
ms.date: 11/3/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bceb1d2cc6682698d0ad06ed4b1865f3d6510442
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "5519132"
---
# <a name="feedback-report"></a>反馈报告

使用 Windows 开发人员中心仪表板中的“反馈报告”****，你可以看到 Windows 10 客户通过“反馈中心”提交的问题、建议和投票。 你可以在仪表板中查看此数据，或导出该数据以供脱机查看。

> [!NOTE]
> 还可以直接通过此报告[回复反馈](respond-to-customer-feedback.md)，让客户知道你正在听取他们的反馈。

鼓励客户向你提供有关你的应用的反馈是了解对客户来说非常重要的问题和功能的绝佳方法。 当客户知道他们可以直接向你发送反馈时，则不太可能在应用商店中留下负面评论的反馈。

可以使用 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) 中的反馈 API，以便允许客户[直接从应用启动“反馈中心”](../monetize/launch-feedback-hub-from-your-app.md)。 请记住，已在支持反馈中心的 Windows 10 设备上下载了你的应用的任何客户可以通过使用“反馈中心”应用来留下该应用的反馈。 因此，你可能会看到在此报告中的客户反馈，即使你未明确要求反馈从你的应用内。

使用[软件包外部测试版](package-flights.md)时反馈也有用，因为当客户留下反馈时，反馈报告将展示每个客户已在其设备上安装的特定程序包。

> [!TIP]
> 对于概览评论、 评分和跨所有应用的用户反馈最近 30 天内，在左侧的导航菜单中展开**参与**并选择**评论和反馈。** 


## <a name="apply-filters"></a>应用筛选器

在页面顶部附近，可以选择希望显示数据的时间段。 默认选择为**生命周期**，但可以选择显示 30 天、3 个月、6 个月或 12 个月的数据。

还可以展开**筛选器**以按下列选项筛选此页面上的所有数据。

- **反馈类型**：默认设置是**全部**。 你可以选择“问题”**** 或“建议”****，以便仅显示该类型的反馈。
- **设备类型**：默认设置是**所有设备**。 如果希望此页面仅显示使用特定设备类型的客户留下的反馈，可以选择该设备类型。
- **程序包版本**：默认设置是**所有程序包**。 你可以选择你的其中一个程序包，以便仅显示使用该特定程序包的客户留下的反馈。
- **市场**：默认设置为“所有市场”****。 你可以选择一个特定市场，以便仅显示来自该市场中的客户的反馈。
- **组**：默认设置是“全部”****。 可以选择只查看 [Windows 预览体验成员](http://insider.windows.com)提交的反馈。

> [!TIP]
> 如果在该页面上未看到任何反馈，请检查以确保筛选器未排除所有反馈。 例如，如果按应用不支持的**设备类型**进行筛选，将看不到任何反馈。


## <a name="viewing-feedback-details"></a>查看反馈详细信息

在此报告中，将看到客户留下的单个反馈。 在每项反馈的反馈文本左侧，将看到其他客户通过“反馈中心”对该反馈的投票数。 你可以采用以下三种方法对反馈进行分类：

- **投票**（默认值）：从收到的投票数最多的反馈开始，显示由其他客户投票的反馈。
- **热门**：从已获得最新活动的反馈开始，显示其他客户在最近七天内进行投票的反馈。
- **最近**：从最近留下的反馈开始，显示所有反馈。

在每个评论的旁边，你将看到留下反馈的日期和反馈类型。 你还将看到客户的市场，他们使用当客户留下反馈、 该设备和**Windows 预览体验成员**的类型的 Windows 预览体验成员提交反馈的客户是否在设备已安装的特定程序包计划。

还将在此看到[回复反馈](respond-to-customer-feedback.md)的选项。


## <a name="translating-feedback"></a>翻译反馈

默认情况下，将为你翻译未在你的首选语言编写的反馈。 如果需要，可以在右上方取消选中**翻译反馈**复选框（页面筛选器旁边）来禁用反馈翻译。

请注意，反馈将由自动翻译系统翻译，所以翻译结果可能不一定准确。 如果你想要对照翻译或者通过其他方式进行翻译，会旧提供原始文本。


## <a name="launching-feedback-hub-directly-from-your-app"></a>直接从应用启动“反馈中心”

如上所述，我们建议直接将“反馈中心”的链接包含你的应用中，以便鼓励客户提供反馈。 有关详细信息，请参阅[从应用启动“反馈中心”](../monetize/launch-feedback-hub-from-your-app.md)。

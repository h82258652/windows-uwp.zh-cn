---
author: jnHs
Description: To view performance data for the ad units in your apps, use the advertising performance report on the Windows Dev Center dashboard.
title: 广告效果报告
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
ms.author: wdg-dev-content
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a1a82c91aeafa253427d8e526b38b8ac304591a2
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2018
ms.locfileid: "4615646"
---
# <a name="advertising-performance-report"></a>广告效果报告


**广告效果报告**显示你的[广告单元](in-app-ads.md)（包括社区广告）的运作情况。 此报告包含来自使用[广告中介](in-app-ads.md#mediation)的 UWP 应用中的多个广告提供商的数据。

若要查看此报告，请展开左侧导航菜单中的**分析**，然后选择**广告效果**。 你可以在仪表板中查看此类数据，或单击页面上的箭头图标下载并离线查看报告数据。 或者，你也可以使用我们的[分析 REST API](../monetize/access-analytics-data-using-windows-store-services.md) 中的[获取广告效果数据](../monetize/get-ad-performance-data.md)方法以编程方式检索此数据。

查看广告效果报告时，请注意最近三天的报告数据可能会发生更改，因为我们会收到并处理来自各种来源的新数据。 此外，数据重述可能发生在过去最多 90 天内。

## <a name="apply-filters"></a>应用筛选器

在页面顶部附近，可以选择希望显示数据的时间段。 默认选择为 30D（30 天），但你可以选择要显示 3、6 或 12 个月的数据或指定的自定义数据范围的数据。

还可以展开**筛选器**，以按照广告单元、应用、广告提供商和设备类型筛选该页面上的所有数据。 你可以从下列选项中进行选择：

* **聚合**：选择聚合此报告数据以及进一步进行筛选的方式。 默认情况下，此筛选器设置为**所有广告单元**。 你可以选择将此筛选器更改为**所有应用**或**所有广告提供商**，也可以选择按在其中使用广告的特定应用来聚合数据。
* **广告提供商**：按特定[广告提供商](in-app-ads.md#paid-networks)的效果数据筛选报告。 默认情况下，此报告显示来自所有广告提供商的数据。 如果你选择**聚合**下拉列表中的**所有广告提供商**，则会禁用此选项。
* **设备**：按特定设备类型的效果数据筛选报告。 默认情况下，此报告显示所有设备类型的数据。

## <a name="overall-performance"></a>总体效果

此版块可根据你在报告筛选器中选择的广告单元、应用和广告提供商，以图表或世界地图的方式显示广告效果指标。

若要切换数据视图，请单击**总体效果**版块顶部的**图表**或**地图**。 在地图视图中，较深色调表示较高的值，而较浅色调表示较低的值。 可以悬停在地图上的某个特定国家或地区，以分析所选指标的值。 还可以放大地图的任何区域，以查看较小国家/地区的数据。

你可以单击**图表**或**地图**下拉列表旁边的筛选器图标，以更加细致地查看图表或地图中的数据。 此筛选器支持选择多达六个不同的广告单元、应用或广告提供商，以在图表或地图视图中进行比较。 此筛选器中可供选择的数据类型取决于你在报告顶部的**聚合**（一级筛选器）中的选择。


## <a name="overall-performance-breakdown"></a>总体效果细目

此版块可根据你在报告中的筛选器中指定的数据集，显示包含所有广告效果指标的表格。

## <a name="performance-metrics"></a>效果指标

**广告效果**报告包含以下效果指标数据。 某些指标仅对特定广告提供商可见。

|  指标  |  描述  |
|----------|---------------|
| 预计收入  |  从应用上运行的广告中获得的预计金额。 |
| eCPM  |  每千次印象有效成本。 |
| 请求数  | 你的应用发送某条广告请求的次数。  |
| 曝光数  | 应用中显示广告的次数。  |
| 填充率  | 从显示广告的应用发送广告请求的百分比。  |
| 点击量  |  客户单击应用中的广告的次数。 |
| CTR  |  点击率，是指单击广告的次数与曝光数之比。 |
| 可见性 | 查看你的应用中的广告曝光量的百分比。 有关如何计算此值的更多详细信息，请参阅[优化广告单元的可见性](../monetize/optimize-ad-unit-viewability.md)。 |
| 获取的积分数  | 如果正在开展[社区广告](https://docs.microsoft.com/windows/uwp/publish/about-community-ads)活动，那么这表示你通过在应用中展示社区广告提升广告空间而获得的积分数。  |
| 花费的积分数  | 如果正在开展[社区广告](https://docs.microsoft.com/windows/uwp/publish/about-community-ads)活动，那么这表示你已为自己的应用广告花费的积分数。  |

## <a name="related-topics"></a>相关主题

* [应用内广告](in-app-ads.md)
* [使用 Microsoft 广告 SDK 在你的应用中显示广告](../monetize/display-ads-in-your-app.md)
* [优化广告单元的可见性](../monetize/optimize-ad-unit-viewability.md)


 

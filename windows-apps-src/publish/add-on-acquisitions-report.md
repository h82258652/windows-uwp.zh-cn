---
author: jnHs
Description: The Add-on acquisitions report in the Windows Dev Center dashboard lets you see how many add-ons you've sold, along with demographic and platform details.
title: 加载项购置报告
ms.assetid: F2DF9188-0A98-4AC3-81C0-3E2C37B15582
ms.author: wdg-dev-content
ms.date: 05/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 加载项销售, 加载项购置, iap 销售, 应用内产品, iap, 加载项
ms.localizationpriority: medium
ms.openlocfilehash: 019bb410e6ac65f9951f06052c78f40e9a5f32e2
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2018
ms.locfileid: "4623169"
---
# <a name="add-on-acquisitions-report"></a>加载项购置报告


在 Windows 开发人员中心仪表板中的**加载项购置**报告可以查看多少加载项，你已售出，以及客户统计和平台详细信息，并显示 Windows 10 （包括 Xbox） 上的客户的转换信息。 你还可以查看附近实时购置数据的最后一个小时或有 70 两小时时段。

可以在仪表板中查看此数据，或[下载报告](download-analytic-reports.md)以供脱机查看。 或者，也可以使用 [Microsoft Store 分析 REST API](../monetize/access-analytics-data-using-windows-store-services.md) 中的[获取加载项购置](../monetize/get-in-app-acquisitions.md)方法以编程方式检索此数据。

在此报告中，加载项购置意味着客户已向你购买了某个加载项（或者当你免费提供时免费获得）。 同一客户多次购买的同一易耗型加载项的操作计为单独的加载项购置。

> [!IMPORTANT]
> **加载项购置**报告不包括有关退款、回退、退单等的数据。若要评估应用收款，请访问[付款摘要](payout-summary.md)。 在**预定**部分中，单击**下载预定交易**链接。


## <a name="apply-filters"></a>应用筛选器

在页面顶部附近，可以选择希望显示数据的时间段。 默认选择为 **30D**（30 天），但你可以选择要显示 3、6 或 12 个月的数据或指定的自定义数据范围的数据。 你还可以选择**1 H**或**72h**显示近乎实时的一个小时或有 70 两小时; 购置数据这些时间段仅应用于**加载项购置**图的**加载项每日**选项卡和**市场**图的**购置**选项卡。 

你还可以展开**筛选器**，以按照特定加载项、市场和/或设备类型筛选该页面上的所有数据。

-   **加载项**：默认筛选器是**所有加载项**，但你可以将数据限制为应用的一个或多个加载项。
-   **市场**：默认筛选器是**所有市场**，但可以将数据限制为一个或多个市场中的购置情况。
-   **设备类型**：默认设置是**所有设备**。 如果你希望仅显示来自某个特定设备类型（如电脑、控制台或平板电脑）的购置数据，可以在此处选择一台特定设备。

下面列出的所有图表中的信息将反映所选的日期范围和任何筛选器。 某些部分还允许应用其他筛选器。


## <a name="add-on-acquisitions"></a>加载项购置数据

**加载项购置**图显示了选定时段内加载项的日购置数或周购置数。 （当你使用**应用筛选器**显示更长时间段的数据时，购置数据将按周进行分组。）

你还可以通过选择**加载项累计**看到应用的终生购置数。 这将显示从首次发布应用起所有购置的累计总数。

你可以选择按照加载项购置是否源于客户端或基于 Web 的应用商店和/或按照操作系统版本来筛选结果。


## <a name="customer-demographic"></a>客户统计

**客户统计**图表显示有关已购置加载项的用户的统计信息。 你可以看到按照特定年龄段内的人以及不同性别（在选定时段内）所进行的购置量。

> [!NOTE]
> 一些客户已选择不共享此信息。 如果我们无法确定年龄段或性别，购置将归类为“未知”****。


## <a name="markets"></a>市场

**市场**图表显示了提供你的应用的每个市场在选定时段内的加载项购置的总数。 

可以在可视**地图**窗体中查看此数据，或切换设置以在**表格**窗体中查看。 “表格”窗体一次显示五个市场，并且按字母顺序或最大/最小购置或安装次数排序。 还可以下载数据，以便一起查看所有市场的信息。


## <a name="add-on-page-views-and-conversions-by-campaign-id"></a>按市场活动 ID 的加载项页面查看次数和转换数量

**按市场活动 ID 的加载项页面查看次数和转换数量**图表向你显示在选定时间段内每个市场活动 ID 所对应的加载项总转换次数（购置次数），从而帮助你跟踪每个[自定义推广市场活动](create-a-custom-app-promotion-campaign.md)中 Windows 10（包括 Xbox）上的客户的转换数量和页面查看次数。 仅加载项转换显示在此图表中。

> [!NOTE]
> 客户可以通过单击不是你创建的自定义市场活动找到你的应用一览。 我们会为会话中的每次页面查看都加盖客户首次进入应用商店时的市场活动 ID 戳记。 然后我们在 24 小时内将转换归因于所有购置的市场活动 ID。 因此，你看到的总转换数可能比你的市场活动 ID 的总转换数多，并且你可能会有页面查看次数为零的转换或加载项转换。 


## <a name="conversions-breakdown-by-campaign-id"></a>按市场活动 ID 的转换细分

你可以使用**按市场活动 ID 的转换细分**图表跟踪来自 Windows 10 上的客户对每个[自定义推广市场活动](create-a-custom-app-promotion-campaign.md)的转换数量和页面查看次数。 应用和加载项转换都按照市场活动 ID 进行显示。

在此图表中，*页面查看*是指某位客户查看了应用的应用商店一览。 *转换*是指客户最近获取了应用或加载项的许可证（无论你是否收费）。

请记住，这些页面查看次数和转换数量不是独特客户的计数。 


## <a name="top-add-ons"></a>加载项排行榜

**加载项排行榜**图表显示选定时段内每个加载项的购置总数，所以你可以看到哪些加载项最受欢迎。 



 

 

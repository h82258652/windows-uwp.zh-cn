---
Description: Get detailed analytics for your Windows apps, in Partner Center or via other methods.
title: 分析应用性能
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, analytics, reports, dashboard, apps, data, metrics
ms.localizationpriority: medium
ms.openlocfilehash: 3f75f861aa4f6f828ff7bb1cf829f5205a8ddaed
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259033"
---
# <a name="analyze-app-performance"></a>分析应用性能

You can view detailed analytics for your apps in [Partner Center](https://partner.microsoft.com/dashboard). 可以通过统计信息和图表了解你的应用的表现（从你已拥有的客户数量到客户使用你的应用的方式）以及他们对你的应用的评价。 你还可以找到应用运行状况、广告使用情况等有关指标。

You can view analytic reports right in Partner Center or [download the reports you need](download-analytic-reports.md) to analyze your data offline. We also provide several ways for you to [access your analytics data outside of Partner Center](#outside).

## <a name="view-key-analytics-for-all-your-apps"></a>查看所有应用的关键分析

若要查看下载量最多的应用的关键分析，请展开**分析**并选择**概述**。 默认情况下，概述页显示有关五个在生命周期内购置量最多的应用的信息。 若要选择用于显示的不同的已发布应用，请选择**筛选器**。

## <a name="view-individual-reports-for-each-app"></a>查看每个应用的单独报告

在本部分中，你将看到以下每个报告中所显示信息的详情：

-   [购置报告](acquisitions-report.md)
-   [加载项购置报告](add-on-acquisitions-report.md)
-   [使用情况报告](usage-report.md)
-   [Health report](health-report.md)
-   [Ratings report](ratings-report.md)
-   [评论报告](reviews-report.md)
-   [反馈报告](feedback-report.md)
-   [Xbox analytics report](xbox-analytics-report.md)
-   [Insights report](insights-report.md)
-   [广告效果报告](advertising-performance-report.md)
-   [广告市场活动报告](promote-your-app-report.md)


> [!NOTE]
> 你可能不会在所有这些报告中看到数据，具体取决于应用的特定功能和实现。

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>Access analytics data outside of Partner Center

In addition to viewing reports in Partner Center, you can access app analytics in other ways.

### <a name="microsoft-store-analytics-api"></a>Microsoft Store 分析 API

使用 [Microsoft Store 分析 API](../monetize/access-analytics-data-using-windows-store-services.md)，针对你的应用以编程方式检索分析数据。 此 REST API 使你可以针对应用和加载项购置、错误、应用评分和评价检索数据。 此 API 使用 Azure Active Directory (Azure AD) 验证来自应用或服务的调用。

### <a name="windows-dev-center-content-pack-for-power-bi"></a>适用于 Power BI 的 Windows 开发人员中心内容包

Use the [Windows Dev Center content pack for Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) to explore and monitor your Partner Center analytics data in Power BI. Power BI 是一种基于云的业务分析服务，可为你提供单个视图的业务数据。

使用以下资源开始使用 Power BI 来访问分析数据。

* [Sign up for Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Learn how to use Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Learn how to use the Windows Dev Center content pack for Power BI to connect to your analytics data](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> To connect to the Windows Dev Center content pack for Power BI, we recommend that you specify credentials from an Azure AD directory that is associated with your Partner Center account. 如果你使用 Microsoft 帐户凭据，Power BI 中的分析数据不会自动刷新，而是需要登录到 Power BI 才能刷新数据。 如果你的组织已经使用 Office 365 或 Microsoft 的其他业务服务，则你已经具有 Azure AD。 否则，你可以[免费获取它](https://account.azure.com/organization)。 For more information about setting up the association, see [Associate Azure Active Directory with your Partner Center account](associate-azure-ad-with-dev-center.md).

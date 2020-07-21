---
Description: 在合作伙伴中心或通过其他方法获取 Windows 应用的详细分析。
title: 分析应用性能
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10，uwp，分析，报表，仪表板，应用，数据，指标
ms.localizationpriority: medium
ms.openlocfilehash: 3f75f861aa4f6f828ff7bb1cf829f5205a8ddaed
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259033"
---
# <a name="analyze-app-performance"></a>分析应用性能

可以在[合作伙伴中心](https://partner.microsoft.com/dashboard)查看应用的详细分析。 可以通过统计信息和图表了解你的应用的表现（从你已拥有的客户数量到客户使用你的应用的方式）以及他们对你的应用的评价。 你还可以找到应用运行状况、广告使用情况等有关指标。

你可以直接在 "合作伙伴中心" 中查看分析报告，或[下载你需要](download-analytic-reports.md)脱机分析数据的报表。 我们还提供了几种方法让你在[合作伙伴中心外访问分析数据](#outside)。

## <a name="view-key-analytics-for-all-your-apps"></a>查看所有应用的关键分析

若要查看下载量最多的应用的关键分析，请展开**分析**并选择**概述**。 默认情况下，概述页显示有关五个在生命周期内购置量最多的应用的信息。 若要选择用于显示的不同的已发布应用，请选择**筛选器**。

## <a name="view-individual-reports-for-each-app"></a>查看每个应用的单独报告

在本部分中，你将看到以下每个报告中所显示信息的详情：

-   [购置报告](acquisitions-report.md)
-   [加载项购置报告](add-on-acquisitions-report.md)
-   [使用情况报告](usage-report.md)
-   [运行状况报告](health-report.md)
-   [分级报表](ratings-report.md)
-   [评论报告](reviews-report.md)
-   [反馈报告](feedback-report.md)
-   [Xbox analytics 报表](xbox-analytics-report.md)
-   [见解报告](insights-report.md)
-   [广告效果报告](advertising-performance-report.md)
-   [广告市场活动报告](promote-your-app-report.md)


> [!NOTE]
> 你可能不会在所有这些报告中看到数据，具体取决于应用的特定功能和实现。

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>合作伙伴中心以外的访问分析数据

除了在合作伙伴中心内查看报表之外，还可以通过其他方式访问应用分析。

### <a name="microsoft-store-analytics-api"></a>Microsoft Store 分析 API

使用 [Microsoft Store 分析 API](../monetize/access-analytics-data-using-windows-store-services.md)，针对你的应用以编程方式检索分析数据。 此 REST API 使你可以针对应用和加载项购置、错误、应用评分和评价检索数据。 此 API 使用 Azure Active Directory (Azure AD) 验证来自应用或服务的调用。

### <a name="windows-dev-center-content-pack-for-power-bi"></a>适用于 Power BI 的 Windows 开发人员中心内容包

使用[用于 Power BI 的 Windows 开发人员中心内容包](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)在 Power BI 中浏览和监视合作伙伴中心分析数据。 Power BI 是一种基于云的业务分析服务，可为你提供单个视图的业务数据。

使用以下资源开始使用 Power BI 来访问分析数据。

* [注册 Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [了解如何使用 Power BI](https://powerbi.microsoft.com/guided-learning/)
* [了解如何使用适用于 Power BI 的 Windows 开发人员中心内容包连接到分析数据](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> 若要连接到用于 Power BI 的 Windows 开发人员中心内容包，建议你从与合作伙伴中心帐户关联的 Azure AD 目录中指定凭据。 如果你使用 Microsoft 帐户凭据，Power BI 中的分析数据不会自动刷新，而是需要登录到 Power BI 才能刷新数据。 如果你的组织已经使用 Office 365 或 Microsoft 的其他业务服务，则你已经具有 Azure AD。 否则，你可以[免费获取它](https://account.azure.com/organization)。 有关设置关联的详细信息，请参阅[将 Azure Active Directory 与合作伙伴中心帐户关联](associate-azure-ad-with-dev-center.md)。

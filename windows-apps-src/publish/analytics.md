---
Description: 在合作伙伴中心中或通过其他方法为 Windows 应用，获取详细的分析。
title: 分析应用性能
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10、 uwp、 分析、 报告、 仪表板、 应用、 数据、 指标
ms.localizationpriority: medium
ms.openlocfilehash: 6f76b1f897c345fb71beec8e37e592165922b2ed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625442"
---
# <a name="analyze-app-performance"></a>分析应用性能

你可以查看详细的分析您的应用程序[合作伙伴中心](https://partner.microsoft.com/dashboard)。 可以通过统计信息和图表了解你的应用的表现（从你已拥有的客户数量到客户使用你的应用的方式）以及他们对你的应用的评价。 你还可以找到应用运行状况、广告使用情况等有关指标。

您可以查看分析报告直接在合作伙伴中心中的或[下载所需的报表](download-analytic-reports.md)分析脱机数据。 我们还为您提供几种方法[访问合作伙伴中心之外的分析数据](#outside)。

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
-   [Xbox 的分析报告](xbox-analytics-report.md)
-   [Insights 报表](insights-report.md)
-   [广告效果报告](advertising-performance-report.md)
-   [广告市场活动报告](promote-your-app-report.md)


> [!NOTE]
> 你可能不会在所有这些报告中看到数据，具体取决于应用的特定功能和实现。

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>合作伙伴中心外部访问分析数据

除了在合作伙伴中心中查看报表，您可以访问以其他方式的应用程序分析。

### <a name="microsoft-store-analytics-api"></a>Microsoft Store 分析 API

使用 [Microsoft Store 分析 API](../monetize/access-analytics-data-using-windows-store-services.md)，针对你的应用以编程方式检索分析数据。 此 REST API 使你可以针对应用和加载项购置、错误、应用评分和评价检索数据。 此 API 使用 Azure Active Directory (Azure AD) 验证来自应用或服务的调用。

### <a name="windows-dev-center-content-pack-for-power-bi"></a>适用于 Power BI 的 Windows 开发人员中心内容包

使用[Power BI 的 Windows 开发人员中心内容包](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)来浏览和监视 Power BI 中的合作伙伴中心分析数据。 Power BI 是一种基于云的业务分析服务，可为你提供单个视图的业务数据。

使用以下资源开始使用 Power BI 来访问分析数据。

* [注册 Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [了解如何使用 Power BI](https://powerbi.microsoft.com/guided-learning/)
* [了解如何使用 Power BI 的 Windows 开发人员中心内容包连接到分析数据](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> 若要连接到 Power BI 的 Windows 开发人员中心内容包，我们建议您指定与你的合作伙伴中心帐户相关联的 Azure AD 目录中的凭据。 如果你使用 Microsoft 帐户凭据，Power BI 中的分析数据不会自动刷新，而是需要登录到 Power BI 才能刷新数据。 如果你的组织已经使用 Office 365 或 Microsoft 的其他业务服务，则你已经具有 Azure AD。 否则，你可以[免费获取它](https://go.microsoft.com/fwlink/p/?LinkId=703757)。 有关设置关联的详细信息，请参阅[关联 Azure Active Directory 与你的合作伙伴中心帐户](associate-azure-ad-with-dev-center.md)。

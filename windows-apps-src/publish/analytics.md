---
author: jnHs
Description: "你可以在 Windows 开发人员中心仪表板中查看应用的详细分析。"
title: "分析"
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
translationtype: Human Translation
ms.sourcegitcommit: dfaf348956b19746aa5332aeb7ad5cbc4b224e8c
ms.openlocfilehash: 8922a53da8b1bc97bef7faf49d0e412a26127188

---

# 分析

你可以在 Windows 开发人员中心仪表板中查看应用的详细分析。 可以通过统计信息和图表了解你的应用的表现（从你已拥有的客户数量到客户使用你的应用的方式）以及他们对你的应用的评价。 你还可以找到应用运行状况、广告使用情况等信息。 在仪表板中查看报告，或[下载所需的报告](download-analytic-reports.md)以脱机分析数据。 我们还为你提供了几种[无需使用仪表板即可访问分析数据](#no-dashboard)的方法。

> **注意** &nbsp;&nbsp;除仪表板报告之外，你也可以使用 [Windows 应用商店分析 REST API](../monetize/access-analytics-data-using-windows-store-services.md) 以编程方式访问某些分析数据。

## 针对所有应用的分析


你的仪表板概述页面还包含一个用于收集有关所有应用的详细信息的汇总视图。 概述页面上显示的统计信息将有所不同，具体取决于你的应用。

[下载分析报告](download-analytic-reports.md)时，你也可以选择下载有关所有应用的报告。 请注意，你将需要针对你的应用之一访问“分析”****部分中的“下载报告”****页面，但你不会仅限于下载该特定应用的数据。

## 每个应用的可用报告


在本部分中，你将看到以下每个报告中所显示信息的详情：

-   [购置报告](acquisitions-report.md)
-   [运行状况报告](health-report.md)
-   [分级报告](ratings-report.md)
-   [评价报告](reviews-report.md)
-   [反馈报告](feedback-report.md)
-   [使用情况报告](usage-report.md)
-   [IAP 购置报告](iap-acquisitions-report.md)
-   [广告中介报告](ad-mediation-report.md)
-   [广告性能报告](advertising-performance-report.md)
-   [应用安装广告报告](app-install-ads-reports.md)
-   [通道和转换报告](channels-and-conversions-report.md)

> **注意** &nbsp;&nbsp;你可能看不到所有报告中的数据，具体取决于应用的特定功能和实现。

## 页面和部分筛选器

每个报告都包含可用于深入了解数据的筛选器。 你将在页面顶部附近看到“应用筛选器”****。 你可以使用这些筛选器限制或扩展页面上所有图表和信息的范围。

在每个特定图表中，你可能还会看到个别部分筛选器。 这些筛选器将限制为仅针对该特定图表显示数据。

特定筛选器因报告而异。 本部分中的主题将介绍哪些筛选器可用，以及每个报告页面上的其他数据。

<span id="no-dashboard"/>
## 不使用开发人员中心仪表板访问分析数据

除了仪表板上的分析报告，还有其他几种方法可以访问你的分析数据。

### Windows 应用商店分析 API

使用 [Windows 应用商店分析 API](../monetize/access-analytics-data-using-windows-store-services.md)，针对你的应用以编程方式检索分析数据。 此 REST API 使你可以针对应用和 IAP 购置、错误、应用评分和评价检索数据。 此 API 使用 Azure Active Directory (Azure AD) 验证来自应用或服务的调用。

### 适用于 Power BI 的 Windows 开发人员中心内容包

使用[适用于 Power BI 的 Windows 开发人员中心内容包](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)来浏览并监视 Power BI 中的开发人员中心分析数据。 Power BI 是一种基于云的业务分析服务，可为你提供单个视图的业务数据。

使用以下资源开始使用 Power BI 来访问分析数据。

* [注册 Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [了解如何使用 Power BI](https://powerbi.microsoft.com/guided-learning/)
* [了解如何使用适用于 Power BI 的 Windows 开发人员中心内容包连接到分析数据](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> **注意** &nbsp;&nbsp;为了连接到适用于 Power BI 的 Windows 开发人员中心内容包，我们建议你从与开发人员中心帐户相关联的 Azure AD 目录指定凭据。 如果你使用 Microsoft 帐户凭据，Power BI 中的分析数据不会自动刷新，而是需要登录到 Power BI 才能刷新数据。 如果你的组织已经使用 Office 365 或 Microsoft 的其他业务服务，则你已经具有 Azure AD。 否则，你可以[免费获取它](http://go.microsoft.com/fwlink/p/?LinkId=703757)。 有关将开发人员中心帐户与 Azure AD 相关联的详细信息，请参阅[管理帐户用户](manage-account-users.md)。

### 开发人员中心应用

安装[开发人员中心](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws)应用，以快速查看有关你的应用在所有 Windows 10 设备上的运行状况和性能的详细信息。 



<!--HONumber=Jun16_HO4-->



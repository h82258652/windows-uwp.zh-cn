---
author: jnHs
Description: "通过“广告中介”报告，你可以查看有效填充率，以及你所使用的广告网络的相应填充率。"
title: "广告中介报告"
ms.assetid: 18A33928-B9F2-4F76-9A9C-F01FEE42FEA1
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 0c08c1061231b033dc5401c77085e16ea7001bb1
ms.sourcegitcommit: fadde8afee46238443ec1cb71846d36c91db9fb9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="ad-mediation-report"></a>广告中介报告

此报告显示 Windows 8.x 或 Windows Phone 8.x 应用的广告中介数据，这些应用使用[适用于 Windows 和 Windows Phone 8.x 的 Microsoft 广告 SDK](http://aka.ms/store-8-sdk) 的 **AdMediatorControl** 协调来自多个广告网络的横幅广告。 对于这些应用，你可以在此报告中查看有效填充率，以及你所使用的广告网络的相应填充率。 它还显示了你的每个中介配置的采用率，并使你能够看到广告网络和中介报告的错误。 可以在仪表板中查看此数据，或[下载报告](download-analytic-reports.md)以供脱机查看。

> [!NOTE]
> **广告中介**报告仅在你在 Windows 8.x 或 Windows Phone 8.x 应用中使用 **AdMediatorControl** 时才可用。 有关详细信息，请参阅[此文章](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359)。 对于在 **AdControl** 或 **InterstitialAd** 控件中使用[广告中介](monetize-with-ads.md#mediation)的 UWP 应用，请使用[广告性能报告](advertising-performance-report.md)查看广告网络的性能数据。

## <a name="page-filters"></a>页面筛选器

在页面顶部附近，你可以展开“页面筛选器”****，按日期范围和/或市场筛选此页上的所有数据。

-   **日期**：默认筛选器为“最近 30 天”****，但是你可以将此条件扩大到“最近 12 个月”****。
-   **市场**：默认设置为“所有市场”****。 如果你希望此页面仅显示特定市场中客户的分级，你可以选择该市场。
-   **平台**：默认设置为“所有平台”****。 如果你的应用面向多个平台，则可以选择某个特定平台。

下面列出的所有图中的信息都将反映**“页面筛选器”**中所选的时段。 默认情况下，这将包括列出你的应用的所有市场和平台中的数据，除非你已使用“页面筛选器”****指定了某个特定市场和/或平台。

## <a name="ad-mediation-performance"></a>广告中介性能

“广告中介性能”****图显示了选定时段内的平均总填充率。 这是所有用户会话的平均填充率，不考虑你的中介配置或调用不同广告网络的频率。

你可以单击“中介请求”****标题查看个别中介请求的平均数，或单击“已提交广告”****查看已提交广告的平均总数。

## <a name="ad-provider-fill-rates"></a>广告提供商填充率

“广告提供商填充率”****图显示了在选定时段内你的每个广告网络的平均填充率。

每个广告网络的信息都集中显示，以帮助你比较每个广告网络的性能。

## <a name="unique-users-per-mediation-configuration"></a>每个中介配置的独特用户

“每个中介配置的独特用户”****图显示了在选定时段内收到每个版本的中介配置的独特用户总数。

## <a name="errors-by-ad-network"></a>广告网络错误

“广告网络错误”****图显示了每个广告网络的请求和错误总数，以及产生错误的请求的百分比。

## <a name="errors-by-type"></a>类型错误

“类型错误”****图显示了每个广告网络所遇到的特定错误。 它还显示了特定错误表示的该网络的总错误数百分比，以便你了解哪些错误将频繁出现在每个广告网络上。

 

 

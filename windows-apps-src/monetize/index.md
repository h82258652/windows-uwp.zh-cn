---
ms.assetid: 4e8cc0c0-b14c-472c-9e1c-4601d10289d2
description: Windows SDK、Microsoft 广告 SDK、Microsoft Store Services SDK 和 Microsoft Store 提供许多功能，可使你从应用中获取更多收益并通过吸引用户来赢得客户。
title: 盈利、参与度和 Microsoft Store 服务
ms.date: 11/29/2017
ms.topic: article
keywords: windows 10, uwp, 盈利, 参与, 促销, Microsoft Store 服务
ms.localizationpriority: medium
ms.openlocfilehash: 7beee974bceceab02984ae6499a9c5db0b0281b9
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "74259294"
---
# <a name="monetization-engagement-and-store-services"></a>盈利、参与度和 Microsoft Store 服务

Windows SDK、Microsoft 广告 SDK、Microsoft Store Services SDK 和 Microsoft Store 提供许多功能，可使你从应用中获取更多收益并通过吸引用户来赢得客户。 本部分的主题介绍了如何将这些功能生成到你的应用中。

有关 Microsoft Store 收费情况以及如何获取应用销售所得的详细信息，请参阅[获得收入](../publish/getting-paid-apps.md)。

## <a name="choose-a-pricing-model"></a>选择定价模式

:::row:::
    :::column:::
        ![对应用进行收费](images/pricing-charge-price.png)
    :::column-end:::
    :::column span="2":::
**对应用进行收费**

你可以预先对应用进行收费。 我们支持广泛的价格梯度范围，包括选择按市场定价。 你甚至可以计划一次促销，限时降价出售你的应用。

[设置应用定价](../publish/set-app-pricing-and-availability.md)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![免费试用](images/pricing-free-trial.png)
    :::column-end:::
    :::column span="2":::
**免费试用**

你可以提供应用的免费试用版本，让更多客户来试用。 为吸引客户购买完整版，你可以限制试用版中的功能（例如，仅包括游戏的第一关）、显示广告，或指定限时试用。

[提供试用版](in-app-purchases-and-trials.md)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![应用内购买](images/pricing-in-app-purchases.png)
    :::column-end:::
    :::column span="2":::
**应用内购买**

无论你的应用是付费还是免费提供，你都可以使用应用中的“应用内购买”功能产生源源不断的收入流。 使用应用内购买，以允许客户将应用从免费版升级到付费版，或在应用中提供可出售的耐用型或易耗型加载项。

[使用应用内购买](in-app-purchases-and-trials.md)
    :::column-end:::
:::row-end:::

## <a name="monetize-your-app-with-ads"></a>通过广告获取应用收益

:::row:::
    :::column:::
        ![适用于各种语境的广告](images/monetize-ads-every-context.png)
    :::column-end:::
    :::column span="2":::
**适用于各种语境的广告**

我们支持多种广告体验以适应大部分需求，包括横幅广告、间隙广告（横幅和视频）、线性视频广告、可播放广告和本机广告。 我们的平台符合 OpenRTB、VAST 2.x、MRAID 2 和 VPAID 3 标准，并与 MOAT 和 IAS 兼容。

[浏览广告选项](../publish/create-an-ad-campaign-for-your-app.md)
[安装广告 SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![广告中介服务](images/monetize-ad-mediation-service.png)
    :::column-end:::
    :::column span="2":::
**广告中介服务**

通过使用 Microsoft 广告中介服务，在多个热门广告网络中投放广告，最大程度地提高应用中的广告收入。 你可以在合作伙伴中心中配置中介设置，无需更改任何代码。 如果让我们为你配置中介，我们的机器学习算法可帮助你在应用支持的多个市场之间最大程度地提高广告收入。

[使用广告服务](https://blogs.windows.com/windowsdeveloper/2017/05/08/announcing-microsofts-ad-mediation-service/)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![分析](images/monetize-analytics-pie-chart.png)
    :::column-end:::
    :::column span="2":::
**分析**

利用详细分析报告，你可以了解应用内广告的运营情况，并为你提供最大化广告收入所需的信息。 另外，我们还提供 RESTful API，供你采用编程方式获取此数据。

[查看效果](../publish/advertising-performance-report.md)
    :::column-end:::
:::row-end:::

## <a name="other-monetization-opportunities"></a>其他盈利机会

![其他盈利机会](images/monetize-other-opportunities.png)

还在寻求其他增加盈利的方法？ 不妨请考虑以下几个选项。

 主题                | 说明                 |
|--------------------|-----------------------------|
| [Microsoft 联盟计划](https://www.microsoftaffiliates.com/)。 | 通过从应用、博客、网页或其他通讯产品链接到 Microsoft 产品，来赚取佣金。 你可以链接到 Microsoft Store 中出售的应用、游戏、音乐、电影、硬件、配件和其他商品。
| [A/B 试验](https://docs.microsoft.com/windows/uwp/monetize/run-app-experiments-with-a-b-testing) | 在应用中运行 A/B 测试，衡量面向部分客户进行的功能更改的成效，然后再针对所有用户执行更改。
| [使用 Microsoft Store Services SDK 与客户互动](microsoft-store-services-sdk.md) | 你可以使用 Microsoft Store Services SDK 提供的库和工具将这些功能添加到你的应用，这可帮助你吸引客户。 这些功能包括定向通知、A/B 测试以及从应用启动“反馈中心”。
| [从应用启动反馈中心](launch-feedback-hub-from-your-app.md) | 将代码添加到 UWP 应用以将 Windows 10 客户定向至“反馈中心”，他们可以在其中提交问题、提出建议并进行投票。 然后，在合作伙伴中心内的[反馈报告](../publish/feedback-report.md)中管理此反馈。 此功能需要 Microsoft Store Services SDK。 
| [配置应用以接收合作伙伴中心推送通知](configure-your-app-to-receive-dev-center-notifications.md) | 为 UWP 应用注册通知通道，以便它可以接收[合作伙伴中心推送通知](../publish/send-push-notifications-to-your-apps-customers.md)，并跟踪由推送通知导致的应用启动的速度。 此功能需要 Microsoft Store Services SDK。
| [记录合作伙伴中心的自定义事件](log-custom-events-for-dev-center.md) | 记录 UWP 应用中的自定义事件，并在合作伙伴中心内的[使用情况报告](../publish/usage-report.md)中查看事件。 此功能需要 Microsoft Store Services SDK。
| [请求评分和评价](request-ratings-and-reviews.md) | 以编程方式显示评分和评价 UI，从而鼓励客户对应用进行评分或评价。
| [Microsoft Store 服务](using-windows-store-services.md) | 了解如何使用 RESTful API 将提交到应用商店和访问应用的分析数据自动化以及将与应用商店相关的其他任务自动化。
| [向应用中添加零售演示 (RDX) 功能](retail-demo-experience.md) | 在你的 Windows 应用中包括零售演示模式，以便在销售场所试用电脑和设备的客户可以直接进入。

## <a name="monetization-analytics"></a>盈利分析

![盈利分析](images/monetize-analytics.png)

通过使用这些报告，密切关注应用在 Microsoft Store 中的盈利情况。

- [支出汇总](../publish/payout-summary.md)
- [购置报告](../publish/acquisitions-report.md)
- [加载项购置报告](../publish/add-on-acquisitions-report.md)
- [广告效果报告](../publish/advertising-performance-report.md)
- [使用我们的 REST API 获取分析数据](access-analytics-data-using-windows-store-services.md)
- [创建客户细分](../publish/create-customer-segments.md)
- [反馈报告](../publish/feedback-report.md)
- [使用情况报告](../publish/usage-report.md)
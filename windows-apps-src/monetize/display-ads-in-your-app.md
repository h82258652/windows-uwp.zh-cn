---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: "Microsoft 广告 SDK 提供几种通过广告从应用中获取收益的方法。"
title: "使用 Microsoft 广告 SDK 在你的应用中显示广告"
ms.author: mcleans
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 广告, 横幅, 广告控件, 间隙"
ms.openlocfilehash: 4730ebaf55af8e7063c444d5b3bbd973b0508db2
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2017
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>使用 Microsoft 广告 SDK 在你的应用中显示广告

通过使用 Microsoft 广告 SDK 将广告放入你的应用中，增加收入机会。 我们的广告盈利平台可提供多种广告类型，并通过各种受欢迎的广告网络支持中介服务。

若要在应用中显示广告，请按照以下步骤执行。

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>步骤 1：安装 Microsoft 广告 SDK

Microsoft 广告 SDK 提供了多种可用于在应用中显示不同类型广告的控件。 有关安装说明，请参阅[此文章](install-the-microsoft-advertising-libraries.md)。

## <a name="step-2-choose-your-ad-type-and-add-code-to-display-test-ads-in-your-app"></a>第 2 步：选择你的广告类型，然后添加代码，以在应用中显示测试广告

Microsoft 广告 SDK 提供了你可以在应用中显示的多种不同类型的广告。 选择哪些类型的广告最适合你的情形，然后在你的应用中添加代码以显示这些广告。

你必须在你的代码中指定应用 ID 和广告单元 ID。 当你开发应用时，应使用[测试应用程序 ID 和广告单元 ID 值](test-mode-values.md)以查看应用在测试期间如何呈现广告。

### <a name="banner-ads"></a>横幅广告

这些广告是一种使用应用中的部分页面的静态显示广告，通常位于页面顶部或底部。

有关说明和代码示例，请参阅 [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md) 以及 [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)。 有关演示如何使用 C# 和 C++ 向 JavaScript/HTML 应用和 XAML 应用添加横幅广告的完整示例项目，请参阅 [GitHub 上的广告示例](http://aka.ms/githubads)。

![addreferences](images/banner-ad.png)

### <a name="interstitial-ads"></a>间隙广告

这些广告是一种全屏广告，通常要求用户观看视频或通过单击它们来继续运行应用或游戏。 我们支持两类间隙广告：视频和横幅。

有关说明和代码示例，请参阅[间隙广告](interstitial-ads.md)。 有关演示如何使用 C# 和 C++ 向 JavaScript/HTML 应用和 XAML 应用添加间隙广告的完整示例项目，请参阅 [GitHub 上的广告示例](http://aka.ms/githubads)。

![addreferences](images/interstitial-ad.png)

### <a name="native-ads"></a>原生广告

这些广告是基于组件的广告。 这种广告的每一部分创意（如标题、图像、说明和行动文本）都作为一种单独的元素交付到应用中，你可以使用你自己的字体、颜色和其他 UI 组件将这些元素集成到你的应用中，从而组合成一种融入环境的不突兀的用户体验。

有关说明和代码示例，请参阅[原生广告](native-ads.md)。

![addreferences](images/native-ad.png)

## <a name="step-3-get-an-ad-unit-from-dev-center-and-configure-your-app-to-receive-live-ads"></a>第 3 步：从开发人员中心获取广告单元并配置你的应用以接收动态广告

完成你的应用的测试并且准备好将其提交到应用商店后，在 Windows 开发人员中心仪表板的[通过广告盈利](../publish/monetize-with-ads.md)页面上创建一个广告单元。 然后，更新应用代码以对此广告单位使用应用 ID 和广告单元 ID 值。 如果你尝试在应用商店中你的应用的已发布版本中使用测试应用程序 ID 和广告单元 ID 值，你的应用不会收到实时广告。 有关详细信息，请参阅[在应用中设置广告单元](set-up-ad-units-in-your-app.md)。

<span id="ad-mediation"/>
### <a name="configure-ad-mediation"></a>配置广告中介

默认情况下，横幅广告、间隙广告和原生广告都显示来自 Microsoft 网络的付费广告。 若要最大化你的广告收益，你可以为你的广告单元启用广告中介，以显示来自其他付费广告网络（如 Taboola 和 Smaato）的广告。 你还可以通过在 Microsoft 应用促销活动中提供广告服务，来提高你的应用推广能力。

要在 UWP 应用中开始使用广告中介，请为广告单元[配置广告中介设置](../publish/monetize-with-ads.md#mediation)。 默认情况下，我们会使用机器学习算法自动配置中介设置，以帮助最大化你的广告在应用支持的市场中的收益。 但是，你也可以手动选择想要使用的网络。 不论采用哪种方式，均需要在服务中完全配置中介设置；你无需在应用中更改任何代码。    

<span id="8.x-mediation"/>
### <a name="mediation-in-windows-81-and-windows-phone-8x-apps"></a>Windows 8.1 和 Windows Phone 8.x 应用中的中介

在 Windows 8.1 和 Windows Phone 8.x 应用中，广告中介仅适用于横幅广告。 若要使用广告中介，你必须使用[适用于 Windows 和 Windows Phone 8.x 的 Microsoft 广告 SDK](http://aka.ms/store-8-sdk) 中的 **AdMediatorControl** 类，而不是 **AdControl** 类。 将此控件添加到应用之后，你可以在仪表板中手动配置你的广告中介设置。

有关在 Windows 8.1 或 Windows Phone 8.x 应用中使用广告中介的更多信息，请参阅[本文](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx)。

> [!NOTE]
> 适用于 Windows 8.1 和 Windows Phone 8.x 应用的广告中介不再处于开发状态。 若要最大化你的潜在广告收益，我们建议你构建通过横幅广告、间隙广告或原生广告使用广告中介的 UWP 应用。

## <a name="step-4-submit-your-app-and-review-performance"></a>步骤 4：提交你的应用并查看性能

在完成带广告的应用开发后，你可以[将更新的应用提交](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)到开发人员中心仪表板，以便在应用商店中上架应用。 显示广告的应用必须满足 [Windows 应用商店政策的 10.10 部分](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10)和[应用开发人员协议的附录 E](https://msdn.microsoft.com/library/windows/apps/hh694058.aspx) 中指定的其他要求。

在你的应用发布并通过应用商店中提供之后，可在仪表板中查看你的[广告业绩报告](../publish/advertising-performance-report.md)，并继续更改中介设置以优化广告效果。 广告收入包含在你的[支出汇总](../publish/payout-summary.md)中。

<span id="additional-help" />
## <a name="additional-help"></a>其他帮助

要获取有关使用 Microsoft 广告 SDK 的其他帮助，请使用以下资源。

|  任务    | 资源 |               
|----------|-------|
| 报告错误或获取对广告的辅助支持     | 访问[支持页面](https://go.microsoft.com/fwlink/p/?LinkId=331508)，然后选择**应用内广告**。        |
| 获取社区支持     | 访问[论坛](http://go.microsoft.com/fwlink/p/?LinkId=401266)。       |
| 下载演示如何向应用添加横幅和间隙广告的示例项目。     | 请参阅 [GitHub 上的广告示例](http://aka.ms/githubads)。       |
| 了解有关 Windows 应用的最新盈利机会     | 访问[获取应用收益](https://developer.microsoft.com/store/monetize)。        |

## <a name="related-topics"></a>相关主题

* [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp)
* [利用广告让应用盈利](http://go.microsoft.com/fwlink/p/?LinkId=699559)
* [广告性能报告](../publish/advertising-performance-report.md)

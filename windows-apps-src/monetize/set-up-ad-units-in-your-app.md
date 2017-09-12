---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: "了解如何在将应用提交到应用商店之前，从 Windows 开发人员中心仪表板向应用添加应用程序 ID 和广告单元 ID 值。"
title: "在应用中设置广告单元"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 广告, 投放广告, 广告单元"
ms.openlocfilehash: f96e81079764682a9f603fe93a9c123a69690507
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2017
---
# <a name="set-up-ad-units-in-your-app"></a>在应用中设置广告单元

应用中的每个横幅广告、间隙广告或本机广告控件都有一个相应的*广告单元*，我们的服务使用它们向控件提供广告。 每个广告单元都包含*广告单元 ID* 和*应用程序 ID*。 开发应用时，请向你的控件分配[测试应用程序 ID 和广告单元 ID 值](test-mode-values.md)，确认你的应用能够显示测试广告。 这些测试值只能在应用的测试版本中使用。

在你完成应用测试并准备将其提交到 Windows 开发人员中心时，必须更新你的应用代码才能使用 Windows 开发人员中心仪表板中[通过广告盈利](../publish/monetize-with-ads.md)页面的应用程序 ID 和广告单元 ID 值。 如果你尝试在动态应用中使用测试值，你的应用不会收到实时广告。

若要为你的动态应用设置广告单元：

1.  在 Windows 开发人员中心仪表板上，选择你的应用，然后依次单击**盈利 > 通过广告盈利**。

2.  在**创建单元**部分的**广告单元名称**字段输入广告单元的名称。

3. 在**广告单元类型**下拉列表中，选择与你将在控件中展示的广告相对应的广告单元类型：

    -   如果在应用中使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 显示横幅广告，请选择**横幅**。

    -   如果在应用中使用 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 显示间隙视频或间隙横幅广告，请选择**视频间隙**或**横幅间隙**（确保为你想要显示的间隙广告类型选择正确的选项）。

    -   如果在应用中使用 [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) 显示本机广告，请选择 **Native**。
      > [!NOTE]
      > 目前只有参与试用计划的部分开发人员能够创建 **Native** 广告单元，但我们打算尽快向所有开发人员提供此功能。 如果你有兴趣参加我们的试用计划，请通过 aiacare@microsoft.com 联系我们。

4.  在**设备系列**下拉菜单中，选择将在其中使用广告单元的应用所针对的设备系列。 可用选项包括：**UWP (Windows 10)**、**电脑/平板电脑 (Windows 8.1)** 或**移动设备 (Windows Phone 8.x)**。

5.  单击**创建广告单元**。 新的广告单元将显示在此页面的**可用广告单元**部分中的列表顶部。

6.  对于生成的每个广告单元，你将会看到一个**应用程序 ID** 和**广告单元 ID**。 若要在应用中显示广告，需要在你的应用代码中使用这些值：

    -   如果你的应用显示横幅广告，请将这些值分配给 **AdControl** 对象的 [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) 和 [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) 属性。 有关详细信息，请参阅 [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md) 以及 [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)。

    -   如果你的应用显示间隙广告，请将这些值传递给 **InterstitialAd** 对象的 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法。 有关详细信息，请参阅[间隙广告](interstitial-ads.md)。

    -   如果你的应用显示本机广告，请将这些值传递给 [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx) 构造函数的 *applicationId* 和 *adUnitId* 参数。 有关详细信息，请参阅[本机广告](../monetize/native-ads.md)。

7. 如果你的应用为面向 Windows 10 的 UWP 应用，则你可以选择性地通过配置**广告中介**部分的设置来为 [AdControl](../publish/monetize-with-ads.md#mediation) 启用广告中介。 广告中介显示来自多个广告网络（包括其他付费广告网络，如 Taboola 和 Smaato）的广告及 Microsoft 应用促销活动的广告，从而使你能够最大化你的广告收益和应用促销能力。 默认情况下，我们会使用机器学习算法为你的应用自动选择广告中介，以帮助最大化你的广告在应用支持的市场中的收益，但是，你也可以选择手动配置中介设置。

<span id="manage" />
## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>管理你的应用中多个广告控件的广告单元

你可以在单个应用中使用多个横幅广告、间隙广告和本机广告控件。 在此情况下，我们建议你为每个控件分配不同的广告单元。 对每个控件使用不同的广告单元使你可以分别[配置中介设置](../publish/monetize-with-ads.md#mediation)并获取每个控件的独立[报告数据](../publish/advertising-performance-report.md)。 这还使我们的服务能够更好地优化我们为你的应用提供的广告。

> [!IMPORTANT]
> 每个广告单元都只能在一个应用中使用。 如果在多个应用中使用某个广告单元，将不为该广告单元提供广告。

## <a name="related-topics"></a>相关主题

* [测试模式值](test-mode-values.md)
* [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)
* [间隙广告](interstitial-ads.md)
* [本机广告](../monetize/native-ads.md)


 

 

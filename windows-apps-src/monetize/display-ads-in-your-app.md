---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: Microsoft 官方商城协定和盈利 SDK 向你提供通过广告获取应用收益的多种方法。
title: 在应用中显示广告
---

# 在应用中显示广告


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

[Microsoft 官方商城协定和盈利 SDK](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md) 向你提供通过广告获取应用收益的多种方法。

## 显示使用 Microsoft Advertising 库的横幅和视频间隙广告

通过包含横幅和视频间隙广告来从 Windows 应用中获取更多收益。 在适用于 PC、平板电脑和手机的 Windows 应用中显示广告。 你可以通过使用 Windows 开发人员中心仪表板来实时监视广告性能。

若要在应用中包含广告，请使用在 Microsoft 官方商城协定和盈利 SDK 分配的广告库中的 **AdControl** 和 **InterstitialAd** 控件。 你可以使用这些控件在适用于 Windows 10、Windows 8.1、Windows Phone 8.1 和 Windows Phone 8 的 XAML或 JavaScript/HTML 应用中显示来自 Microsoft 的横幅和视频间隙广告。

有关详细信息，请参阅[使用 Microsoft Advertising 库显示广告](display-ads-using-the-microsoft-advertising-libraries.md)。 发布某个带广告的应用后，请使用[广告性能报告](../publish/advertising-performance-report.md)来跟踪广告的性能。                                           

## 为来自多个广告网络的横幅广告使用广告中介

你可以在 XAML 应用中使用 **AdMediatorControl** 类以通过显示来自多个广告网络的横幅广告优化广告收益。 将此控件添加到应用后，在 Windows 开发人员中心仪表板上配置广告中介设置，我们将负责来自你选择的广告网络的横幅广告请求的中介工作。 有关详细信息，请参阅[使用广告中介使广告收益最大化](use-ad-mediation-to-maximize-revenue.md)。

## Microsoft Advertising 库和广告中介之间的区别

Microsoft 官方商城协定和盈利 SDK 中包括 Microsoft Advertising 库和广告中介。 但是，这些库提供不同的类，并且用于不同的用途。

* 如果要在 XAML 或 JavaScript 应用中显示横幅或视频间隙广告，请使用 Microsoft Advertising 库中的 **AdControl** 和 **InterstitialAd** 类。
* 如果要在 XAML 应用中显示来自多个广告网络的横幅广告，请使用广告中介库中的 **AdMediatorControl** 类。

有关详细信息，请参阅[区别是什么：AdMediatorControl 或 AdControl](what-is-the-difference-admediatorcontrol-or-adcontrol.md)。

## 相关主题

* [Microsoft 官方商城协定和盈利 SDK](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md)
* [通过广告获取应用收益]( http://go.microsoft.com/fwlink/p/?LinkId=699559)


<!--HONumber=May16_HO2-->



---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: "Microsoft Store Services SDK 提供几种通过广告从应用中获取收益的方法。"
title: "在应用中显示广告"
translationtype: Human Translation
ms.sourcegitcommit: 2f0835638f330de0ac2d17dae28347686cc7ed97
ms.openlocfilehash: 35dfe2864958a15cf01133d6017b7dd03f382e4a

---

# 在应用中显示广告


通用 Windows 平台 (UWP) 和 Windows 应用商店提供几种通过广告从应用中获取收益的方法。

## 显示使用 Microsoft Advertising 库的横幅和视频间隙广告

将横幅和视频间隙广告包括在应用中，从 UWP 应用以及 Windows 8.1 和 Windows Phone 8.x 应用中获取收益。 在适用于 PC、平板电脑和手机的 Windows 应用中显示广告。 你可以通过使用 Windows 开发人员中心仪表板中的[广告性能报告](../publish/advertising-performance-report.md)实时监视广告性能。

若要在应用中包含这些类型的广告，请使用分配在 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)（适用于 UWP 应用）和[适用于 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)（适用于 Windows 8.1 和 Windows Phone 8.x 应用）广告库中的 **AdControl** 和 **InterstitialAd** 控件。


以下主题提供有关涉及 Windows 广告库的常见任务的信息。

|  任务    | 主题 |               
|----------|-------|
| 安装并开始使用 Microsoft Advertising 库。     | 请参阅[开始使用 Microsoft Advertising 库](get-started-with-microsoft-advertising-libraries.md)。        |
| 在 XAML/C# 应用中显示横幅广告。     | 请参阅 [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)。        |
| 在 HTML/JavaScript 应用中显示横幅广告。     | 请参阅 [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)。        |
| 在 Windows Phone Silverlight 8.x 应用中显示横幅广告。     | 请参阅 [Windows Phone Silverlight 中的 AdControl](adcontrol-in-windows-phone-silverlight.md)。        |
| 在应用中显示视频间隙广告。     | 请参阅[间隙广告](interstitial-ads.md)。       |
| 在使用 JavaScript 与 HTML 编写的通用 Windows 平台 (UWP) 应用中，向视频内容添加广告。   |  请参阅[使用 HTML 5 和 JavaScript 向视频内容添加广告](add-advertisements-to-video-content.md)。  |
| 下载演示如何向应用添加横幅和间隙广告的示例项目。     |请参阅 [GitHub 上的广告示例](http://aka.ms/githubads)。       |
| 处理应用中的 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 错误。     | 请参阅[错误处理](error-handling-with-advertising-libraries.md)和 [AdControl 错误处理](adcontrol-error-handling.md)下的演练。       |
| 报告 Microsoft Advertising 库中的 bug。     | 访问[支持页面](https://go.microsoft.com/fwlink/p/?LinkId=331508)。        |
| 获取社区支持。     | 访问[论坛](http://go.microsoft.com/fwlink/p/?LinkId=401266)。       |

                            

## 使用横幅广告的广告中介（Windows 8.1 和 Windows Phone 8.x）

对于 Windows 8.1 和 Windows Phone 8.x 应用，可以使用 **AdMediatorControl** 类以通过显示多个广告网络的横幅广告以优化广告收益。 将此控件添加到应用后，在 Windows 开发人员中心仪表板上配置广告中介设置，我们将负责调解所选广告网络提出的横幅广告请求。 有关详细信息，请参阅[使用广告中介使广告收益最大化](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx)。

>**注意**&nbsp;&nbsp;Windows 10 的 UWP 应用当前不支持使用 **AdMediatorControl** 类的广告中介。 即将使用与用于横幅广告 (**AdControl**) 和视频间隙广告 (**InterstitialAd**) 相同的 API 推出用于 UWP 应用的服务器端中介。 有关在 UWP 应用中从 **AdMediatorControl** 迁移到 **AdControl** 的指南，请参阅[针对 UWP 应用从 AdMediatorControl 迁移到 AdControl](migrate-from-admediatorcontrol-to-adcontrol.md)。

<span id="silverlight_support"/>
## 用于 Windows Phone 8.x Silverlight 项目的广告支持

某些开发人员方案已不再受 Windows Phone 8.x Silverlight 项目支持。 有关详细信息，请参阅下表。

|  平台版本  |  现有项目    |   新建项目  |
|-----------------|----------------|--------------|
| Windows Phone 8.0 Silverlight     |  如果现有的 Windows Phone 8.0 Silverlight 项目已在使用之前版本的通用广告客户端 SDK 或 Microsoft Advertising 的 **AdControl** 或 **AdMediatorControl**，并且此应用已在 Windows 应用商店中发布，则可以修改和重建该项目，并且可以在设备上调试或测试更改。 不支持在仿真器中调试或测试项目。  |  不支持。  |
| Windows Phone 8.1 Silverlight    |  如果现有的 Windows Phone 8.1 Silverlight 项目使用之前 SDK 的 **AdControl** 或 **AdMediatorControl**，则可以修改和重建该项目。 但是，若要调试或测试应用，必须在仿真器中运行该应用，并且将[测试模式值](test-mode-values.md)用于应用程序 ID 和广告单元 ID。 在不受支持的设备上调试或测试应用。  |   可以将 **AdControl** 或 **AdMediatorControl** 添加到新的 Windows Phone 8.1 Silverlight 项目。 但是，若要调试或测试应用，必须在仿真器中运行该应用，并且将[测试模式值](test-mode-values.md)用于应用程序 ID 和广告单元 ID。 在不受支持的设备上调试或测试应用。 |

## 相关主题

* [Microsoft Store Services SDK](microsoft-store-services-sdk.md)
* [通过广告获取应用收益](http://go.microsoft.com/fwlink/p/?LinkId=699559)
* [广告性能报告](../publish/advertising-performance-report.md)



<!--HONumber=Sep16_HO2-->



---
author: mcleanbyron
ms.assetid: fcebd659-438b-4d03-bc73-6b662ed6f1f3
description: "了解有关开发和发布带广告的应用的端到端过程。"
title: "用于创建带广告的应用的工作流"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 0f832674f0e635f609f09fa15acead109b1d67ff


---

# 用于创建带广告的应用的工作流




若要在应用中显示广告，你的应用需要能够接收来自广告网络的广告。 Microsoft 提供的 Web 服务允许 Windows 应用开发人员接收广告。 当用户单击应用中的广告时，你（作为广告的*发布者*）就会从广告的创建者（*广告商*）获取收益。 从广告商赚取的收益会通过你的帐户支付给你。

以下高级步骤介绍开发和发布带广告的应用的一般过程。

1.  开发阶段：

    * 设置 Windows 开发人员中心帐户。
    * 使用测试模式值开发应用。

2.  准备就绪，可以发布：

    * 配置应用以接收实时广告。
    * 将应用提交到 Windows 开发人员中心，并查看性能数据。

有关每个步骤的详细信息，请阅读其以下相应部分。

## 设置 Windows 开发人员中心帐户

你需要有 Windows 开发人员中心帐户才能发布应用和接收广告。 无论你是否使用广告中介，都是如此。 与广告相关的应用管理也在 Windows 开发人员中心中完成。 如果你已使用 Microsoft pubCenter 来管理应用中的广告，这会替换为 Windows 开发人员中心中的功能

若要设置 Windows 开发人员中心帐户，请访问[主页](https://dev.windows.com/windows-apps)。 Windows 开发人员中心[帮助页面](https://dev.windows.com/develop)提供了其他信息。

## 使用测试模式值开发应用

使用以下演练中的说明添加 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 或 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 以在应用中显示广告：

-   [间隙广告](interstitial-ads.md)
-   [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)
-   [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)
-   [Windows Phone Silverlight 中的 AdControl](adcontrol-in-windows-phone-silverlight.md)

当你使用 **AdControl** 或 **InterstitialAd** 在应用中显示广告时，必须在代码中指定应用程序 ID 和广告单元 ID，以便将你的应用链接到 Windows 开发人员中心帐户，并投放广告。 当你开发应用时，请使用测试应用程序 ID 和广告单元 ID 值以查看应用在测试期间如何呈现广告。 这样，你便可以在测试期间查看应用如何接收和呈现广告。 有关详细信息，请参阅[测试模式值](test-mode-values.md)。

有关演示如何使用 C# 和 C++ 向 JavaScript/HTML 应用和 XAML 应用添加横幅和间隙视频广告的完整示例项目，请参阅 [GitHub 上的广告示例](http://aka.ms/githubads)。

## 配置应用以接收实时广告

在你完成应用测试并准备将其提交到 Windows 开发人员中心时，必须更新你的应用代码才能使用 [Windows 开发人员中心仪表板](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx)中的应用程序 ID 和广告单元 ID 值。 如果你尝试在动态应用中使用测试值，你的应用不会收到实时广告。 有关详细信息，请参阅[在应用中设置广告单元](set-up-ad-units-in-your-app.md)。

## 提交应用

在应用开发完成后，你可以通过使用 Windows 开发人员中心仪表板在 Windows 应用商店中发布应用。 除了要符合 Windows 应用商店中所有应用的要求外，显示广告的应用还必须满足几项附加要求。 有关详细信息，请参阅[将带广告的应用提交到 Windows 应用商店](submit-an-app-with-ads-to-the-windows-store.md)。

在应用发布并在 Windows 应用商店提供后，你可以在开发人员中心仪表板中查看你的[广告性能报告](../publish/advertising-performance-report.md)。

 

 



<!--HONumber=Aug16_HO3-->



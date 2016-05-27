---
author: mcleanbyron
ms.assetid: 9165f709-71d7-42cf-9b30-3190fe029fb4
description: 了解 Microsoft Advertising 库中的 AdControl 类与广告中介库中的 AdMediatorControl 类之间的区别。
title: 区别是什么 - AdMediatorControl 或 AdControl
---

# 区别是什么：AdMediatorControl 或 AdControl


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

当你想要显示来自 Microsoft 的横幅广告或间隙视频广告时，请在应用中使用适用于 XAML 和 JavaScript 的 Microsoft Advertising 库。 这些库不同于广告中介库，前者用于显示来自多个广告网络的广告。 对于 Microsoft Advertising 库（XAML 和 JavaScript），请在以下情况下使用本文档：

* 你正在使用 XAML 或 JavaScript 应用中的 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 或 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx)，而不是 **AdMediatorControl**。
* 你正在查找有关广告中介使用的基础 **AdControl** API 的参考信息。

Microsoft Advertising 库和广告中介库均包含在 Microsoft 官方商城协定和盈利 SDK 中。 有关安装此 SDK 以及其中包含的不同 Microsoft Advertising 库的详细信息，请参阅[安装 Microsoft Advertising 库](install-the-microsoft-advertising-libraries.md)。

>**注意** 若要显示间隙广告，请使用 **InterstitialAd** 控件。 **AdControl** 和 **AdMediatorControl** 不能显示间隙广告。 有关详细信息，请参阅[间隙广告](interstitial-ads.md)。

 

## 广告中介


显示来自 Microsoft 的横幅广告（不是间隙广告）的建议方法是使用应用中的 **AdMediatorControl**。 **AdMediatorControl** 显示来自多个广告网络的横幅广告。

当你在项目中使用 **AdMediatorControl** 时，必须使用 Visual Studio 的“连接的服务”****功能来指定要使用哪些广告网络。 Visual Studio 将尝试以编程方式获取某些广告网络所需的程序集。 如果存在任何无法自动检索到的程序集，必须针对每个广告网安装它们。 有关广告中介的详细信息，请参阅[使用广告中介使收益最大化](use-ad-mediation-to-maximize-revenue.md)。

**AdMediatorControl** 抽象化了广告单元 ID和应用程序 ID 的使用。 这些 ID 连同 Windows 开发人员中心仪表板中的广告中介设置一起由 **AdMediatorControl** 管理。 有关详细信息，请参阅[提交应用和配置广告中介](submit-your-app-and-configure-ad-mediation.md)。

**AdMediatorControl** 使用自己的属性和语法来支持其调节的每个广告服务的 API。 有关详细信息，请参阅[添加和使用广告中介控件](add-and-use-the-ad-mediator-control.md)。

## AdControl


如果你仅希望显示来自 Microsoft（没有其他广告网络）的横幅广告，则可以在 XAML 和 JavaScript 应用中单独使用 **AdControl**。 测试使用 **AdControl** 的应用时，请将[测试模式值](test-mode-values.md)用于应用程序 ID 和广告单元 ID。 在你完成应用测试后并准备将其提交到 Windows 开发人员中心后，请使用 Windows 开发人员中心仪表板获取实时广告单元 ID 和应用程序 ID，并更新代码以使用这些值。 有关详细信息，请参阅[在应用中设置广告单元](set-up-ad-units-in-your-app.md)。

 

 


<!--HONumber=May16_HO2-->



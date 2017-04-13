---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: "了解如何在将应用提交到应用商店之前，从 Windows 开发人员中心仪表板向应用添加应用程序 ID 和广告单元 ID 值。"
title: "在应用中设置广告单元"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 广告, 投放广告, 广告单元"
ms.openlocfilehash: daf0887462a4c84aa827a6261793a0eaf4d512ca
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="set-up-ad-units-in-your-app"></a>在应用中设置广告单元




当你使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 或 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 在应用中显示广告时，必须指定应用程序 ID 和广告单元 ID。 当你开发应用时，请使用适当的[测试应用程序 ID 和广告单元 ID 值](test-mode-values.md)以查看应用在测试期间如何呈现广告。

在你完成应用测试并准备将其提交到 Windows 开发人员中心时，必须更新你的应用代码才能使用 [Windows 开发人员中心仪表板](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx)中的应用程序 ID 和广告单元 ID 值。 如果你尝试在动态应用中使用测试值，你的应用不会收到实时广告。

若要为你的动态应用设置应用程序 ID 和广告单元：

1.  在 Windows 开发人员中心仪表板上，选择你的应用，然后依次单击“盈利”&gt;“利用广告来盈利”****。

2.  在此页面的“Microsoft Advertising 广告单元”****部分中，创建一个广告单元。 对于广告单元类型，请选择以下选项之一：

  * 如果在应用中使用 **AdControl** 显示横幅广告，请将广告单元类型选为**横幅**。

  * 如果在应用中使用 **InterstitialAd** 显示间隙视频或间隙横幅广告，请选择**视频间隙**或**横幅间隙**（确保为你想要显示的间隙广告类型选择正确的选项）。

3.  对于每个生成的广告单元，你将在此页面上看到“应用程序 ID”****和“广告单元 ID”****。 若要在应用中显示广告，需要在你的应用代码中使用这些值：

  * 如果你的应用显示横幅广告，请将这些值分配给 **AdControl** 对象的 [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) 和 [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) 属性。 有关详细信息，请参阅 [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md) 以及 [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)。

  * 如果你的应用显示间隙广告，请将这些值传递给 **InterstitialAd** 对象的 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法。 有关详细信息，请参阅[间隙广告](interstitial-ads.md)。

有关**利用广告来盈利**页面的详细信息，请参阅[利用广告来盈利](../publish/monetize-with-ads.md)。

## <a name="related-topics"></a>相关主题

* [测试模式值](test-mode-values.md)
* [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)
* [间隙广告](interstitial-ads.md)


 

 

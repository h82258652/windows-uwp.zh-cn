---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: "了解如何在将应用提交到应用商店之前，从 Windows 开发人员中心仪表板向应用添加应用程序 ID 和广告单元 ID 值。"
title: "在应用中设置广告单元"
translationtype: Human Translation
ms.sourcegitcommit: c6e0cf98c6eb2cdc656d5b4555d794ff6a94d2bc
ms.openlocfilehash: 705955faf7ddd67f80098f8c3ac7b2844553de95


---

# 在应用中设置广告单元




当你使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 或 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 在应用中显示广告时，必须指定应用程序 ID 和广告单元 ID。 当你开发应用时，请使用适当的[测试应用程序 ID 和广告单元 ID 值](test-mode-values.md)以查看应用在测试期间如何呈现广告。

在你完成应用测试并准备将其提交到 Windows 开发人员中心时，必须更新你的应用代码才能使用 [Windows 开发人员中心仪表板](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx)中的应用程序 ID 和广告单元 ID 值。 如果你尝试在动态应用中使用测试值，你的应用不会收到实时广告。

若要为你的动态应用设置应用程序 ID 和广告单元：

1.  在 Windows 开发人员中心仪表板上，选择你的应用，然后依次单击“盈利”&gt;“利用广告来盈利”****。
2.  在此页面的“Microsoft Advertising 广告单元”****部分中，创建一个广告单元。 对于广告单元类型，如果你使用的是 **AdControl**，则选择“横幅”****，如果你使用的是 **InterstitialAd**，则选择“间隙视频”****。 有关此页面的详细信息，请参阅[利用广告来盈利](../publish/monetize-with-ads.md)。

3.  对于每个生成的广告单元，你将在此页面上看到“应用程序 ID”****和“广告单元 ID”****。 若要在应用中显示广告，需要在你的应用代码中使用这些值：

    * 如果你的应用显示横幅广告，请将这些值分配给 **AdControl** 对象的 **ApplicationId** 和 **AdUnitId** 属性。

    * 如果你的应用显示视频间隙广告，请将这些值传递给 **InterstitialAd** 对象的 **RequestAd** 方法。

 

## 相关主题

[测试模式值](test-mode-values.md)


 

 



<!--HONumber=Aug16_HO3-->



---
author: Xansky
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: 了解如何将应用程序 ID 和广告单元 ID 值从合作伙伴中心添加到你的应用之前提交到应用商店应用。
title: 在应用中设置广告单元
ms.author: mhopkins
ms.date: 05/11/2018
ms.topic: article
keywords: Windows 10, uwp, 广告, 投放广告, 广告单元, 测试
ms.localizationpriority: medium
ms.openlocfilehash: 11c66756d95e041a45fbc075b02eb744bf542871
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6045477"
---
# <a name="set-up-ad-units-in-your-app"></a>在应用中设置广告单元

通用 Windows 平台 (UWP) 应用中的每个广告控件都有一个相应的*广告单元*，我们的服务使用它们向控件提供广告。 每个广告单元都由*广告单元 ID* 和*应用程序 ID* 组成，你必须将这两者分配给应用中的代码。

我们提供了[测试广告单元值](#test-ad-units)，你可以在测试过程中使用这些值来确定应用是否显示测试广告。 这些测试值只能在应用的测试版本中使用。 如果你尝试在发布动态应用后在其中使用测试值，应用将不会收到广告。

完成你的 UWP 应用的测试并准备好将其提交到合作伙伴中心后，你必须从合作伙伴中心中的[应用内广告](../publish/in-app-ads.md)页面[创建实时广告单元](#live-ad-units)，并更新你的应用代码才能使用此广告单元的应用程序 ID 和广告单元 ID 值。

有关在应用代码中分配应用程序 ID 和广告单元 ID 值的详细信息，请参阅以下文章：
* [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)
* [间隙广告](../monetize/interstitial-ads.md)
* [本机广告](../monetize/native-ads.md)

<span id="test-ad-units" />

## <a name="test-ad-units"></a>测试广告单元

当你开发应用时，请使用本部分中的测试应用程序 ID 和广告单元 ID 值，查看应用在测试期间如何呈现广告。

### <a name="banner-ads-using-the-adcontrol-class"></a>横幅广告（使用 AdControl 类）

* 广告单元 ID： ```test```
* 应用程序 ID：  ```3f83fe91-d6be-434d-a0ae-7351c5a997f1```

    > [!IMPORTANT]
    > 对于 **AdControl**，实时广告的大小由 **Width** 和 **Height** 属性定义。 为获得最佳结果，请确保代码中的 **Width** 和 **Height** 属性是[横幅广告的受支持广告大小](supported-ad-sizes-for-banner-ads.md)之一。 **Width** 和 **Height** 属性不会根据实时广告的大小而发生更改。

### <a name="interstitial-ads-and-native-ads"></a>间隙广告和本机广告

* 广告单元 ID： ```test```
* 应用程序 ID：  ```d25517cb-12d4-4699-8bdc-52040c712cab```

<span id="live-ad-units" />

## <a name="live-ad-units"></a>实时广告单元

若要从合作伙伴中心中获取实时广告单元，并将其用于你的应用：

1.  合作伙伴中心中的**应用内广告**页面上[创建一个广告单元](../publish/in-app-ads.md#create-ad-unit)。 请务必为应用中要使用的广告控件指定正确的广告单元类型。
    > [!NOTE]
    > 你可以选择通过配置[中介设置](../publish/in-app-ads.md#mediation)部分的设置为广告单元启用广告中介。 广告中介显示来自多个广告网络（包括其他付费广告网络）的广告以及 Microsoft 应用促销活动的广告，能够最大化广告收益和应用促销能力。 默认情况下，我们会使用机器学习算法为你的应用自动选择广告中介，以帮助最大化你的广告在应用支持的市场中的收益，但是，你也可以选择手动配置中介设置。

2.  创建新的广告单元后，在**盈利** &gt; **应用内的广告**页面的可用广告单元表中检索**应用程序 ID** 和**广告单位 ID**。
    > [!NOTE]
    > 测试广告单元和实时 UWP 广告单元的应用程序 ID 值采用不同的格式。 测试应用程序 ID 值为 GUID。 在合作伙伴中心中创建实时 UWP 广告单元时，该广告单元的应用程序 ID 值始终与应用商店 ID 为你的应用 （示例应用商店 ID 值类似于 9NBLGGH4R315） 匹配。

3.  在应用的代码中指定应用程序 ID 和广告单元 ID 值。 有关详细信息，请参阅以下文章：
    * [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)
    * [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)
    * [间隙广告](../monetize/interstitial-ads.md)
    * [本机广告](../monetize/native-ads.md)

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>在应用中管理多个广告控件的广告单元

你可以在单个应用中使用多个横幅广告、间隙广告和本机广告控件。 在此情况下，我们建议你为每个控件分配不同的广告单元。 对每个控件使用不同的广告单元使你可以分别[配置中介设置](../publish/in-app-ads.md#mediation)并获取每个控件的独立[报告数据](../publish/advertising-performance-report.md)。 这还使我们的服务能够更好地优化我们为你的应用提供的广告。

> [!IMPORTANT]
> 每个广告单元都只能在一个应用中使用。 如果在多个应用中使用某个广告单元，将不为该广告单元提供广告。

## <a name="related-topics"></a>相关主题

* [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)
* [间隙广告](interstitial-ads.md)
* [本机广告](native-ads.md)


 

 

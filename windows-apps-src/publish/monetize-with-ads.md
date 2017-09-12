---
author: jnHs
Description: "如果应用使用广告中介，或者显示使用 Microsoft Store Services SDK 的横幅或间隙广告，请使用“利用广告来盈利”页面管理广告使用。"
title: "利用广告来盈利"
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.author: wdg-dev-content
ms.date: 07/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 6ecd37e54de266764570606ceaa575614dfb050c
ms.sourcegitcommit: 10f8dcf69d37cdb61562fc9f4d268ccb499c368f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# <a name="monetize-with-ads"></a>利用广告来盈利

仪表板中的每个应用包括一个**盈利** &gt; **利用广告来盈利**页面。 使用此页面以在应用中针对以下开发方案来管理广告使用：

* UWP 应用使用来自 [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp) 的 [AdControl](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)、[InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 或 [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx)。
* Windows 8.x 或 Windows Phone 8.x 应用使用来自[适用于 Windows 和 Windows Phone 8.x 的 Microsoft 广告 SDK](http://aka.ms/store-8-sdk) 的 **AdControl** 或 **InterstitialAd**。
* Windows 8.x 或 Windows Phone 8.x 应用使用来自[适用于 Windows 和 Windows Phone 8.x 的 Microsoft 广告 SDK](http://aka.ms/store-8-sdk) 的 **AdMediatorControl**。

有关这些开发方案的详细信息，请参阅[使用 Microsoft 广告 SDK 在应用中显示广告](../monetize/display-ads-in-your-app.md)。

<span id="create-ad-unit" />
## <a name="create-ad-units"></a>创建广告单元

使用本部分以针对以下方案创建广告单元：

* 应用通过使用 **AdControl** 显示横幅广告。 有关详细信息，请参阅 [XAML 和 .NET 中的 AdControl](../monetize/adcontrol-in-xaml-and--net.md) 以及 [HTML5 和 JavaScript 中的 AdControl](../monetize/adcontrol-in-html-5-and-javascript.md)。
* 应用通过使用 **InterstitialAd** 显示间隙视频广告或间隙横幅广告。 有关详细信息，请参阅[间隙广告](../monetize/interstitial-ads.md)。
* 应用通过使用 **NativeAd** 显示本机广告。 有关详细信息，请参阅[本机广告](../monetize/native-ads.md)。

若要创建广告单元：

1.  在**广告单元名称**字段中，输入广告单元的名称。 这可以是希望因报告目的而用来标识广告单元的任意描述性字符串。
2.  在**广告单元类型**下拉框中，选择与将在控件中显示的广告相对应的广告单元类型。 可用选项包括：**横幅**、**横幅间隙**、**视频间隙**和**本机**。
    > [!NOTE]
    > 目前只有参与试用计划的部分开发人员能够创建**本机**广告单元，但我们希望尽快向所有开发人员提供此功能。 如果有兴趣参加我们的试用计划，请通过 aiacare@microsoft.com 联系我们。

3.  在**设备系列**下拉框中，选择将在其中使用广告单元的应用所适用的设备系列。 可用选项包括：**UWP (Windows 10)**、**电脑/平板电脑 (Windows 8.1)** 或**移动设备 (Windows Phone 8.x)**。
4.  单击**创建广告单元**。

新的广告单元将显示在此页面的**可用广告单元**部分中的列表顶部。 有关在应用中使用广告单元的详细信息，请参阅[在应用中设置广告单元](../monetize/set-up-ad-units-in-your-app.md)。

> [!NOTE]
> 如果 Windows 8.x 或 Windows Phone 8.x 应用使用 **AdMediatorControl** 显示横幅广告，则无需在此处请求广告单元。 在此情况下，将为你自动生成广告单元。

<span id="available-ad-units" />
## <a name="available-ad-units"></a>可用广告单元

你的广告单元将显示在本部分底部的表格中。 你将会看到每个广告单元的“应用程序 ID”****和“广告单元 ID”****。 若要在应用中显示广告，需要在你的代码中使用这些值：

-   如果你的应用显示横幅广告，请将这些值分配给 [AdControl](https://msdn.microsoft.com/library/mt313154.aspx) 对象的 [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) 和 [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) 属性。 有关详细信息，请参阅 [XAML 和 .NET 中的 AdControl](../monetize/adcontrol-in-xaml-and--net.md) 以及 [HTML5 和 JavaScript 中的 AdControl](../monetize/adcontrol-in-html-5-and-javascript.md)。
-   如果你的应用显示间隙广告，请将这些值传递给 [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx) 对象的 [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) 方法。 有关详细信息，请参阅[间隙广告](../monetize/interstitial-ads.md)。
-   如果应用显示本机广告，请将这些值传递给 [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx) 构造函数的 *applicationId* 和 *adUnitId* 参数。 有关详细信息，请参阅[本机广告](../monetize/native-ads.md)。

> [!IMPORTANT]
> 每个广告单元都只能在一个应用中使用。 如果在多个应用中使用某个广告单元，将不为该广告单元提供广告。

> [!NOTE]
> 可以在单个应用中使用多个横幅广告、间隙广告和本机广告控件。 在此情况下，我们建议为每个控件分配不同的广告单元。 对每个控件使用不同的广告单元可以分别[配置中介设置](../publish/monetize-with-ads.md#mediation)并获取每个控件的独立[报告数据](../publish/advertising-performance-report.md)。 这还使我们的服务能够更好地优化我们为应用提供的广告。

<span id="mediation" />
## <a name="ad-mediation"></a>广告中介

如果应用是适用于 Windows 10 的 UWP 应用，则可以使用此部分中的选项来为应用中与横幅广告、间隙广告或本机广告相关联的 UWP 广告单元启用广告中介。 广告中介显示来自多个广告网络（包括其他付费广告网络）的广告以及 Microsoft 应用促销活动的非盈利性广告，能够最大化广告收益和应用促销能力。 我们负责调停来自所选广告网络的横幅广告请求。

如果已在应用中拥有与横幅广告、间隙广告或本机广告相关联的 UWP 广告单元，则启用广告中介无需在应用中进行任何代码更改。

> [!NOTE]
> 本部分介绍用于 UWP 应用包的**广告中介**选项。 如果你的应用包面向 Windows 8.x 或 Windows Phone 8.x 并且使用来自[适用于 Windows 和 Windows Phone 8.x 的 Microsoft 广告 SDK](http://aka.ms/store-8-sdk) 的 **AdMediatorControl**，则仪表板中的**广告中介**部分将显示一组不同的选项。 有关如何为使用 **AdMediatorControl** 的 Windows 8.x 或 Windows Phone 8.x 应用包配置中介设置的详细信息，请参阅[此文章](https://msdn.microsoft.com/library/windows/apps/mt219689)。

若要为应用中的 UWP 广告单元配置广告中介设置：

1. 在**配置中介对象**下拉框中，选择包含希望配置的横幅广告单元、间隙广告单元或本机广告单元的 UWP 应用包。

2. 在**广告单元类型**下拉框中，选择希望配置的广告单元类型（**横幅**、**横幅间隙**、**视频间隙**或**本机**）。

3. 在**广告单元**下拉框中，选择希望配置的 UWP 广告单元的名称。
    > [!NOTE]
    > 为 UWP 广告单元启用广告中介时，不需要从第三方广告网络获取广告单元。 我们的广告中介服务会自动创建任何必要的第三方广告单元。

4. 默认情况下，**让 Microsoft 为你的应用选择最佳中介设置**复选框处于选中状态。 此选项会使用机器学习算法自动为你的应用选择广告中介设置，以帮助最大化你在你的应用所支持的各市场中的广告收益。 我们建议你使用此选项。 否则如果你希望选择自己的广告中介设置，请清除此复选框。
    > [!NOTE]
    > 只有在你清除此复选框并选择自己的广告中介设置后，本部分中的余下步骤才可用。

5. 在**目标**下拉框中，选择**基线**为你的广告中介设置配置默认配置。 此默认配置将应用于除你在其中定义了市场特定配置之外的所有市场。

6. 接下来，指定你希望在控件中显示的来自付费网络（针对广告印象向你支付收益）和其他广告网络（不针对广告印象向你支付收益）的广告的比例。 要执行此操作，请在**付费广告网络**和**其他广告网络**的**权重**字段中输入介于 0 至 100 之间的值。  

7. 在**付费广告网络**部分中，针对要使用的每种付费网络，选中**活动**栏中的复选框，然后使用**排名**栏中的箭头按名次为网络排序（这指定控件应使用每种网络的频率）。

    当前支持以下付费网络。 请注意，这之中的某些网络[并非适用于所有市场](#network-markets)。

    -   **AOL 和 AppNexus**。 这是一个可通过我们的合作伙伴网络提供广告的 Microsoft 托管的广告网络，即 AOL 和 AppNexus。 此网络受**横幅**和**视频间隙**广告单元的支持。
        > [!NOTE]
        > 对于**横幅**广告单元，**AOL 和 AppNexus**始终在**付费广告网络**列表中排名第一，且针对这些类型的广告不能更改为更低排名。

    -   **AppNexus（直接）**：选择此选项以提供来自 [AppNexus](https://www.appnexus.com) 的视频间隙广告。 此网络受**视频间隙**和**本机**广告单元的支持。

    -   **Microsoft 应用安装广告**。 选择此选项以提供由 Windows 生态系统中[为其应用创建促销广告市场活动](create-an-ad-campaign-for-your-app.md)的其他开发人员创建的应用安装广告或应用重新参与广告。 此网络受**横幅**、**横幅间隙**和**本机**广告单元的支持。

    -   **Smaato**：选择此选项以提供来自 [Smaato](https://www.smaato.com/) 的广告。 此网络受**横幅**广告单元的支持。

    -   **smartclip**：选择此选项以提供来自 [smartclip](http://www.smartclip.com/) 的广告。 此网络受**视频间隙**广告单元的支持。

    -   **SpotX**：选择此选项以提供来自 [SpotX](https://www.spotx.tv/) 的广告。 此网络受**视频间隙**广告单元的支持。

    -   **Taboola**：选择此选项以提供来自 [Taboola](https://www.taboola.com/) 的广告。 此网络受**横幅**广告单元的支持。

8. 如果选择了**横幅**或**横幅间隙**广告单元，将看到一个**其他广告网络**部分。 此部分中的网络不会因广告曝光而为你挣取收益。 相反，这些网络显示来自应用促销市场活动等来源的广告。 

    在**其他广告网络**部分中，针对希望使用的每种其他网络，选中**活动**列中的复选框，然后使用**排名**列中的箭头按名次为网络排序（这指定控件应使用每种网络的频率）。 当前支持以下其他网络：

    -   **Microsoft Community 广告**。 如果[为应用之一创建促销广告市场活动](create-an-ad-campaign-for-your-app.md)并将此市场活动配置为[社区广告市场活动](about-community-ads.md)，则选择此选项以显示来自此市场活动的广告。 

    -   **Microsoft 自家广告**。 如果[为应用之一创建促销广告市场活动](create-an-ad-campaign-for-your-app.md)并将此市场活动配置为[自家广告市场活动](about-house-ads.md)，则选择此选项以显示来自此市场活动的广告。

9. 对于你想要为其替代默认中介配置的每个市场，请在**目标**下拉框中选择市场，然后更新广告网络选择和排名。

10. 单击**保存**。

<span id="network-markets" />
### <a name="supported-markets-for-ad-networks"></a>广告网络支持的市场

可用的广告网络在所有 UWP 应用[支持的市场](define-pricing-and-market-selection.md#windows-store-consumer-markets)中提供广告，以下市场例外。

|  广告网络  |  支持的市场  |
|--------------|---------------------|
| Smaato | 巴西、加拿大、法国、德国、印度、日本、西班牙、英国、美国 |
| smartclip | 奥地利、比利时、丹麦、芬兰、德国、意大利、荷兰、挪威、瑞典、瑞士  |


## <a name="microsoft-affiliate-ads"></a>Microsoft 联盟广告

如果想要在你的应用中显示 Microsoft 关联广告，请选中此部分中的框。 如果选中此框，当其他广告网络没有可用的广告时，将为你的应用在应用商店中提供这些产品（包括音乐、游戏、电影、应用、硬件和软件）的广告。 当客户在给定特性窗口的应用商店中单击广告和购买产品时，你将赚取已批准购买的佣金。

如果更改此选择，你无需重新发布应用以让更改生效。 有关 Microsoft 联盟广告的详细信息，请参阅[关于联盟广告](about-affiliate-ads.md)。

## <a name="coppa-compliance"></a>COPPA 符合性

根据《儿童在线隐私保护法》（“COPPA”），如果你的应用面向 13 岁以下的儿童，则你必须通知 Microsoft。 如果你使用开发人员中心向 Microsoft 指示你的应用将会面向13 岁以下的儿童，则 Microsoft 将采取措施在向你的应用发送广告时禁用其行为广告服务。 如果你的应用面向 13 岁以下的儿童，则你承担 COPPA 规定的某些义务。

有关 COPPA 规定的义务的详细信息，请参阅[此页面](http://go.microsoft.com/fwlink/p/?linkid=536558)。

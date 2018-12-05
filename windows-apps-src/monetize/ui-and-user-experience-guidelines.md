---
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: 了解应用中广告的 UI 和用户体验指南。
title: 广告的 UI 和用户体验指南
ms.date: 05/11/2018
ms.topic: article
keywords: windows 10, uwp, 广告, 指南, 最佳做法
ms.localizationpriority: medium
ms.openlocfilehash: 78f044890e49f4631abf710764bc2f9746a1306f
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8697589"
---
# <a name="ui-and-user-experience-guidelines-for-ads"></a>广告的 UI 和用户体验指南

本文将简要介绍如何使用应用中的横幅广告、间隙广告和本机广告提供出色的体验。 有关如何设计应用外观的通用指南，请参阅 [设计和 UI](https://developer.microsoft.com/windows/apps/design)。

> [!IMPORTANT]
> 对应用内广告的任何使用均必须符合 Microsoft Store 策略，包括但不限于策略 [10.10](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content)（广告行为和内容）。 特别是，应用的横幅广告或间隙广告实现必须满足 Microsoft Store 策略 [策略 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) 中的要求。 本文将举例说明违反此策略的一些实现。 这些示例仅供参考，以此方式帮助你更好地理解策略。 这些示例并不全面，可能有许多其他违反 Microsoft Store 策略的方式未在本文列出。

## <a name="general-best-practices"></a>一般最佳做法

在查看本文中针对不同类型广告的指南之前，我们先看一下有助于增加广告收入的一般性最佳做法。

* [仔细计划广告位置](https://blogs.windows.com/buildingapps/2017/04/10/monetizing-app-advertisement-placement/)。 请参见我们[优化广告单元可见性](optimize-ad-unit-viewability.md)的相关指南。
* [使用间隙横幅广告作为间隙视频广告的次选广告](https://blogs.windows.com/buildingapps/2017/04/17/monetizing-app-use-interstitial-banner-fallback-interstitial-video)。
* [了解用户以提供更好的定向广告](https://blogs.windows.com/buildingapps/2017/05/17/monetize-app-know-user-serve-better-targeted-ads/)。
* [使用最新的 Advertising 库](https://blogs.windows.com/buildingapps/2017/05/22/earn-money-moving-latest-advertising-libraries/)。
* [为应用设置正确 COPPA 设置](https://blogs.windows.com/buildingapps/2017/06/21/monetizing-app-set-coppa-settings-app)。


## <a name="guidelines-for-banner-ads"></a>横幅广告指南

以下部分针对如何使用 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 在应用中实施[横幅广告](banner-ads.md)提供了一些建议，以及违反 Microsoft Store 策略的[策略 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) 的实现示例。

### <a name="best-practices"></a>最佳做法

建议在应用中实施横幅广告时遵循下列最佳做法：

* [采用美国互动广告局建议尺寸](https://blogs.windows.com/buildingapps/2017/04/03/monetizing-app-use-interactive-advertising-bureau-ad-sizes)（契合设备布局）。

* 将大部分应用 UI 投入到功能性空间和内容中。

* 将广告设计与你的体验相融合。 为设计人员提供示例广告，以设计广告的预期外观。 应用中两个精心设计的广告示例是广告即内容布局和拆分布局。

  若要在开发和测试阶段查看应用中广告大小的外观差异和运行状况，可以利用我们的 [测试广告单元](set-up-ad-units-in-your-app.md#test-ad-units)。 在结束使用测试后，请记住[使用实时广告单元更新应用](set-up-ad-units-in-your-app.md#live-ad-units)，然后再提交应用以供认证。

* 针对广告不可用时进行规划。 可能会多次出现广告未发送到应用的情况。 设计页面的版式，以使其不管是否显示广告都看起来很精美。 有关详细信息，请参阅[处理错误](error-handling-with-advertising-libraries.md)。

* 如有对用覆盖进行最佳处理的客户发出警报的场景，请在显示覆盖时调用 [AdControl.Suspend](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend)，并在完成警报场景后调用 [AdControl.Resume](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.resume)。

### <a name="practices-to-avoid"></a>要避免的做法

建议在应用中实施横幅广告时避免下列做法：

* 请勿将广告放在开阔的使用空间。 广告空间不应放置在你可以找到的第一块开阔的使用空间。 而应将广告融入应用的整体设计。

* 应用中的广告不宜过多。 应用中的广告太多会有损于应用的外观和可用性。 你希望通过广告赚钱，但不应以牺牲应用本身为代价。

* 使用户专注其核心任务。 主要关注点应始终在应用上。 应合并广告空间，使其处于次要关注点。

### <a name="examples-of-policy-violations"></a>策略违反示例

本部分提供违反 Microsoft Store 策略的[策略 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) 的横幅广告场景示例。 这些示例仅供教学，以此方式帮助你更好地理解策略。 这些示例并不完善，可能有许多其他违反策略 10.10.1 的方式未在此处列出。

* 通过一些处理来干扰用户查看横幅广告的能力，例如，更改 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 的不透明度或将其他控件置于 **AdControl** 之上（不会先调用 [AdControl.Suspend](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend)）。

* 要求用户单击横幅广告才能完成应用中的任务，或强制用户单击横幅广告作为应用设计的结果。

* 以任何方式绕过横幅广告的内置最小刷新计时器，包括（但不限于）交换 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 对象，或不进行用户交互就强制刷新页面。

* 在开发和测试期间使用实时广告单元 （即，从合作伙伴中心获得的广告单元） 或在仿真器。

* 通过在应用程序上下文中运行的 Microsoft Advertising 库之外的方式编写或分发调用广告服务的代码。

* 与未记录的界面或 Microsoft Advertising 库创建的子对象（例如，**WebView** 或 **MediaElement**）交互。

<span id="interstitialbestpractices10" />

## <a name="guidelines-for-interstitial-ads"></a>间隙广告指南

如果使用得当，[间隙广告](interstitial-ads.md)可极大提高应用的收益，而不会对用户满意度带来负面影响。 如果使用不当，此类广告可能会产生相反的效果。

以下部分针对如何使用 [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 在应用中实施间隙视频广告和间隙横幅广告提供一些建议，以及违反 Microsoft Store 策略的[策略 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) 的实现示例。 由于你比其他任何人都更了解你的应用（涉及到策略除外），因此我们将其留给你来做最佳的最后决定。 最重要的是，请务必记住你的应用评分与收益密切相关。

### <a name="best-practices"></a>最佳做法

建议你在应用中实施间隙广告时遵循下列最佳做法：

* 将间隙广告安插在应用的自然流程中，如游戏关卡之间。

* 将广告与切实好处相关联，例如：

    * 关卡完成的提示。

    * 重试某个关卡的额外时间。

    * 自定义虚拟形象功能，如纹身或帽子。

* 如果你的应用需要看完整个间隙视频广告，请提前指出这一规则，这样他们就不会对点击关闭按钮时出现错误消息感到意外。

* 理想情况下，在你需要显示广告之前，请预取广告（通过调用 [InterstitialAd.RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)）30 至 60 秒。

* 订阅 [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 类中显示的全部四个事件（**Canceled**、**Completed**、**AdReady** 和 **ErrorOccurred**），并使用它们来做出适用于应用的正确决策。

* 将一些内置体验用于替代服务器匹配的广告。 你会发现这在以下几种情况下非常有用：

    * 脱机模式，即无法访问广告服务器时。

    * 引发 **ErrorOccurred** 事件时。

    * 如果你选择保存基于 [ConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) 的用户带宽，**ConnectionProfile** 类中的 API 可以提供帮助。

* 如果没有合理的理由，请使用默认（30 秒）超时，否则，在这种情况下，不能低于 10 秒。 与标准横幅广告相比，间隙视频广告实质上需要花费更长的时间来下载，尤其是在没有高速连接的市场中。

* 注意用户的流量套餐。 例如，在接近或超出其流量上限的移动设备上投放间隙视频广告前，不要向用户显示广告，或者向用户发出警告。 [ConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) 类中的 API 可以提供帮助。

* 在初始提交后持续改进你的应用。 查看[广告报告](../publish/advertising-performance-report.md)并对设计进行更改，以提高填充率和间隙视频完成率。

### <a name="practices-to-avoid"></a>要避免的做法

建议在应用中实施间隙广告时避免下列做法：

* 请勿过度放置间隙广告。 不要每超过约 5 分钟就强制插入间隙广告，除非用户除了玩游戏以外，明确对某个可选的切实好处感兴趣。

* 请勿在应用启动时显示间隙广告，这样可能会导致用户认为单击了错误的磁贴。

* 请勿在退出时显示间隙广告。 这是很糟糕的库存，因为完成率接近于零。

* 请勿来回播放两个或多个间隙广告。 用户看到广告进度条重置到起始点时会感到非常失望。 许多人会认为这只是代码编写或广告投放 Bug。

* 请勿在调用 [InterstitialAd.Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) 之前提取一个超过 5 分钟的间隙视频广告。 良好的库存会最大限度地将预取的广告转换为可计费的广告展示。

* 请勿对广告投放出现故障（如没有可用的广告）的用户进行处罚。 例如，如果你显示“观看广告获取 *xxx*”的 UI 选项，那么在用户看完以后，你应提供 *xxx*。 有两个选项可以考虑：

    * 不包括该选项，除非已引发 [InterstitialAd.AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready) 事件。

    * 使应用包含内置体验，以产生与真实广告相同的效益。

* 请勿使用间隙广告让用户在多玩家游戏中获得竞争优势。 例如，如果用户看到了间隙广告，请勿在第一人称射击游戏中用一把较好的枪吸引用户。 玩家的虚拟形象穿的定制衬衫很精美，只要不提供伪装就行！

### <a name="examples-of-policy-violations"></a>策略违反示例

本部分提供违反 Microsoft Store 策略的[策略 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) 的间隙广告场景示例。 这些示例仅供教学，以此方式帮助你更好地理解策略。 这些示例并不完善，可能有许多其他违反策略 10.10.1 的方式未在此处列出。

* 将所有 UI 元素放在间隙广告容器上。

* 在用户使用应用时调用 [InterstitialAd.Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show)。

* 使用间隙广告获取任何可以作为货币消费或与其他用户交易的内容。

* 在 [InterstitialAd.ErrorOccurred](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.erroroccurred) 事件的事件处理程序上下文中请求新的间隙广告。 这可能会导致无限循环，以及广告服务的操作问题。

* 只是为了具有瀑布序列广告的备份广告而请求间隙广告。 如果请求间隙广告后收到 [InterstitialAd.AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready) 事件，则应用中所示的下一个间隙广告必须是已准备好通过 [InterstitialAd.Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) 方法显示的广告。

* 在开发和测试期间使用实时广告单元 （即，从合作伙伴中心获得的广告单元） 或在仿真器。

* 通过在应用程序上下文中运行的 Microsoft Advertising 库之外的方式编写或分发调用广告服务的代码。

* 与未记录的界面或 Microsoft Advertising 库创建的子对象（例如，**WebView** 或 **MediaElement**）交互。

## <a name="guidelines-for-native-ads"></a>本机广告指南

[本机广告](native-ads.md)让你能够良好地控制向用户展示广告内容的方式。 遵守下述要求和指南有助于确保将广告商的信息传达给用户，同时也可帮助避免为用户提供混乱的本机广告体验。

### <a name="register-the-container-for-your-native-ad"></a>为本机广告注册容器

你必须在代码中调用 [NativeAdV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) 对象的 [RegisterAdContainer](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2.registeradcontainer) 方法注册用作本机广告容器的 UI 元素，并有选择地注册你希望注册为广告的可点击目标的任何特定控件。 要正确跟踪广告曝光数和点击数，就必须执行此操作。

**RegisterAdContainer** 方法有两个重载：

* 如果你希望所有单个本机广告元素的整个容器变成可点击状态，请调用 **RegisterAdContainer(FrameworkElement)** 方法并向方法传递容器控件。 例如，你在各个独立控件中显示所有本机广告元素，所有这些控件都托管在 **StackPanel** 中，你希望整个 **StackPanel** 变成可点击状态，请向此方法传递 **StackPanel**。

* 如果你只想让特定本机广告元素变成可点击状态，请调用 **RegisterAdContainer(FrameworkElement, IVector(FrameworkElement))** 方法。 只有你传递给第二个参数的控件会变成可点击状态。

### <a name="required-native-ad-elements"></a>必需的本机广告元素

在本机广告设计中，至少必须始终向用户显示由 [NativeAdV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) 对象的属性提供的以下本机广告元素。 若不包含这些元素，广告单元的业绩会很差，收益率也很低。

1. 始终显示本机广告的标题（由 **Title** 属性提供）。 提供足以显示至少 25 个字符的空间。 如果标题较长，请使用省略号替换其他文字。
2. 始终显示以下元素中的至少一种，以帮助区分本机广告体验与你的应用体验，清楚地表明此内容由广告商提供：
    * 可区分的*广告* 图标（由 **AdIcon** 属性提供）。 此图标由 Microsoft 提供。
    * *赞助商* 文本（由 **SponsoredBy** 属性提供）。 此文本由广告商提供。
    * 除了*赞助商*文本以外，你也可以选择显示一些其他的文本，以帮助区分本机广告体验与你的应用体验，例如“赞助内容”、“促销内容”、“推荐内容”等。

### <a name="user-experience"></a>用户体验

本机广告应与你的应用的其余部分清晰分隔，并且周围有防止意外点击的空间。 使用边框、不同背景或其他 UI 将广告内容与应用的其余部分隔开。 请记住，从长远来看，意外点击广告不会给你的广告收益或最终用户体验带来任何好处。

### <a name="description"></a>说明

如果选择显示广告说明（由 **NativeAdV2** 对象的 **Description** 属性提供），请提供足以显示至少 75 个字符的空间。 我们建议你使用动画显示广告描述的完整内容。

### <a name="call-to-action"></a>行动号召

*行动号召* 文本（由 **NativeAdV2** 对象的 **CallToAction** 属性提供）是广告的重要组成部分。 如果选择显示此文本，请遵循以下准则：

* 始终在可点击的控件（例如按钮或超链接）上向用户显示*行动号召*文本。
* 始终显示完整的*行动号召*文本。
* 确保*行动号召*文本与广告商的其他促销文本分隔开。

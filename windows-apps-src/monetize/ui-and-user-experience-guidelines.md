---
author: mcleanbyron
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: "了解应用中广告的 UI 和用户体验指南。"
title: "应用中广告的 UI 和用户体验指南"
translationtype: Human Translation
ms.sourcegitcommit: e44392a1dc69a98655ba7e576d2af102a608acaa
ms.openlocfilehash: ce39829cd6cd2dfb0c6a3aef930dd8fa82351b75

---

# <a name="ui-and-user-experience-guidelines-for-ads-in-apps"></a>应用中广告的 UI 和用户体验指南

本文将简要介绍如何使用应用中的横幅广告和间隙广告提供出色的体验。 有关如何设计应用外观的通用指南，请参阅 [设计和 UI](https://developer.microsoft.com/windows/design)。

>**重要**&nbsp;&nbsp;对应用内广告的任何使用均必须符合 Windows 应用商店策略，包括但不限于[策略 10.10](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx#pol_10_10)（广告行为和内容）。 特别是，应用的横幅广告或间隙广告实现必须满足 Windows 应用商店策略 [策略 10.10.1](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx#pol_10_10) 中的要求。 本文将举例说明违反此策略的一些实现。 这些示例仅供参考，以此方式帮助你更好地理解策略。 这些示例并不全面，可能有许多其他违反 Windows 应用商店策略的方式未在本文列出。

## <a name="guidelines-for-banner-ads"></a>横幅广告指南

以下部分针对如何使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 在应用中实施横幅广告提供了一些建议，以及违反 Windows 应用商店策略的[策略 10.10.1](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx#pol_10_10) 的实现示例。

### <a name="best-practices"></a>最佳做法

建议在应用中实施横幅广告时遵循下列最佳做法：

* 将大部分应用 UI 投入到功能性空间和内容中。

* 将广告设计与你的体验相融合。 为设计人员提供示例广告，以设计广告的预期外观。 应用中两个精心设计的广告示例是广告即内容布局和拆分布局。

  若要在开发和测试阶段查看应用中广告大小的外观差异和运行状况，可以利用我们的 [测试模式广告单元](test-mode-values.md)。 在结束使用测试后，请记住 [使用实际广告单元 ID 更新应用](set-up-ad-units-in-your-app.md)，然后再提交应用以供认证。

* 使用适合运行设备布局的 [广告大小](supported-ad-sizes-for-banner-ads.md)。

* 针对广告不可用时进行规划。 可能会多次出现广告未发送到应用的情况。 设计页面的版式，以使其不管是否显示广告都看起来很精美。 有关详细信息，请参阅 [错误处理](error-handling-with-advertising-libraries.md)。

* 如有对用覆盖进行最佳处理的客户发出警报的场景，请在显示覆盖时调用 [AdControl.Suspend](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.suspend.aspx)，并在完成警报场景后调用 [AdControl.Resume](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.resume.aspx)。

<span />
### <a name="practices-to-avoid"></a>要避免的做法

建议在应用中实施横幅广告时避免下列做法：

* 请勿将广告放在开阔的使用空间。 广告空间不应放置在你可以找到的第一块开阔的使用空间。 而应将广告融入应用的整体设计。

* 应用中的广告不宜过多。 应用中的广告太多会有损于应用的外观和可用性。 你希望通过广告赚钱，但不应以牺牲应用本身为代价。

* 使用户专注其核心任务。 主要关注点应始终在应用上。 应合并广告空间，使其处于次要关注点。

<span />
### <a name="examples-of-policy-violations"></a>策略违反示例

本部分提供了违反 Windows 应用商店策略的[策略 10.10.1](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx#pol_10_10) 的横幅广告场景示例。 这些示例仅供教学，以此方式帮助你更好地理解策略。 这些示例并不完善，可能有许多其他违反策略 10.10.1 的方式未在此处列出。

* 通过一些处理来干扰用户查看横幅广告的能力，例如，更改 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 的不透明度或将其他控件置于 **AdControl** 之上（不会先调用 [AdControl.Suspend](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.suspend.aspx)）。

* 要求用户单击横幅广告才能完成应用中的任务，或强制用户单击横幅广告作为应用设计的结果。

* 以任何方式绕过横幅广告的内置最小刷新计时器，包括（但不限于）交换 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 对象，或不进行用户交互就强制刷新页面。

* 在开发和测试期间（或在仿真器中），使用实时广告单元（即，从 Windows 开发人员中心仪表板获得的广告单元）。

* 通过在应用程序上下文中运行的 Microsoft Advertising 库之外的方式编写或分发调用广告服务的代码。

* 与未记录的界面或 Microsoft Advertising 库创建的子对象（例如，**WebView** 或 **MediaElement**）交互。

<span id="interstitialbestpractices10">
## <a name="guidelines-for-interstitial-ads"></a>间隙广告指南

如果使用得当，间隙视频广告可极大提高应用的收益，而不会对用户满意度带来负面影响。 如果使用不当，此类广告可能会产生相反的效果。

以下部分针对如何使用 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 在应用中实施间隙广告提供了一些建议，以及违反 Windows 应用商店策略的[策略 10.10.1](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx#pol_10_10) 的实现示例。 由于你比其他任何人都更了解你的应用（涉及到策略除外），因此我们将其留给你来做最佳的最后决定。 最重要的是，请务必记住你的应用评分与收益密切相关。

### <a name="best-practices"></a>最佳做法

建议你在应用中实施间隙广告时遵循下列最佳做法：

* 将间隙广告安插在应用的自然流程中，如游戏关卡之间。

* 将广告与切实好处相关联，例如：

    * 关卡完成的提示。

    * 重试某个关卡的额外时间。

    * 自定义虚拟形象功能，如纹身或帽子。

* 如果你的应用需要看完整个视频广告，请提前指出这一规则，这样他们就不会对点击关闭按钮时出现错误消息感到意外。

* 理想情况下，在你需要显示广告之前，请预取广告（通过调用 [InterstitialAd.RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx)）30 至 60 秒。

* 订阅 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 类中显示的全部四个事件（**Canceled**、**Completed**、**AdReady** 和 **ErrorOccurred**），并使用它们来做出适用于应用的正确决策。

* 将一些内置体验用于替代服务器匹配的广告。 你会发现这在以下几种情况下非常有用：

    * 脱机模式，即无法访问广告服务器时。

    * 引发 **ErrorOccurred** 事件时。

    * 如果你选择保存基于 [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx) 的用户带宽，**ConnectionProfile** 类中的 API 可以提供帮助。

* 如果没有合理的理由，请使用默认（30 秒）超时，否则，在这种情况下，不能低于 10 秒。 与横幅广告相比，视频广告实质上需要花费更长的时间来下载，尤其是在没有高速连接的市场中。

<span/>

* 注意用户的流量套餐。 例如，在接近或超出其流量上限的移动设备上投放视频广告前，不要向用户显示广告，或者向用户发出警告。 [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx) 类中的 API 可以提供帮助。

* 在初始提交后持续改进你的应用。 查看[广告报告](../publish/advertising-performance-report.md)并对设计进行更改，以提高填充率和视频完成率。

<span />
### <a name="practices-to-avoid"></a>要避免的做法

建议在应用中实施间隙广告时避免下列做法：

* 请勿过度放置间隙广告。 不要每超过约 5 分钟就强制插入间隙广告，除非用户除了玩游戏以外，明确对某个可选的切实好处感兴趣。

* 请勿在应用启动时显示隙视频广告，这样可能会导致用户认为单击了错误的磁贴。

* 请勿在退出时显示间隙视频广告。 这是很糟糕的库存，因为完成率接近于零。

* 请勿来回播放两个或多个间隙广告。 用户看到广告进度条重置到起始点时会感到非常失望。 许多人会认为这只是代码编写或广告投放 Bug。

* 请勿在调用 [InterstitialAd.Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 之前提取一个超过 5 分钟的视频广告。 良好的库存会最大限度地将预取的广告转换为可计费的广告展示。

* 请勿对广告投放出现故障（如没有可用的广告）的用户进行处罚。 例如，如果你显示“观看广告获取 *xxx*”的 UI 选项，那么在用户看完以后，你应提供 *xxx*。 有两个选项可以考虑：

    * 不包括该选项，除非已引发 [InterstitialAd.AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx) 事件。

    * 使应用包含内置体验，以产生与真实广告相同的效益。

* 请勿使用间隙广告让用户在多玩家游戏中获得竞争优势。 例如，如果用户看到了间隙广告，请勿在第一人称射击游戏中用一把较好的枪吸引用户。 玩家的虚拟形象穿的定制衬衫很精美，只要不提供伪装就行！

<span />
### <a name="examples-of-policy-violations"></a>策略违反示例

本部分提供了违反 Windows 应用商店策略的[策略 10.10.1](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx#pol_10_10) 的间隙广告场景示例。 这些示例仅供教学，以此方式帮助你更好地理解策略。 这些示例并不完善，可能有许多其他违反策略 10.10.1 的方式未在此处列出。

* 将所有 UI 元素放在间隙广告容器上。

* 在用户使用应用时调用 [InterstitialAd.Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx)。

* 使用间隙广告获取任何可以作为货币消费或与其他用户交易的内容。

* 在 [InterstitialAd.ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.erroroccurred.aspx) 事件的事件处理程序上下文中请求新的间隙广告。 这可能会导致无限循环，以及广告服务的操作问题。

* 只是为了具有瀑布序列广告的备份广告而请求间隙广告。 如果请求间隙广告后收到 [InterstitialAd.AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx) 事件，则应用中所示的下一个间隙广告必须是已准备好通过 [InterstitialAd.Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 方法显示的广告。

* 在开发和测试期间（或在仿真器中）使用实时广告单元（即，从 Windows 开发人员中心仪表板获得的广告单元）

* 通过在应用程序上下文中运行的 Microsoft Advertising 库之外的方式编写或分发调用广告服务的代码。

* 与未记录的界面或 Microsoft Advertising 库创建的子对象（例如，**WebView** 或 **MediaElement**）交互。

 

 



<!--HONumber=Jan17_HO1-->



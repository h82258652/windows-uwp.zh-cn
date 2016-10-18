---
author: mcleanbyron
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: "了解应用中广告的 UI 和用户体验指南。"
title: "应用中广告的 UI 和用户体验指南"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: d464a2de442e6f1833f429c8460c27bf85e577d1


---

# 应用中广告的 UI 和用户体验指南




## 适用于 Windows 应用的常规 UI 资源

你可以在[设计和 UI](https://developer.microsoft.com/windows/design) 找到有关如何设计应用的外观的信息。

## AdControl 最佳做法

* [AdControl 最佳做法：应做事项](#adcontrolbestpracticesdo10)
* [AdControl 最佳做法：禁止事项](#adcontrolbestpracticesdont10)

<span id="adcontrolbestpracticesdo10"/>
### AdControl 最佳做法：应做事项

* 将广告设计到体验中。 为你的设计人员提供示例广告，以设计广告的外观。 应用中两个精心设计的广告示例是广告即内容布局和拆分布局。

  若要查看应用中广告大小的外观和运行状况的差异，可以利用我们适用于 Windows Phone、Windows 8.1 和 Windows 10 的测试模式广告单元。 在结束使用测试模式广告单元后，请记得[使用实际广告单元 ID 更新应用](set-up-ad-units-in-your-app.md)，然后再提交应用以供认证。

* 针对广告不可用时进行设计。 可能会多次出现广告未发送到应用的情况。 设计页面的版式，以使其不管是否显示广告都看起来很精美。 有关详细信息，请参阅[错误处理](error-handling-with-advertising-libraries.md)。

<span id="adcontrolbestpracticesdont10"/>
### AdControl 最佳做法：禁止事项

* 将广告放在开阔的使用空间。 广告空间不应置于你可以找到的第一块开阔的使用空间。 而应将广告融入应用的整体设计。

* 应用中的广告过多。 应用中有太多广告有损于应用的外观和可用性。 你想要通过广告赚钱，但应以牺牲应用本身为代价。

* 使用户无法专注其核心任务。 主要关注点应该始终在应用上。 应合并广告空间，使其保持为次要关注点。

<span id="interstitialbestpractices10"/>
## 间隙最佳做法

* [间隙最佳做法：应做事项](#interstitialbestpracticesdo10)
* [间隙最佳做法：避免事项](#interstitialbestpracticesavoid10)
* [间隙最佳做法：禁止事项（强制执行的策略）](#interstitialbestpracticesnever10)

如果使用得当，间隙视频广告可极大地提高你的应用收益，而不会对用户满意度带来负面影响。 如果使用不当，此类广告可能会产生正好相反的效果。

我们旨在帮助你实现优雅的设计。 由于你比其他任何人都更了解你的应用（涉及到策略除外），因此我们将其留给你来做最佳的最后决定。 最重要的是请务必记住你的应用评分与收益密切相关。

<span id="interstitialbestpracticesdo10"/>
### 间隙最佳做法：应做事项

* 将间隙广告安插在应用的自然流程中，如游戏关卡之间。

* 将广告与切实好处相关联，例如：

    * 关卡完成的提示。

    * 重试某个关卡的额外时间。

    * 自定义虚拟形象功能，如纹身或帽子。

* 如果你的应用需要观看完整个视频广告，请提前指出这一规则，这样他们就不会对点击关闭按钮时出现错误消息感到意外。

* 理想情况下，在你需要显示广告之前，请预取广告（通过调用 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx)）30 至 60 秒。

* 订阅 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 类中显示的全部四个事件（**Canceled**、**Completed**、**AdReady** 和 **ErrorOccurred**），并使用它们来做出适用于应用的正确决策。

* 将一些内置体验用于替代服务器匹配的广告。 你会发现这在以下几种情况下非常有用：

    * 脱机模式，即无法访问广告服务器时。

    * 引发 **ErrorOccurred** 事件时。

    * 如果你选择保存基于 [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx) 的用户带宽，**ConnectionProfile** 类中的 API 可以提供帮助。

* 如果没有合理理由，请使用默认（30 秒）超时，否则，在这种情况下，不能低于 10 秒。

    * 与横幅广告相比，视频广告实质上需要花费更长的时间来下载，尤其是在没有高速连接的市场中。


* 注意用户的流量套餐。 例如，在接近或超出其流量上限的移动设备上投放视频广告前，不要向用户显示广告，或者向用户发出警告。 [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx) 类中的 API 可以提供帮助。

* 在初始提交后持续改进你的应用。 查看广告报告并对设计进行更改以提高填充率和视频完成率。

<span id="interstitialbestpracticesavoid10"/>
### 间隙最佳做法：避免事项

* 过度间隙广告。 不要每超过约 5 分钟就强制间隙广告，除非用户除了玩游戏以外，明确对某个可选的切实好处感兴趣。

* 在应用启动时间隙视频广告，这样可能会导致用户认为单击了错误的磁贴。

* 退出时间隙视频广告。 这是很糟糕的库存，因为完成率接近于零。

    * 来回播放两个或多个视频广告。

    * 用户看到广告进度条重置到起始点时会感到非常失望。

    * 许多人会认为这只是代码编写或广告投放 Bug。

* 在调用 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 之前提取一个超过 5 分钟的视频广告。

    * 良好的库存会最大限度地将预取的广告转换为可计费的广告展示。


* 对广告投放出现故障（如没有可用的广告）的用户进行处罚。 例如，如果你显示“观看广告获取 *xxx*”的 UI 选项，那么在用户看完以后，你应当提供 *xxx*。 有两个选项可以考虑：

    * 不包括该选项，除非已引发 [AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx) 事件。

    * 使应用包含内置体验以产生与真实广告相同的效益。

* 让用户在多玩家游戏中获得竞争优势。

    * 某个第一人称射击游戏中的一把比较好的枪会明确掉落在此存储段中。

    * 玩家的虚拟形象穿的自定义衬衫很精美，只要不提供伪装就行！

<span id="interstitialbestpracticesnever10"/>
### 间隙最佳做法：禁止事项（强制执行的策略）

* 将所有 UI 元素都放在广告容器上。

    * 广告商已为全屏显示广告付款。


* 当用户在使用应用时，调用 **Show**。

    * 由于 **InterstitialAd** 将创建全屏覆盖层，用户将发现这样会产生抖动。

    * 还可能会导致过大的点击率。

* 使用广告获取任何可以作为货币消费或与其他用户交易的东西。

 

 



<!--HONumber=Aug16_HO3-->



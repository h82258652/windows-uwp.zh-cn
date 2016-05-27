---
author: mcleanbyron
ms.assetid: 54ECD653-7FC2-4A95-AC5A-972C4FB5A54B
description: 在提交你的应用之前，我们建议测试你的广告中介实现。
title: 测试广告中介实现
---

# 测试广告中介实现


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

在提交你的应用之前，我们建议测试你的广告中介实现。

## 使用测试广告网络配置值测试


在 Visual Studio 中，如果通过启动你的项目的“连接的服务”****来运行未输入广告网络配置的应用，在你的开发计算机（用于通用 Windows 平台 (UWP) 和 Windows 8.1 XAML 应用）或在模拟器或设备（用于 Windows Phone 应用）上运行你的应用时，广告中介将自动使用测试配置值。 这使你可以快速测试你的应用，并确保它是正确编码的，之后输入你的广告网络必需的参数。

广告网络将按顺序轮换，以相同的时间量逐个显示网络。 确保等待足够长的时间以完成几个周期，以便你可以查看所有广告网络，并减少可能发生的任何临时连接性问题。

将显示测试广告（针对支持它们的广告网络）。 请注意，测试广告有时看起来像错误。 确保查看你的事件以确定错误是否已发生。

> **注意** 在测试 Windows Phone Silverlight 应用时，由于 Google AdMob 不使用测试元数据，因此它将始终返回**“无效请求”**错误。 若要验证 Google AdMob 实现，你必须输入必需参数，如下一节中所述。

 

如果使用了[添加和使用广告中介控件](add-and-use-the-ad-mediator-control.md)中显示的事件处理代码，任何错误都将显示在控制台输出中。

## 使用你的广告网络配置值测试


在使用测试配置数据测试你的应用之后，你将希望使用广告网络配置值（这些值旨在用于你要发布到 Windows 应用商店的应用的版本）测试你的应用。

首先，打开**“添加连接的服务”**窗口 (Visual Studio 2015) 或**“服务管理器”**窗口 (Visual Studio 2013) 并配置每个广告网络，如[添加和使用广告中介控件](add-and-use-the-ad-mediator-control.md)中所述。 输入每个广告网络的必需参数。

现在开始准备测试你的应用。 确保运行应用足够长的时间以验证每个广告网络是否可以正常显示广告。 在提交应用前检查有无任何异常并修复编码错误。

当你将你的应用包提交到 Windows 开发人员中心仪表板时，你在 Visual Studio 中输入的配置值将自动填充到“使用广告盈利”****仪表板页面中。 你可以修改这些值以配置 Windows 应用商店中你的应用的广告网络行为。 有关详细信息，请参阅[提交应用和配置广告中介](submit-your-app-and-configure-ad-mediation.md)。

## 相关主题

* [选择和管理广告网络](select-and-manage-your-ad-networks.md)
* [添加和使用广告中介控件](add-and-use-the-ad-mediator-control.md)
* [提交应用和配置广告中介](submit-your-app-and-configure-ad-mediation.md)
* [广告中介疑难解答](troubleshoot-ad-mediation.md)
 

 


<!--HONumber=May16_HO2-->



---
author: mcleanbyron
ms.assetid: 772DEBF2-1578-4330-9C14-70BCC6F55005
description: Microsoft 提供广告中介支持，使你可以通过调解来自多个广告网络的横幅广告请求来提高应用内广告收益。
title: 使用广告中介使收益最大化
---

#  使用广告中介使收益最大化


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Microsoft 提供广告中介支持，使你可以通过调解来自多个广告网络的横幅广告请求来提高应用内广告收益。 不同的广告网络可能有其自己的优势，某些广告网络在某些市场中相较于其他广告网络具有较高的每千次浏览费用 (eCPM) 或较高的填充率（当应用发出请求时所提供的广告的百分比）。 使用单个广告网络，你最终可能得到未填充的广告请求，导致你失去潜在的收益。 广告中介通过确保始终显示动态广告来帮助你实现广告盈利的最大化。

广告中介支持通过 [Microsoft 官方商城协定和盈利 SDK](http://aka.ms/store-em-sdk) 提供。 安装了该 SDK 并向应用添加了广告中介控件后，你将可以通过在开发人员中心中更新中介配置来指定使用每个网络的频率。 你可以按市场优化它，以便在相应的地区中使用最有效的广告网络。 并且你可以对使用每个广告网络的方式进行更改，而无需重新发布你的应用。

## 广告中介使用入门


按照以下步骤，在应用中设置和配置广告中介：

1.  查看广告网络列表和广告中介支持的项目类型化、使用所需的广告网络设置帐户，并且按照每个广告网络的说明上架应用。 有关详细信息，请参阅[选择和管理广告网络](select-and-manage-your-ad-networks.md)。

2.  使用 Visual Studio 2015 或 Visual Studio 2013 安装 [Microsoft 官方商城协定和盈利 SDK](http://aka.ms/store-em-sdk)。

3.  在 Visual Studio 中打开项目或创建新项目。 打开你想要承载广告的页面，并将**“AdMediatorControl”**拖动到该页面。 如果你在使用 Microsoft Advertising，请调整该控件的高度和宽度以适应受支持的广告大小。 有关详细信息，请参阅[添加和使用广告中介控件](add-and-use-the-ad-mediator-control.md)。

4.  运行**“连接的服务”**以选择你想要面向的广告网络，并配置每个广告网络的所需参数。 有关详细信息，请参阅[添加和使用广告中介控件](add-and-use-the-ad-mediator-control.md)。

5.  在应用中测试广告中介实现。 有关详细信息，请参阅[测试广告中介实现](test-your-ad-mediation-implementation.md)。

6.  将你的应用提交到 Windows 开发人员中心仪表板，并在该仪表板上配置你的广告中介设置。 有关详细信息，请参阅[提交应用和配置广告中介](submit-your-app-and-configure-ad-mediation.md)。

7.  在开发人员中心仪表板上查看广告中介报告。 有关详细信息，请参阅[广告中介报告](https://msdn.microsoft.com/library/windows/apps/mt148521)

## 使用没有广告中介的 Microsoft Advertising


如果不想使用广告中介或项目类型当前不受广告中介支持，你仍可以在不使用广告中介的情况下提供 Microsoft 横幅广告。 有关详细信息，请参阅 [XAML 和 .NET 中的 AdControl](https://msdn.microsoft.com/library/mt313186.aspx) 以及 [HTML 5 和 JavaScript 中的 AdControl](https://msdn.microsoft.com/library/mt313130.aspx)。

## 相关主题

* [选择和管理广告网络](select-and-manage-your-ad-networks.md)
* [添加和使用广告中介控件](add-and-use-the-ad-mediator-control.md)
* [测试广告中介实现](test-your-ad-mediation-implementation.md)
* [提交应用和配置广告中介](submit-your-app-and-configure-ad-mediation.md)
* [广告中介疑难解答](troubleshoot-ad-mediation.md)
 

 


<!--HONumber=May16_HO2-->



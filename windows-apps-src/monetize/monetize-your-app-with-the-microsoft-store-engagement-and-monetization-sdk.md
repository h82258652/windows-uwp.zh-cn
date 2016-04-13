---
Description: 你可以使用 Microsoft 官方商城协定和盈利 SDK 提供的库和工具来将这些功能添加到你的应用，这可帮助你赚取更多的利益并赢得客户。
title: 使用 Microsoft 官方商城协定和盈利 SDK 来获取应用收益并吸引客户
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
---

# 使用 Microsoft 官方商城协定和盈利 SDK 来获取应用收益并吸引客户

Microsoft 官方商城协定和盈利 SDK 提供的库和工具可帮助你赚取更多的利益并赢得客户，例如在你的应用中显示广告并通过 A/B 测试运行实验。 此 SDK 将替代 Microsoft 通用广告客户端 SDK，并且它会随着时间的推移包含新的协定和盈利功能。


## SDK 中可用的功能

Microsoft 官方商城协定和盈利 SDK 提供的库和工具可支持以下功能。

### 通过 A/B 测试运行 UWP 应用的实验

在你的通用 Windows 平台 (UWP) 应用上运行 A/B 测试以为某些客户测量这些功能的有效性，之后再将它们发布给每位用户。 在你的开发人员中心仪表板中定义某个实验后，请使用 [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.engagementclient.aspx) 类在你的应用中为实验获取变体、使用该数据修改正在测试的功能的性能，然后使用 [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) 方法将视图事件和转换事件发送到开发人员中心。 最后，使用你的仪表板查看结果并管理实验。

有关详细信息，请参阅[通过 A/B 测试运行实验](run-app-experiments-with-a-b-testing.md)。

### UWP 应用的应用反馈

使用 UWP 应用中的 [Feedback](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.aspx) 类来指导“反馈中心”的 Windows 10 客户，他们可以在其中提交问题、提出建议并进行投票。 然后，在开发人员中心仪表板的[“反馈报告”](../publish/feedback-report.md)中管理此反馈。

有关详细信息，请参阅[从应用启动“反馈中心”](launch-feedback-hub-from-your-app.md)。

### 在应用中显示广告

通过在 UWP 应用以及 Windows 8.1 和 Windows Phone 8.x 应用中显示来自 Microsoft 的横幅广告或视频间隙广告增加收入。 还可以通过使用广告中介显示来自多个广告网络提供商的广告来最大化你的广告填充率。

有关详细信息，请参阅[在应用中显示广告](display-ads-in-your-app.md)。

>**注意** 以前版本的通用广告客户端 SDK、广告中介扩展和 Microsoft 广告 SDK 中的广告功能现在都包含在 Microsoft 官方商城盈利和协定 SDK 中。

### API 参考

有关 SDK 中 API 的参考文档，请参阅 [Microsoft 官方商城协定和盈利 SDK API 参考](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)。

## 安装 SDK

若要安装 Microsoft 官方商城盈利和协定 SDK：

1.  关闭 Visual Studio 2013 或 Visual Studio 2015 的所有实例，并卸载任何以前版本的通用广告客户端 SDK、广告中介扩展或 Microsoft Advertising SDK。
2.  下载和安装 [SDK](http://aka.ms/store-em-sdk)。 它可能需要几分钟来安装。 请确信并等待，直到进程结束。
3.  重新启动 Visual Studio。

Microsoft 会定期发布附带了性能改进和新功能的新版本的 Microsoft 官方商城盈利和协定 SDK。 如果你的现有项目使用的是 Microsoft 官方商城盈利和协定 SDK，并且你希望使用最新版本，只需下载和安装最新版本的 SDK 即可。

以前版本的通用广告客户端 SDK、广告中介扩展和 Microsoft 广告 SDK 中的广告功能现在都包含在 Microsoft 官方商城盈利和协定 SDK 中。 如果你的现有的 Visual Studio 2015 或 Visual Studio 2013 项目使用的是这些以前版本之一的广告功能，则你可以在安装 Microsoft 官方商城盈利和协定 SDK 后继续处理你的项目，而无需作出任何更改。

>**注意** 若要使用 Visual Studio 2015 安装 Microsoft 官方商城协定和盈利 SDK，你必须安装适用于通用 Windows 应用的 Visual Studio Tools 的版本 1.1 或更高版本。 有关适用于通用 Windows 应用的 Visual Studio Tools 的此项更新的详细信息，请参阅[发行说明](http://go.microsoft.com/fwlink/?LinkID=624516)。

## SDK 中的框架程序包

在 Microsoft 官方商城盈利和协定 SDK 中将以下库配置为*框架程序包*：

* Microsoft.Advertising.dll（仅适用于 UWP 应用项目）。 此库包含 **Microsoft.Advertising** 和 **Microsoft.Advertising.WinRT.UI** 命名空间中的广告 API。

这意味着在你的开发计算机上安装 SDK 后，无论我们何时发布附带修补程序和性能改进的新版本的库，该库都会通过 Windows 更新自动更新。 这有助于确保你始终可以在你的开发计算机上安装提供的最新版本的库。

此外，在用户安装使用此库的应用版本后，无论我们何时发布附带修补程序和性能改进的新版本的库，该库还将在其设备上自动更新。 这意味着用户将始终拥有最新版本的库，而无需将更新版本的应用发布到应用商店。

但是，如果我们发布的新版本的 SDK 引入了此库中新的 API 或功能，你将需要安装最新版本的 SDK 才能使用这些功能。 在本方案中，你还需要将更新的应用发布到应用商店。

SDK 中的其他库（包括其他目标平台的 Microsoft.Advertising.dll 和广告中介的库）当前未配置为框架库。

## 相关主题

* [通过 A/B 测试运行实验](run-app-experiments-with-a-b-testing.md)
* [Microsoft 官方商城协定和盈利 SDK API 参考](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [从应用启动“反馈中心”](launch-feedback-hub-from-your-app.md)


<!--HONumber=Mar16_HO5-->



---
author: mcleanbyron
Description: "你可以使用 Microsoft Store Services SDK 提供的库和工具将这些功能添加到你的应用，这可帮助你赚取更多的收益并吸引客户。"
title: Microsoft Store Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
translationtype: Human Translation
ms.sourcegitcommit: 011b370b7bd7ad7c7d8f60281261b6da954e2256
ms.openlocfilehash: 840a5e76d409f547d55e558262af09c8fa36a544

---

# Microsoft Store Services SDK

Microsoft Store Services SDK 提供的功能可帮助你在通用 Windows 平台 (UWP) 应用中赚取更多收益和吸引客户，例如在应用中显示广告和运行 A/B 实验。 此 SDK 是 Visual Studio 2015 和更高版本的 Visual Studio 的扩展。

## SDK 支持的方案

SDK 当前支持以下适用于 UWP 应用的方案。 此 SDK 将随时间升级，以支持新的协定和盈利方案。 有关 SDK 中 API 的参考文档，请参阅 [Microsoft Store Services SDK API 参考](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)。

|  方案  |  说明   |
|------------|----------------|
|  [在 UWP 应用中使用 A/B 测试运行实验](run-app-experiments-with-a-b-testing.md)    |  在通用 Windows 平台 (UWP) 应用上运行 A/B 测试，测量这些功能对某些客户的有效性，之后再将它们发布给每位用户。 在开发人员中心仪表板中定义某个实验后，请使用 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 类在应用中为实验获取变体、使用该数据修改正在测试的功能的性能，然后使用 [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) 方法将视图事件和转换事件发送到开发人员中心。 最后，使用仪表板查看结果并管理实验。  |
|  [从 UWP 应用启动“反馈中心”](launch-feedback-hub-from-your-app.md)    |  使用 UWP 应用中的 [StoreServicesFeedbackLauncher](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesfeedbacklauncher.aspx) 类将 Windows 10 客户定向到“反馈中心”，他们可以在其中提交问题、建议和赞成票。 然后，在开发人员中心仪表板的[反馈报告](../publish/feedback-report.md)中管理此反馈。 |
|  [配置 UWP 应用以接收开发人员中心推送通知](configure-your-app-to-receive-dev-center-notifications.md)    |  使用 UWP 应用中的 [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) 类注册应用以接收使用 Windows 开发人员中心仪表板发送给客户的定向推送通知。  |
|   [在开发人员中心中为使用情况报告记录 UWP 应用中的自定义事件](log-custom-events-for-dev-center.md)   |  使用 UWP 应用中的 [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) 类在开发人员中心中记录与应用相关联的自定义事件。 然后，在开发人员中心仪表板中的[使用情况报告](https://msdn.microsoft.com/windows/uwp/publish/usage-report)的“自定义事件”部分中查看自定义事件的总发生次数。  |
|  [在 UWP 应用中显示广告](display-ads-in-your-app.md)    |  使用 UWP 应用中的 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 或 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 控件通过显示横幅广告或间隙广告增加收入。<br/><br/>**注意**  Microsoft Store Services SDK 仅支持适用于 Windows 10 的 UWP 应用。 若要在 Windows 8.1 和 Windows Phone 8.x 应用中显示广告，请使用[适用于 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)。  |

<span id="prerequisites" />
## 先决条件

Microsoft Store Services SDK 需要：

* Visual Studio 2015 或更高版本。
* 与你的 Visual Studio 版本一起安装适用于通用 Windows 应用的 Visual Studio Tools。

>**注意**  若要将该 SDK 与 Visual Studio 2015 一起安装，必须已安装适用于通用 Windows 应用的 Visual Studio Tools 的版本 1.1 或更高版本。 有关适用于通用 Windows 应用的 Visual Studio Tools 的此项更新的详细信息，请参阅[发行说明](http://go.microsoft.com/fwlink/?LinkID=624516)。

<span id="install" />
## 安装 SDK

安装 Microsoft Store Services SDK 以便在开发计算机上与 Visual Studio 2015（或更高版本）一起使用有两个选项：

* **MSI安装程序**  可通过[此处](http://aka.ms/store-em-sdk)提供的 MSI 安装程序安装该 SDK。 使用此选项，SDK 库安装在开发计算机上的共享位置中，以便它们可供 Visual Studio 中的任何 UWP 项目引用。
* **NuGet 包**  可使用 NuGet 为 Visual Studio 中的特定 UWP 项目安装 SDK 库。 使用此选项，仅为已安装 NuGet 包的项目安装 SDK 库。

Microsoft 会定期发布带有性能改进和新功能的 Microsoft Store Services SDK 新版本。 如果你有使用该 SDK 的现有项目，并且你希望使用最新版本，请在开发计算机上下载并安装最新版本的 SDK。

>**注意**  若要将该 SDK 与 Visual Studio 2015 一起安装，必须已安装适用于通用 Windows 应用的 Visual Studio Tools 的版本 1.1 或更高版本。 有关适用于通用 Windows 应用的 Visual Studio Tools 的此项更新的详细信息，请参阅[发行说明](http://go.microsoft.com/fwlink/?LinkID=624516)。

<span id="install-msi" />
### 通过 MSI 安装

若要通过 MSI 安装程序安装 Microsoft Store Services SDK：

1.  关闭 Visual Studio 2015（或更高版本）的所有实例。 如果之前已安装 Microsoft Advertising SDK、通用广告客户端 SDK、广告中介扩展或 Microsoft 官方商城协定和盈利 SDK 的任何以前版本，请立即卸载这些 SDK 版本。

2.  打开“命令提示符”窗口并运行这些命令以清除可能与 Visual Studio 一起安装（但可能未显示在计算机上的已安装程序列表中）的任何早期广告 SDK 版本：
  ```
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.  下载并安装 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)。 安装它可能需要几分钟。 请确信并等待，直到进程结束。

4.  重新启动 Visual Studio。

5.  如果你的现有项目引用任何较早版本的 Microsoft Store Services SDK、Microsoft Advertising SDK、通用广告客户端 SDK 或 Microsoft 官方商城协定和盈利 SDK 中的库，我们建议你在 Visual Studio 中打开项目，然后清除并重新生成项目（在“解决方案资源管理器”中，右键单击项目节点并选择“清除”，然后再次右键单击项目节点并选择“重新生成”）。

  否则，如果你在项目中首次使用该 SDK，则你现在可以[将相应的 Microsoft Store Services SDK 库引用添加到项目](#references)。

<span id="install-nuget" />
### 通过 NuGet 安装

若要通过 NuGet 为特定项目安装 Microsoft Store Services SDK 库：

1.  关闭 Visual Studio 2015（或更高版本）的所有实例。 如果之前已安装 Microsoft Advertising SDK、通用广告客户端 SDK、广告中介扩展或 Microsoft 官方商城协定和盈利 SDK 的任何以前版本，请立即卸载这些 SDK 版本。

2.  打开“命令提示符”窗口并运行这些命令以清除可能与 Visual Studio 一起安装（但可能未显示在计算机上的已安装程序列表中）的任何早期广告 SDK 版本：
  ```
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.  启动 Visual Studio 并打开要使用 Microsoft Store Services SDK 库的项目。

  >**注意**  如果项目已经包含来自 SDK 的较早 MSI 安装的库引用，请从项目中删除这些引用。 这些引用的旁边将出现警告图标，因为它们引用的库已在之前的步骤中删除。

4. 在 Visual Studio 中，依次单击“项目”和“管理 NuGet 包”。

5. 在搜索框中，键入“Microsoft.Services.Store.SDK”并安装 Microsoft.Services.Store.SDK 包。

  >**注意**  如果“输出”窗口报告指示指定路径过长的 *Install-Package* 错误，则可能需要配置 NuGet 以将软件包提取到路径短于默认位置的备用位置。 若要执行此操作，将 ```repositoryPath``` 值添加到计算机上的 nuget.config 文件，并将其分配到可从中提取 NuGet 包的短文件夹路径。 有关详细信息，请参阅 NuGet 文档中的[此文章](http://docs.nuget.org/ndocs/consume-packages/configuring-nuget-behavior)。 或者，可尝试将 Visual Studio 项目移到路径较短的备用文件夹。

6. 关闭项目，然后重新打开它。

7.  如果项目已引用来自通过 NuGet 安装的较早版本 Microsoft Store Services SDK 的库，并且你已将项目更新到更新版本的 SDK，则我们建议你清除并重新生成项目（在**解决方案资源管理器**中，右键单击项目节点并选择“清除”，然后再次右键单击项目节点并选择“重新生成”）。

  否则，如果你在项目中首次使用该 SDK，则你现在可以[将相应的 Microsoft Store Services SDK 库引用添加到项目](#references)。

<span id="references" />
## 将 SDK 库引用添加到项目

通过 MSI 安装程序或 NuGet 安装 Microsoft Store Services SDK 后，请按照以下说明在 UWP 项目中引用 SDK 库。

1. 在 Visual Studio 中打开你的项目。

  >**注意**  如果项目是面向**任何 CPU** 的 JavaScript 应用，请更新项目以使用特定于体系结构的生成输出（例如，**x86**）。

2. 在“解决方案资源管理器”中，右键单击“引用”，然后选择“添加引用…”。

3. 在“引用管理器”中，展开“通用 Windows”、单击“扩展”，然后选中以下各项之一旁边的复选框。

  * 若要为客户参与方案使用 [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.aspx) 命名空间中的 API，请选中 **Microsoft 参与框架**旁边的复选框。 如果要[运行 A/B 实验](run-app-experiments-with-a-b-testing.md)、[启动反馈中心](launch-feedback-hub-from-your-app.md)、[接收来自开发人员中心的定向推动通知](configure-your-app-to-receive-dev-center-notifications.md)或[将自定义事件记录到开发人员中心](log-custom-events-for-dev-center.md)，请选择此选项。

  * 若要将这些 API 用于[在应用中显示横幅广告或间隙广告](display-ads-in-your-app.md)，请选中“Microsoft Advertising SDK for XAML”或“Microsoft Advertising SDK for JavaScript”旁边的复选框，具体取决于项目类型。

3. 单击“确定”。

>**注意**  如果已通过 NuGet 安装 SDK 库，除了 **Microsoft Advertising SDK for XAML** 或 **Microsoft Advertising SDK for JavaScript**，项目还将包含 **Microsoft.Services.Store.SDK** 引用。 **Microsoft.Services.Store.SDK** 引用表示 NuGet 包（而不是其中的库），并且你可以忽略它。

<span id="framework" />
## 了解 SDK 中的框架包

Microsoft Store Services SDK 中的以下库将配置为*框架包*：

* Microsoft.Advertising.dll。 此库包含 [Microsoft.Advertising](https://msdn.microsoft.com/en-us/library/windows/apps/mt313187.aspx) 和 [Microsoft.Advertising.WinRT.UI](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.advertising.winrt.ui.aspx) 命名空间中的广告 API。
* Microsoft.Services.Store.Engagement.dll。 此库包含 [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.services.store.engagement.aspx) 命名空间中的 API。

这意味着在用户安装使用这些库的应用版本后，每当我们发布附带修复和性能改进的新版本库时，这些库都在其设备上自动更新。 这有助于确保客户始终在其设备上安装最新可用版本的库。

如果我们发布的新版本 SDK 引入了这些库中新的 API 或功能，你将需要安装最新版本的 SDK 才能使用这些功能。 在本方案中，你还需要将更新的应用发布到应用商店。

## 相关主题

* [Microsoft Store Services SDK API 参考](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [通过 A/B 测试运行实验](run-app-experiments-with-a-b-testing.md)
* [从应用启动“反馈中心”](launch-feedback-hub-from-your-app.md)
* [配置应用以接收开发人员中心推送通知](configure-your-app-to-receive-dev-center-notifications.md)
* [记录开发人员中心的自定义事件](log-custom-events-for-dev-center.md)
* [在应用中显示广告](display-ads-in-your-app.md)



<!--HONumber=Nov16_HO1-->



---
Description: 你可以使用 Microsoft Store Services SDK 提供的库和工具将这些功能添加到你的应用，这可帮助你赚取更多的收益并吸引客户。
title: 使用 Microsoft Store Services SDK 与客户互动
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
ms.date: 08/21/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK
ms.localizationpriority: medium
ms.openlocfilehash: 679dde6802a2c0d27444fbcda040f2ba19039457
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2020
ms.locfileid: "78852791"
---
# <a name="engage-customers-with-the-microsoft-store-services-sdk"></a>使用 Microsoft Store Services SDK 与客户互动

Microsoft Store Services SDK 提供了一些功能，可帮助你与通用 Windows 平台（UWP）应用中的客户联系，例如将目标通知发送到应用，并在应用中运行 A/B 试验。 此 SDK 是 Visual Studio 2015 和更高版本的 Visual Studio 的扩展。

> [!NOTE]
> 要在 UWP 应用中显示广告，请使用 [Microsoft 广告 SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) 而不是 Microsoft Store Services SDK。 广告库已从 Microsoft Store Services SDK 移至 Microsoft 广告 SDK。 有关详细信息，请参阅[在应用中显示广告](display-ads-in-your-app.md)。



## <a name="scenarios-supported-by-the-microsoft-store-services-sdk"></a>Microsoft Store Services SDK 支持的方案

Microsoft Store Services SDK 当前支持以下适用于 UWP 应用的方案。 有关 API 的参考文档，请参阅 [Microsoft Store Services SDK API 参考](https://docs.microsoft.com/uwp/api/overview/engagement)。

|  方案  |  说明   |
|------------|----------------|
|  [在 UWP 应用中使用 A/B 测试运行试验](run-app-experiments-with-a-b-testing.md)    |  在通用 Windows 平台 (UWP) 应用上运行 A/B 测试，测量这些功能对某些客户的有效性，之后再将它们发布给每位用户。 在伙伴中心定义试验后，请使用[StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation)类在应用中获取试验的变体，使用此数据修改正在测试的功能的行为，然后使用[LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation)方法将查看事件和转换事件发送到合作伙伴中心。 最后，使用 "合作伙伴中心" 查看结果并管理试验。  |
|  [从 UWP 应用启动反馈中心](launch-feedback-hub-from-your-app.md)    |  使用 UWP 应用中的 [StoreServicesFeedbackLauncher](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) 类将 Windows 10 客户定向到“反馈中心”，他们可以在其中提交问题、建议和赞成票。 然后，在合作伙伴中心内的[反馈报告](../publish/feedback-report.md)中管理此反馈。 |
|  [配置 UWP 应用以接收合作伙伴中心推送通知](configure-your-app-to-receive-dev-center-notifications.md)    |  在 UWP 应用中使用[StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager)类来注册你的应用，以接收你使用合作伙伴中心向客户发送的目标推送通知。  |
|   [为合作伙伴中心内的使用情况报表记录 UWP 应用中的自定义事件](log-custom-events-for-dev-center.md)   |  在 UWP 应用中使用[StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)类来记录与合作伙伴中心中的应用关联的自定义事件。 然后，在 "合作伙伴中心" 中的 "[使用情况报告](https://docs.microsoft.com/windows/uwp/publish/usage-report)" 的 "**自定义事件**" 部分中查看自定义事件的总次数。  |

<span id="prerequisites" />

## <a name="prerequisites"></a>先决条件

Microsoft Store Services SDK 需要：

* Visual Studio 2015 或更高版本。
* 与你的 Visual Studio 版本一起安装适用于通用 Windows 应用的 Visual Studio Tools。

<span id="install" />

## <a name="install-the-sdk"></a>安装 SDK

在开发计算机上安装 Microsoft Store Services SDK 的方法有两种：

* **MSI 安装程序**&nbsp;&nbsp;可通过[此处](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK)提供的 MSI 安装程序安装该 SDK。
* **NuGet 包**&nbsp;&nbsp;你可以将 SDK 安装为 NuGet 包。

Microsoft 会定期发布带有性能改进和新功能的 Microsoft Store Services SDK 新版本。 如果你有使用该 SDK 的现有项目，并且你希望使用最新版本，请在开发计算机上下载并安装最新版本的 SDK。

<span id="install-msi" />

### <a name="install-via-msi"></a>通过 MSI 安装

若要通过 MSI 安装程序安装 Microsoft Store Services SDK：

1.  关闭 Visual Studio 的所有实例。

2. 如果之前已安装 Microsoft 官方商城协定和盈利 SDK、通用广告客户端 SDK 或 Ad Mediator 扩展，请先卸载这些 SDK。 （可选）打开**命令提示符**窗口并运行这些命令以清除可能与 Visual Studio 一起安装（但可能未显示在计算机上的已安装程序列表中）的任何早期 SDK 版本：
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  下载并安装 [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK)。 安装它可能需要几分钟。 请确信并等待，直到进程结束。

4.  重新启动 Visual Studio。

5.  如果你的现有项目引用任何较早版本的 Microsoft Store Services SDK、Microsoft 广告 SDK、通用广告客户端 SDK 或 Microsoft 官方商城协定和盈利 SDK 中的库，我们建议你在 Visual Studio 中打开项目，然后清除并重新生成项目（在**解决方案资源管理器**中，右键单击项目节点并选择**清除**，然后再次右键单击项目节点并选择**重新生成**）。

  如果你在项目中首次使用此 SDK，则你现在可以[为你的项目添加程序集引用](#references)。

<span id="install-nuget" />

### <a name="install-via-nuget"></a>通过 NuGet 安装

若要通过 NuGet 安装 Microsoft Store Services SDK 库：

1.  关闭 Visual Studio 的所有实例。

2. 如果之前已安装 Microsoft 官方商城协定和盈利 SDK、通用广告客户端 SDK 或 Ad Mediator 扩展，请先卸载这些 SDK。 （可选）打开**命令提示符**窗口并运行这些命令以清除可能与 Visual Studio 一起安装（但可能未显示在计算机上的已安装程序列表中）的任何早期 SDK 版本：
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  启动 Visual Studio 并打开要使用 Microsoft Store Services SDK 的项目。
    > [!NOTE]
    > 如果项目已经包含来自 SDK 的较早 MSI 安装的库引用，请从项目中删除这些引用。 这些引用的旁边将出现警告图标，因为它们引用的库已在之前的步骤中删除。

4. 在 Visual Studio 中，依次单击“项目”和“管理 NuGet 包”。

5. 在搜索框中，键入 **Microsoft.Services.Store.Engagement** 并安装 Microsoft.Services.Store.Engagement 包。 程序包安装完成后，保存你的解决方案。
    > [!NOTE]
    > 如果**输出**窗口报告指示指定路径过长的 *Install-Package* 错误，则可能需要配置 NuGet 以将软件包提取到路径短于默认位置的备用位置。 若要执行此操作，将 `repositoryPath` 值添加到计算机上的 nuget.config 文件，并将其分配到可从中提取 NuGet 包的短文件夹路径。 有关详细信息，请参阅 NuGet 文档中的[此文章](https://docs.microsoft.com/nuget/consume-packages/configuring-nuget-behavior)。 或者，可尝试将 Visual Studio 项目移到路径较短的备用文件夹。 此问题也可能是由于全局包路径过长造成的。 在这种情况下，请将 `globalPackagesFolder` 值添加到 nuget .config 文件中。

6. 关闭包含你的项目的 Visual Studio 解决方案，然后重新打开解决方案。

7.  如果项目已引用来自通过 NuGet 安装的较早版本 Microsoft Store Services SDK 的库，并且你已将项目更新到更新版本的 SDK，则我们建议你清除并重新生成项目（在**解决方案资源管理器**中，右键单击项目节点并选择**清除**，然后再次右键单击项目节点并选择**重新生成**）。

  如果你在项目中首次使用此 SDK，则你现在可以[为你的项目添加程序集引用](#references)。

<span id="references" />

## <a name="add-the-assembly-reference-to-your-project"></a>为你的项目添加程序集应用

通过 MSI 安装程序或 NuGet 安装 Microsoft Store Services SDK 后，请按照以下说明在 UWP 项目中引用 SDK 程序集。

1. 在 Visual Studio 中打开项目。
    > [!NOTE]
    > 如果项目是面向**任何 CPU**的 JavaScript 应用，请更新项目以使用特定于体系结构的生成输出（例如，**x86**）。

2. 在**解决方案资源管理器**中，右键单击**引用**，然后选择**添加引用…**

3. 在**引用管理器**中，展开**通用 Windows**、单击**扩展**，然后选中 **Microsoft 协议框架**旁边的复选框。 这使你能够使用 [Microsoft.Services.Store.Engagement](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement) 命名空间中的 API。

3. 单击“确定”。

> [!NOTE]
> 如果你已通过 NuGet 安装了 SDK 库，则你的项目将包含 **Microsoft.Services.Store.Engagement** 引用。 **Microsoft.Services.Store.Engagement** 引用表示 NuGet 包（而不是其中的库），因此，你可以将其忽略。

<span id="framework" />

## <a name="understanding-framework-packages-in-the-sdk"></a>了解 SDK 中的框架包

Microsoft Store Services SDK 中的 Microsoft.Services.Store.Engagement.dll 库配置为*框架包*。 此库包含 [Microsoft.Services.Store.Engagement](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement) 命名空间中的 API。

此库是一个框架包，因此，这意味着在用户安装使用此库的应用版本之后，无论我们何时发布新版本的库及修复和性能增强，Windows 更新均会在其设备上自动更新此库。 这有助于确保客户始终在其设备上安装最新可用版本的库。

如果我们发布的新版本 SDK 引入了此库中新的 API 或功能，你将需要安装最新版本的 SDK 才能使用这些功能。 在本方案中，你还需要将更新的应用发布到应用商店。

## <a name="related-topics"></a>相关主题

* [Microsoft Store Services SDK API 参考](https://docs.microsoft.com/uwp/api/overview/engagement)
* [通过 A/B 测试运行实验](run-app-experiments-with-a-b-testing.md)
* [从应用启动反馈中心](launch-feedback-hub-from-your-app.md)
* [配置应用以接收合作伙伴中心推送通知](configure-your-app-to-receive-dev-center-notifications.md)
* [记录合作伙伴中心的自定义事件](log-custom-events-for-dev-center.md)

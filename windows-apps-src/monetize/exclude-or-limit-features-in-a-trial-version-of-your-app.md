---
author: mcleanbyron
Description: If you enable customers to use your app for free during a trial period, you can entice your customers to upgrade to the full version of your app by excluding or limiting some features during the trial period.
title: 排除或限制试用版中的功能
ms.assetid: 1B62318F-9EF5-432A-8593-F3E095CA7056
keywords: windows 10, uwp, 试用, 应用内购买, IAP, Windows.ApplicationModel.Store
ms.author: mcleans
ms.date: 08/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: f382f6ddb2666fe546ba2180e56fb8cd028253ec
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
ms.locfileid: "1690453"
---
# <a name="exclude-or-limit-features-in-a-trial-version"></a>排除或限制试用版中的功能

如果允许客户在试用期内免费使用你的应用，则可以通过排除或限制试用期内的某些功能，吸引客户升级到完整版应用。 请在开始编码之前确定哪些功能应受到限制，然后确保你的应用只在已购买完整版许可之后才允许这些功能运作。 也可以在客户购买你的应用之前，启用仅在试用期才会出现的某些功能，如横幅或水印。

> [!IMPORTANT]
> 本文介绍如何使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空间的成员来实现试用功能。 此命名空间不再更新新功能，我们建议你使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空间。 **Windows.Services.Store** 命名空间支持最新的加载项类型（如应用商店管理的易耗型加载项和订阅），并且设计为与 Windows 开发人员中心和应用商店将来支持的产品和功能类型兼容。 **Windows.Services.Store** 命名空间在 Windows 10 版本 1607 中引入，它仅可用于面向 **Windows 10 周年纪念版（10.0；版本 14393）或 Visual Studio** 更高版本的项目中。 有关使用 **Windows.Services.Store** 命名空间实现试用功能的更多信息，请参阅[此文章](implement-a-trial-version-of-your-app.md)。

## <a name="prerequisites"></a>先决条件

可添加供客户购买的功能的 Windows 应用。

## <a name="step-1-pick-the-features-you-want-to-enable-or-disable-during-the-trial-period"></a>步骤 1：选取要在试用期内启用或禁用的功能

应用的当前许可证状态存储为 [LicenseInformation](https://msdn.microsoft.com/library/windows/apps/br225157) 类的属性。 通常，将取决于许可证状态的功能放在我们在下一步介绍的条件块中。 在考虑这些功能时，确保实现该功能的方式允许这些功能在所有许可证状态下均能正常工作。

另外，决定你希望在应用运行时如何处理对应用许可证的更改。 你的试用版可以是全功能的，但具有付费版所没有的应用内广告横幅。 或者，你的试用应用可以禁用某些功能，或定期显示消息要求用户购买应用。

考虑你正设计的应用类型，什么是适合它的试用或到期策略。 对于试用版的游戏，一个好的策略是限制用户可以玩的游戏内容量。 对于试用版的实用工具，可能需要考虑设置一个到期日期，或限制潜在购买者可以使用的功能。

对于大部分非游戏应用，设置一个过期日期很有用，因为用户可很好地理解整个应用。 以下是一些常见的过期场景和处理它们的选项。

-   **试用许可证在应用正在运行时过期。**

    如果应用正在运行时试用许可证过期，应用可以：

    -   不执行任何操作。
    -   向客户显示一条消息。
    -   关闭。
    -   提示客户购买应用。

    最佳做法是显示一条消息，提示客户购买应用，如果客户购买它，则继续启用所有功能。 如果用户决定不购买应用，则关闭它或定期提醒他们购买应用。

-   **在应用启动前试用许可证过期。**

    如果在应用启动前试用许可证过期，应用将不会启动。 相反，用户将看到一个对话框，该对话框为用户提供从应用商店购买你的应用的选项。

-   **客户在应用运行时购买它**

    如果客户在应用运行时购买它，以下是应用可执行的一些操作。

    -   不执行任何操作，让他们继续在试用模式下操作，直到重新启动应用。
    -   感谢他们购买，或者显示一条消息。
    -   静默地启用在完整许可证下可用的功能（或禁用仅限试用的通知）。

如果希望检测许可证更改并在应用中执行某种操作，你必须为此添加一个事件处理程序，如下一步中所述。

## <a name="step-2-initialize-the-license-info"></a>步骤 2：初始化许可证信息

在你的应用初始化时，获取你的应用的 [LicenseInformation](https://msdn.microsoft.com/library/windows/apps/br225157) 对象，如该示例所示。 我们假设 **licenseInformation** 是 **LicenseInformation** 类型的全局变量或字段。

现在，你将通过使用 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) 而不是 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) 获取模拟的许可证信息。 在将应用的发行版本提交到**应用商店**之前，必须将代码中的所有 **CurrentAppSimulator** 引用替换为 **CurrentApp**。

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#InitializeLicenseTest)]

接下来，添加事件处理程序，以便许可证在应用运行的情况下发生更改时接收通知。 如果在试用期到期或客户通过 Microsoft Store 购买应用等情况下，应用的许可证将发生更改。

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#InitializeLicenseTestWithEvent)]

## <a name="step-3-code-the-features-in-conditional-blocks"></a>步骤 3：将功能编码到条件块中

在引发许可证更改事件时，你的应用必须调用许可证 API 以确定试用状态是否已发生更改。 本步骤中的代码显示如何构造此事件的处理程序。 此时，如果用户购买了应用，则向用户提供许可状态已发生更改的反馈是一个好做法。 你可能需要请求用户重新启动应用（如果你已经这样编码）。 但是一定要让这一过渡尽可能无缝和轻松地进行。

此示例显示如何评估应用的许可证状态，以便你可以相应启用或禁用应用的功能。

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#ReloadLicense)]

## <a name="step-4-get-an-apps-trial-expiration-date"></a>步骤 4：获取应用的试用过期日期

包含用于确定应用的试用到期日期的代码。

此示例中的代码定义一个函数来获取应用试用许可证的过期日期。 如果许可证仍然有效，则显示到期日期及试用到期之前剩余的天数。

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#DisplayTrialVersionExpirationTime)]

## <a name="step-5-test-the-features-using-simulated-calls-to-the-license-api"></a>步骤 5：使用对许可证 API 的模拟调用测试功能

现在，使用模拟的数据测试应用。 **CurrentAppSimulator** 从称为“WindowsStoreProxy.xml”的 XML 文件（位于 %UserProfile%\\AppData\\local\\packages\\&lt;程序包名称&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData）中获取测试特定的许可信息。 你可以编辑 WindowsStoreProxy.xml 以更改你的应用及其功能的模拟过期日期。 测试所有可能的到期和许可配置，以确保任一方面都按预期运行。 有关详细信息，请参阅[将 WindowsStoreProxy.xml 文件与 CurrentAppSimulator 一起使用](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy)。

如果此路径和文件不存在，则必须在安装或运行时期间创建它们。 如果尝试在 WindowsStoreProxy.xml 未出现在特定位置的情况下访问 [CurrentAppSimulator.LicenseInformation](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.licenseinformation) 属性，则会出现错误。

## <a name="step-6-replace-the-simulated-license-api-methods-with-the-actual-api"></a>步骤 6：将模拟的许可证 API 方法替换为实际 API

在使用模拟的许可证服务器测试你的应用后，以及向应用商店提交应用进行认证前，将 **CurrentAppSimulator** 替换为 **CurrentApp**，如下一个代码示例所示。

> [!IMPORTANT]
> 在将应用提交到 Microsoft Store 时，你的应用必须使用 **CurrentApp** 对象，否则它将无法通过认证。

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#InitializeLicenseRetailWithEvent)]

## <a name="step-7-describe-how-the-free-trial-works-to-your-customers"></a>步骤 7：描述免费试用版如何为你的客户所用

请务必阐述你的应用在免费试用期间及之后的行为，以便客户不会对应用行为感到惊讶。

有关描述应用的详细信息，请参阅[创建应用提要](https://msdn.microsoft.com/library/windows/apps/mt148529)。

## <a name="related-topics"></a>相关主题

* [应用商店示例（演示试用版和应用内购买）](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [设置应用定价和可用性](https://msdn.microsoft.com/library/windows/apps/mt148548)
* [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765)
* [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766)
 

 

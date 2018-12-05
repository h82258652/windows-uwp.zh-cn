---
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: 了解如何安装 Microsoft 广告 SDK。
title: 安装 Microsoft 广告 SDK
ms.date: 08/23/2017
ms.topic: article
keywords: windows 10, uwp, 广告, 安装, SDK, 广告库
ms.localizationpriority: medium
ms.openlocfilehash: 2066d055f7abf0e9a34e245d9c6a95e14596d362
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8736357"
---
# <a name="install-the-microsoft-advertising-sdk"></a>安装 Microsoft 广告 SDK

若要在面向 Windows10 的 UWP 应用中显示广告，请安装 [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp)。 此 SDK 是 Visual Studio 2015 和更高版本的扩展。

> [!NOTE]
> 如果你要开发是 JavaScript/HTML UWP 应用，你已安装 Windows 10 SDK 版本 10.0.14393 （周年更新） 或更高版本，还必须安装[WinJS](https://github.com/winjs/winjs)库。 此库过去包含在以前版本的 Windows SDK 中，但从 Windows 10 SDK 版本 10.0.14393（周年更新）开始，此库必须单独安装。

<span id="install-msi" />

## <a name="install-via-msi"></a>通过 MSI 安装

若要通过 MSI 安装程序安装 Microsoft 广告 SDK：

1.  关闭 Visual Studio 的所有实例。

2. 如果之前已安装 Microsoft Advertising SDK、通用广告客户端 SDK、广告中介扩展或 Microsoft 官方商城协定和盈利 SDK 的任何以前版本，请立即卸载这些 SDK 版本。 （可选）打开**命令提示符**窗口并运行这些命令以清除可能与 Visual Studio 一起安装（但可能未显示在计算机上的已安装程序列表中）的任何早期广告 SDK 版本：
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  下载并安装 [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp)。 安装它可能需要几分钟。 请确信并等待，直到进程结束。

4.  重新启动 Visual Studio。

5.  如果你的现有项目引用了任何较早版本的 Microsoft 广告 SDK、通用广告客户端 SDK 或 Microsoft 官方商城协定和盈利 SDK 中的广告库，我们建议你在 Visual Studio 中打开项目，然后清除并重新生成项目（在**解决方案资源管理器**中，右键单击项目节点并选择**清除**，然后再次右键单击项目节点并选择**重新生成**）。

  如果你在项目中首次使用 Microsoft 广告 SDK，则你现在可以[添加对 Microsoft 广告 SDK 的引用](#reference)。

<span id="install-nuget" />

## <a name="install-via-nuget"></a>通过 NuGet 安装

若要通过 NuGet 为特定 UWP 项目安装 Microsoft 广告 SDK：

1.  关闭 Visual Studio 的所有实例。

2.  如果之前已安装 Microsoft Advertising SDK、通用广告客户端 SDK、广告中介扩展或 Microsoft 官方商城协定和盈利 SDK 的任何以前版本，请立即卸载这些 SDK 版本。 （可选）打开**命令提示符**窗口并运行这些命令以清除可能与 Visual Studio 一起安装（但可能未显示在计算机上的已安装程序列表中）的任何早期广告 SDK 版本：
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  启动 Visual Studio 并打开要使用 Microsoft 广告 SDK 的项目。
    > [!NOTE]
    > 如果项目已经包含来自 SDK 的较早 MSI 安装的库引用，请从项目中删除这些引用。 这些引用的旁边将出现警告图标，因为它们引用的库已在之前的步骤中删除。

4. 在 Visual Studio 中，依次单击**项目**和**管理 NuGet 包**。

5. 在搜索框中，键入 **Microsoft.Advertising.XAML**（对于 XAML 项目）或 **Microsoft.Advertising.JS**（对于 JavaScript/HTML 项目）并安装相应程序包。 程序包安装完成后，保存你的解决方案。
    > [!NOTE]
    > 如果**输出**窗口报告指示指定路径过长的 *Install-Package* 错误，则可能需要配置 NuGet 以将软件包提取到路径短于默认位置的备用位置。 若要执行此操作，将 ```repositoryPath``` 值添加到计算机上的 nuget.config 文件，并将其分配到可从中提取 NuGet 包的短文件夹路径。 有关详细信息，请参阅 NuGet 文档中的[此文章](http://docs.nuget.org/ndocs/consume-packages/configuring-nuget-behavior)。 或者，可尝试将 Visual Studio 项目移到路径较短的备用文件夹。

6. 关闭解决方案，然后重新打开它。

7.  如果项目已引用来自通过 NuGet 安装的较早版本 Microsoft 广告 SDK 的库，并且你已将项目更新到更新版本的 SDK，则我们建议你清除并重新生成项目（在**解决方案资源管理器**中，右键单击项目节点并选择**清除**，然后再次右键单击项目节点并选择**重新生成**）。

  如果你在项目中首次使用此 SDK，则你现在可以[添加对 Microsoft 广告 SDK 的引用](#reference)。

<span id="reference" />

## <a name="add-a-reference-to-the-microsoft-advertising-sdk"></a>添加对 Microsoft 广告 SDK 的引用

安装 Microsoft 广告 SDK 后，请按照以下说明在你的项目中引用此 SDK，以使用广告 API。

1. 在 Visual Studio 中打开你的项目。
    > [!NOTE]
    > 如果你的项目面向**任何 CPU**，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 如果你的项目面向**任何 CPU**，你将无法在以下步骤中成功添加对 Microsoft 广告 SDK 的引用。 有关详细信息，请参阅[项目中由面向任何 CPU 引起的引用错误](known-issues-for-the-advertising-libraries.md#reference_errors)。

2. 在**解决方案资源管理器**中，右键单击**引用**，然后选择**添加引用…**

3. 在**引用管理器R**中，展开**通用 Windows**，单击**扩展**，然后选择**适用于 XAML 的 Microsoft 广告 SDK**（适用于 XAML 应用）或**适用于 JavaScript 的 Microsoft 广告 SDK**（适用于使用 JavaScript 和 HTML 构建的应用）。

4.  在**引用管理器**中，单击“确定”。

有关显示如何开始使用广告 API 的操作实例，请参阅以下文章：

* [间隙广告](interstitial-ads.md)
* [本机广告](native-ads.md)
* [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)

<span id="framework" />

## <a name="understanding-framework-packages-in-the-microsoft-advertising-sdk"></a>了解 Microsoft 广告 SDK 中的框架包

适用于 UWP 应用的 [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp) 中的 Microsoft.Advertising.dll 库已配置为*框架包*。 此库包含 [Microsoft.Advertising](https://docs.microsoft.com/uwp/api/microsoft.advertising) 和 [Microsoft.Advertising.WinRT.UI](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui) 命名空间中的广告 API。

此库是一个框架包，因此，这意味着在用户安装使用此库的应用版本之后，无论我们何时发布新版本的库及修复和性能增强，Windows 更新均会在其设备上自动更新此库。 这有助于确保客户始终在其设备上安装最新可用版本的库。

如果我们发布的新版本 SDK 引入了此库中新的 API 或功能，你将需要安装最新版本的 SDK 才能使用这些功能。 在本方案中，你还需要将更新的应用发布到应用商店。

---
description: 了解如何更新应用以使用最新的受支持 Microsoft 广告库，并确保应用继续收到横幅广告。
title: 将应用更新到最新的横幅广告库
ms.date: 08/23/2017
ms.topic: article
keywords: windows 10, uwp, 广告, AdControl, AdMediatorControl, 迁移
ms.assetid: f8d5b2ad-fcdb-4891-bd68-39eeabdf799c
ms.localizationpriority: medium
ms.openlocfilehash: adac5cfdb1b4a10674fb7173e5b84a86b509f130
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8794596"
---
# <a name="update-your-app-to-the-latest-advertising-libraries-for-banner-ads"></a>将应用更新到最新的横幅广告库

截止 2017 年 4 月 1 日，我们不再为使用不受支持的广告 SDK 版本的应用提供横幅广告。 如果你使用 **AdControl** 在通用 Windows 平台 (UWP) 应用中显示横幅广告，请使用本文中的信息来确定你使用的是否是不受支持的广告 SDK 并将应用迁移到受支持 SDK。

## <a name="overview"></a>概述

显示横幅广告的 UWP 应用必须使用 **AdControl**，它位于 [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp) 中分布的广告库中。 此 SDK 支持最低广告功能集，包括通过互动广告局 (IAB) 中的[移动富媒体广告界面定义 (MRAID) 1.0 规范](http://www.iab.com/wp-content/uploads/2015/08/IAB_MRAID_VersionOne.pdf)提供 HTML5 富媒体的功能。 我们的许多广告厂商寻求这些功能，并且我们要求应用开发人员使用这些 SDK 版本之一来增加我们的应用生态系统对广告厂商的吸引力，并最终为你带来更多收益。

在发布此 SDK 之前，我们先前已在多个较早的广告 SDK 版本中提供了 **AdControl** 类。 这些较早的广告 SDK 版本不再受支持，因为它们不支持以上所述的最低广告功能。 截止 2017 年 4 月 1 日，我们不再为使用不受支持的广告 SDK 版本的应用提供横幅广告。 如果你的应用仍使用的是不受支持的广告 SDK 版本，则会看到以下行为：

* 将不再向应用中的任何 **AdControl** 提供横幅广告，并且你将不再从这些控件中获取任何广告收益。

* 当应用中的 **AdControl** 请求新广告时，将引发控件的 **ErrorOccurred** 事件，并且事件参数的 **ErrorCode** 属性将具有值 **NoAdAvailable**。

* 与你的应用关联的任何广告单元均会停用。 你无法从 DePartnerv 中心帐户中删除这些已停用的广告单元。 如果你已将应用更新为使用 [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp)，请忽略这些广告单元并创建新的广告单元。

* 此外，不再为任何用于多个应用的广告单元提供横幅广告。 请确保你的每个广告单元只用于一个应用中。

如果现有应用（已存在于 Microsoft Store 中或仍在开发中）使用 **AdControl** 显示横幅广告并且你不确定你的应用正在使用的是哪个广告 SDK，请按照文章中的说明确定你是否需要将应用更新为受支持的 SDK。 如果你遇到任何问题或需要帮助，请[联系支持人员](http://go.microsoft.com/fwlink/?LinkId=393643)。

> [!NOTE]
> 如果你的应用已使用 [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp)（适用于 UWP 应用），则无需对应用进行任何更改。

## <a name="prerequisites"></a>先决条件

* 使用 **AdControl** 的应用的完整源代码和 Visual Studio 项目文件。
* 应用的 .appx 包。

> [!NOTE]
> 如果你不再有应用的 .appx 包，但仍然有包含曾用于生成应用的 Visual Studio 和广告 SDK 版本的开发计算机，则可以在 Visual Studio 中重新生成 .appx 包。

<span id="part-1" />

## <a name="part-1-determine-whether-you-need-to-update-your-uwp-app"></a>第 1 部分：确定是否需要更新你的 UWP 应用

按照以下部分中的说明确定是否需要更新应用。

1. 创建应用的 .appx 包副本以便不干扰原始软件包包、重命名该副本使其具有 .zip 扩展名，然后提取文件内容。

2. 检查应用包的提取内容：
  * 如果看到 Microsoft.Advertising.dll 文件，则表示应用使用早期的 SDK，必须按照下面部分中的说明更新项目。 继续到[第 2 部分](update-your-app-to-the-latest-advertising-libraries.md#part-2)。
  * 如果未看到 Microsoft.Advertising.dll 文件，则表示 UWP 应用已经使用最新的可用广告 SDK，并且你无需对项目进行任何更改。


<span id="part-2" />

## <a name="part-2-install-the-latest-sdk"></a>第 2 部分：安装最新的 SDK

如果应用使用早期的 SDK 版本，请按照以下说明确保你的开发计算机上具有最新的 SDK。

1. 确保你的开发计算机已安装 Visual Studio 2015 或更高版本。
    > [!NOTE]
    > 如果 Visual Studio 在开发计算机上处于打开状态，请在执行以下步骤前关闭它。

1.  从开发计算机中卸载 Microsoft Advertising SDK 和广告中介 SDK 的所有以前版本。

2.  打开**命令提示符**窗口并运行这些命令以清除可能与 Visual Studio 一起安装（但可能未显示在计算机上的已安装程序列表中）的任何 SDK 版本：
    ```syntax
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  安装 [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp)。

## <a name="part-3-update-your-project"></a>第 3 部分：更新项目

请从项目中删除对 Microsoft 广告库的所有现有引用，并按照[这些说明](install-the-microsoft-advertising-libraries.md#reference)添加所需的引用。 这将确保项目使用正确的库。 可保留现有标记和代码。

## <a name="part-4-test-and-republish-your-app"></a>第 4 部分：测试和重新发布应用

测试应用以确保它按预期显示横幅广告。

如果你的应用的以前版本已在应用商店中，为在合作伙伴中心，以重新发布你的应用更新的应用[创建新提交](../publish/app-submissions.md)中可用。

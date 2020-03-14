---
Description: 了解如何向你的客户提供应用的程序包，以及如何管理特定的程序包方案。
title: 应用包管理指南
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9f5caa2610e19234cfd83119d570f858c540b401
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210193"
---
# <a name="guidance-for-app-package-management"></a>应用包管理指南

了解如何向你的客户提供应用的程序包，以及如何管理特定的程序包方案。

-   [操作系统版本和包分发](#os-versions-and-package-distribution)
-   [将 Windows 10 包添加到以前发布的应用](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [从应用商店中删除应用](#removing-an-app-from-the-store)
-   [删除以前支持的设备系列的包](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>操作系统版本和程序包分发

不同的操作系统可以运行不同类型的程序包。 如果有多个程序包可在客户的设备上运行，则 Microsoft Store 将提供最佳的可用匹配。

通常来说，较高的操作系统版本可运行适用于相同设备系列的面向以前操作系统版本的程序包。 Windows 10 设备可以运行所有以前受支持的操作系统版本（每个设备家族）。 Windows 10 桌面版设备可以运行为 Windows 8.1 或 Windows 8 构建的应用程序;Windows 10 移动版设备可以运行为 Windows Phone 8.1、Windows Phone 8 甚至 Windows Phone 7、windows 构建的应用程序。 但是，如果应用不包括面向适用设备系列的 UWP 包，则 Windows 10 上的客户将只获得这些包。

> [!IMPORTANT]
> 从2018年10月31日起，新创建的产品不能包含面向 Windows 8.x/Windows Phone 3.x 或更早版本的包。 有关详细信息，请参阅此[博客文章](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)。


## <a name="removing-an-app-from-the-store"></a>从应用商店中删除应用

有时，你可能想要停止向客户提供应用，即有效地“取消发布”它。 若要执行此操作，请从**应用概述**页上单击**禁止提供应用**。 在你对禁止提供的应用进行确认后，数小时内它不会再出现在 Microsoft Store 中，并且新客户将无法获取它（除非他们拥有[促销代码](generate-promotional-codes.md)并使用 Windows 10 设备）。

> [!IMPORTANT]
> 此选项将覆盖你在提交中选定的任何[可见性](choose-visibility-options.md#discoverability)设置。 

此选项与你创建提交并通过**停止获取**选项选择**使此产品在 Microsoft Store 中可用，但不可被发现**具有相同效果。 但是，它不需要你创建新提交。

请注意，任何已拥有该应用的客户仍然可以使用并重新下载它（如果你以后提交新程序包，甚至还可以获取更新）。

应用程序不可用后，仍会在合作伙伴中心看到它。 如果决定将应用重新提供给客户，你可以从“应用概述”页中单击“提供应用”。 确认后，应用将在数小时内提供给新客户（除非受上一次提交中的设置的限制）。

> [!NOTE]
> 如果你想要保持应用可用，但又不想继续将它提供给使用特定操作系统版本的新用户，你可以创建新提交，并删除要阻止新获取的操作系统版本的所有程序包。 例如，如果你以前有 Windows Phone 8.1 和 Windows 10 的包，并且不希望为 Windows Phone 8.1 上的新客户提供此应用，请从提交中删除所有 Windows Phone 8.1 包。 发布更新后，Windows Phone 8.1 上的新客户将无法获取该应用程序，但已有的客户可以继续使用该应用。 但是，应用仍可用于 Windows 10 上的新客户。


## <a name="removing-packages-for-a-previously-supported-device-family"></a>删除以前受支持的设备系列中的程序包

如果你删除了应用以前支持的某个[设备系列](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)的所有包，系统将提示你确认这是你的意图，然后才能在 "**包**" 页上保存更改。

如果你发布的提交将删除可以在你的应用之前支持的设备系列上运行的所有包，新客户将无法在该设备系列上获取该应用。 稍后您可以始终发布其他更新以为该设备系列重新提供程序包。

请注意，即使您删除支持某些设备系列的所有程序包，已在该设备类型上安装应用的任何现有客户仍可以使用它，并且他们将获取您以后提供的任何更新。


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>将 Windows 10 包添加到以前发布的应用

如果你的应用商店中的应用仅包含 Windows 8.x 和/或 Windows Phone 3.x 的程序包，并且你想要更新适用于 Windows 10 的应用，请在 "[包](upload-app-packages.md)" 步骤中创建新的提交并添加 msixupload 或 .appxupload 包。 在应用完成认证过程之后，UWP 包还将可供客户在 Windows 10 上进行新的收购。

> [!NOTE]
> 在 Windows 10 上的客户获取 UWP 包后，就不能将该客户恢复为使用适用于任何以前操作系统版本的包。 

请注意，Windows 10 包的版本号必须高于你使用的任何 Windows 8、Windows 8.1 和/或 Windows Phone 8.1 包的版本号。 有关详细信息，请参阅[程序包版本编号](package-version-numbering.md)。

有关如何包装 UWP 应用以上架 Microsoft Store 的详细信息，请参阅[包装应用](../packaging/index.md)。

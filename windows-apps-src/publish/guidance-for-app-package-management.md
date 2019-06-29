---
Description: 了解如何向你的客户提供应用的程序包，以及如何管理特定的程序包方案。
title: 应用包管理指南
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f514177ad5de7774e6926165435fd3b2d7b5e1f7
ms.sourcegitcommit: 4aef8c01ba9321401d5729a1ec6d46452ee76faf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/29/2019
ms.locfileid: "67468939"
---
# <a name="guidance-for-app-package-management"></a>应用包管理指南

了解如何向你的客户提供应用的程序包，以及如何管理特定的程序包方案。

-   [操作系统版本和包分发](#os-versions-and-package-distribution)
-   [适用于 Windows 10 的包添加到以前发布的应用程序](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [适用于 Windows Phone 8.1 维护程序包兼容性](#maintaining-package-compatibility-for-windows-phone-81)
-   [删除从应用商店应用程序](#removing-an-app-from-the-store)
-   [删除以前支持的设备系列的包](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>操作系统版本和程序包分发

不同的操作系统可以运行不同类型的程序包。 如果有多个程序包可在客户的设备上运行，则 Microsoft Store 将提供最佳的可用匹配。

通常来说，较高的操作系统版本可运行适用于相同设备系列的面向以前操作系统版本的程序包。 Windows 10 设备可以运行所有以前支持的 OS 版本 （每个设备系列）。 Windows 10 桌面版设备可以运行 Windows 8.1 或 Windows 8; 生成的应用程序Windows 10 移动设备可以运行的应用程序生成为 Windows Phone 8.1 和 Windows Phone 8，甚至 Windows Phone 7.x。 但是，Windows 10 上的客户将仅获得这些包，如果应用程序不包括 UWP 包以适用的设备系列为目标。

> [!IMPORTANT]
> 自 2018 年 10 月 31 日起，新创建的产品不能包括面向 Windows 8.x/Windows 包 Phone 8.x 或更早版本。 有关详细信息，请参阅此[博客文章](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)。


## <a name="removing-an-app-from-the-store"></a>从应用商店中删除应用

有时，你可能想要停止向客户提供应用，即有效地“取消发布”它。 若要执行此操作，请从**应用概述**页上单击**禁止提供应用**。 在你对禁止提供的应用进行确认后，数小时内它不会再出现在 Microsoft Store 中，并且新客户将无法获取它（除非他们拥有[促销代码](generate-promotional-codes.md)并使用 Windows 10 设备）。

> [!IMPORTANT]
> 此选项将覆盖你在提交中选定的任何[可见性](choose-visibility-options.md#discoverability)设置。 

此选项与你创建提交并通过**停止获取**选项选择**使此产品在 Microsoft Store 中可用，但不可被发现**具有相同效果。 但是，它不需要你创建新提交。

请注意，任何已拥有该应用的客户仍然可以使用并重新下载它（如果你以后提交新程序包，甚至还可以获取更新）。

使应用程序不可用之后, 您将仍看到它在合作伙伴中心。 如果决定将应用重新提供给客户，你可以从“应用概述”页中单击“提供应用”  。 确认后，应用将在数小时内提供给新客户（除非受上一次提交中的设置的限制）。

> [!NOTE]
> 如果你想要保持应用可用，但又不想继续将它提供给使用特定操作系统版本的新用户，你可以创建新提交，并删除要阻止新获取的操作系统版本的所有程序包。 例如，如果不想要保留到 Windows Phone 8.1 上的新客户提供应用适用于 Windows Phone 8.1 和 Windows 10 上有包，删除所有的 Windows Phone 8.1 包从提交。 发布此更新后，Windows Phone 8.1 上的任何新客户将不能够获取该应用程序，尽管已有的客户可以继续使用它）。 但是，应用程序仍将适用于 Windows 10 上的新客户。


## <a name="removing-packages-for-a-previously-supported-device-family"></a>删除以前受支持的设备系列中的程序包

如果你删除所有包的某一特定[设备系列](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)，您的应用程序以前支持，系统将提示你确认这是您的意图，然后才能保存所做的更改上**包**页。

在发布中移除所有可以在您的应用程序以前支持的设备系列运行的包的提交时，新客户不能获取该设备系列上的应用程序。 稍后您可以始终发布其他更新以为该设备系列重新提供程序包。

请注意，即使您删除支持某些设备系列的所有程序包，已在该设备类型上安装应用的任何现有客户仍可以使用它，并且他们将获取您以后提供的任何更新。


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>适用于 Windows 10 的包添加到以前发布的应用程序

如果已有的应用的 Windows，仅包含包的存储区中 8.x 和/或 Windows Phone 8.x 以及你想要更新适用于 Windows 10 的应用程序、 创建新的提交并添加期间你 UWP.msixupload 或.appxupload 包[包](upload-app-packages.md)步骤。 您的应用程序都要通过认证过程后，UWP 包也将适用于 Windows 10 上的客户的新收购。

> [!NOTE]
> Windows 10 上的客户获得 UWP 包之后, 不能将该客户鼠标移回使用包的任何以前的操作系统版本。 

请注意，Windows 10 包的版本号必须高于这些已使用任何 Windows 8/Windows 8.1 或 Windows Phone 8.1 的包。 有关详细信息，请参阅[程序包版本编号](package-version-numbering.md)。

有关如何包装 UWP 应用以上架 Microsoft Store 的详细信息，请参阅[包装应用](../packaging/index.md)。

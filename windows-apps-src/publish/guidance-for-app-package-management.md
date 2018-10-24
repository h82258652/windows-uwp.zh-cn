---
author: jnHs
Description: Learn how your app's packages are made available to your customers, and how to manage specific package scenarios.
title: 应用包管理指南
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.author: wdg-dev-content
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a43f3b4c5684d93ea6986c4d1f1e4dae46c1a959
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5475883"
---
# <a name="guidance-for-app-package-management"></a>应用包管理指南

了解如何向你的客户提供应用的程序包，以及如何管理特定的程序包方案。

-   [操作系统版本和程序包分发](#os-versions-and-package-distribution)
-   [将适用于 Windows 10 的程序包添加到以前发布的应用](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [使程序包维持对 Windows Phone 8.1 的兼容性](#maintaining-package-compatibility-for-windows-phone-81)
-   [从应用商店中删除应用](#removing-an-app-from-the-store)
-   [删除以前受支持的设备系列中的程序包](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>操作系统版本和程序包分发

不同的操作系统可以运行不同类型的程序包。 如果有多个程序包可在客户的设备上运行，则 Microsoft Store 将提供最佳的可用匹配。

通常来说，较高的操作系统版本可运行适用于相同设备系列的面向以前操作系统版本的程序包。 但是，客户将只获取这些程序包，如果该应用不包含面向其当前的操作系统版本的程序包。

例如，windows 10 设备可以运行所有以前受支持的操作系统版本 （每个设备系列）。 Windows 10 桌面设备可以运行 Windows8.1 或 Windows8; 生成的应用Windows 10 移动设备可以运行为 Windows Phone 8.1，WindowsPhone8，甚至是 Windows Phone 7.x。 

以下示例说明一个应用的各种方案，该应用包含面向不同操作系统版本的程序包（除非程序包的特定约束不允许它们在每个操作系统版本/此处所列的设备类型上运行；例如程序包的架构必须适用于设备）。 

### <a name="example-app-1"></a>应用示例 1

| 程序包的目标操作系统 | 将获得此程序包的操作系统 |
|-------------------------------------|----------------------------------------------|
| Windows8.1                         | Windows 10 桌面设备、 Windows8.1      |
| Windows Phone 8.1                   | Windows 10 移动设备、 Windows Phone 8.1 |
| WindowsPhone8                     | WindowsPhone8                              |
| Windows Phone 7.1                   | Windows Phone 7.x                            |

在示例应用 1 中，应用尚不具有专门为 windows 10 设备生成的通用 Windows 平台 (UWP) 程序包，但在 windows 10 上的客户仍然可以获得该应用。 这些客户将获得为其设备类型提供的最佳程序包。

### <a name="example-app-2"></a>应用示例 2

| 程序包的目标操作系统  | 将获得此程序包的操作系统 |
|--------------------------------------|----------------------------------------------|
| Windows 10 （通用设备系列） | Windows 10 （所有设备系列）             |
| Windows8.1                          | Windows8.1                                  |
| Windows Phone 8.1                    | Windows Phone 8.1                            |
| Windows Phone 7.1                    | Windows Phone 7.x、 WindowsPhone8           |

在示例应用 2 中，没有可在 Windows8 上运行的程序包。 使用任何其他操作系统版本的客户都可获得该应用。 Windows 10 上的所有客户均会获得相同程序包。

### <a name="example-app-3"></a>示例应用 3

| 程序包的目标操作系统 | 将获得此程序包的操作系统                  |
|-------------------------------------|---------------------------------------------------------------|
| Windows 10 （桌面设备系列）  | Windows 10 桌面版设备                                    |
| WindowsPhone8                     | Windows 10 移动设备、 WindowsPhone8，Windows Phone 8.1 |

在示例应用 3，因为没有 UWP 程序包面向移动设备系列，在 windows 10 移动设备上的客户将获得 WindowsPhone8 程序包。 如果此应用以后添加面向移动设备系列 （或通用设备系列） 的程序包，该程序包将提供给 windows 10 移动设备，而不是 WindowsPhone8 程序包的客户。

另请注意，此示例应用不包括可在 Windows Phone 7.x 上运行的任何程序包。

### <a name="example-app-4"></a>应用示例 4

| 程序包的目标操作系统  | 将获得此程序包的操作系统 |
|--------------------------------------|----------------------------------------------|
| Windows 10 （通用设备系列） | Windows 10 （所有设备系列）             |

在示例应用 4，任何运行 windows 10 的设备可以获得该应用，但它不会向使用任何以前操作系统版本的客户可用。 由于 UWP 程序包面向通用设备系列，它会提供给任何 windows 10 设备 （每个[设备系列可用性选择](device-family-availability.md)）。


## <a name="removing-an-app-from-the-store"></a>从 Microsoft Store 中删除应用

有时，你可能想要停止向客户提供应用，即有效地“取消发布”它。 若要执行此操作，请从**应用概述**页上单击**禁止提供应用**。 在你对禁止提供的应用进行确认后，数小时内它不会再出现在 Microsoft Store 中，并且新客户将无法获取它（除非他们拥有[促销代码](generate-promotional-codes.md)并使用 Windows 10 设备）。

> [!IMPORTANT]
> 此选项将覆盖你在提交中选定的任何[可见性](choose-visibility-options.md#discoverability)设置。 

此选项与你创建提交并通过**停止获取**选项选择**使此产品在 Microsoft Store 中可用，但不可被发现**具有相同效果。 但是，它不需要你创建新提交。

请注意，任何已拥有该应用的客户仍然可以使用并重新下载它（如果你以后提交新程序包，甚至还可以获取更新）。

在禁止提供应用后，你仍然可以在自己的仪表板中看到它。 如果决定将应用重新提供给客户，你可以从“应用概述”页中单击“提供应用”****。 确认后，应用将在数小时内提供给新客户（除非受上一次提交中的设置的限制）。

> [!NOTE]
> 如果你想要保持应用可用，但又不想继续将它提供给使用特定操作系统版本的新用户，你可以创建新提交，并删除要阻止新获取的操作系统版本的所有程序包。 例如，如果你先前有包用于 Windows Phone 8.1 和 windows 10，并且你不希望应用继续提供给新客户 WindowsPhone8.1，删除所有 WindowsPhone8.1 程序包从提交。 发布更新后，WindowsPhone8.1 任何新客户将不能够获取该应用，尽管已拥有它的客户可以继续使用它）。 但是，应用将仍然可供 windows 10 的新客户。


## <a name="removing-packages-for-a-previously-supported-device-family"></a>删除以前受支持的设备系列中的程序包

如果你删除的某些[设备系列](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)的支持你的应用之前，将提示您确认，这是您可以在**程序包**页面上保存更改之前的所有程序包。

当发布的提交，删除所有可在你的应用以前支持的设备系列运行的程序包时，新客户将无法获取该设备系列上的应用。 稍后你可以始终发布其他更新以为该设备系列重新提供程序包。

请注意，即使你删除支持某些设备系列的所有程序包，已在该设备类型上安装应用的任何现有客户仍可以使用它，并且他们将获取你以后提供的任何更新。


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>将 windows 10 程序包添加到以前发布的应用

如果你有一个应用中仅包括包在 Windows 应用商店 8.x 和/或 Windows Phone 8.x 的应用，并希望为 windows 10 更新你的应用、 创建新提交并在[程序包](upload-app-packages.md)阶段添加 UWP.msixupload 或.appxupload 程序包。 你的应用完成认证过程后，UWP 程序包也可用于新的购置，通过使用 windows 10 的客户。

> [!NOTE]
> 后 windows 10 的客户获取 UWP 程序包，你不能倾斜使客户回退到使用任何以前操作系统版本的程序包。 

请注意，windows 10 程序包的版本号必须始终高于那些你使用了任何 Windows8、 Windows8.1，和/或 Windows Phone 8.1 程序包。 有关详细信息，请参阅[程序包版本编号](package-version-numbering.md)。

有关如何包装 UWP 应用以上架 Microsoft Store 的详细信息，请参阅[包装应用](../packaging/index.md)。

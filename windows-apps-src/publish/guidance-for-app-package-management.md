---
author: jnHs
Description: Learn how your app's packages are made available to your customers, and how to manage specific package scenarios.
title: 应用包管理指南
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.author: wdg-dev-content
ms.date: 03/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9b0b6315b1177138c3ede7834e2dbc792ee106dd
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2018
ms.locfileid: "2820075"
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

通常来说，较高的操作系统版本可运行适用于相同设备系列的面向以前操作系统版本的程序包。 但是，客户将只获得这些程序包，如果应用程序不包括针对其当前的操作系统版本包。

例如，Windows 10 设备可以运行所有以前的受支持操作系统版本（每个设备系列）。 Windows 10 桌面设备可以运行为 Windows 8.1 或 Windows 8 生成的应用；Windows 10 移动设备可以运行为 Windows Phone 8.1、Windows Phone 8 甚至 Windows Phone 7.x 生成的应用。 

以下示例说明一个应用的各种方案，该应用包含面向不同操作系统版本的程序包（除非程序包的特定约束不允许它们在每个操作系统版本/此处所列的设备类型上运行；例如程序包的架构必须适用于设备）。 

### <a name="example-app-1"></a>应用示例 1

| 程序包的目标操作系统 | 将获得此程序包的操作系统 |
|-------------------------------------|----------------------------------------------|
| Windows 8.1                         | Windows 10 桌面设备、Windows 8.1      |
| Windows Phone 8.1                   | Windows 10 移动设备，Windows Phone 8.1 |
| Windows Phone 8                     | Windows Phone 8                              |
| Windows Phone 7.1                   | Windows Phone 7.x                            |

在示例应用 1 中，应用尚不具有专门为 Windows 10 设备生成的通用 Windows 平台 (UWP) 程序包，但使用 Windows 10 的客户仍然可以获得该应用。 这些客户将获得为其设备类型提供的最佳程序包。

### <a name="example-app-2"></a>应用示例 2

| 程序包的目标操作系统  | 将获得此程序包的操作系统 |
|--------------------------------------|----------------------------------------------|
| Windows 10（通用设备系列） | Windows 10（所有设备系列）             |
| Windows 8.1                          | Windows 8.1                                  |
| Windows Phone 8.1                    | Windows Phone 8.1                            |
| Windows Phone 7.1                    | Windows Phone 7.x，Windows Phone 8           |

在示例应用 2 中，没有可在 Windows 8 上运行的程序包。 使用任何其他操作系统版本的客户都可获得该应用。 Windows 10 上的所有客户均会获得相同程序包。

### <a name="example-app-3"></a>示例应用 3

| 程序包的目标操作系统 | 将获得此程序包的操作系统                  |
|-------------------------------------|---------------------------------------------------------------|
| Windows 10（桌面设备系列）  | Windows 10 桌面设备                                    |
| Windows Phone 8                     | Windows 10 移动设备、Windows Phone 8、Windows Phone 8.1 |

在示例应用 3 中，由于没有面向移动设备系列的 UWP 程序包，因此使用 Windows 10 移动设备的客户将获得 Windows Phone 8 程序包。 如果此应用以后添加面向移动设备系列（或通用设备系列）的程序包，则此时将向使用 Windows 10 移动设备的客户提供这些程序包，而不是 Windows Phone 8 程序包。

另请注意，此示例应用不包括可在 Windows Phone 7.x 上运行的任何程序包。

### <a name="example-app-4"></a>应用示例 4

| 程序包的目标操作系统  | 将获得此程序包的操作系统 |
|--------------------------------------|----------------------------------------------|
| Windows 10（通用设备系列） | Windows 10（所有设备系列）             |

在示例应用 4 中，运行 Windows 10 的任何设备都可获得该应用，但不会向使用任何以前操作系统版本的客户提供该应用。 因为 UWP 包目标通用设备系列，它将可供任何 Windows 10 设备 （每个您的[设备系列可用性选择](device-family-availability.md)）。


## <a name="removing-an-app-from-the-store"></a>从 Microsoft Store 中删除应用

有时，你可能想要停止向客户提供应用，即有效地“取消发布”它。 若要执行此操作，请从**应用概述**页上单击**禁止提供应用**。 在你对禁止提供的应用进行确认后，数小时内它不会再出现在 Microsoft Store 中，并且新客户将无法获取它（除非他们拥有[促销代码](generate-promotional-codes.md)并使用 Windows 10 设备）。

> [!IMPORTANT]
> 此选项将覆盖你在提交中选定的任何[可见性](choose-visibility-options.md#discoverability)设置。 

此选项与你创建提交并通过**停止获取**选项选择**使此产品在 Microsoft Store 中可用，但不可被发现**具有相同效果。 但是，它不需要你创建新提交。

请注意，任何已拥有该应用的客户仍然可以使用并重新下载它（如果你以后提交新程序包，甚至还可以获取更新）。

在禁止提供应用后，你仍然可以在自己的仪表板中看到它。 如果决定将应用重新提供给客户，你可以从“应用概述”页中单击“提供应用”****。 确认后，应用将在数小时内提供给新客户（除非受上一次提交中的设置的限制）。

> [!NOTE]
> 如果你想要保持应用可用，但又不想继续将它提供给使用特定操作系统版本的新用户，你可以创建新提交，并删除要阻止新获取的操作系统版本的所有程序包。 例如，如果你先前有适用于 Windows Phone 8.1 和 Windows 10 的程序包，但你不想将该应用继续提供给使用 Windows Phone 8.1 的新客户，则可从提交中删除所有 Windows Phone 8.1 程序包。 发布更新后，使用 Windows Phone 8.1 的任何新客户将不能够获取该应用（尽管已拥有它的客户可以继续使用它）。 但是，该应用仍将提供给使用 Windows 10 的新客户。


## <a name="removing-packages-for-a-previously-supported-device-family"></a>删除以前受支持的设备系列中的程序包

如果您删除的特定[设备系列](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)您的应用程序之前支持，您将会提示您确认这是您的目的，在**包**页上保存所做的更改之前的所有包。

在发布中移除所有可以运行在您的应用程序以前支持的设备系列的包提交时，新客户不能以获取该设备系列上的应用程序。 稍后你可以始终发布其他更新以为该设备系列重新提供程序包。

请注意，即使你删除支持某些设备系列的所有程序包，已在该设备类型上安装应用的任何现有客户仍可以使用它，并且他们将获取你以后提供的任何更新。


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>将适用于 Windows 10 的程序包添加到以前发布的应用

如果你在应用商店中有一个面向 Windows 8.x 和/或 Windows Phone 8.x 的应用，并希望为 Windows 10 更新你的应用，请创建一个新提交并在[程序包](upload-app-packages.md) 步骤阶段添加 UWP .appxupload 程序包。 您的应用程序经过认证过程后，则已经过您的应用程序和现在在 Windows 10 上的客户将从存储 UWP 程序包获取作为更新。 该 UWP 程序包也适用于使用 Windows 10 的客户进行新的购置。

> [!NOTE]
> 使用 Windows 10 的客户获取 UWP 程序包后，你无法使客户回退到使用任何以前操作系统版本的程序包。 

请注意，你的 Windows 10 程序包的版本号必须始终高于那些你包含的 Windows 8、Windows 8.1 和/或 Windows Phone 8.1 程序包（或你以前发布的那些操作系统的程序包）的任一版本号。 有关详细信息，请参阅[程序包版本编号](package-version-numbering.md)。

有关如何包装 UWP 应用以上架 Microsoft Store 的详细信息，请参阅[包装应用](../packaging/index.md)。

> [!IMPORTANT]
> 请谨记，如果你提供面向通用设备系列的程序包，每个已经在任何较早的操作系统（Windows Phone 8、Windows 8.1 等）上拥有你的应用然后升级到 Windows 10 的客户将更新以获取你的 Windows 10 程序包。
> 
> 发生这种情况即使您已排除您提交的[设备系列可用性](device-family-availability.md)步骤中的特定设备系列相部分仅适用于新收购。 如果你不希望每个以前的客户获取你的通用 Windows 10 程序包，请确保在 appx 清单中将 [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) 元素更新为仅包含你希望支持的特定设备系列。
> 
> 例如，假设您希望 Windows 8 和 Windows 8.1 客户已升级到一个 Windows 10 桌面设备，以获取新 UWP 应用程序，但您希望现在保留就像以前的程序包的 Windows 10 Mobile 设备进行 availabl 任何 Windows Phone 客户e （面向 Windows Phone 8 或 Windows Phone 8.1）。 要执行此操作，您将需要更新[**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily)约清单中包括仅**Windows.Desktop** （对于桌面设备系列），而不是将其保留为**Windows.Universal**值 （对于通用设备系列）此 Microsoft Visual Studio 默认情况下包括在清单中。 不要提交任何面向通用或移动设备系列的 UWP 程序包（**Windows.Universal** 或 **Windows.Universal**）。 这样，你的 Windows 10 Mobile 客户将不会获得你的任何 UWP 程序包。


## <a name="maintaining-package-compatibility-for-windows-phone-81"></a>使程序包维持对 Windows Phone 8.1 的兼容性

当更新之前为 Windows Phone 8.1 发布的应用时，会针对程序包类型应用某些要求：

-   在应用具有已发布的 Windows Phone 8.1 程序包后，所有后续更新必须也包含 Windows Phone 8.1 程序包。
-   在应用具有已发布的 Windows Phone 8.1 XAP 后，后续更新必须具有 Windows Phone 8.1 XAP、Windows Phone 8.1 appx 或 Windows Phone 8.1 appxbundle。
-   当应用具有已发布的 Windows Phone 8.1 .appx 时，后续更新必须具有 Windows Phone 8.1 .appx 或 Windows Phone 8.1 .appxbundle。 换言之，不允许 Windows Phone 8.1 XAP。 这也适用于包含 Windows Phone 8.1 .appx 的 .appxupload。
-   在应用具有已发布的 Windows Phone 8.1 .appxbundle 后，后续更新必须具有 Windows Phone 8.1 .appxbundle。 换言之，不允许 Windows Phone 8.1 XAP 或 Windows Phone 8.1 .appx。 这也适用于包含 Windows Phone 8.1 .appxbundle 的 .appxupload。

若未遵循这些规则，可能导致程序包上载错误，从而阻止你完成提交。
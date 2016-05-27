---
author: jnHs
Description: 了解如何向你的客户提供应用的程序包，以及如何管理特定的程序包方案。
title: 应用包管理指南
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
---

# 应用包管理指南


了解如何向你的客户提供应用的程序包，以及如何管理特定的程序包方案。

-   [操作系统版本和程序包分发](#os-versions-and-package-distribution)
-   [将适用于 Windows 10 的程序包添加到以前发布的应用](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [使程序包维持对 Windows Phone 8.1 的兼容性](#maintaining-package-compatibility-for-windows-phone-8-1)
-   [从应用商店中删除应用](#removing-an-app-from-the-store)
-   [删除以前受支持的设备系列中的程序包](#removing-packages-for-a-previously-supported-device-family)

## 操作系统版本和程序包分发


不同的操作系统可以运行不同类型的程序包。 如果有多个程序包可在客户的设备上运行，则 Windows 应用商店将提供最佳的可用匹配。

通常来说，较高的操作系统版本可运行适用于相同设备系列的面向以前操作系统版本的程序包。 但是，客户将只在该应用不包含面向当前操作系统版本的程序包时获取这些程序包。

例如，Windows 10 设备可以运行所有以前的受支持操作系统版本（每个设备系列）。 Windows 10 桌面设备可以运行为 Windows 8.1 或 Windows 8 生成的应用；Windows 10 移动设备可以运行为 Windows Phone 8.1、Windows Phone 8 甚至 Windows Phone 7.x 生成的应用。

以下示例说明一个应用的各种方案，该应用包含面向不同操作系统版本的程序包。 在某些情况下，程序包的特定约束可能不允许它们在此处列出的所有操作系统版本或设备类型上运行（例如，体系结构必须合适），但是这些示例应有助于你了解哪些操作系统版本可以运行特定程序包。

### 应用示例 1

| 程序包的目标操作系统 | 将获得此程序包的操作系统 |
|-------------------------------------|----------------------------------------------|
| Windows 8.1                         | Windows 10 桌面设备、Windows 8.1      |
| Windows Phone 8.1                   | Windows 10 移动设备，Windows Phone 8.1 |
| Windows Phone 8                     | Windows Phone 8                              |
| Windows Phone 7.1                   | Windows Phone 7.x                            |

在示例应用 1 中，应用尚不具有专门为 Windows 10 设备生成的通用 Windows 平台 (UWP) 程序包，但使用 Windows 10 的客户仍然可以获得该应用。 根据其设备类型，这些客户将获得可用的最佳程序包。

### 应用示例 2

| 程序包的目标操作系统  | 将获得此程序包的操作系统 |
|--------------------------------------|----------------------------------------------|
| Windows 10（通用设备系列） | Windows 10（所有设备系列）             |
| Windows 8.1                          | Windows 8.1                                  |
| Windows Phone 8.1                    | Windows Phone 8.1                            |
| Windows Phone 7.1                    | Windows Phone 7.x，Windows Phone 8           |

在示例应用 2 中，没有可在 Windows 8 上运行的程序包。 使用所有其他操作系统版本的客户都可获得该应用。

### 示例应用 3

| 程序包的目标操作系统 | 将获得此程序包的操作系统                  |
|-------------------------------------|---------------------------------------------------------------|
| Windows 10（桌面设备系列）  | Windows 10 桌面设备                                    |
| Windows Phone 8                     | Windows 10 移动设备、Windows Phone 8、Windows Phone 8.1 |

在示例应用 3 中，由于没有面向移动设备系列的 UWP 程序包，因此使用 Windows 10 移动设备的客户将获得 Windows Phone 8 程序包。 如果此应用以后添加面向移动设备系列（或通用设备系列）的程序包，则此时将向使用 Windows 10 移动设备的客户提供这些程序包，而不是 Windows Phone 8 程序包。

另请注意，此示例应用不包括可在 Windows 7.x 上运行的任何程序包。

### 应用示例 4

| 程序包的目标操作系统  | 将获得此程序包的操作系统 |
|--------------------------------------|----------------------------------------------|
| Windows 10（通用设备系列） | Windows 10（所有设备系列）             |

在示例应用 4 中，运行 Windows 10 的任何设备都可获得该应用，但不会向使用任何以前操作系统版本的客户提供该应用。 由于 UWP 程序包面向通用设备系列，因此它将同时向桌面和移动 Windows 10 设备提供。

## 将适用于 Windows 10 的程序包添加到以前发布的应用


如果你在应用商店中有一个应用并希望为 Windows 10 更新你的应用，请创建一个新提交并在[程序包](upload-app-packages.md)步骤阶段添加 UWP .appxupload 程序包。 在你的应用完成认证过程后，对于在升级到 Windows 10 之前已拥有你的应用的客户，将能够从应用商店作为更新获取 UWP 程序包。 该 UWP 程序包也适用于使用 Windows 10 的客户进行新的购置。

> **重要提示** 使用 Windows 10 的客户获取 UWP 程序包后，你无法使客户回退到使用任何以前操作系统版本的程序包。 在提交 UWP 程序包之前，请确保已在 Windows 10 上对其进行了彻底测试。

你可以同时更新你的任何其他程序包，或对提交进行其他更改（例如，你可能要[创建特定于平台的提要](create-platform-specific-descriptions.md)以向使用较早操作系统版本的客户显示）。 如果你愿意，也可以使其他的一切保持原样。

> **注意** 你的 Windows 10 程序包的版本号必须始终高于那些你正在为相同应用发布的 Windows 8、Windows 8.1 和/或 Windows Phone 8.1 程序包（或你以前发布的那些操作系统的程序包）的任一版本号。 有关 Windows 10 版本编号的详细信息，请参阅[程序包版本编号](package-version-numbering.md)。

新提交完成认证过程后，UWP 程序包以及你向尚未使用 Windows 10 的客户提供的任何其他程序包将可用。

有关打包适用于应用商店的 UWP 应用的详细信息，请参阅[打包适用于 Windows 10 的通用 Windows 应用](http://go.microsoft.com/fwlink/p/?LinkId=620193 )。

> **重要提示** 请谨记，如果你提供面向通用设备系列的程序包，每个已经在任何较早的操作系统（Windows Phone 8、Windows 8.1 等）上拥有你的应用然后升级到 Windows 10 的客户将更新到你的 Windows 10 通用程序包。
> 
> 由于**“设备系列”**选择仅适用于新的购置，因此即使你在提交的[“定价和可用性”](set-app-pricing-and-availability.md#windows-10-device-families)步骤中排除了特定设备系列，也会发生这种情况。 如果你不希望每个以前的客户获取你的新 Windows 10 程序包，请确保在 appx 清单中将 [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) 元素更新为仅包含你希望支持的特定设备系列。
> 
> 例如，假设你只希望已升级到 Windows 10 的 Windows 8 和 Windows 8.1 客户获取 UWP 应用，并且希望使用 Windows Phone 8.1 和更早版本的客户保留你以前提供的程序包（面向 Windows Phone 8 或 Windows Phone 8.1）。 为此，你将需要确保在 appx 清单中将 [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) 更新为仅包含 **Windows.Desktop**（面向桌面设备系列），而不是将其保留为 Microsoft Visual Studio 默认包含在 appx 清单中的 **Windows.Universal** 值（面向通用设备系列）。 不要提交任何面向通用或移动设备系列的 UWP 程序包（**Windows.Universal** 或 **Windows.Universal**）。 这样，你的 Windows 10 移动客户将不会获得你的任何 UWP 程序包。
> 
> 有关设备系列的详细信息，请参阅[通用 Windows 平台 (UWP) 应用指南](https://msdn.microsoft.com/library/windows/apps/dn894631)。

## 使程序包维持对 Windows Phone 8.1 的兼容性


当更新之前为 Windows Phone 8.1 发布的应用时，会针对程序包类型应用某些要求：

-   在应用具有已发布的 Windows Phone 8.1 程序包后，所有后续更新必须也包含 Windows Phone 8.1 程序包。
-   在应用具有已发布的 Windows Phone 8.1 XAP 后，后续更新必须具有 Windows Phone 8.1 XAP、Windows Phone 8.1 appx 或 Windows Phone 8.1 appxbundle。
-   当应用具有已发布的 Windows Phone 8.1 .appx 时，后续更新必须具有 Windows Phone 8.1 .appx 或 Windows Phone 8.1 .appxbundle。 换言之，不允许 Windows Phone 8.1 XAP。 这也适用于包含 Windows Phone 8.1 .appx 的 .appxupload。
-   在应用具有已发布的 Windows Phone 8.1 .appxbundle 后，后续更新必须具有 Windows Phone 8.1 .appxbundle。 换言之，不允许 Windows Phone 8.1 XAP 或 Windows Phone 8.1 .appx。 这也适用于包含 Windows Phone 8.1 .appxbundle 的 .appxupload。

若未遵循这些规则，将导致程序包上载错误，从而阻止你完成提交。

## 从应用商店中删除应用


有时，你可能想要完全停止向客户提供应用，即有效地“取消发布”它。 若要执行此操作，请从“应用概述”页上单击“禁止提供应用”****。 在你对要禁止提供的应用进行确认后，数小时内它不会再出现在应用商店中，并且新客户将无法通过包含促销充值码在内的任何方法获取它。

> **重要提示** 这将覆盖你在提交中选定的所有[“分发和可见性”](set-app-pricing-and-availability.md#distribution-and-visibility)设置。

请注意，任何已拥有该应用的客户仍然可以使用它（如果你以后提交新程序包，甚至还可以获取更新）。

在禁止提供应用后，你仍然可以在自己的仪表板中看到它。 如果决定将应用重新提供给客户，你可以从“应用概述”页中单击“提供应用”****。 确认后，应用将在数小时内提供给新客户（除非受上一次提交中的设置的限制）。

> **注意** 如果你想要保持应用可用，但又不想继续将它提供给使用特定操作系统版本的用户，你可以创建新提交，并删除要阻止新获取的操作系统版本的所有程序包。 例如，如果你先前有适用于 Windows Phone 8、Windows Phone 8.1 和 Windows 10 的程序包，但你不想将该应用继续提供给使用 Windows Phone 8 的新客户，请从提交中删除 Windows Phone 8 程序包。 发布更新后，使用 Windows Phone 8 的任何新客户将不能够获取该应用（尽管已拥有它的客户可以继续使用它）。 该应用仍将提供给使用 Windows Phone 8.1 和 Windows 10 的新客户。

## 删除以前受支持的设备系列中的程序包


如果删除您的应用以前支持的某些设备系列中的所有程序包，在“程序包”****页面上保存更改之前将提示您确认您是否要这样做。

当您发布删除您的应用以前支持的某个设备系列中的程序包的提交时，新客户将无法在该设备系列上获取应用。 稍后您可以始终发布其他更新以为该设备系列重新提供程序包。

请注意，即使您删除支持某些设备系列的所有程序包，已在该设备类型上安装应用的任何现有客户仍可以使用它，并且他们将获取您以后提供的任何更新。

 

 






<!--HONumber=May16_HO2-->



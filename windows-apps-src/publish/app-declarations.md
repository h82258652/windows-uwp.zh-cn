---
author: jnHs
Description: Product declarations help make sure your app is displayed appropriately in the Microsoft Store and offered to the right set of customers.
title: 产品声明
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.author: wdg-dev-content
ms.date: 12/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 959e056d5edf5e1fe7a1c51a2f855c9e11512cb0
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "4151664"
---
# <a name="product-declarations"></a>产品声明

在[提交过程](app-submissions.md)的[属性](enter-app-properties.md)页的**产品声明**部分可帮助确保你的应用适当的方式显示并交付给正确的客户，并帮助他们了解如何使用你的应用。

以下部分介绍一些声明和你需要确定是否每个声明都适用于你的应用时的注意事项。 请注意，这些声明的两个检查默认情况下 （如下面所述）。具体取决于你的产品类别，你还可能会看到其他声明。 请务必查看所有声明并确保它们准确反映你的提交。

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>此应用使用户可以进行购买，但不使用 Microsoft 应用商店商务系统。

几乎每个提交中，你应取消选中此框，由于应用提供给要购买的机会是，或可消耗或在你的应用中使用的项目必须使用 Microsoft 应用商店应用内购买 API 创建和提交加载项。 每个[应用开发人员协议](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)的创建和提交之前，在 2015 年 6 月 29 日应用无法继续无需使用 Microsoft 商务引擎，提供应用内购买功能，但前提是该购买功能符合[Microsoft 应用商店策略](https://docs.microsoft.com/legal/windows/agreements/store-policies#108-financial-transactions)。 如果这适用于你的应用，你必须选中此框。 否则，请不要选中。

## <a name="this-app-has-been-tested-to-meet-accessibility-guidelines"></a>已对此应用进行辅助功能指南符合度测试。

勾选此框可使你的应用能够被在应用商店中特意寻找辅助应用的客户发现。

仅应在已完成以下所有事项后才勾选此框：

-   为 UI 元素设置所有相关的辅助功能信息，例如辅助名称。
-   已实现键盘导航和操作，考虑选项卡顺序、键盘激活、箭头键导航和快捷方式。
-   已通过包含如 4.5:1 文本对比率等设置，确保实现辅助视觉体验，并且在将信息传递给用户时，不要仅依赖颜色。
-   已使用辅助功能测试工具（例如“检查”或 AccChecker）验证你的应用，并解决了这些工具检测到的所有高优先级错误。
-   已使用“讲述人”、“放大镜”、“屏幕键盘”、“高对比度”和“高 DPI”等设备和工具，从头到尾验证应用的关键场景。

将应用声明为可以访问，意味着你同意所有客户（包括残障人士）访问你的应用。 例如，这意味着你已经使用高对比度模式和屏幕阅读器对该应用进行了测试。 同时还验证键盘、放大镜及其他辅助工具是否能够正确操作用户界面功能。

有关详细信息，请参阅[辅助功能](../design/accessibility/accessibility.md)、[辅助功能测试](../design/accessibility/accessibility-testing.md)和[应用商店中的辅助功能](../design/accessibility/accessibility-in-the-store.md)。

> [!IMPORTANT]
> 除非已针对此目的特别设计和测试过应用，否则不要将其列为辅助应用。 如果将你的应用声明为辅助应用，但它实际上并不支持辅助功能，你可能会收到来自社区的负面反馈。

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>客户可以将此应用安装到备用驱动器或可移动存储。

默认情况下，以使客户能够将应用安装到外部或可移动存储媒体 SD 卡，如或非系统卷驱动器等外部驱动器情况下，此框处于选中状态。 （适用于 Windows Phone 8.1，这是通过 StoreManifest.xml 指示。）

如果你想要防止你的应用安装到备用驱动器或可移动存储，并仅允许安装到其设备上的内部硬盘驱动器，请取消选中此框。

请注意，如果没有限制安装，以便*仅*可以应用的选项安装到可移动存储媒体。


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>Windows 可以将此应用的数据包含在 OneDrive 的自动备份中。

此框在默认情况下处于选中状态，从而允许在客户选择使用 Windows 创建 OneDrive 自动备份时，将你的应用的数据包含在内。 （适用于 Windows Phone 8.1，这是通过 StoreManifest.xml 指示。）

如果要防止你的应用数据包含在自动备份中，请取消选中此框。


## <a name="this-app-sends-kinect-data-to-external-services"></a>此应用将 Kinect 数据发送到外部服务。 

如果你的应用使用 Kinect 数据并将其发送到任何外部服务，你必须选中此框。



 

 

 





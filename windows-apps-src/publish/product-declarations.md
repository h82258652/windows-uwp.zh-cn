---
Description: 产品声明可帮助确保你的应用在 Microsoft Store 中正确显示，并向正确的客户提供。
title: 产品声明
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 47011a22353f26361a392690d857bde1fc180c03
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79211003"
---
# <a name="product-declarations"></a>产品声明

[提交过程](app-submissions.md)的 "[属性](enter-app-properties.md)" 页的 "**产品声明**" 部分可帮助确保你的应用程序正确显示并向正确的客户提供，帮助他们了解他们如何使用你的应用程序。

以下各节介绍了一些声明以及在确定每个声明是否适用于你的应用程序时需要考虑的事项。 请注意，默认情况下会选中其中的两个声明（如下所述）。根据你的产品类别，你可能还会看到其他声明。 确保查看所有声明，并确保它们准确反映您的提交。

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>此应用允许用户购买，但不使用 Microsoft Store commerce 系统。

对于几乎每个提交，你都应将此框保持为未选中状态，因为提供购买项目的机会的应用程序必须使用 Microsoft Store 应用内购买 API 来创建和提交外接程序。 根据[应用开发人员协议](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)，在2015年6月29日之前创建并提交的应用程序可以继续提供应用内购买功能，而无需使用 Microsoft 的商业引擎，只要购买功能符合[Microsoft Store 策略](store-policies.md#108-financial-transactions)即可。 如果这适用于你的应用，你必须选中此框。 否则，请不要选中。

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
>除非您出于此目的专门设计和测试应用程序，否则  不会列出您的应用程序。 如果将你的应用声明为辅助应用，但它实际上并不支持辅助功能，你可能会收到来自社区的负面反馈。

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>客户可以将此应用安装到备用驱动器或可移动存储。

默认情况下，此框处于选中状态，以允许客户将应用安装到外部或可移动存储媒体（如 SD 卡）或非系统卷驱动器（如外部驱动器）。

如果要阻止将应用安装到备用驱动器或可移动存储，并仅允许安装到设备上的内部硬盘驱动器，请取消选中此框。 （请注意，不能选择限制安装，以便*只能*将应用程序安装到可移动存储媒体。）


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>Windows 可以将此应用的数据包含在 OneDrive 的自动备份中。

此框在默认情况下处于选中状态，从而允许在客户选择使用 Windows 创建 OneDrive 自动备份时，将你的应用的数据包含在内。

如果要防止你的应用数据包含在自动备份中，请取消选中此框。


## <a name="this-app-sends-kinect-data-to-external-services"></a>此应用向外部服务发送 Kinect 数据。 

如果你的应用使用 Kinect 数据并将它发送到任何的外部服务，则必须选中该复选框。



 

 

 





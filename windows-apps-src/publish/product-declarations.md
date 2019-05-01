---
Description: 产品声明有助于确保适当地显示在 Microsoft Store 中您的应用程序并将其提供给一组合适的客户。
title: 产品声明
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2bab1d6bd629aa85351e28a907d4b5ad705fb377
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63788365"
---
# <a name="product-declarations"></a>产品声明

**产品声明**一部分[属性](enter-app-properties.md)页[提交过程](app-submissions.md)有助于确保您的应用程序是相应地进行显示，并且会提供给一组正确客户，并可帮助他们了解如何使用您的应用程序。

以下各节介绍了声明和需要确定每个声明是否适用于您的应用程序时考虑的一些。 请注意两个这些声明检查了默认情况下 （如下面所述）。具体取决于您的产品的类别，也可能会看到其他声明。 请务必查看的所有声明，并确保它们能准确反映你的提交。

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>此应用，用户可以进行购买，但不使用 Microsoft Store 商务系统。

几乎每次提交时，您应保持未选中此框，因为这些工具提供了途径购买的应用是，或可以使用或在您的应用程序中使用的项必须使用 Microsoft Store 应用内购买 API 创建和提交外接程序。 每个[应用程序开发人员协议](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)，已创建并在 2015 年 6 月 29 日之前提交的应用程序无法继续而无需购买功能只要使用 Microsoft 的商务引擎，提供应用内购买功能符合[Microsoft Store 策略](https://docs.microsoft.com/legal/windows/agreements/store-policies#108-financial-transactions)。 如果这适用于你的应用，你必须选中此框。 否则，请不要选中。

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
> 除非专门设计，并对其进行测试实现此目的，不列表为可以访问你的应用。 如果将你的应用声明为辅助应用，但它实际上并不支持辅助功能，你可能会收到来自社区的负面反馈。

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>客户可以将此应用安装到备用驱动器或可移动存储。

若要允许客户将应用安装到外部或可移动存储媒体中的，如 SD 卡，或者为非系统卷驱动器等外部驱动器默认情况下，选中此框。

如果你想要防止您的应用程序安装到备用驱动器或可移动存储，且仅允许安装到其设备上的内部硬盘驱动器，请取消选中此框。 (请注意，没有任何选项，以便应用可以限制安装*仅*安装到可移动存储媒体。)


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>Windows 可以将此应用的数据包含在 OneDrive 的自动备份中。

此框在默认情况下处于选中状态，从而允许在客户选择使用 Windows 创建 OneDrive 自动备份时，将你的应用的数据包含在内。

如果要防止你的应用数据包含在自动备份中，请取消选中此框。


## <a name="this-app-sends-kinect-data-to-external-services"></a>此应用向外部服务发送 Kinect 数据。 

如果你的应用使用 Kinect 数据并将它发送到任何的外部服务，则必须选中该复选框。



 

 

 





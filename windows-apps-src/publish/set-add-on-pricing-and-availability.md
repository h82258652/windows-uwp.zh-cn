---
author: jnHs
Description: When submitting an add-on, the options on the Pricing and availability page determine what to charge for your add-on and how it should be offered to customers.
title: 设置加载项定价和可用性
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.author: wdg-dev-content
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 加载项, iap, 价格
ms.localizationpriority: medium
ms.openlocfilehash: b5b7a6424fea3d62849e992f56b0b40ab72a55f5
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2018
ms.locfileid: "3111988"
---
# <a name="set-add-on-pricing-and-availability"></a>设置加载项定价和可用性


在提交加载项时，**定价和可用性**页上的选项决定了你的加载项的付费内容以及应向客户提供它的方式。

## <a name="markets"></a>市场

默认情况下，你的加载项将以其基价在所有可能的市场中列出，包括任何我们稍后可能会添加的未来市场。

但是，和应用一样，你可以选择你希望提供你的加载项的市场。 在大部分情况下，你将希望选取与应用相同的市场集合，但你也可以根据需要灵活更改。 

有关详细信息和可用市场的完整列表，请参阅[定义市场选择](define-pricing-and-market-selection.md)。

## <a name="visibility"></a>可见性

你可以确定是否提供你的加载项以供客户购买。 

默认选项为**可在父产品的应用商店一览中显示**。 对于将向任何客户提供的加载项，将此选项保持选中状态。 

如果不希望广泛提供加载项，请选择**在应用商店中隐藏**以及以下选项之一：

-   **仅可从父产品中购买**：通过选择此选项，可允许所有客户从你的应用内购买加载项，但是该加载项将不会在应用的应用商店一览中显示。 仅当优惠不能广泛提供时（例如初期的内部测试期间），使用此选项。
-   **停止获取：任何使用直接链接的客户均可看到产品的应用商店一览，但仅当曾经拥有过该产品或拥有促销代码，同时使用的是 Windows 10 设备时，这些客户才能够下载该产品。加载项不在父产品一览中显示**：选择此选项意味着加载项将不会在应用一览中显示，并且任何新客户都无法购买该加载项。 但是，**对于 Windows 8.1 或更早版本上的客户不支持此选项**。 如果你的应用在 Windows 8.1 或更早版本上可用，该加载项将仍然可供这些客户购买。 若要在 Windows 8.1 或更早版本上停止提供此加载项，你将需要更改你的应用以删除提供该加载项的代码，然后为该应用发布新提交。 即使你的应用不面向 Windows 8.1 或更早版本，仍然建议使用此方法；如果你从未向客户提供你后来选择下架的加载项，这对于客户是更好的体验。
    
 > [!NOTE] 
 > 选择**停止获取**选项，和/或提供从应用代码删除该加载项的应用更新不会影响已购买该加载项的客户，无论他们的操作系统是何种版本。


## <a name="schedule"></a>计划

默认情况下（除非选择了**可见性**部分中**在应用商店中隐藏**选项中的一个），加载项一旦通过认证并完成发布流程，客户便可马上获取。 若要选择其他日期，请选择**显示选项**以展开此部分。 

有关详细信息，请参阅[配置精确的发布计划](configure-precise-release-scheduling.md)。


## <a name="pricing"></a>定价

（除非选择了**停止获取**选项中的**可见性**部分），必须为你的加载项选择一个基价。 默认选择是**免费**的因此如果你想要收取加载项，请确保选择一个可用价格段 （.99 美元）。

还可计划价格变更，以指示加载项价格应更改的日期和时间。 此外，还可选择针对特定市场自定义这些更改。 

> [!TIP]
> 对于订阅加载项之后通过在更高版本的提交中选择一个更高版本的基本价格或通过计划价格更改，提高价格发布加载项，, 不能提高价格。 你可以选择使用这些方法之一以较低价格，但后价格已降低你将无法引发高于该新价格。 因此，它是确保选择订阅加载项的相应价格段尤其重要。 

有关详细信息，请参阅[设置和计划应用定价](set-and-schedule-app-pricing.md)。


## <a name="sale-pricing"></a>销售定价

如果你想要限时降价出售你的加载项，可以创建和计划一次促销。 有关详细信息，请参阅[打折出售应用和加载项](put-apps-and-add-ons-on-sale.md)。




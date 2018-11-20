---
author: Xansky
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
description: 如果你的应用提供较大的应用内产品目录，你可以选择按照本主题中描述的过程来帮助管理你的目录。
title: 管理应用内产品的大目录
ms.author: mhopkins
ms.date: 08/25/2017
ms.topic: article
keywords: windows 10, uwp, 应用内购买, IAP, 加载项, 目录, Windows.ApplicationModel.Store
ms.localizationpriority: medium
ms.openlocfilehash: f57adf62939c28794e3ecdf6e59f2c4763de9c21
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2018
ms.locfileid: "7441244"
---
# <a name="manage-a-large-catalog-of-in-app-products"></a>管理应用内产品的大目录

如果你的应用提供较大的应用内产品目录，可以选择按照本主题中描述的过程帮助管理你的目录。 在 Windows 10 之前的版本中，应用商店限制每个开发者帐户可以列出 200 个产品，并且本主题中所述的过程可以用于解决此限制。 从 windows 10 开始，应用商店的每个开发人员帐户的产品列表数没有限制并且在本文中所述的过程也不再必需。

> [!IMPORTANT]
> 本文演示了如何使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空间的成员。 此命名空间不再更新新功能，我们建议你使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空间。 **Windows.Services.Store**命名空间支持最新的加载项类型，如应用商店管理的易耗型加载项和订阅，并且设计为与将来合作伙伴中心和应用商店支持的产品和功能类型兼容。 **Windows.Services.Store** 命名空间在 Windows 10 版本 1607 中引入，它仅可用于面向 **Windows 10 周年纪念版（10.0；版本 14393）或 Visual Studio** 更高版本的项目中。 有关详细信息，请参阅[应用内购买和试用](in-app-purchases-and-trials.md)。

若要启用此功能，你将创建少量特定价格段的产品条目，每个条目都能表示目录中数以百计的产品。 使用 [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 方法重载，它指定了与应用商店中列出的应用内产品相关联的应用定义的产品/服务。 除了在调用期间指定付费内容和产品关联之外，你的应用还应当传递包含大目录付费内容详细信息的 [ProductPurchaseDisplayProperties](https://msdn.microsoft.com/library/windows/apps/dn263384) 对象。 如果未提供上述详细信息，则将改用已列出产品的详细信息。

应用商店将仅使用生成的 [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392) 的购买请求中的 *offerId*。 此过程不会直接修改[在应用商店中列出应用内产品](../publish/add-on-submissions.md)时提供的原始信息。

## <a name="prerequisites"></a>先决条件

-   本主题介绍应用商店对于使用应用商店中列出的单个应用内产品表现多个应用内付费内容的支持。 如果你不熟悉应用内购买，请查看[启用应用内产品购买](enable-in-app-product-purchases.md)，以了解许可证信息以及如何在应用商店中恰当地列出你的应用内产品。
-   首次编码和测试新应用内付费内容时，必须使用 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) 对象而不是 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) 对象。 这样，你可以使用对许可证服务器的模拟调用验证许可证逻辑，而不是调用实时服务器。 若要实现此目的，需要在 %userprofile%\\AppData\\local\\packages\\&lt;程序包名称&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData 中自定义名为“WindowsStoreProxy.xml”的文件。 Microsoft Visual Studio 仿真器会在你首次运行应用时创建此文件，你也可以在运行时加载一个自定义文件。 有关详细信息，请参阅[将 WindowsStoreProxy.xml 文件与 CurrentAppSimulator 一起使用](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy)。
-   本主题还参考了[应用商店示例](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)中提供的代码示例。 若要获得为通用 Windows 平台 (UWP) 应用提供的不同货币化选项的实际体验，此示例是一个不错的选择。

## <a name="make-the-purchase-request-for-the-in-app-product"></a>提出针对应用内产品的购买请求

针对大目录内特定产品的购买请求的处理方式与任何其他应用内购买请求的处理方式相同。 当你的应用调用新的 [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 方法重载时，你的应用将提供 *OfferId* 和由应用内产品名称填充的 [ProductPurchaseDisplayProperties](https://msdn.microsoft.com/library/windows/apps/dn263390) 对象。

> [!div class="tabbedCodeSnippets"]
[!code-cs[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#MakePurchaseRequest)]

## <a name="report-fulfillment-of-the-in-app-offer"></a>报告应用产品/服务的实施情况

在本地实施付费内容后，你的应用需要立即向应用商店报告产品实施情况。 在大目录场景中，如果你的应用未报告付费内容实施情况，用户将无法使用相同的应用商店产品列表来购买任何应用内付费内容。

如前所述，应用商店仅使用提供的付费内容信息来填充 [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392)，并且不在大目录付费内容和应用商店产品列表之间创建永久性关联。 因此，你需要追踪产品的用户权利，并向 [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 操作之外的用户提供特定于产品的上下文（例如正在购买的项目名称或关于它的详细信息）。

下列代码将演示实施调用，以及插入特定产品/服务信息所采用的 UI 消息模式。 当不存在该特定产品信息时，示例将使用产品 [ListingInformation](https://msdn.microsoft.com/library/windows/apps/br225163) 中的信息。

> [!div class="tabbedCodeSnippets"]
[!code-cs[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#ReportFulfillment)]

## <a name="related-topics"></a>相关主题

* [启用应用内产品购买](enable-in-app-product-purchases.md)
* [启用可消费应用内产品购买](enable-consumable-in-app-product-purchases.md)
* [应用商店示例（演示试用版和应用内购买）](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263382)
* [ProductPurchaseDisplayProperties](https://msdn.microsoft.com/library/windows/apps/dn263384)

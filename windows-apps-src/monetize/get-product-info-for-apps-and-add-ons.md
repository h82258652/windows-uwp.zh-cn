---
ms.assetid: 89178FD9-850B-462F-9016-1AD86D1F6F7F
description: 了解如何使用 Windows.Services.Store 命名空间获取当前应用或其中一项加载项的与应用商店相关的产品信息。
title: 获取应用和加载项的产品信息
ms.date: 02/08/2018
ms.topic: article
keywords: windows 10, uwp, 应用内购买, IAP, 加载项, Windows.Services.Store
ms.localizationpriority: medium
ms.openlocfilehash: 9b923764c6374e403d2652db715f65a80c48bacf
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7974125"
---
# <a name="get-product-info-for-apps-and-add-ons"></a>获取应用和加载项的产品信息

本文介绍了如何使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空间中 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 类的方法，来访问当前应用或它的一个加载项的与 Microsoft Store 相关的信息。

有关完整的应用程序示例，请参阅[Microsoft Store 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)。

> [!NOTE]
> **Windows.Services.Store** 命名空间在 Windows 10 版本 1607 中引入，它仅可用于面向 **Windows 10 周年纪念版（10.0；版本 14393）或 Visual Studio** 更高版本的项目中。 如果你的应用面向 Windows 10 的较早版本，则必须使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空间来替代 **Windows.Services.Store** 命名空间。 有关详细信息，请参阅[此文](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)。

## <a name="prerequisites"></a>先决条件

这些示例有以下先决条件：
* 适用于面向 **Windows 10 周年纪念版（10.0；版本 14393）或**更高版本的通用 Windows 平台 (UWP) 应用的 Visual Studio 项目。
* 你有合作伙伴中心中的[创建应用提交](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)，并在应用商店中发布此应用。 在测试应用期间，你可以选择将应用配置为在应用商店中隐藏。 有关详细信息，请参阅我们的[测试指南](in-app-purchases-and-trials.md#testing)。
* 如果你想要获取应用的加载项的产品信息，则必须[创建合作伙伴中心中的加载项](../publish/add-on-submissions.md)。

这些示例中的代码假设：
* 代码在含有 [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx)（名为 ```workingProgressRing```）和 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)（名为 ```textBlock```）的 [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) 上下文中运行。 这些对象分别用于指示是否正在进行异步操作和显示输出消息。
* 代码文件有一个适用于 **Windows.Services.Store** 命名空间的 **using** 语句。
* 该应用是单用户应用，仅在启动该应用的用户上下文中运行。 有关详细信息，请参阅[应用内购买和试用](in-app-purchases-and-trials.md#api_intro)。

> [!NOTE]
> 如果你有使用[桌面桥](https://developer.microsoft.com/windows/bridges/desktop)的桌面应用程序，可能需要添加未在这些示例中显示的额外代码来配置 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 对象。 有关详细信息，请参阅[在使用桌面桥的桌面应用程序中使用 StoreContext 类](in-app-purchases-and-trials.md#desktop)。

## <a name="get-info-for-the-current-app"></a>获取当前应用的信息

若要获取当前应用的应用商店产品信息，请使用 [GetStoreProductForCurrentAppAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductforcurrentappasync) 方法。 这是一种异步方法，它返回 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 对象，该对象可用于获取价格等信息。

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAppInfoPage.xaml.cs#GetAppInfo)]

## <a name="get-info-for-add-ons-with-known-store-ids-that-are-associated-with-the-current-app"></a>通过与当前应用关联的已知 Store ID 获取加载项的信息

若要获取你已知道其 [Store ID](in-app-purchases-and-trials.md#store_ids) 的当前应用关联加载项的 Microsoft Store 产品信息，请使用 [GetStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductsasync) 方法。 这是一种异步方法，可返回一组代表每一个加载项的 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 对象。 除了 Store ID 之外，还必须向此方法传递一列字符串，用于标识加载项的类型。 有关受支持的字符串值列表，请参阅 [ProductKind](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.productkind) 属性。

> [!NOTE]
> **GetStoreProductsAsync** 方法返回与应用关联的指定加载项的产品信息，无论加载项当前是否可购买。 若要检索当前可购买的当前应用的所有加载项的信息，请改用 **GetAssociatedStoreProductsAsync** 方法，如[下一部分](#get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app)所述。

此示例使用与当前应用关联的指定 Store ID 检索持久型加载项的信息。

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetProductInfoPage.xaml.cs#GetProductInfo)]

## <a name="get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app"></a>获取可从当前应用购买的加载项的信息

若要获取目前可从当前应用购买的加载项的 Microsoft Store 产品信息，请使用 [GetAssociatedStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstoreproductsasync) 方法。 这是一种异步方法，可返回一组代表每一个可用加载项的 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 对象。 必须向此方法传递一列字符串，用于标识你想要检索的加载项的类型。 有关受支持的字符串值的列表，请参阅 [ProductKind](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.productkind) 属性。

> [!NOTE]
> 如果应用有许多加载项可供购买，你也可以使用 [GetAssociatedStoreProductsWithPagingAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsWithPagingAsync) 方法分页返回加载项结果。

以下示例检索可从当前应用购买的所有持久型加载项、Microsoft Store 管理的易耗型加载项和开发人员管理的可消耗加载项的信息。

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAddOnInfoPage.xaml.cs#GetAddOnInfo)]


## <a name="get-info-for-add-ons-for-the-current-app-that-the-user-has-purchased"></a>获取用户购买的当前应用的加载项的信息

若要获取当前用户购买的加载项的 Microsoft Store 产品信息，请使用 [GetUserCollectionAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getusercollectionasync) 方法。 这是一种异步方法，可返回代表每一个加载项的 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 对象的集合。 必须向此方法传递一列字符串，用于标识你想要检索的加载项的类型。 有关受支持的字符串值列表，请参阅 [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx) 属性。

> [!NOTE]
> 如果应用具有许多加载项，你也可以使用 [GetUserCollectionWithPagingAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getusercollectionwithpagingasync) 方法分页返回加载项结果。

以下示例使用指定的 [Store ID](in-app-purchases-and-trials.md#store_ids) 检索持久型加载项的信息。

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetUserCollectionPage.xaml.cs#GetUserCollection)]

## <a name="related-topics"></a>相关主题

* [应用内购买和试用](in-app-purchases-and-trials.md)
* [获取应用和加载项的许可证信息](get-license-info-for-apps-and-add-ons.md)
* [支持应用内购买应用和加载项](enable-in-app-purchases-of-apps-and-add-ons.md)
* [支持购买易耗型加载项](enable-consumable-add-on-purchases.md)
* [实现应用的试用版](implement-a-trial-version-of-your-app.md)
* [应用商店示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)

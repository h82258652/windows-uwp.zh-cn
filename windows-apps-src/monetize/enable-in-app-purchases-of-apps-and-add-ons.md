---
author: mcleanbyron
ms.assetid: B356C442-998F-4B2C-B550-70070C5E4487
description: "了解如何使用 Windows.Services.Store 命名空间购买应用或其加载项之一。"
title: "支持应用内购买应用和加载项"
keywords: "应用内付费内容代码示例"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 0347c3a72ccdf26ddd885a5bad944ae36e09a190

---

# 支持应用内购买应用和加载项

面向 Windows 10 版本 1607 或更高版本的应用可以使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空间中的成员，请求为用户购买当前应用或其加载项之一（也称为应用内产品或 IAP）。 例如，如果用户当前有应用的试用版，可以使用此过程为用户购买完整版。 或者，你可以使用此过程购买加载项，如用户的新游戏关卡。

若要请求购买应用或加载项，[Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 提供了几个不同方法：
* 如果你知道应用或加载项的[应用商店 ID](in-app-purchases-and-trials.md#store_ids)，可以使用 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 类的 [RequestPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.requestpurchaseasync.aspx) 方法。
* 如果你已有表示应用或加载项的 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)、[StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) 或 [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) 对象，可以使用这些对象的 **RequestPurchaseAsync** 方法。

对于用户，每个方法都表示标准购置 UI，随后在交易完成后以异步方式完成这些方法。 该方法返回一个指示该交易是否已成功的对象。

>**注意**&nbsp;&nbsp;本文适用于面向 Windows 10 版本 1607 或更高版本的应用。 如果你的应用面向 Windows 10 的较早版本，则必须使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空间来替代 **Windows.Services.Store** 命名空间。 有关详细信息，请参阅[使用 Windows.ApplicationModel.Store 命名空间进行应用内购买和试用](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)。

## 先决条件

本示例有以下先决条件：
* 适用于面向 Windows 10 版本 1607 或更高版本的通用 Windows 平台 (UWP) 应用的 Visual Studio 项目。
* 你已在 Windows 开发人员中心仪表板中创建了一个应用，并且该应用已发布，并在应用商店中上架。 该应用可以是你想要发布给客户的应用，也可以是符合 [Windows 应用认证工具包](https://developer.microsoft.com/windows/develop/app-certification-kit)最低要求、仅用于测试目的的基本应用。 有关详细信息，请参阅[测试指南](in-app-purchases-and-trials.md#testing)。

此示例中的代码假设：
* 代码在含有 [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx)（名为 ```workingProgressRing```）和 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)（名为 ```textBlock```）的 [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) 上下文中运行。 这些对象分别用于指示是否正在进行异步操作和显示输出消息。
* 代码文件有一个适用于 **Windows.Services.Store** 命名空间的 **using** 语句。
* 该应用是单用户应用，仅在启动该应用的用户上下文中运行。 有关详细信息，请参阅[应用内购买和试用](in-app-purchases-and-trials.md#api_intro)。

## 代码示例

此示例演示了如何使用 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 类的 [RequestPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.requestpurchaseasync.aspx) 方法，购置具有已知[应用商店 ID](in-app-purchases-and-trials.md#store_ids) 的应用或加载项。

```csharp
private StoreContext context = null;

public async void PurchaseAddOn(string storeId)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    workingProgressRing.IsActive = true;
    StorePurchaseResult result = await context.RequestPurchaseAsync(storeId);
    workingProgressRing.IsActive = false;

    if (result.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {result.ExtendedError.Message}";
        return;
    }

    switch (result.Status)
    {
        case StorePurchaseStatus.AlreadyPurchased:
            textBlock.Text = "The user has already purchased the product.";
            break;

        case StorePurchaseStatus.Succeeded:
            textBlock.Text = "The purchase was successful.";
            break;

        case StorePurchaseStatus.NotPurchased:
            textBlock.Text = "The purchase did not complete. " +
                "The user may have cancelled the purchase.";
            break;

        case StorePurchaseStatus.NetworkError:
            textBlock.Text = "The purchase was unsuccessful due to a network error.";
            break;

        case StorePurchaseStatus.ServerError:
            textBlock.Text = "The purchase was unsuccessful due to a server error.";
            break;

        default:
            textBlock.Text = "The purchase was unsuccessful due to an unknown error.";
            break;
    }
}
```

有关完整的示例应用程序，请参阅[应用商店示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)。

## 相关主题

* [应用内购买和试用](in-app-purchases-and-trials.md)
* [获取应用和加载项的产品信息](get-product-info-for-apps-and-add-ons.md)
* [获取应用和加载项的许可证信息](get-license-info-for-apps-and-add-ons.md)
* [支持购买易耗型加载项](enable-consumable-add-on-purchases.md)
* [实现应用的试用版](implement-a-trial-version-of-your-app.md)
* [应用商店示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Aug16_HO5-->



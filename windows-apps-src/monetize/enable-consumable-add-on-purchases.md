---
author: mcleanbyron
ms.assetid: FD381669-F962-465E-940B-AED9C8D19C90
description: "了解如何使用 Windows.Services.Store 命名空间处理可消耗加载项。"
title: "支持购买可消耗加载项"
keywords: "应用内付费选项代码示例"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 1e9ecad5abb9addbe41b38d0b56b84404716f2a8

---

# 支持购买可消耗加载项

面向 Windows 10 版本 1607 或更高版本的应用可以使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空间中 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 类的方法，管理 UWP 应用中可消耗加载项的实施情况（加载项也称为应用内产品或 IAP）。 对可以购买、使用并再次购买的商品使用可消耗加载项。 这对游戏内货币（金子、硬币等）等来说尤为有用，可以购买此类货币，然后将其用于购买特定道具。

>**注意**&nbsp;&nbsp;本文适用于面向 Windows 10 版本 1607 或更高版本的应用。 如果你的应用面向 Windows 10 的较早版本，则必须使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空间来替代 **Windows.Services.Store** 命名空间。 有关详细信息，请参阅[使用 Windows.ApplicationModel.Store 命名空间进行应用内购买和试用](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)。

## 可消耗加载项概述

面向 Windows 10 版本 1607 或更高版本的应用可以提供在实施情况管理方式上大不相同的两种类型的可消耗加载项：

* **开发人员管理的消耗品**。 对于此类型的消耗品，你负责跟踪加载项所表示的商品的用户余量，并在用户消耗完所有商品后向应用商店将加载项购买报告为已完成。 在你的应用将之前的加载项购买报告为已完成前，用户无法再次购买该加载项。

  例如，如果你的加载项表示游戏中的 100 个硬币，并且用户消耗了 10 个硬币，则你的用户或服务必须为该用户保留 90 个硬币的新剩余余额。 在用户消耗完全部 100 个硬币后，你的应用必须将加载项报告为已完成，然后用户才可以再次购买 100 个硬币的加载项。

* **应用商店管理的消耗品**。 对于此类型的消耗品，应用商店会跟踪用户拥有的加载项所表示商品的余量。 当用户消耗任何商品时，你负责向应用商店报告这些商品已完成，然后应用商店会更新用户的余量。 你的应用可以随时查询用户的当前余量。 用户消耗完所有商品后，可以再次购买加载项。

  例如，如果你的加载项在游戏中表示 100 个硬币的初始数量，并且用户消耗了 10 个硬币，则你的应用将向应用商店报告 10 个单位的加载项已完成，然后应用商店会更新剩余余额。 用户消耗完全部 100 个硬币后，可以再次购买 100 个硬币的加载项。

  >**注意**&nbsp;&nbsp;应用商店管理的易耗品从 Windows 10 版本 1607 开始可用。 在 Windows 开发人员中心仪表板中创建应用商店管理的易耗品的功能即将推出。

若要向用户提供可消耗加载项，请遵循此一般过程：

1. 让用户可以从你的应用中[购买加载项](enable-in-app-purchases-of-apps-and-add-ons.md)。
3. 用户消耗掉该加载项后（例如，他们在游戏中将硬币花掉），[请将该加载项报告为已完成](enable-consumable-add-on-purchases.md#report_fulfilled)。

你还可以随时获取应用商店管理的易耗品的[剩余余额](enable-consumable-add-on-purchases.md#get_balance)。

## 先决条件

这些示例有以下先决条件：
* 适用于面向 Windows 10 版本 1607 或更高版本的通用 Windows 平台 (UWP) 应用的 Visual Studio 项目。
* 你已使用可消耗加载项（也称为应用内产品或 IAP）在 Windows 开发人员中心仪表板中创建了一个应用，并且该应用已发布，并在应用商店中上架。 该应用可以是你想要发布给客户的应用，也可以是符合 [Windows 应用认证工具包](https://developer.microsoft.com/windows/develop/app-certification-kit)最低要求、仅用于测试目的的基本应用。 有关详细信息，请参阅[测试指南](in-app-purchases-and-trials.md#testing)。

这些示例中的代码假设：
* 代码在含有 [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx)（名为 ```workingProgressRing```）和 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)（名为 ```textBlock```）的 [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) 上下文中运行。 这些对象分别用于指示是否正在进行异步操作和显示输出消息。
* 代码文件有一个适用于 **Windows.Services.Store** 命名空间的 **using** 语句。
* 该应用是单用户应用，仅在启动该应用的用户上下文中运行。 有关详细信息，请参阅[应用内购买和试用](in-app-purchases-and-trials.md#api_intro)。

有关完整的示例应用程序，请参阅[应用商店示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)。

<span id="report_fulfilled" />
## 将可消耗加载项报告为已完成

在用户从你的应用中[购买加载项](enable-in-app-purchases-of-apps-and-add-ons.md)并消耗完后，你的应用必须通过调用 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 类的 [ReportConsumableFulfillmentAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.reportconsumablefulfillmentasync.aspx) 方法，将该加载项报告为已完成。 必须将以下信息传递给此方法：

* 你想要将其报告为已完成的加载项的[应用商店 ID](in-app-purchases-and-trials.md#store_ids)。
* 你想要将其报告为已完成的加载项的单位。
  * 对于开发人员管理的消耗品，将 *quantity* 参数指定为 1。 这将通知应用商店该消耗品已完成，然后客户可以再次购买该消耗品。 只有在你的应用通知应用商店消耗品已完成后，用户才能再次购买该消耗品。
  * 对于应用商店管理的消耗品，请指定已消耗的实际数量。 应用商店将更新该消耗品的剩余余额。
* 完成情况的追踪 ID。 这是开发人员提供的 GUID，可标识实施情况与之相关联的具体交易记录以用于跟踪。 有关详细信息，请参阅 [ReportConsumableFulfillmentAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.reportconsumablefulfillmentasync.aspx) 中的备注。

此示例展示了如何将应用商店管理的消耗品报告为已完成。

```csharp
private StoreContext context = null;

public async void ConsumeAddOn(string storeId)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // This is an example for a Store-managed consumable, where you specify the actual number
    // of units that you want to report as consumed so the Store can update the remaining
    // balance. For a developer-managed consumable where you maintain the balance, specify 1
    // to just report the add-on as fulfilled to the Store.
    uint quantity = 10;
    string addOnStoreId = "9NBLGGH4TNNR";
    Guid trackingId = Guid.NewGuid();

    workingProgressRing.IsActive = true;
    StoreConsumableResult result = await context.ReportConsumableFulfillmentAsync(
        addOnStoreId, quantity, trackingId);
    workingProgressRing.IsActive = false;

    if (result.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {result.ExtendedError.Message}";
        return;
    }

    switch (result.Status)
    {
        case StoreConsumableStatus.Succeeded:
            textBlock.Text = "The fulfillment was successful. Remaining balance: " +
                result.BalanceRemaining;
            break;

        case StoreConsumableStatus.InsufficentQuantity:
            textBlock.Text = "The fulfillment was unsuccessful because the user " +
            "doesn't have enough remaining balance." + result.BalanceRemaining;
            break;

        case StoreConsumableStatus.NetworkError:
            textBlock.Text = "The fulfillment was unsuccessful due to a network error.";
            break;

        case StoreConsumableStatus.ServerError:
            textBlock.Text = "The fulfillment was unsuccessful due to a server error.";
            break;

        default:
            textBlock.Text = "The fulfillment was unsuccessful due to an unknown error.";
            break;
    }
}
```

<span id="get_balance" />
## 获取应用商店管理的消耗品的剩余余额

此示例演示了如何使用 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 类的 [GetConsumableBalanceRemainingAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getconsumablebalanceremainingasync.aspx) 方法获取应用商店管理的可消耗加载项的剩余余额。

```csharp
private StoreContext context = null;

public async void GetRemainingBalance(string storeId)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    string addOnStoreId = "9NBLGGH4TNNR";

    workingProgressRing.IsActive = true;
    StoreConsumableResult result = await context.GetConsumableBalanceRemainingAsync(addOnStoreId);
    workingProgressRing.IsActive = false;

    if (result.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {result.ExtendedError.Message}";
        return;
    }

    switch (result.Status)
    {
        case StoreConsumableStatus.Succeeded:
            textBlock.Text = "Remaining balance: " + result.BalanceRemaining;
            break;

        case StoreConsumableStatus.NetworkError:
            textBlock.Text = "Could not retrieve balance due to a network error.";
            break;

        case StoreConsumableStatus.ServerError:
            textBlock.Text = "Could not retrieve balance due to a server error.";
            break;

        default:
            textBlock.Text = "Could not retrieve balance due to an unknown error.";
            break;
    }
}
```

## 相关主题

* [应用内购买和试用](in-app-purchases-and-trials.md)
* [获取应用和加载项的产品信息](get-product-info-for-apps-and-add-ons.md)
* [获取应用和加载项的许可证信息](get-license-info-for-apps-and-add-ons.md)
* [支持应用内购买应用和加载项](enable-in-app-purchases-of-apps-and-add-ons.md)
* [实现应用的试用版](implement-a-trial-version-of-your-app.md)
* [应用商店示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Aug16_HO5-->



---
ms.assetid: FD381669-F962-465E-940B-AED9C8D19C90
description: 了解如何使用 Windows.Services.Store 命名空间处理可消耗加载项。
title: 支持购买可消耗加载项
keywords: windows 10, uwp, 易耗品, 加载项, 应用内购买, IAP, Windows.Services.Store
ms.date: 05/09/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 601d4d0a2cfe7e6d024e9cc07fefcdb2be688a36
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371829"
---
# <a name="enable-consumable-add-on-purchases"></a>支持购买可消耗加载项

本文介绍了如何使用 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空间中 [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 类的方法，管理 UWP 应用中可消耗加载项的实施情况。 对可以购买、使用并再次购买的商品使用可消耗加载项。 这对游戏内货币（金子、金币等）等来说尤为有用，可以购买此类货币，然后将其用于购买特定道具。

> [!NOTE]
> **Windows.Services.Store** 命名空间在 Windows 10 版本 1607 中引入，它仅可用于面向 **Windows 10 周年纪念版（10.0；版本 14393）或 Visual Studio** 更高版本的项目中。 如果你的应用面向 Windows 10 的较早版本，则必须使用 [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) 命名空间来替代 **Windows.Services.Store** 命名空间。 有关详细信息，请参阅[此文章](enable-consumable-in-app-product-purchases.md)。

## <a name="overview-of-consumable-add-ons"></a>可消耗加载项概述

应用可以提供在实施情况管理方式上大不相同的两种类型的可消耗加载项：

* **开发人员管理的消耗品**。 对于此类型的消耗品，你负责跟踪加载项所表示的商品的用户余量，并在用户消耗完所有商品后向应用商店将加载项购买报告为已完成。 在你的应用将之前的加载项购买报告为已完成前，用户无法再次购买该加载项。

  例如，如果你的加载项表示游戏中的 100 个硬币，并且用户消耗了 10 个硬币，则你的用户或服务必须为该用户保留 90 个硬币的新剩余余额。 在用户消耗完全部 100 个硬币后，你的应用必须将加载项报告为已完成，然后用户才可以再次购买 100 个硬币的加载项。

* **应用商店管理的消耗品**。 对于此类型的消耗品，应用商店会跟踪用户拥有的加载项所表示商品的余量。 当用户消耗任何商品时，你负责向应用商店报告这些商品已完成，然后应用商店会更新用户的余量。 用户可以根据需要多次购买加载项（他们不需要首先使用这些项目）。 你的应用可以随时向 Microsoft Store 查询用户的当前余量。

  例如，如果你的加载项在游戏中表示 100 个硬币的初始数量，并且用户消耗了 50 个硬币，则你的应用将向 Microsoft Store 报告 50 个单位的加载项已完成，然后 Microsoft Store 会更新剩余余额。 如果用户再次购买你的加载项（增加 100 个硬币），他们现在总共有 150 个硬币。
    > [!NOTE]
    > Microsoft Store 管理的易耗品是在 Windows 10 版本 1607 引入。

若要向用户提供可消耗加载项，请遵循此一般过程：

1. 让用户可以从你的应用中[购买加载项](enable-in-app-purchases-of-apps-and-add-ons.md)。
3. 用户消耗掉该加载项后（例如，他们在游戏中将硬币花掉），[请将该加载项报告为已完成](enable-consumable-add-on-purchases.md#report_fulfilled)。

你还可以随时获取应用商店管理的易耗品的[剩余余额](enable-consumable-add-on-purchases.md#get_balance)。

## <a name="prerequisites"></a>先决条件

这些示例有以下先决条件：
* 适用于面向 **Windows 10 周年纪念版（10.0；版本 14393）或**更高版本的通用 Windows 平台 (UWP) 应用的 Visual Studio 项目。
* 你有[创建应用程序提交](https://docs.microsoft.com/windows/uwp/publish/app-submissions)在合作伙伴中心和此应用程序发布的存储区中。 在测试应用期间，你可以选择将应用配置为在 Microsoft Store 中隐藏。 有关详细信息，请参阅我们的[测试指南](in-app-purchases-and-trials.md#testing)。
* 你有[创建的应用程序可使用外接程序](../publish/add-on-submissions.md)在合作伙伴中心。

这些示例中的代码假设：
* 代码在含有 [ProgressRing](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressring)（名为 ```workingProgressRing```）和 [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock)（名为 ```textBlock```）的 [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) 上下文中运行。 这些对象分别用于指示是否正在进行异步操作和显示输出消息。
* 代码文件有一个适用于 **Windows.Services.Store** 命名空间的 **using** 语句。
* 该应用是单用户应用，仅在启动该应用的用户上下文中运行。 有关详细信息，请参阅[应用内购买和试用](in-app-purchases-and-trials.md#api_intro)。

有关完整的示例应用程序，请参阅[应用商店示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)。

> [!NOTE]
> 如果你有使用[桌面桥](https://developer.microsoft.com/windows/bridges/desktop)的桌面应用程序，可能需要添加未在这些示例中显示的额外代码来配置 [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 对象。 有关详细信息，请参阅[在使用桌面桥的桌面应用程序中使用 StoreContext 类](in-app-purchases-and-trials.md#desktop)。

<span id="report_fulfilled" />

## <a name="report-a-consumable-add-on-as-fulfilled"></a>将可消耗加载项报告为已完成

在用户从你的应用中[购买加载项](enable-in-app-purchases-of-apps-and-add-ons.md)并消耗完后，你的应用必须通过调用 [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 类的 [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync) 方法，将该加载项报告为已完成。 必须将以下信息传递给此方法：

* 你想要将其报告为已完成的加载项的[应用商店 ID](in-app-purchases-and-trials.md#store-ids)。
* 你想要将其报告为已完成的加载项的单位。
  * 对于开发人员管理的消耗品，将 *quantity* 参数指定为 1。 这将通知应用商店该消耗品已完成，然后客户可以再次购买该消耗品。 只有在你的应用通知应用商店消耗品已完成后，用户才能再次购买该消耗品。
  * 对于应用商店管理的消耗品，请指定已消耗的实际数量。 应用商店将更新该消耗品的剩余余额。
* 完成情况的追踪 ID。 这是开发人员提供的 GUID，可标识实施情况与之相关联的具体交易记录以用于跟踪。 有关详细信息，请参阅 [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync) 中的备注。

此示例展示了如何将应用商店管理的易耗品报告为已完成。

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/ConsumeAddOnPage.xaml.cs#ConsumeAddOn)]

<span id="get_balance" />

## <a name="get-the-remaining-balance-for-a-store-managed-consumable"></a>获取应用商店管理的易耗品的剩余余额

此示例演示了如何使用 [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 类的 [GetConsumableBalanceRemainingAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getconsumablebalanceremainingasync) 方法获取应用商店管理的易耗型加载项的剩余余额。

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/GetRemainingAddOnBalancePage.xaml.cs#GetRemainingAddOnBalance)]

## <a name="related-topics"></a>相关主题

* [应用内购买和试用](in-app-purchases-and-trials.md)
* [获取产品信息的应用程序和外接程序](get-product-info-for-apps-and-add-ons.md)
* [获取应用程序和外接程序的许可证信息](get-license-info-for-apps-and-add-ons.md)
* [启用应用内购买的应用程序和外接程序](enable-in-app-purchases-of-apps-and-add-ons.md)
* [实现您的应用程序的试用版](implement-a-trial-version-of-your-app.md)
* [应用商店示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)

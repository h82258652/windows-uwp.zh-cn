---
Description: author:mcleanbyron 通过应用商店商业平台提供可消费应用内产品&amp;\#8212;这些项目可以进行购买、使用和再次购买&amp;\#8212;，以便为客户提供强大可靠的购买体验。
title: 启用可消费应用内产品购买 ms.assetid：F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4 keywords:应用内付费 keywords:可消费 keywords:应用内购买 keywords:应用内产品 keywords:如何支持应用内 keywords:应用内购买代码示例 keywords:应用内付费代码示例
---

# 启用可消费应用内产品购买


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

通过应用商店商业平台提供可消费应用内产品（这些项目可以进行购买、使用和再次购买），以便为客户提供强大可靠的购买体验。 这对游戏内货币（金子、金币等）等来说尤为有用，可以购买此类货币，然后将其用于购买特定道具。

## 先决条件

-   本主题介绍可消费应用内产品的购买和实施报告。 如果你不熟悉应用内产品，请查看[启用应用内产品购买](enable-in-app-product-purchases.md)，了解许可证相关信息以及如何在应用商店中恰当地列出应用内产品。
-   首次编码和测试新应用内产品时，必须使用 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 对象而不是 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 对象。 这样，你可以使用对许可证服务器的模拟调用验证许可证逻辑，而不是调用实时服务器。 若要实现此目的，需要在 %userprofile%\\AppData\\local\\packages\\&lt;程序包名称&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData 中自定义名为“WindowsStoreProxy.xml”的文件。 Microsoft Visual Studio 仿真器会在你首次运行应用时创建此文件，你也可以在运行时加载一个自定义文件。 有关详细信息，请参阅 **CurrentAppSimulator**。
-   本主题还参考了[应用商店示例](http://go.microsoft.com/fwlink/p/?LinkID=627610)中提供的代码示例。 若要获得为通用 Windows 平台 (UWP) 应用提供的不同货币化选项的实际体验，此示例是一个不错的选择。

## 步骤 1：提出购买请求

初始购买请求是通过 [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263381) 提出的，其方式与通过应用商店提出的任何其他购买相似。 可消费应用内产品的不同之处在于在成功购买后，客户无法再次购买同一个产品，直到应用通知应用商店之前的购买已成功实施。 应用负责实施购买的消费品，并通知应用商店该项实施。

下面的示例显示了一个可消费应用内产品的购买请求。 你会注意到代码注释，指明应用何时应该为下面两种不同情况的可消费应用内产品执行本地实施：已成功提出请求的情况和请求因同一产品未实施购买而失败的情况。

```CSharp
PurchaseResults purchaseResults = await CurrentAppSimulator.RequestProductPurchaseAsync("product1");
switch (purchaseResults.Status)
{
    case ProductPurchaseStatus.Succeeded:
        product1TempTransactionId = purchaseResults.TransactionId;

        // Grant the user their purchase here, and then pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
        // To indicate local fulfillment to the Windows Store.
        break;

    case ProductPurchaseStatus.NotFulfilled:
        product1TempTransactionId = purchaseResults.TransactionId;

        // First check for unfulfilled purchases and grant any unfulfilled purchases from an earlier transaction.
        // Once products are fulfilled pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
        // To indicate local fulfillment to the Windows Store.
        break;
}
```

## 步骤 2：跟踪可消费应用内产品的本地实施

当授权客户访问可消费应用内产品时，持续跟踪实施的产品 (*productId*) 以及跟踪与实施相关联的交易 (*transactionId*) 非常重要。

**重要提示** 应用负责向应用商店准确报告实施情况。 对于维护客户公平、可靠的购买体验来说，此步骤非常重要。

以下示例说明了使用上一步骤中 [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263381) 调用的 [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392) 属性，以标识要实施的已购买产品。 使用一个数组将产品信息存储在可供稍后引用的位置中，以便确认成功完成本地的实施。

```CSharp
private void GrantFeatureLocally(string productId, Guid transactionId)
{
    if (!grantedConsumableTransactionIds.ContainsKey(productId))
    {
        grantedConsumableTransactionIds.Add(productId, new List<Guid>());
    }
    grantedConsumableTransactionIds[productId].Add(transactionId);

    // Grant the user their content. You will likely increase some kind of gold/coins/some other asset count.
}
```

下一个示例显示了如何使用上一个示例的数组来访问产品 ID/交易 ID 对，稍后将使用它们向应用商店报告实施情况。

**重要提示** 无论应用使用何种方法来跟踪和确认实施，应用都必须恪尽职守，确保客户不会为没有收到的项目付费。

```CSharp
private Boolean IsLocallyFulfilled(string productId, Guid transactionId)
{
    return grantedConsumableTransactionIds.ContainsKey(productId) && grantedConsumableTransactionIds[productId].Contains(transactionId);
}
```

## 步骤 3：向应用商店报告产品实施情况

完成本地实施后，应用必须调用 [**ReportConsumableFulfillmentAsync**](https://msdn.microsoft.com/library/windows/apps/dn263380)，其中包含 *productId* 和包括产品购买在内的交易。

**重要提示** 如果无法向应用商店报告已实施的可消费应用内产品，则会导致在上次购买的实施情况得到报告之前，用户无法再次购买该产品。

```CSharp
FulfillmentResult result = await CurrentAppSimulator.ReportConsumableFulfillmentAsync("product2", product2TempTransactionId);
```

## 步骤 4：标识未实施的购买

你的应用可以随时使用 [**GetUnfulfilledConsumablesAsync**](https://msdn.microsoft.com/library/windows/apps/dn263379) 方法来检查是否存在尚未实施的可消费应用内产品。 你应该定期调用此方法，以便检查是否存在由未参与的应用事件（如网络连接中断或应用终止）所导致的未实施的消费品。

以下示例说明了如何使用 [**GetUnfulfilledConsumablesAsync**](https://msdn.microsoft.com/library/windows/apps/dn263379) 枚举未实施的消费品，以及应用如何通过此列表进行迭代以便完成本地实施。

```CSharp
private async void GetUnfulfilledConsumables()
{
    products = await CurrentApp.GetUnfulfilledConsumablesAsync();

    foreach (UnfulfilledConsumable product in products)
    {
        logMessage += "\nProduct Id: " + product.ProductId + " Transaction Id: " + product.TransactionId;
        // This is where you would pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
    // To indicate local fulfillment to the Windows Store.
    }
}
```

## 相关主题

* [启用应用内产品购买](enable-in-app-product-purchases.md)
* [应用商店示例（演示试用版和应用内购买）](http://go.microsoft.com/fwlink/p/?LinkID=627610)
* [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197)
 

 






<!--HONumber=May16_HO2-->



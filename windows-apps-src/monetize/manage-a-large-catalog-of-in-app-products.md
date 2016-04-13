---
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
如果你的应用提供较大的应用内产品目录，你可以选择按照本主题中描述的过程来帮助管理你的目录。
管理应用内产品的大目录
---

# 管理应用内产品的大目录


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

如果你的应用提供较大的应用内产品目录，你可以选择按照本主题中描述的过程来帮助管理你的目录。 你将创建少量特定价格段的产品条目，每个条目都能表示目录中数以百计的产品。

若要启用此功能，请使用 [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382) 方法重载，它指定了与应用商店中列出的应用内产品相关联的应用定义的产品/服务。 除了在调用期间指定付费内容和产品关联之外，你的应用还应当传递包含大目录付费内容详细信息的 [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263384) 对象。 如果未提供上述详细信息，则将改用已列出产品的详细信息。

应用商店将仅使用生成的 [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392) 的购买请求中的 *offerId*。 此过程不会直接修改[在应用商店中列出应用内产品](https://msdn.microsoft.com/library/windows/apps/mt148551)时提供的原始信息。

**注意** 从 Windows 10 开始，应用商店将不会限制每个开发者帐户所列出的产品数量。 在以前的版本中，应用商店限制每个开发者帐户可以列出 200 个产品，并且本主题中所述的过程可以用于解决此限制。

## 先决条件

-   本主题介绍应用商店对于使用应用商店中列出的单个应用内产品表现多个应用内付费内容的支持。 如果你不熟悉应用内购买，请查看[启用应用内产品购买](enable-in-app-product-purchases.md)，以了解许可证信息以及如何在应用商店中恰当地列出你的应用内产品。
-   首次编码和测试新应用内付费内容时，必须使用 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 对象而不是 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 对象。 这样，你可以使用对许可证服务器的模拟调用验证许可证逻辑，而不是调用实时服务器。 若要实现此目的，需要在 %userprofile%\\AppData\\local\\packages\\<程序包名称>\\LocalState\\Microsoft\\Windows Store\\ApiData 中自定义名为“WindowsStoreProxy.xml”的文件。 Microsoft Visual Studio 仿真器会在你首次运行应用时创建此文件，你也可以在运行时加载一个自定义文件。 有关详细信息，请参阅 **CurrentAppSimulator**。
-   本主题还参考了[应用商店示例](http://go.microsoft.com/fwlink/p/?LinkID=627610)中提供的代码示例。 若要获得为通用 Windows 平台 (UWP) 应用提供的不同货币化选项的实际体验，此示例是一个不错的选择。

## 提出针对应用内产品的购买请求

针对大目录内特定产品的购买请求的处理方式与任何其他应用内购买请求的处理方式相同。 当你的应用调用新的 [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382) 方法重载时，你的应用将提供 *OfferId* 和由应用内产品名称填充的 [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263390) 对象。

```CSharp
string offerId = "1234";
string displayPropertiesName = "MusicOffer1";
var displayProperties = new ProductPurchaseDisplayProperties(displayPropertiesName);

try
{
    PurchaseResults purchaseResults = await CurrentAppSimulator.RequestProductPurchaseAsync("product1", offerId, displayProperties);
    switch (purchaseResults.Status)
    {
        case ProductPurchaseStatus.Succeeded:
            // Grant the user their purchase here, and then pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
            // To indicate local fulfillment to the Windows Store.
            break;
        case ProductPurchaseStatus.NotFulfilled:
            // First check for unfulfilled purchases and grant any unfulfilled purchases from an earlier transaction.
            // Once products are fulfilled pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
            // To indicate local fulfillment to the Windows Store.
            break;
        case ProductPurchaseStatus.NotPurchased:
            // Notify user that the purchase was not completed due to cancellation or error.
            break;
    }
}
catch (Exception)
{
    //Notify the user of the purchase error.
}
```

## 报告应用产品/服务的实施情况

在本地实施付费内容后，你的应用需要立即向应用商店报告产品实施情况。 在大目录场景中，如果你的应用未报告付费内容实施情况，用户将无法使用相同的应用商店产品列表来购买任何应用内付费内容。

如前所述，应用商店仅使用提供的产品/服务信息来填充 [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392)，并且不在大目录产品/服务和应用商店产品列表之间创建永久性关联。 因此，你需要追踪产品的用户权利，并向 [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382) 操作之外的用户提供特定于产品的上下文（例如正在购买的项目名称或关于它的详细信息）。

下列代码将演示实施调用，以及插入特定产品/服务信息所采用的 UI 消息模式。 当不存在该特定产品信息时，示例将使用产品 [**ListingInformation**](https://msdn.microsoft.com/library/windows/apps/br225163) 中的信息。

```CSharp
string offerId = "1234";
product1ListingName = product1.Name;
string displayPropertiesName = "MusicOffer1";

if (String.IsNullOrEmpty(displayPropertiesName))
{
    displayPropertiesName = product1ListingName;
}
var offerIdMsg = " with offer id " + offerId;
if (String.IsNullOrEmpty(offerId))
{
    offerIdMsg = " with no offer id";
}

FulfillmentResult result = await CurrentAppSimulator.ReportConsumableFulfillmentAsync(productId, transactionId);
switch (result)
{
    case FulfillmentResult.Succeeded:
        Log("You bought and fulfilled " + displayPropertiesName + offerIdMsg, NotifyType.StatusMessage);
        break;
    case FulfillmentResult.NothingToFulfill:
        Log("There is no purchased product 1 to fulfill.", NotifyType.StatusMessage);
        break;
    case FulfillmentResult.PurchasePending:
        Log("You bought product 1. The purchase is pending so we cannot fulfill the product.", NotifyType.StatusMessage);
        break;
    case FulfillmentResult.PurchaseReverted:
        Log("You bought product 1. But your purchase has been reverted.", NotifyType.StatusMessage);
        // Since the user' s purchase was revoked, they got their money back.
        // You may want to revoke the user' s access to the consumable content that was granted.
        break;
    case FulfillmentResult.ServerError:
        Log("You bought product 1. There was an error when fulfilling.", NotifyType.StatusMessage);
        break;
}
```

## 相关主题

* [启用应用内产品购买](enable-in-app-product-purchases.md)
* [启用可消费应用内产品购买](enable-consumable-in-app-product-purchases.md)
* [应用商店示例（演示试用版和应用内购买）](http://go.microsoft.com/fwlink/p/?LinkID=627610)
* [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382)
* [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263384)


<!--HONumber=Mar16_HO1-->



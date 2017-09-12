---
author: mcleanbyron
ms.assetid: 9F0A59A1-FAD7-4AD5-B78B-C1280F215D23
description: "使用 Windows 应用商店定向优惠 API 来声明为应用提供的定向优惠。"
title: "使用应用商店服务管理定向优惠"
ms.author: mcleans
ms.date: 05/11/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 应用商店服务, Windows 应用商店定向优惠 API, 定向优惠"
ms.openlocfilehash: 684c37d4439f415ad607b7f3e6a166966cc9f835
ms.sourcegitcommit: eaacc472317eef343b764d17e57ef24389dd1cc3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/17/2017
---
# <a name="manage-targeted-offers-using-store-services"></a>使用应用商店服务管理定向优惠

如果你在 Windows 开发人员中心仪表板的**参与 > 定向优惠**页面中为应用创建了*定向优惠*，请在应用代码中使用 *Windows 应用商店定向优惠 API* 为此定向优惠实现应用内体验。 有关定向优惠和如何在仪表板中创建定向优惠的更多信息，请参阅[使用定向优惠最大化参与度和转换率](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)。

定向优惠 API 是一种 REST API，你可用它执行以下任务：

* 获取为当前用户提供的定向优惠，具体取决于用户是否属于该定向优惠所针对的客户细分的一部分。
* 如果用户购买了定向优惠，则你可以向应用商店提交声明，以便获得与该定向优惠相关的好处。 仅当定向优惠与 Microsoft 支持的、将为每次成功转换向开发人员支付好处的促销关联时才需要提交声明。

若要在你的应用中使用此 API，请执行以下步骤：

1.  为当前已登录应用的用户[获取 Microsoft 帐户令牌](#obtain-a-microsoft-account-token)。
2.  [为当前用户获取定向优惠](#get-targeted-offers)。
3.  [购买与定向优惠关联的加载项](#purchase-add-on)。
3.  [声明定向优惠](#claim-targeted-offer)（如果定向优惠与 Microsoft 支持的、将会为每次成功转换向开发人员支付好处的促销关联）。

> [!NOTE]
> 部分开发者帐户目前还不能将定向优惠与 Microsoft 支持的促销关联并提交优惠声明。

有关演示所有上述步骤的完整代码示例，请参阅本文末尾的[代码示例](#code-example)。 以下部分提供有关每个步骤的更多详细信息。

<span id="obtain-a-microsoft-account-token" />
## <a name="get-a-microsoft-account-token-for-the-current-user"></a>为当前用户获取 Microsoft 帐户令牌

在你的应用代码中，为当前已登录的用户获取 Microsoft 帐户 (MSA) 令牌。 对于 Windows 应用商店定向优惠 API 中的每种方法，你都必须将此令牌传递到 ```Authorization``` 请求标头中。 此令牌供应用商店用于检索为当前用户提供的定向优惠。

若要获取 MSA 令牌，请使用 [WebAuthenticationCoreManager](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webauthenticationcoremanager) 类来获取使用范围为 ```devcenter_implicit.basic,wl.basic``` 的令牌。 以下示例展示了如何进行此操作。 此示例是[完整示例](#code-example)]的一个片段，它需要完整示例中提供的 **using** 语句。

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetMSAToken)]

有关获取 MSA 令牌的详细信息，请参阅 [Web 帐户管理器](../security/web-account-manager.md)。

<span id="get-targeted-offers" />
## <a name="get-the-targeted-offers-for-the-current-user"></a>为当前用户获取定向优惠

为当前用户获得了 MSA 令牌之后，请调用 ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` URI 的 GET 方法为当前用户获取可用的定向优惠。 有关此 REST 方法的更多信息，请参阅[获取定向优惠](get-targeted-offers.md)。

此方法将会返回与针对当前用户提供的定向优惠关联的加载项产品 ID。 你可以通过这些信息将一个或多个定向优惠作为应用内购买提供给用户。 此方法将会返回一个可用于向应用商店[提交声明](#claim-targeted-offer)的跟踪 ID，从而使你可以接收与定向优惠之一关联的任何好处。

下面的示例演示如何为当前用户获取定向优惠。 此示例是[完整示例](#code-example)的一个片段。 它需要 Newtonsoft 的 [Json.NET](http://www.newtonsoft.com/json) 库和完整示例中提供的其他类和 **using** 语句。

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffers)]

<span id="purchase-add-on" />
## <a name="purchase-the-add-on-that-is-associated-with-a-targeted-offer"></a>购买与定向优惠关联的加载项

接着，向用户提供可供购买的定向优惠之一。 如果用户同意购买定向优惠，请使用以下方法之一，以编程方式购买与该定向优惠关联的加载项。 使用你在上一步中检索到的产品 ID 值来确定想要购买的加载项。

* 如果你的应用针对的是 Windows 10 版本 1607 或更高版本，我们建议你使用 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) 命名空间中的 **RequestPurchaseAsync** 方法之一。 有关使用这些方法的更多信息，请参阅[启用应用和加载项的应用内购买](enable-in-app-purchases-of-apps-and-add-ons.md)。

* 如果你的应用针对的是更早版本的 Windows 10，请使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空间中的 [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_RequestProductPurchaseAsync_System_String_) 方法来购买加载项。 有关使用此方法的详细信息，请参阅[启用应用内产品购买](enable-in-app-product-purchases.md)。

有关演示每个选项的代码示例，请参阅本文末尾的[代码示例](#code-example)。

> [!NOTE]
> 在 UWP 应用中可以通过两种不同的命名空间实现应用内购买：[Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)（包括在 Windows 10 版本 1607 中）和 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)（所有 Windows 10 版本均可用）。 有关这些命名空间之间的差异的详细信息，请参阅[应用内购买和试用](in-app-purchases-and-trials.md)。

<span id="claim-targeted-offer" />
## <a name="submit-a-claim-for-a-targeted-offer"></a>提交定向优惠声明

如果用户购买的定向优惠与 Microsoft 支持的、将会为每次成功转换向开发人员支付好处的促销关联，则你必须向应用商店提交声明才能收到你的好处。 提交声明时，你必须提供表示购买确认的值。 获取此购买确认的方式取决于你的应用在购买加载项时使用的是 **Windows.ApplicationModel.Store** 命名空间还是 **Windows.Services.Store** 命名空间中的 API。

> [!NOTE]
> 部分开发者帐户目前还不能将定向优惠与 Microsoft 支持的促销关联并提交优惠声明。

### <a name="apps-that-use-the-windowsapplicationmodelstore-namespace"></a>使用 Windows.ApplicationModel.Store 命名空间的应用

1. 使用 **Windows.ApplicationModel.Store** 命名空间中的 [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_RequestProductPurchaseAsync_System_String_) 方法购买加载项后，请务必检索[购买收据](use-receipts-to-verify-product-purchases.md)。 此收据由 **RequestProductPurchaseAsync** 方法返回的 [PurchaseResults.ReceiptXml](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.purchaseresults#Windows_ApplicationModel_Store_PurchaseResults_ReceiptXml_) 对象提供。 此收据代表你将在下面的步骤中使用的购买确认。

2. 向 ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` URI 发送一条 POST 消息，提交定向优惠转换声明。 有关此 REST 方法的详细信息，请参阅[声明定向优惠](claim-a-targeted-offer.md)。 在请求正文中，传递以下数据：

  * *storeOffer* 字段：传递你在[获取定向优惠](get-targeted-offers.md)方法的请求正文中收到的 JSON 对象之一（此对象应包含 *offers* 和 *trackingId* 字段）。

  * *receipt* 字段：传递你在上一步中检索的收据字符串（位于 **RequestProductPurchaseAsync** 方法返回的 **PurchaseResults.ReceiptXml** 对象中）。

下面的示例演示如何使用 **Windows.ApplicationModel.Store** 命名空间中的 API 购买和声明定向优惠。 此示例是[完整示例](#code-example)的一个片段。 它需要 Newtonsoft 的 [Json.NET](http://www.newtonsoft.com/json) 库和完整示例中提供的其他类和 **using** 语句。

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#ClaimOfferOnAnyVersionWindows10)]

### <a name="apps-that-use-the-windowsservicesstore-namespace"></a>使用 Windows.Services.Store 命名空间的应用

1. 使用 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) 命名空间中的 **RequestPurchaseAsync** 方法之一购买加载项后，按照以下步骤获取购买的 *order ID*。 此值代表购买确认。

  1. 调用 [GetUserCollectionAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext#Windows_Services_Store_StoreContext_GetUserCollectionAsync_Windows_Foundation_Collections_IIterable_System_String__) 方法获取 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 对象集合，它代表用户已购买的加载项。

  2. 在此集合中，获取 **StoreProduct** 对象，它代表为此定向优惠购买的加载项。

  3. 获取此 **StoreProduct** 对象的 [ExtendedJsonData](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct#Windows_Services_Store_StoreProduct_ExtendedJsonData_) 属性。 此属性返回一个 JSON 格式的字符串，其中包含此加载项完整的应用商店相关数据。

  4. 解析此 JSON 字符串并检索字符串中 *orderId* 字段的值。

2. 向 ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` URI 发送一条 POST 消息，提交定向优惠转换声明。 有关此 REST 方法的详细信息，请参阅[声明定向优惠](claim-a-targeted-offer.md)。 在请求正文中，传递以下对象：

  * *storeOffer* 字段：传递你在[获取定向优惠](get-targeted-offers.md)方法的请求正文中收到的 JSON 对象之一（此对象应包含 *offers* 和 *trackingId* 字段）。

  * *receipt* 字段：传递你在上一步中检索的 *orderId* 值。

下面的示例演示如何使用 **Windows.Services.Store** 命名空间中的 API 购买和声明定向优惠。 此示例是[完整示例](#code-example)的一个片段。 它需要 Newtonsoft 的 [Json.NET](http://www.newtonsoft.com/json) 库和完整示例中提供的其他类和 **using** 语句。

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#ClaimOfferOnWindows1607AndLater)]

<span id="code-example" />
## <a name="complete-code-example"></a>完整代码示例

以下代码示例演示了以下任务：

* 为当前用户获取 MSA 令牌。
* 通过使用[获取定向优惠](get-targeted-offers.md)方法为当前用户获取所有定向优惠。
* 购买与定向优惠关联的加载项。
* 通过使用[声明定向优惠](claim-a-targeted-offer.md)方法来声明定向优惠。

此示例需要 Newtonsoft 的 [Json.NET](http://www.newtonsoft.com/json) 库。 此示例使用此库对 JSON 格式的数据进行序列化和反序列化。

> [!NOTE]
> 在 UWP 应用中可以通过两种不同的命名空间实现应用内购买：[Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)（包括在 Windows 10 版本 1607 中）和 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)（所有 Windows 10 版本均可用）。 此示例使用[版本自适应代码](../debug-test-perf/version-adaptive-code.md)在同一个应用中使用这两个命名空间购买加载项和提交定向优惠声明。 虽然最低操作系统版本可能为早期版本，以允许你的应用在 Windows 10 早期版本上运行；但若要使用此代码，你的项目的目标操作系统版本必须为 **Windows 10 周年版本（10.0；内部版本 14394）**或更高版本。 有关这些命名空间之间的差异的详细信息，请参阅[应用内购买和试用](in-app-purchases-and-trials.md)。

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffersSample)]

## <a name="related-topics"></a>相关主题

* [使用定向优惠最大程度地提高参与度和转换率](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)
* [获取定向优惠](get-targeted-offers.md)
* [声明定向优惠](claim-a-targeted-offer.md)

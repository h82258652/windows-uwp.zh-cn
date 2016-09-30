---
author: jnHs
Description: "IAP 通过 Windows 开发人员中心仪表板发布。"
title: "IAP 提交"
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
translationtype: Human Translation
ms.sourcegitcommit: 97f4aee47cab9064ac053e7a6e16441d6960d41f
ms.openlocfilehash: 4a1764dfb8f94409aba973a28ba2999854179196

---

# IAP 提交


IAP（应用内产品）是你的应用的补充项，可供客户购买。 IAP 可以是有趣的新附加功能、新的游戏级别或任何你希望用来维系用户的内容。 IAP 不但是赚钱的绝佳方法，它们还有助于促进客户互动和参与。

IAP 通过 Windows 开发人员中心仪表板发布。 你还需要在你的应用代码中[启用 IAP](../monetize/enable-in-app-product-purchases.md)。

IAP 提交过程的第一步是通过[定义其产品类型和产品 ID](set-your-iap-product-id.md) 来在仪表板中创建 IAP。 然后，你可以创建一个提交，以便你的 IAP 可以通过 Windows 应用商店购买。 你可以在[提交应用](app-submissions.md)的同时提交 IAP，或者可以单独处理它。 并且你可以在应用在应用商店中上架后对 IAP 进行[更新](#updating-an-iap-after-submission)，而无需重新提交该应用。

## IAP 提交清单

以下是你在创建 IAP 提交时提供的信息的列表。 下面标出了你需要提供的项目。 其中某些是选填的，或已提供你可以按需更改的默认值。

### 创建新的 IAP 页面
| 字段名称                    | 注意                            | 
|-------------------------------|----------------------------------|
| [**产品类型**](set-your-iap-product-id.md#product-type)      | 必填。 如果为“持久”****，则“产品生命周期”****为必填项。 |  
| [**产品 ID**](set-your-iap-product-id.md#product-id)          | 必需 |        

### 属性页面
| 字段名称                    | 注意                              |   
|-------------------------------|------------------------------------|
| [**产品生命周期**](enter-iap-properties.md#product-lifetime)  | 在产品类型为“耐用品”****时为必需。 在产品类型为“易耗品”****时不适用。 | 
| [**内容类型**](enter-iap-properties.md#content-type)          | 必填       |               
| [**关键字**](enter-iap-properties.md#keywords)                  | 选填（最多 10 个关键字，每个限 30 个字符） | 
| [**标记**](enter-iap-properties.md#tag)                               | 选填（限 3000 个字符）             | 

### 定价和可用性页面 
| 字段名称                    | 注意                                       | 
|-------------------------------|---------------------------------------------|
| [**基价**](set-iap-pricing-and-availability.md#base-price)                | 必需                                    | 
| [**市场和自定义定价**](set-iap-pricing-and-availability.md#markets-and-custom-prices)  | 默认：在所有可能的市场中提供 | 
| [**促销价格**](put-apps-and-iaps-on-sale.md)               | 可选                             |
| [**分发和可见性**](set-iap-pricing-and-availability.md#distribution-and-visibility)   | 默认：浏览或搜索应用商店的客户可以找到 IAP | 
| [**发布日期**](set-iap-pricing-and-availability.md#publish-date)                | 默认：IAP 通过认证后立即发布 |

### 提要页面
需要一份提要。 我们建议为应用支持的每种[语言](create-iap-descriptions.md#languages)提供描述。

| 字段名称                    | 注意                                       | 
|-------------------------------|---------------------------------------------|
| [**标题**](create-iap-descriptions.md#title)                    | 必填（限 100 个字符）              |
| [**提要**](create-iap-descriptions.md#description)       | 选填（限 200 个字符）              |
| [**图标**](create-iap-descriptions.md#icon)                    | 可选（.png、300x300 像素）             | 

当你完成输入此信息时，请单击“提交到应用商店”****。 在大多数情况下，认证过程需要大约一个小时。 然后，你的 IAP 将发布到应用商店并且可供客户购买。

**注意** IAP 还必须在应用的代码中实现。 有关详细信息，请参阅[启用应用内产品购买](../monetize/enable-in-app-product-purchases.md)。


## 在发布后更新 IAP

你可以随时对已发布的 IAP 进行更改。 IAP 更改会独立于应用提交和发布，因此通常不需要更新整个应用即可对 IAP 进行更改，如更新其价格或说明。

> **重要提示** 如果你的应用要提供给 Windows 8.x 客户，你将需要创建并发布新的应用提交才能使这些客户看到 IAP 更新。 同样，如果你在应用发布后向面向 Windows 8.x 的应用添加新的 IAP，你将需要更新应用的代码来引用这些 IAP，然后才能重新提交应用。 否则，Windows 8.x 上的客户将无法看到新的 IAP。

若要提交更新，请转到仪表板中 IAP 的页面，然后单击“更新”****。 这将为该 IAP 创建新的提交，使用之前提交中的信息作为起始点。 按需更改信息，然后单击**“提交到应用商店”**。

如果你希望删除之前提供的 IAP，可通过创建新提交并将[“分发和可见性”](set-iap-pricing-and-availability.md)选项更改为**“不再提供购买。不显示在你的应用一览中”**来实现。 请确保按需更新应用代码以同时删除对 IAP 的引用。




<!--HONumber=Jun16_HO5-->



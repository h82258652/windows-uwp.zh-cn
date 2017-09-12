---
author: mcleanbyron
description: "描述用于 Windows.Services.Store 命名空间中应用商店产品的扩展 JSON 数据架构。"
title: "应用商店产品的数据架构"
ms.author: mcleans
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: 
ms.openlocfilehash: 2a294dc490a0b7bed5e426ff26dfed948f337dfa
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2017
---
# <a name="data-schemas-for-store-products"></a>应用商店产品的数据架构

当你向应用商店提交产品（如应用或加载项）时，应用商店将维护该产品及其许可的全套数据。 在你的应用代码中，可通过使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空间中的属性以编程方式访问部分数据。 例如，可以使用 [StoreProduct.Description](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_Description) 和 [StoreProduct.Price](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_Price) 属性检索当前应用或当前应用的加载项的描述和价格。

但是，应用商店中产品的许多数据都不是由 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空间中的预定义属性来公开。 要在你的代码中访问产品的完整数据，可以改用以下常规属性：

* [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_ExtendedJsonData_)
* [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku#Windows_Services_Store_StoreSku_ExtendedJsonData_)
* [StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability#Windows_Services_Store_StoreAvailability_ExtendedJsonData_)
*   [StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata#Windows_Services_Store_StoreCollectionData_ExtendedJsonData_)
*   [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense#Windows_Services_Store_StoreAppLicense_ExtendedJsonData_)
* [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense#Windows_Services_Store_StoreLicense_ExtendedJsonData_)
*   [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties#Windows_Services_Store_StorePurchaseProperties_ExtendedJsonData_)

这些属性以 JSON 格式字符串的形式返回相应对象的完整数据。 本文提供了这些属性所返回数据的完整架构。

> [!NOTE]
> 应用商店中的产品以[产品](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct)、[SKU](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) 和[可用性](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability)对象的层次结构来组织。 有关详细信息，请参阅[产品、SKU 和可用性](in-app-purchases-and-trials.md#products-skus)。

## <a name="schema-for-storeproduct-storesku-storeavailability-and-storecollectiondata"></a>StoreProduct、StoreSku、StoreAvailability 和 StoreCollectionData 的架构

以下架构描述了 [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_ExtendedJsonData_) 返回的 JSON 格式的字符串。 [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku#Windows_Services_Store_StoreSku_ExtendedJsonData_)、[StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability#Windows_Services_Store_StoreAvailability_ExtendedJsonData_) 和 [StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata#Windows_Services_Store_StoreCollectionData_ExtendedJsonData_) 属性只返回分别在 ```DisplaySkuAvailabilities\Sku```、```DisplaySkuAvailabilities\Availabilities``` 和 ```DisplaySkuAvailabilities\Sku\CollectionData``` 路径下定义的架构部分。

有关 [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_ExtendedJsonData_) 返回的 JSON 格式字符串的示例，请参阅[本部分](#product-example)。

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonData.json#L1-L603)]

<span id="product-example" />
### <a name="example"></a>示例

以下示例演示应用的 [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_ExtendedJsonData_) 属性所返回的 JSON 格式字符串。

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonDataExample.json#L1-L268)]

## <a name="schema-for-storeapplicense-and-storelicense"></a>StoreAppLicense 和 StoreLicense 的架构

以下架构描述了 [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability#Windows_Services_Store_StoreAppLicense_ExtendedJsonData_) 返回的 JSON 格式的字符串。 [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability#Windows_Services_Store_StoreLicense_ExtendedJsonData_) 属性只返回在 ```productAddOns``` 路径下定义的架构部分。

有关 [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability#Windows_Services_Store_StoreAppLicense_ExtendedJsonData_) 返回的 JSON 格式字符串的示例，请参阅[本部分](#license-example)。

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonData.json#L1-L80)]

<span id="license-example" />
### <a name="example"></a>示例

以下示例演示应用的 [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability#Windows_Services_Store_StoreAppLicense_ExtendedJsonData_) 属性所返回的 JSON 格式字符串。

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonDataExample.json#L1-L28)]

## <a name="schema-for-storepurchaseproperties"></a>StorePurchaseProperties 的架构

以下架构描述了 [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties#Windows_Services_Store_StorePurchaseProperties_ExtendedJsonData_) 返回的 JSON 格式的字符串。

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StorePurchaseProperties.ExtendedJsonData.json#L1-L12)]

## <a name="related-topics"></a>相关主题

* [应用内购买和试用](in-app-purchases-and-trials.md)
* [获取应用和加载项的产品信息](get-product-info-for-apps-and-add-ons.md)
* [获取应用和加载项的许可证信息](get-license-info-for-apps-and-add-ons.md)
* [支持应用内购买应用和加载项](enable-in-app-purchases-of-apps-and-add-ons.md)
* [支持购买易耗型加载项](enable-consumable-add-on-purchases.md)
* [实现应用的试用版](implement-a-trial-version-of-your-app.md)

---
author: jnHs
Description: "加载项通过 Windows 开发人员中心仪表板发布。"
title: "加载项提交"
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.author: wdg-dev-content
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 85ad5e297eed8daa5205aedc1253191fcff5fd49
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2017
---
# <a name="add-on-submissions"></a>加载项提交

加载项（有时称为应用内产品）是应用的补充项，可供客户购买。 加载项可以是有趣的新附加功能、新的游戏级别或任何你希望用来维系用户的内容。 加载项不但是赚钱的绝佳方法，它们还有助于促进客户互动和参与。

加载项通过 Windows 开发人员中心仪表板发布。 你还需要在你的应用代码中[启用加载项](../monetize/in-app-purchases-and-trials.md)。

加载项提交过程的第一步是通过[定义其产品类型和产品 ID](set-your-add-on-product-id.md) 来在仪表板中创建加载项。 然后，你可以创建一个提交，以便你的加载项可以通过 Windows 应用商店购买。 你可以在[提交应用](app-submissions.md)的同时提交加载项，或者可以单独处理它。 并且你可以在应用在应用商店中上架后[更新](#updating-an-add-on-after-publication)加载项，而无需重新提交该应用。

> [!NOTE]
> 文档的此部分介绍如何在开发人员中心仪表板中提交加载项。 此外，你也可以使用 [Windows Store 提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) 自动执行加载项提交。

## <a name="checklist-for-submitting-an-add-on"></a>加载项提交清单

以下是你在创建加载项提交时提供的信息的列表。 下面标出了你需要提供的项目。 其中某些是选填的，或已提供你可以按需更改的默认值。

### <a name="create-a-new-add-on-page"></a>创建新加载项页面
| 字段名称                    | 注意                            |
|-------------------------------|----------------------------------|
| [**产品类型**](set-your-add-on-product-id.md#product-type)      | 必填。 如果为**持久**，则**产品生命周期**为必填项。 |  
| [**产品 ID**](set-your-add-on-product-id.md#product-id)          | 必需 |        

<span/>

### <a name="properties-page"></a>属性页面
| 字段名称                    | 注意                              |   
|-------------------------------|------------------------------------|
| [**产品生命周期**](enter-add-on-properties.md#product-lifetime)  | 在产品类型为**耐用品**时必填。 不适用于其他产品类型。 |
| [**数量**](enter-add-on-properties.md#quantity)  | 在产品为**应用商店管理的易耗品**必填。 不适用于其他产品类型。
| [**内容类型**](enter-add-on-properties.md#content-type)          | 必填       |               
| [**关键字**](enter-add-on-properties.md#keywords)                  | 选填（最多 10 个关键字，每个限 30 个字符） |
| [**自定义开发人员数据**](enter-add-on-properties.md#custom-developer-data)                               | 选填（限 3000 个字符）             |

<span/>

### <a name="pricing-and-availability-page"></a>定价和可用性页面
| 字段名称                    | 注意                                       |
|-------------------------------|---------------------------------------------|
| [**市场**](set-add-on-pricing-and-availability.md#markets)  | 默认：所有可能的市场 |
| [**可见性**](set-add-on-pricing-and-availability.md#visibility)   | 默认：可供购买。 可能在应用一览中显示 |
| [**计划**](set-add-on-pricing-and-availability.md#schedule)    | 默认：尽快发布
| [**定价**](set-add-on-pricing-and-availability.md#pricing)                | 必填                                    |
| [**销售定价**](put-apps-and-add-ons-on-sale.md)               | 可选                             |
| [**发布日期**](set-add-on-pricing-and-availability.md#publish-date)                | 默认：尽快发布 |

<span/>

### <a name="store-listings"></a>应用商店一览
需要一个应用商店一览。 我们建议为应用支持的每种[语言](create-add-on-store-listings.md#store-listing-languages)提供应用商店一览。

| 字段名称                    | 注意                                       |
|-------------------------------|---------------------------------------------|
| [**标题**](create-add-on-store-listings.md#title)                    | 必填（限 100 个字符）              |
| [**提要**](create-add-on-store-listings.md#description)       | 选填（限 200 个字符）              |
| [**图标**](create-add-on-store-listings.md#icon)                    | 可选（.png、300x300 像素）             |

<span/>

当你完成输入此信息时，请单击**提交到应用商店**。 在大多数情况下，认证过程需要大约一个小时。 然后，你的加载项将发布到应用商店并且可供客户购买。

> [!NOTE]
> 加载项还必须在应用的代码中实现。 有关详细信息，请参阅[应用内购买和试用](../monetize/in-app-purchases-and-trials.md)。


## <a name="updating-an-add-on-after-publication"></a>在发布后更新加载项

你可以随时更改已发布的加载项。 加载项更改会独立于应用提交和发布，因此通常不需要更新整个应用即可对加载项进行更改，如更新其价格或说明。

> [!IMPORTANT]
> 如果你的应用要提供给使用 Windows 8.x 的客户，将需要创建并发布新的应用提交才能使这些客户看到加载项更新。 同样，如果你在应用发布后向面向 Windows8.x 的应用添加新的加载项，你将需要更新应用的代码来引用这些加载项，然后才能重新提交应用。 否则，使用 Windows8.x 的客户将无法看到新的加载项。

若要提交更新，请转到仪表板中的加载项页面，然后单击**更新**。 这将使用之前提交中的信息作为起始点，为该加载项创建新的提交。 按需更改信息，然后单击**提交到应用商店**。

如果你希望删除之前提供的加载项，可通过创建新提交并将[分发和可见性](set-add-on-pricing-and-availability.md)选项更改为**不再提供购买。不显示在你的应用一览中**来实现。 请确保按需更新应用代码以同时删除对加载项的引用。

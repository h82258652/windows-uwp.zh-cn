---
Description: Add-ons (or in-app products) are published through Partner Center.
title: 加载项提交
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, iap, 应用内购买, 应用内产品, iap 提交
ms.localizationpriority: medium
ms.openlocfilehash: 87e5298f677a078eb1d6f89b72af3ea15d44b75f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259058"
---
# <a name="add-on-submissions"></a>加载项提交

加载项（有时称为应用内产品）是应用的补充项，可供客户购买。 An add-on can be a fun new feature, a new game level, or anything else you think will keep users engaged. 加载项不但是赚钱的绝佳方法，它们还有助于促进客户互动和参与。

Add-ons are published through [Partner Center](https://partner.microsoft.com/dashboard), and require you to have an active [developer account](https://developer.microsoft.com/store/register). 你还需要在你的应用代码中[启用加载项](../monetize/in-app-purchases-and-trials.md)。

The first step in the add-on submission process is to create the add-on in Partner Center by [defining its product type and product ID](set-your-add-on-product-id.md). After that, you'll create a submission so that your add-on can be purchased via the Microsoft Store. 你可以在[提交应用](app-submissions.md)的同时提交加载项，或者可以单独处理它。 并且你可以在应用在 Store 中上架后[更新](#updating-an-add-on-after-publication)加载项，而无需重新提交该应用。

> [!NOTE]
> This section of the documentation describes how to submit add-ons in Partner Center. 此外，你也可以使用 [Microsoft Store 提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) 自动执行加载项提交。


## <a name="checklist-for-submitting-an-add-on"></a>加载项提交清单

以下是你在创建加载项提交时提供的信息的列表。 下面标出了你需要提供的项目。 其中某些是选填的，或已提供你可以按需更改的默认值。


### <a name="create-a-new-add-on-page"></a>创建新加载项页面

| 字段名称                    | 注释                            |
|-------------------------------|----------------------------------|
| [**Product type**](set-your-add-on-product-id.md#product-type)      | 必需 |  
| [**Product ID**](set-your-add-on-product-id.md#product-id)          | 必需 |        


### <a name="properties-page"></a>属性页面

| 字段名称                    | 注释                              |   
|-------------------------------|------------------------------------|
| [**Product lifetime**](enter-add-on-properties.md#product-lifetime)  | 在产品类型为**耐用品**时必填。 不适用于其他产品类型。 |
| [**Quantity**](enter-add-on-properties.md#quantity)  | 在产品为 **Store 管理的易耗品**必填。 不适用于其他产品类型。 |
| [**Subscription period**](enter-add-on-properties.md#subscription-period)          | 在产品类型为**订阅**时必填。 不适用于其他产品类型。       |  
| [**Free trial**](enter-add-on-properties.md#free-trial)          | 在产品类型为**订阅**时必填。 不适用于其他产品类型。       |
| [**Content type**](enter-add-on-properties.md#content-type)          | 必需    |               
| [**Keywords**](enter-add-on-properties.md#keywords)                  | 选填（最多 10 个关键字，每个限 30 个字符） |
| [**Custom developer data**](enter-add-on-properties.md#custom-developer-data)   | 选填（限 3000 个字符）            |


### <a name="pricing-and-availability-page"></a>定价和可用性页面

| 字段名称                    | 注释                                       |
|-------------------------------|---------------------------------------------|
| [**Markets**](set-add-on-pricing-and-availability.md#markets)  | 默认：所有可能的市场 |
| [**Visibility**](set-add-on-pricing-and-availability.md#visibility)   | 默认：可供购买。 可能在应用一览中显示 |
| [**Schedule**](set-add-on-pricing-and-availability.md#schedule)    | 默认：尽快发布
| [**Pricing**](set-add-on-pricing-and-availability.md#pricing)                | 必需                                    |
| [**Sale pricing**](put-apps-and-add-ons-on-sale.md)               | 可选                    |


### <a name="store-listings"></a>Store 一览

需要一个 Store 一览。 我们建议为应用支持的每种[语言](create-add-on-store-listings.md#store-listing-languages)提供 Store 一览。

| 字段名称                    | 注释                                       |
|-------------------------------|---------------------------------------------|
| [**Title**](create-add-on-store-listings.md#title)                    | 必填（限 100 个字符）           |
| [**Description**](create-add-on-store-listings.md#description)       | 选填（限 200 个字符）            |
| [**Icon**](create-add-on-store-listings.md#icon)                    | 可选（.png、300x300 像素）            |


当你完成输入此信息时，请单击**提交到 Store**。 在大多数情况下，认证过程需要大约一个小时。 然后，你的加载项将发布到 Store 并且可供客户购买。

> [!NOTE]
> 加载项还必须在应用的代码中实现。 有关详细信息，请参阅[应用内购买和试用](../monetize/in-app-purchases-and-trials.md)。


## <a name="updating-an-add-on-after-publication"></a>在发布后更新加载项

你可以随时更改已发布的加载项。 Add-on changes are submitted and published independently of your app, so you generally don't need to update the entire app in order to make changes to an add-on such as updating its price or description.

To submit updates, go to the add-on's page in Partner Center and click **Update**. This will create a new submission for the add-on, using the info from your previous submission as a starting point. Make the changes you'd like, and then click **Submit to the Store**.

如果你希望删除之前提供的加载项，可通过创建新提交并通过**停止获取**选项将[分发和可见性](set-add-on-pricing-and-availability.md)选项更改为**在 Microsoft Store 中隐藏**。 Be sure to update your app's code as needed to also remove references to the add-on (especially if your previously-published app supports Windows 8.1 earlier; this visibility setting won't apply to those customers).

> [!IMPORTANT]
> If your previously-published app is available to customers on Windows 8.x, you will need to create and publish a new app submission in order to make the add-on updates visible to those customers. 同样，如果你在应用发布后向面向 Windows 8.x 的应用添加新的加载项，你将需要更新应用的代码来引用这些加载项，然后才能重新提交应用。 否则，使用 Windows 8.x 的客户将无法看到新的加载项。

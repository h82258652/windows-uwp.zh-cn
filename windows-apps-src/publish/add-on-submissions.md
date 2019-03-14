---
Description: 通过合作伙伴中心发布外接程序 （或在应用内产品）。
title: 加载项提交
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, iap, 应用内购买, 应用内产品, iap 提交
ms.localizationpriority: medium
ms.openlocfilehash: 28383ed82c418ff15806c325d6eab5a05f9987bf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620362"
---
# <a name="add-on-submissions"></a>加载项提交

加载项（有时称为应用内产品）是应用的补充项，可供客户购买。 外接程序可以是一个有趣的新功能，您将一个新游戏级别，或任何其他将保留用户参与。 加载项不但是赚钱的绝佳方法，它们还有助于促进客户互动和参与。

外接程序通过发布[合作伙伴中心](https://partner.microsoft.com/dashboard)，并要求您有一个有效[开发人员帐户](https://go.microsoft.com/fwlink/p/?LinkId=615100)。 你还需要在你的应用代码中[启用加载项](../monetize/in-app-purchases-and-trials.md)。

外接程序提交过程的第一步是创建外接程序在合作伙伴中心按[定义其产品类型和产品 ID](set-your-add-on-product-id.md)。 之后，你将创建提交，以便可以通过 Microsoft Store 购买外接程序。 你可以在[提交应用](app-submissions.md)的同时提交加载项，或者可以单独处理它。 并且你可以在应用在应用商店中上架后[更新](#updating-an-add-on-after-publication)加载项，而无需重新提交该应用。

> [!NOTE]
> 文档本部分介绍如何将提交合作伙伴中心中的加载项。 此外，你也可以使用 [Microsoft Store 提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) 自动执行加载项提交。


## <a name="checklist-for-submitting-an-add-on"></a>加载项提交清单

以下是你在创建加载项提交时提供的信息的列表。 下面标出了你需要提供的项目。 其中某些是选填的，或已提供你可以按需更改的默认值。


### <a name="create-a-new-add-on-page"></a>创建新加载项页面

| 字段名称                    | 注释                            |
|-------------------------------|----------------------------------|
| [**产品类型**](set-your-add-on-product-id.md#product-type)      | 必需 |  
| [**产品 ID**](set-your-add-on-product-id.md#product-id)          | 必需 |        


### <a name="properties-page"></a>属性页面

| 字段名称                    | 注释                              |   
|-------------------------------|------------------------------------|
| [**产品生命周期内**](enter-add-on-properties.md#product-lifetime)  | 在产品类型为“耐用品”时必填。 不适用于其他产品类型。 |
| [**数量**](enter-add-on-properties.md#quantity)  | 在产品为“应用商店管理的易耗品”必填。 不适用于其他产品类型。 |
| [**订阅期内**](enter-add-on-properties.md#subscription-period)          | 在产品类型为**订阅**时必填。 不适用于其他产品类型。       |  
| [**免费试用版**](enter-add-on-properties.md#free-trial)          | 在产品类型为**订阅**时必填。 不适用于其他产品类型。       |
| [**内容类型**](enter-add-on-properties.md#content-type)          | 必需    |               
| [**关键字**](enter-add-on-properties.md#keywords)                  | 选填（最多 10 个关键字，每个限 30 个字符） |
| [**自定义开发人员的数据**](enter-add-on-properties.md#custom-developer-data)   | 选填（限 3000 个字符）            |


### <a name="pricing-and-availability-page"></a>定价和可用性页面

| 字段名称                    | 注释                                       |
|-------------------------------|---------------------------------------------|
| [**市场**](set-add-on-pricing-and-availability.md#markets)  | Default：所有可能的市场 |
| [**可见性**](set-add-on-pricing-and-availability.md#visibility)   | Default：可供购买。 可能在应用一览中显示 |
| [**计划**](set-add-on-pricing-and-availability.md#schedule)    | Default：尽可能快地发布
| [**定价**](set-add-on-pricing-and-availability.md#pricing)                | 必需                                    |
| [**销售定价**](put-apps-and-add-ons-on-sale.md)               | 可选                    |


### <a name="store-listings"></a>应用商店一览

需要一个应用商店一览。 我们建议为应用支持的每种[语言](create-add-on-store-listings.md#store-listing-languages)提供应用商店一览。

| 字段名称                    | 注释                                       |
|-------------------------------|---------------------------------------------|
| [**Title**](create-add-on-store-listings.md#title)                    | 必填（限 100 个字符）           |
| [**说明**](create-add-on-store-listings.md#description)       | 选填（限 200 个字符）            |
| [**Icon**](create-add-on-store-listings.md#icon)                    | 可选（.png、300x300 像素）            |


当你完成输入此信息时，请单击“提交到应用商店”。 在大多数情况下，认证过程需要大约一个小时。 然后，你的加载项将发布到应用商店并且可供客户购买。

> [!NOTE]
> 加载项还必须在应用的代码中实现。 有关详细信息，请参阅[应用内购买和试用](../monetize/in-app-purchases-and-trials.md)。


## <a name="updating-an-add-on-after-publication"></a>在发布后更新加载项

你可以随时更改已发布的加载项。 外接程序的更改提交并发布独立于您的应用程序，因此通常不需要更新整个应用程序，以便对更新其价格或说明如外接程序进行更改。

若要提交的更新，请转到合作伙伴中心中添加登录页，然后单击**更新**。 这将创建新提交的外接程序，使用来自以前提交的信息作为起始点。 进行的更改，您会喜欢，，然后单击**提交到应用商店**。

如果你希望删除之前提供的加载项，可通过创建新提交并通过**停止获取**选项将[分发和可见性](set-add-on-pricing-and-availability.md)选项更改为**在 Microsoft Store 中隐藏**。 请务必根据需要也会删除该外接程序的引用更新你的应用的代码 （尤其是如果您以前发布的应用程序支持 Windows 8.1 更早版本; 此可见性设置将不适用于这些客户）。

> [!IMPORTANT]
> 如果您以前发布的应用程序可供客户可在 Windows 8.x，您将需要创建和发布新的应用程序提交以使对这些客户可见的外接程序更新。 同样，如果你在应用发布后向面向 Windows 8.x 的应用添加新的加载项，你将需要更新应用的代码来引用这些加载项，然后才能重新提交应用。 否则，使用 Windows 8.x 的客户将无法看到新的加载项。

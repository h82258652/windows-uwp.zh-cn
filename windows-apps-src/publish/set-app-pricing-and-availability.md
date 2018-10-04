---
author: jnHs
Description: The Pricing and availability page of the app submission process lets you determine how much your app will cost, whether you'll offer a free trial, and how, when, and where it will be available to customers.
title: 设置应用定价和可用性
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
ms.author: wdg-dev-content
ms.date: 05/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 价格, 可用, 可发现, 免费试用版, 试用版, 试用, 应用, 发布日期
ms.localizationpriority: medium
ms.openlocfilehash: 20c52687b375f9bf33dd491eeb37d4142acace99
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4351713"
---
# <a name="set-app-pricing-and-availability"></a>设置应用定价和可用性


[应用提交过程](app-submissions.md)的“定价和可用性”**** 页可让你确定你的应用价格应为多少钱、是否提供免费试用以及向客户提供的方式、时间和地点。 此处，我们将演练此页面上的选项以及你在输入此信息时应考虑的事项。


## <a name="markets"></a>市场

Microsoft Store 的客户遍及世界 200 多个国家和地区。 默认情况下，我们将在所有潜在市场提供你的应用。 如果需要，可选择在特定市场提供应用。 

有关详细信息，请参阅[定义市场选择](define-pricing-and-market-selection.md)。


## <a name="visibility"></a>可见性

**可见性**部分允许你设置对如何发现和获取你的应用的限制，包括用户是能够在 Microsoft Store 中找到应用还是能够查看应用的 Microsoft Store 一览。

有关详细信息，请参阅[选择可见性选项](choose-visibility-options.md)。


## <a name="schedule"></a>计划

默认情况下（除非选择了[可见性](choose-visibility-options.md#discoverability)部分中**在 Microsoft Store 中提供此产品，但它不可被发现**选项中的一个），你的应用一旦通过认证并完成发布流程，客户便可马上获取你的应用。 若要选择其他日期，请选择**显示选项**以展开此部分。 

有关详细信息，请参阅[配置精确的发布计划](configure-precise-release-scheduling.md)。


## <a name="pricing"></a>定价

需要为应用选择一个基本价格（除非选择了[可见性](choose-visibility-options.md#discoverability)部分中**在 Microsoft Store 中提供此产品，但它不可被发现**下的**停止获取**选项），可选择**免费**或任一可选的价格段。 还可计划价格更改，以指示应用价格应更改的日期和时间。 此外，还可选择针对特定市场自定义这些更改。 

有关详细信息，请参阅[设置和计划应用定价](set-and-schedule-app-pricing.md)。


## <a name="free-trial"></a>免费试用

许多开发人员选择允许客户使用应用商店提供的试用功能免费试用他们的应用。 默认情况下，将选择**没有免费试用**，这样你的应用则不提供免费试用。 如果想要提供试用，可从**免费试用**下拉列表中选择一个值。

有两种类型的试用可供选择，并且可选择配置开始提供和停止提供试用的日期和时间。

### <a name="time-limited"></a>限期提供

选择**限期提供**以使客户能够在指定的一段时间内免费试用应用：**1 天**，**7 天**，**15 天**或 **30 天**。 可通过向[排除或限制试用版中的功能](../monetize/in-app-purchases-and-trials.md)中添加代码来限制功能，也可允许客户在试用期间访问完整功能。 
> [!NOTE]
> 使用 Windows 10（内部版本号为 10.0.10586 或更早版本）系统或 Windows Phone 8.1 及更早版本系统的客户将看不到限期提供的试用版。

### <a name="unlimited"></a>无限制

选择**无限制**，即可让客户无限期免费访问你的应用。 如果你想要鼓励他们购买完整版本，请确保添加相关代码以[排除或限制试用版中的某些功能](../monetize/in-app-purchases-and-trials.md)。

### <a name="start-and-end-dates"></a>开始和结束日期

默认情况下，应用一经发布，其试用版将立即可用，并将永远提供。 如果需要，可指定开始提供和停止提供试用版的日期和时间。 

>[!NOTE]
> 这些日期仅适用于使用 Windows 10（包括 Xbox）的客户。 如果你的应用对使用更早操作系统版本的客户可用，则只要产品可用，试用版将一直向这些客户提供。 

要设置何时应将试用版提供给 Windows 10 上的客户的日期，请将**开始日期**和/或**结束日期**下拉列表更改为**开始/结束时间**，然后选择日期和时间。 如果执行此操作，可选择 **UTC**，这样所选择的时间便为协调世界时 (UTC)，或选择**本地**，这样这些时间便会应用于分别与某个市场相关联的各时区。 （请注意，对于跨多个时区的市场，将仅使用该市场中的某一个时区。 对于美国东部时区使用。）如果你想要设置的任何市场的不同日期，你可以选择**针对特定市场自定义**。


## <a name="sale-pricing"></a>销售定价

如果你想要限时降价出售你的应用，可以创建和计划一次促销。

有关详细信息，请参阅[打折出售应用和加载项](put-apps-and-add-ons-on-sale.md)。


## <a name="organizational-licensing"></a>组织授权

默认情况下，你的应用将提供给要批量购买的组织。 你可以指示是否在本部分中提供你的应用以及如何提供它。

有关详细信息，请参阅[组织授权选项](organizational-licensing.md)。


## <a name="publish-date"></a>发布日期

以前，**发布日期**部分出现在此页面上。 现在可以在[提交选项](manage-submission-options.md)页的**发布暂停选项**部分找到此功能。 （请注意，要控制应用应在何时发布到 Microsoft Store，我们建议使用**定价和可用性**页面的[计划](configure-precise-release-scheduling.md)部分。）



---
description: 了解如何使用 Windows.Services.Store 命名空间实现订阅加载项。
title: 为应用启用订阅加载项
keywords: windows 10, uwp, 订阅, 加载项, 应用内购买, IAP, Windows.Services.Store
ms.date: 12/06/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ba436ab760f589debeaf6909acd64d61df89a43d
ms.sourcegitcommit: 912146681b1befc43e6db6e06d1e3317e5987592
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79295730"
---
# <a name="enable-subscription-add-ons-for-your-app"></a>为应用启用订阅加载项

通用 Windows 平台 (UWP) 应用可以向客户提供*订阅*加载项的应用内购买。 你可以按照自动定期帐单期间间使用订阅在你的应用中销售数字产品（如应用功能或数字内容）。

> [!NOTE]
> 若要支持应用内的订阅加载项购买，你的应用必须在 Visual Studio 中面向 **Windows 10 周年纪念版（10.0；版本 14393）** 或更高版本（这对应于 Windows 10 版本 1607），并且必须使用 **Windows.Services.Store** 命名空间（而不是 **Windows.ApplicationModel.Store** 命名空间）中的 API 来实现应用内购买体验。 有关这些命名空间之间的差异的详细信息，请参阅[应用内购买和试用](in-app-purchases-and-trials.md)。

## <a name="feature-highlights"></a>功能特点

UWP 应用的订阅加载项支持以下功能：

* 你可以选择订阅期为 1 个月、3 个月、6 个月、1 年或 2 年。
* 你可以在你的订阅中添加 1 周或 1 个月的免费试用期间。
* Windows SDK [提供了一些 API](#code-examples)，可用于在应用中获取有关应用的可用订阅加载项的信息并支持购买订阅加载项。 我们还提供你可以从你的服务调用的 REST API 来[管理用户订阅](#manage-subscriptions)。
* 你可以查看分析报告，其中提供了订阅购置、活动订户以及在指定的时间段内取消的订阅的数量。
* 客户可以在他们的 Microsoft 帐户的 [https://account.microsoft.com/services](https://account.microsoft.com/services) 页面管理他们的订阅。 客户可以使用此页面查看他们购买的所有订阅、取消订阅，以及更改与其订阅关联的付款方式。

## <a name="steps-to-enable-a-subscription-add-on-for-your-app"></a>为你的应用启用订阅加载项的步骤

若要在你的应用中支持购买订阅加载项，请执行以下步骤。

1. 在合作伙伴中心为订阅[创建附加项提交](../publish/add-on-submissions.md)，并发布提交。 在你执行加载项提交流程时，请密切注意以下属性：

    * [产品类型](../publish/set-your-add-on-product-id.md#product-type)：确保选择**订阅**。

    * [订阅期](../publish/enter-add-on-properties.md#subscription-period)：为你的订阅选择定期帐单期间。 在发布加载项之后，你将不能更改订阅期。

        每个订阅加载项都支持单个订阅期和试用期。 你必须在应用中为你想要提供的每种订阅创建不同的订阅加载项。 例如，如果你想要提供按月订阅无试用、按月订阅送一个月试用、按年订阅无试用以及按年订阅送一个月试用，则需要创建四个订阅加载项。

    * [试用期间](../publish/enter-add-on-properties.md#free-trial)：考虑为你的订阅选择 1 周或 1 个月的试用期，让用户可以在购买之前先试用。 在发布订阅加载项之后，你将不能更改或删除试用期。

        要免费试用你的订阅，用户必须通过标准的应用内购买流程（包括有效的付款方式）购买你的订阅。 在试用期内不收取任何费用。 在试用期结束时，订阅自动将转换为完全订阅，并将按用户的支付方式收取第一期的付费订阅费用。 如果用户在试用期内选择取消订阅，则订阅在试用期结束之前仍然会保持有效。 一些试用期并非对所有订阅期都可用。

        > [!NOTE]
        > 每位客户一次只能获取一个订阅加载项的免费试用版。 客户获得某项订阅的免费试用版后，Microsoft Store 将阻止该客户再次获取相同的免费试用订阅。

    * [可见性](../publish/set-add-on-pricing-and-availability.md#visibility)：如果要创建测试加载项并且仅用它来测试订阅的应用内购买体验，建议你选择一个**在 Microsoft Store 中隐藏**选项。 否则，你可以选择最适合你的情形的可见性选项。

    * [定价](../publish/set-add-on-pricing-and-availability.md?#pricing)：在此部分中选择你的订阅的价格。 在发布加载项之后，你将不能提高订阅价格。 不过，可以在以后降价。
        > [!IMPORTANT]
        > 默认情况下，当你创建任何加载项时，价格最初都设置为**免费**。 因为你在完成加载项提交之后不能提高订阅加载项的价格，请确保在此处选择你的订阅的价格。

2. 在你的应用中，使用 [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空间中的 API 来确定当前用户是否已获取你的订阅加载项，然后以应用内购买的形式将它提供给用户。 请参阅本文中的[代码示例](#code-examples)以了解更多详细信息。

3. 测试你的应用中订阅的应用内购买实现情况。 你需要将你的应用从应用商店下载到你的开发设备，才能使用其许可证进行测试。 有关详细信息，请参阅我们的应用内购买[测试指南](in-app-purchases-and-trials.md#testing)。  

4. 创建并发布应用提交，并使之包含你已更新的应用包（包括你已测试的代码）。 有关详细信息，请参阅[应用提交](../publish/app-submissions.md)。

<span id="code-examples"/>

## <a name="code-examples"></a>代码示例

此部分中的代码示例介绍如何使用 [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空间中的 API 来获取有关当前应用的订阅加载项的信息，以及代表当前用户请求购买订阅加载项。

这些示例有以下先决条件：
* 适用于面向 **Windows 10 周年纪念版（10.0；版本 14393）或**更高版本的通用 Windows 平台 (UWP) 应用的 Visual Studio 项目。
* 你已在合作伙伴中心[创建了一个应用提交](https://docs.microsoft.com/windows/uwp/publish/app-submissions)，此应用在应用商店中发布。 在测试应用期间，你可以选择将应用配置为在 Microsoft Store 中隐藏。 有关详细信息，请参阅[测试指南](in-app-purchases-and-trials.md#testing)。
* 已在合作伙伴中心[为应用创建订阅外接程序](../publish/add-on-submissions.md)。

这些示例中的代码假设：
* 此代码文件包含对 **Windows.Services.Store** 和 **System.Threading.Tasks** 命名空间的 **using** 语句。
* 该应用是单用户应用，仅在启动该应用的用户上下文中运行。 有关详细信息，请参阅[应用内购买和试用](in-app-purchases-and-trials.md#api_intro)。

> [!NOTE]
> 如果你有使用[桌面桥](https://developer.microsoft.com/windows/bridges/desktop)的桌面应用程序，可能需要添加未在这些示例中显示的额外代码来配置 [**StoreContext**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) 对象。 有关详细信息，请参阅[在使用桌面桥的桌面应用程序中使用 StoreContext 类](in-app-purchases-and-trials.md#desktop)。

### <a name="purchase-a-subscription-add-on"></a>购买订阅加载项

此示例演示如何代表当前客户请求购买已知订阅加载项。 此示例还显示如何处理订阅有试用期的情况。

1. 这段代码首先确定客户是否已经拥有一个活动订阅许可证。 如果客户已经有活动的许可证，则你的代码应根据需要解锁订阅功能（因为它专用于你的应用，这在示例中已用注释指明）。
2. 接下来，这段代码获取表示你想要代表客户购买的订阅的 [**StoreProduct**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) 对象。 代码假定你已知道你想要购买的订阅的 [Store ID](in-app-purchases-and-trials.md#store-ids)，并且你已对 *subscriptionStoreId* 变量指定此值。
3. 然后代码确定该订阅是否有试用版。 （可选）你的应用可以使用此信息向客户显示详情，说明存在可用的试用版或完整订阅。
4. 最后，代码调用 [**RequestPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.RequestPurchaseAsync) 方法来请求购买订阅。 如果该订阅有试用版，则会向客户提供试用版以便购买。 否则，将提供完整订阅以便其购买。

> [!div class="tabbedCodeSnippets"]
[!code-csharp[Subscriptions](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseSubscriptionAddOnTrialPage.xaml.cs#PurchaseTrialSubscription)]

### <a name="get-info-about-subscription-add-ons-for-the-current-app"></a>获取有关当前应用的订阅加载项的信息

此代码示例介绍如何获取有关可用于你的应用的所有订阅加载项的信息。 要获取此信息，请先使用 [**GetAssociatedStoreProductsAsync**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsAsync) 方法来获取表示应用提供的每个可用加载项的 [**StoreProduct**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct) 对象的集合。 然后，获取每个产品的 [**StoreSku**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku)，并使用 [**IsSubscription**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.IsSubscription) 和 [**SubscriptionInfo**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.SubscriptionInfo) 属性访问订阅信息。

> [!div class="tabbedCodeSnippets"]
[!code-csharp[Subscriptions](./code/InAppPurchasesAndLicenses_RS1/cs/GetSubscriptionAddOnsPage.xaml.cs#GetSubscriptions)]

<span id="manage-subscriptions" />

## <a name="manage-subscriptions-from-your-services"></a>从你的服务管理订阅

在你更新过的应用在应用商店中发布且客户可以购买你的订阅加载项之后，你可能会遇到需要管理某个客户的订阅的情况。 我们提供了你可以从你的服务中调用的 REST API 来执行以下订阅管理任务：

* [获取用户有权使用的订阅](get-subscriptions-for-a-user.md)。 如果你的订阅是跨平台服务的一部分，则可以调用此 API 来确定指定的用户对于你的订阅是否拥有授权以及他们的订阅在你的 UWP 应用的上下文中的状态。 然后，你可以使用此信息来更新你的服务支持的其他平台上的订阅状态。

* [更改给定用户订阅的计费状态](change-the-billing-state-of-a-subscription-for-a-user.md). 使用此 API 可取消、延长或禁用订阅的自动续订。

## <a name="cancellations"></a>取消

客户可以使用其 Microsoft 帐户的 [https://account.microsoft.com/services](https://account.microsoft.com/services) 页面查看他们购买的所有订阅、取消订阅，以及更改与其订阅关联的付款方式。 当客户使用此页面取消订阅时，他们在当前帐单期间内将仍然拥有对该订阅的访问权限。 他们不能获得当前帐单周期任何部分的退款。 在当前帐单周期结束时，他们的订阅会停止。

你还可以使用我们的 REST API [更改给定用户的订阅的帐单状态](change-the-billing-state-of-a-subscription-for-a-user.md)，以代表用户取消订阅。

## <a name="subscription-renewals-and-grace-periods"></a>订阅续订和宽限期

在每个计费周期内的某个时间点，我们将尝试向客户的信用卡收取下一个计费周期的费用。 如果收费失败，客户的订阅将进入*催缴*状态。 这意味着他们的订阅在当前计费周期剩余时间内仍然保持活动状态，但是我们将定期尝试向其信用卡收费以便自动续订订阅。 这种状态最多可以持续两周，直到当前计费周期结束和下一个帐单期间的续订日期。

对于订阅帐单，我们不提供宽限期。 如果在当前计费周期结束时我们都无法对客户的信用卡收费，订阅将取消，在当前计费周期结束后，客户将不再能够访问订阅。

## <a name="unsupported-scenarios"></a>不支持的方案

目前订阅加载项不支持以下情形。

* 目前不支持直接通过应用商店向客户销售订阅。 订阅只能通过数字产品的应用内购买提供。
* 客户不能使用他们的 Microsoft 帐户的 [https://account.microsoft.com/services](https://account.microsoft.com/services) 页面切换到不同订阅期。 若要切换到其他订阅期，客户必须取消其当前订阅，然后从你的应用购买包含不同订阅期限的订阅。
* 订阅加载项目前不支持分级订阅（例如，将客户从基本订阅切换到包含更多功能的高级订阅）。
* 目前订阅加载项不支持[销售](../publish/put-apps-and-add-ons-on-sale.md)和[促销代码](../publish/generate-promotional-codes.md)。
* 在设置订阅外接程序的可见性以**停止购置**后，续订现有订阅。 有关更多详细信息，请参阅[设置附加定价和可用性](../publish/set-add-on-pricing-and-availability.md)。

## <a name="related-topics"></a>相关主题

* [应用内购买和试用](in-app-purchases-and-trials.md)
* [获取应用和外接程序的产品信息](get-product-info-for-apps-and-add-ons.md)

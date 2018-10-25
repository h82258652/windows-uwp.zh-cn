---
author: JnHs
description: 除了创建将在 Windows 应用中运行的应用的广告市场活动外，你还可以使用其他渠道推广你的应用。
title: 创建自定义应用促销市场活动
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.author: wdg-dev-content
ms.date: 09/27/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 自定义, 应用, 促销, 市场活动
ms.localizationpriority: medium
ms.openlocfilehash: 13ee8d7482a2ce0716d4e133af329cd0ea42c184
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "5521370"
---
# <a name="create-a-custom-app-promotion-campaign"></a>创建自定义应用促销市场活动

除了创建将在 Windows 应用中运行的[应用的广告市场活动](create-an-ad-campaign-for-your-app.md)外，你还可以使用其他渠道推广你的应用。 例如，你可以使用第三方应用营销提供商推广你的应用，或者可以在社交媒体站点上发布指向你的应用的链接。 这些活动被称为*自定义市场活动*。

如果你为应用举行自定义市场活动，你可以通过为每个自定义市场活动创建不同的 URL（其中每个 URL 包含不同的*市场活动 ID*）来跟踪每个市场活动的相对表现。 当运行 windows 10 的客户单击包含市场活动 ID 的 URL 时，Microsoft 将该单击与相应的自定义市场活动相关联，并向你提供此数据。

> [!IMPORTANT]
> 仅，在 windows 10 上的客户对此数据被跟踪。 使用其他操作系统的客户仍可以跟踪你的应用一览的链接，但有关这些客户的活动的数据将不包括在内。

有两种主要类型的数据与自定义市场活动相关联：应用的 Microsoft Store 一览的*页面查看次数*和*转换数量*。 转换是由客户通过包括自定义市场活动 ID 的 URL 查看应用的应用商店一览页面所导致的应用购置。 有关转换的详细信息，请参阅本主题中的[了解应用购买如何限定为转换](#understanding-how-acquisitions-qualify-as-conversions)。

你可以通过以下方式检索应用的自定义市场活动表现数据：

* 通过开发人员中心仪表板中[购置报告](acquisitions-report.md)中的**按市场活动 ID 的应用页面查看次数和转换数量**和**市场活动转换总量**图表，你可以查看你的应用或加载项的页面查看次数和转换数量的相关数据。
* 如果你的应用是通用 Windows 平台 (UWP) 应用，则可以使用 Windows SDK 中的 API 以编程方式检索导致转换的自定义市场活动 ID。

## <a name="example-custom-campaign-scenario"></a>自定义市场活动方案示例

假设有一位游戏开发人员已完成一款新游戏，并希望向她的现有游戏玩家推广该游戏。 她在其 Facebook 页面上发布新游戏发布的通知，其中包括指向该游戏的 Microsoft Store 一览的链接。 她的许多玩家也在 Twitter 上关注她，所以她也发布了该通知的推文，并附带指向该游戏的 Microsoft Store 一览的链接。

为了跟踪其中每个推广渠道是否成功，开发人员为该游戏的 Microsoft Store 一览创建了两个 URL 变体：

* 她将发布到其 Facebook 页面的 URL 包含自定义市场活动 ID `my-facebook-campaign`

* 她将发布到 Twitter 的 URL 包含自定义市场活动 ID `my-twitter-campaign`

当她的 Facebook 和 Twitter 关注者单击这些 URL 时，Microsoft 将跟踪每次单击，并将其与相应的自定义市场活动相关联。 后续的合格游戏购置行为和任何加载项的购买将与自定义市场活动相关联并报告为转换。

<span id="conversions" />

## <a name="understanding-how-acquisitions-qualify-as-conversions"></a>了解购置如何限定为转换

自定义市场活动*转换*是由客户单击通过自定义市场活动推广的 URL 所导致的购置。 限定为开发人员中心仪表板中[购置报告](acquisitions-report.md)中的**按市场活动 ID 的应用页面查看次数和转换数量**与**市场活动转换总量**图表中的转换与限定为[以编程方式检索市场活动 ID](#programmatically) 的转换具有不同的方案。

### <a name="qualifying-conversions-in-the-dashboard-report"></a>限定仪表板报告中的转换

以下情形限定为[购置报告](acquisitions-report.md)中**按市场活动 ID 的应用页面查看次数和转换数量**与**市场活动转换总量**图表中的转换：

* *具有或没有已识别的 Microsoft 帐户*的客户单击包含自定义市场活动 ID 的应用 URL，然后重定向到该应用的应用商店一览。 然后，同一客户在首次单击包含自定义市场活动 ID 的 Microsoft Store URL 后，在 24 小时内购置应用。

* 如果客户在其他设备（而非在其上单击包含自定义市场活动 ID 的 URL 的设备）上购置应用，那么只有客户在单击 URL 时使用相同的 Microsoft 帐户登录的情况下，才会计算此转换。

> [!NOTE]
> 对于计为自定义市场活动转换的应用购置，该应用中的任何加载项购买也计为同一自定义市场活动的转换。

### <a name="qualifying-conversions-when-programmatically-retrieving-the-campaign-id"></a>在以编程方式检索市场活动 ID 时限定为转换

若要在以编程方式检索与应用相关联的市场活动 ID 时限定为转换，必须满足以下条件：

* 在运行 **Windows 10 版本 1607 或更高版本**的设备上：客户（无论是否已登录到已识别的 Microsoft 帐户）单击包含自定义市场活动 ID 的 URL，然后重定向到该应用的应用商店一览页面。 客户在通过单击 URL 查看应用商店一览时购置应用。

* 在运行 **Windows 10 版本 1511 或更早版本**的设备上：客户（必须使用已识别的 Microsoft 帐户登录）单击包含自定义市场活动 ID 的 URL，然后重定向到该应用的应用商店一览页面。 客户在通过单击 URL 查看应用商店一览时购置应用。 在这些版本的 Windows 10 上，用户必须使用已识别的 Microsoft 帐户登录，才能在以编程方式检索市场活动 ID 时将购置限定为转换。

> [!NOTE]
> 如果客户离开应用商店一览页面，但在 24 小时内返回该页面（使用相同的 Microsoft 帐户登录时，在相同设备上或在其他设备上均可）并购置该应用，这**将**在[购置报告](acquisitions-report.md)中的**按市场活动 ID 的应用页面查看次数和转换数量**与**市场活动转换总量**图表中限定为转换。 但是，如果你以编程方式检索市场活动 ID，则**不会**将此限定为转换。

## <a name="embed-a-custom-campaign-id-to-your-apps-microsoft-store-page-url"></a>将自定义市场活动 ID 嵌入应用的 Microsoft Store 页面 URL

若要为应用创建带有自定义市场活动 ID 的 Microsoft Store 页面 URL，请执行以下操作：

1.  为自定义市场活动创建 ID 字符串。 此字符串最多可包含 100 个字符，尽管我们建议你定义容易识别的简短市场活动 ID。

   > [!NOTE]
   > 市场活动 ID 字符串可能在其他开发人员查看其应用的[购置报告](acquisitions-report.md)时对他们可见。 当客户单击你的自定义市场活动 ID 进入应用商店，并在同一会话内购买其他开发人员的应用，从而将该转化归因于你的市场活动 ID 时，会发生这种情况。 该开发人员将看到由初次单击你的市场活动 ID 导致的自己的应用的转换数量，包括市场活动 ID 的名称，但他们不会看到单击你的市场活动 ID 后购买你自己的应用（或任何其他开发人员的应用）的用户数的任何相关数据。

2.  获取 HTML 或协议格式的应用 Microsoft Store 一览的链接。

    * 如果要让客户在任何操作系统上使用浏览器导航到应用基于 Web 的应用商店一览，请使用 HTML URL。 在 Windows 设备上，应用商店应用还将启动并显示应用一览。 此 URL 格式为 **`https://www.microsoft.com/store/apps/*your app ID*`**。 例如，Skype 的 HTML URL 是 `https://www.microsoft.com/store/apps/9wzdncrfj364`。 可以在[应用标识](view-app-identity-details.md#link-to-your-apps-listing)页面上找到此 URL。

    * 如果你要从在已安装 UWP 应用的设备或计算机上运行的其他 Windows 应用推广你的应用，或者知道客户使用支持 Microsoft Store 的设备时，请使用协议格式。 此链接将直接转到应用的应用商店一览，而无需打开浏览器。 此 URL 格式为 **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**。 例如，Skype 的协议 URL 是 `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`。

3.  将以下字符串附加到应用的 URL 的末尾：

    * 对于 HTML 格式 URL，请附加 **`?cid=*my custom campaign ID*`**。 例如，如果 Skype 引入一个带有值 **custom\_campaign** 的市场活动 ID，则包含该市场活动 ID 的新 URL 将是：`https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`。

    * 对于协议格式 URL，请附加 **`&cid=*my custom campaign ID*`**。 例如，如果 Skype 引入一个带有值 **custom\_campaign** 的市场活动 ID，则包含该市场活动 ID 的新协议 URL 将是：`ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`。

<span id="programmatically" />

## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>以编程方式检索某个应用的自定义市场活动 ID

如果你的应用是 UWP 应用，则可以使用 Windows SDK 中的 API 以编程方式检索与应用的购置相关联的自定义市场活动 ID。 这些 API 使许多分析和盈利方案成为可能。 例如，你可以查明当前用户是否在通过你的 Facebook 市场活动发现了你的应用后获取了该应用，然后对应用体验进行相应的自定义。 或者，如果你使用的是第三方应用营销提供商，则可以将数据发送回该提供商。

仅当客户单击带有嵌入的市场活动 ID 的 URL、查看应用的 Microsoft Store 页面，然后在没有离开 Microsoft Store 一览页面的情况下购置应用时，这些 API 才会返回市场活动 ID 字符串。 使用这些 API 时，如果用户离开该页面，然后稍后返回并购买应用，这不会[限定为转换](#conversions)。

根据你的应用所针对的 Windows 10 版本，你可以使用不同的 API：

* Windows 10 版本 1607 或更高版本：使用 **Windows.Services.Store** 命名空间中的 [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 类。 在使用此 API 时，无论用户是否使用已识别的 Microsoft 帐户进行登录，你都可以检索任何[限定购置](#conversions)的自定义市场活动 ID。

* Windows 10 版本 1511 或更早版本：使用 **Windows.ApplicationModel.Store** 命名空间中的 [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 类。 在使用此 API 时，只有在用户使用已识别的 Microsoft 帐户登录的情况下，才能检索[限定购置](#conversions)的自定义市场活动 ID。

> [!NOTE]
> 虽然 **Windows.ApplicationModel.Store** 命名空间可用于所有版本的 Windows 10，但是如果你的应用针对 Windows 10 版本 1607 或更高版本，我们建议你使用 **Windows.Services.Store** 命名空间中的 API。 有关这些命名空间之间的差异的详细信息，请参阅[应用内购买和试用](../monetize/in-app-purchases-and-trials.md#choose-namespace)。 以下代码示例介绍如何构建代码以在同一项目中使用两个 API。

### <a name="code-example"></a>代码示例

以下代码示例介绍如何检索自定义市场活动 ID。 通过使用[版本自适应代码](../debug-test-perf/version-adaptive-code.md)，此示例将使用 **Windows.Services.Store** 和 **Windows.ApplicationModel.Store** 命名空间中的两组 API。 按照此过程，你的代码可以在任何版本的 Windows 10 上运行。 若要使用此代码，你的项目的目标操作系统版本必须为 **Windows 10 周年纪念版（10.0；内部版本 14394）** 或更高版本，不过最低操作系统版本可能为早期版本。

``` csharp
// This example assumes the code file has using statements for
// System.Linq, System.Threading.Tasks, Windows.Data.Json,
// and Windows.Services.Store.
public async Task<string> GetCampaignId()
{
    // Use APIs in the Windows.Services.Store namespace if they are available
    // (the app is running on a device with Windows 10, version 1607, or later).
    if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent(
         "Windows.Services.Store.StoreContext"))
    {
        StoreContext context = StoreContext.GetDefault();

        // Try to get the campaign ID for users with a recognized Microsoft account.
        StoreProductResult result = await context.GetStoreProductForCurrentAppAsync();
        if (result.Product != null)
        {
            StoreSku sku = result.Product.Skus.FirstOrDefault(s => s.IsInUserCollection);

            if (sku != null)
            {
                return sku.CollectionData.CampaignId;
            }
        }

        // Try to get the campaign ID from the license data for users without a
        // recognized Microsoft account.
        StoreAppLicense license = await context.GetAppLicenseAsync();
        JsonObject json = JsonObject.Parse(license.ExtendedJsonData);
        if (json.ContainsKey("customPolicyField1"))
        {
            return json["customPolicyField1"].GetString();
        }

        // No campaign ID was found.
        return String.Empty;
    }
    // Fall back to using APIs in the Windows.ApplicationModel.Store namespace instead
    // (the app is running on a device with Windows 10, version 1577, or earlier).
    else
    {
#if DEBUG
        return await Windows.ApplicationModel.Store.CurrentAppSimulator.GetAppPurchaseCampaignIdAsync();
#else
        return await Windows.ApplicationModel.Store.CurrentApp.GetAppPurchaseCampaignIdAsync() ;
#endif
    }
}
```

此代码执行以下任务：

1. 首先，进行检查以查看 **Windows.Services.Store** 命名空间中的 [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 类在当前设备上是否可用（这意味着设备运行的是 Windows 10 版本 1607 或更高版本）。 如果可用，该代码将继续使用此类。

2. 接下来，针对当前用户具有已识别的 Microsoft 帐户的情况，它将尝试获取自定义市场活动 ID。 为此，该代码会获取一个表示当前应用 SKU 的 [**StoreSku**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreSku) 对象，然后它将访问 [**CampaignId**](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.CampaignId) 属性，以检索市场活动 ID（如果可用）。
3. 然后，针对当前用户没有已识别的 Microsoft 帐户的情况，该代码将尝试检索市场活动 ID。 在此情况下，市场活动 ID 嵌入在应用许可证中。 该代码使用 [**GetAppLicenseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetAppLicenseAsync) 方法检索许可证，然后分析许可证的 JSON 内容以获取名为 *customPolicyField1* 的键的值。 此值包含市场活动 ID。

4. 如果 **Windows.Services.Store** 命名空间中的 [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 类不可用，则该代码将回退，使用 **Windows.ApplicationModel.Store** 命名空间中的 [**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_GetAppPurchaseCampaignIdAsync) 方法检索自定义市场活动 ID（此命名空间适用于所有版本的 Windows 10，包括版本 1511 及更早版本）。 注意，在使用此方法时，只有在用户具有已识别的 Microsoft 帐户的情况下，你才能检索[限定购买](#conversions) 的自定义市场活动 ID。

### <a name="specify-the-campaign-id-in-the-proxy-file-for-the-windowsapplicationmodelstore-namespace"></a>在代理文件中为 Windows.ApplicationModel.Store 命名空间指定市场活动 ID

**Windows.ApplicationModel.Store** 命名空间包括 [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator)，这是在你将应用提交到应用商店之前模拟应用商店操作以测试代码的特殊类。 此类从[名为 Windows.StoreProxy.xml 的本地文件](../monetize/in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#using-the-windowsstoreproxyxml-file-with-currentappsimulator) 中检索数据。 上一个代码示例介绍如何在项目内的调试和非调试代码中包括或使用 **CurrentApp** 和 **CurrentAppSimulator**。 若要在调试环境中测试此代码，请将 **AppPurchaseCampaignId** 元素添加到开发计算机上的 WindowsStoreProxy.xml 文件中，如以下示例所示。 当你运行应用时，[**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.GetAppPurchaseCampaignIdAsync) 方法将始终返回此值。

``` xml
<CurrentApp>
    ...
    <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
</CurrentApp>
```

**Windows.Services.Store** 命名空间不提供可用于在测试期间模拟许可证信息的类。 必须将应用发布到应用商店，并将该应用下载到开发设备，才能将其许可证用于测试。 有关详细信息，请参阅[应用内购买和试用](../monetize/in-app-purchases-and-trials.md#testing)。

## <a name="test-your-custom-campaign"></a>测试你的自定义市场活动

推广自定义市场活动 URL 前，我们建议你通过执行以下操作测试自定义市场活动：

1.  在你用于测试的设备上登录 Microsoft 帐户。

2.  单击你的自定义市场活动 URL。 请确保进入了应用页面，然后关闭 UWP 应用或浏览器页面。

3.  再多次单击该 URL，在每次访问应用的页面后关闭 UWP 应用或浏览器页面。 在其中**一次**访问你的应用页面时，购置你的应用可生成转换。 计算你单击 URL 的总次数。

4. 确认预期的页面查看次数和转换数量是否显示在开发人员中心仪表板中[购置报告](acquisitions-report.md)中的**按市场活动 ID 的应用页面查看次数和转换数量**与**市场活动转换总量**图表中，并测试应用的代码以确认其是否可以使用上述 API 成功检索市场活动 ID。

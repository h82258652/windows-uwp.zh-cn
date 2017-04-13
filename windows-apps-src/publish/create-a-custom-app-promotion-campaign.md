---
author: shawjohn
Description: "除了创建将在 Windows 应用中运行的应用的广告市场活动外，你还可以使用其他渠道推广你的应用。"
title: "创建自定义应用促销市场活动"
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 自定义, 应用, 促销, 市场活动"
ms.openlocfilehash: 1e56b1df4b5501ded5db799c3ad67f97c0e1cfcd
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="create-a-custom-app-promotion-campaign"></a>创建自定义应用促销市场活动

除了创建将在 Windows 应用中运行的[应用的广告市场活动](create-an-ad-campaign-for-your-app.md)外，你还可以使用其他渠道推广你的应用。 例如，你可以使用第三方应用营销提供商推广你的应用，或者可以在社交媒体站点上发布指向你的应用的链接。 这些活动被称为*自定义市场活动*。

如果你为应用举行自定义市场活动，你可以通过为每个自定义市场活动创建不同的 Windows 应用商店应用 URL（其中每个 URL 包含不同的市场活动 ID）来跟踪每个市场活动的相对表现。 当运行 Windows 10 的客户单击包含市场活动 ID 的 URL 时，Microsoft 会将该单击操作与相应的自定义市场活动相关联，并向你提供此数据。

有两种主要类型的数据与自定义市场活动相关联：应用的页面查看和*转换*。 转换是由用户从通过自定义市场活动推广的 URL 单击应用的 Windows 应用商店页面所导致的应用购买。 有关转换的详细信息，请参阅本主题中的[了解应用购买如何限定为转换](#understanding-how-app-acquisitions-qualify-as-conversions)。

你可以通过以下方式检索应用的自定义市场活动表现数据：

* 你可以从开发人员中心仪表板上的[通道和转换报告](channels-and-conversions-report.md)中查看有关应用或加载项的页面查看和转换的数据。
* 如果你的应用是通用 Windows 平台 (UWP) 应用，则它可以使用 Windows SDK 中的 API 以编程方式检索导致转换的自定义市场活动 ID。

> **重要提示**&nbsp;&nbsp;将仅为运行 Windows 10 的客户跟踪此数据。 使用其他操作系统的客户仍可以跟踪你的应用一览的链接，但有关这些客户的活动的数据将不包括在内。

## <a name="example-custom-campaign-scenario"></a>自定义市场活动方案示例

假设有一位游戏开发人员已完成一款新游戏，并希望向她的现有游戏玩家推广该游戏。 她在其 Facebook 页面上发布新游戏发布的通知，其中包括指向该游戏的 Windows 应用商店页面的链接。 她的许多玩家也在 Twitter 上关注她，所以她也发布了该通知的推文，并附带指向该游戏的 Windows 应用商店页面的链接。

为了跟踪其中每个推广渠道是否成功，开发人员为该游戏的 Windows 应用商店页面创建了两个 URL 变体。

* 她将发布到其 Facebook 页面的 URL 包含自定义市场活动 ID `my-facebook-campaign`。
* 她将发布到 Twitter 的 URL 包含自定义市场活动 ID `my-twitter-campaign`。

当她的 Facebook 和 Twitter 关注者单击这些 URL 时，Microsoft 将跟踪每次单击，并将其与相应的自定义市场活动相关联。 后续的合格游戏购置行为和任何加载项的购买将与自定义市场活动相关联并报告为转换。

<span id="conversions" />
## <a name="understanding-how-app-acquisitions-qualify-as-conversions"></a>了解应用购买如何限定为转换

*转换*是由用户从通过自定义市场活动推广的 URL 单击应用的 Windows 应用商店页面所导致的应用购买。 限定为开发人员中心仪表板上的[通道和转换报告](channels-and-conversions-report.md)中的转换与限定为[以编程方式检索市场活动 ID](#programmatically) 的转换具有不同的方案。

### <a name="qualifying-conversions-for-the-dashboard-report"></a>限定仪表板报告转换

若要限定为[通道和转换报告](channels-and-conversions-report.md)中的转换，必须满足以下情形：

* *具有或没有已识别的 Microsoft 帐户*的客户单击包含自定义市场活动 ID 的应用 URL，然后重定向到该应用的 Windows 应用商店页面。 同一客户在首次单击包含自定义市场活动 ID 的 Windows 应用商店 URL 后，在 24 小时内购买应用。

* 如果客户在其他计算机或设备（而非在其上单击包含自定义市场活动 ID 的 Windows 应用商店 URL 的计算机或设备）上购买应用，那么只有客户拥有 Microsoft 帐户，才会计数此转换。

> **注意**&nbsp;&nbsp;对于计为自定义市场活动转换的应用购买，该应用中的任何加载项购买也计为同一自定义市场活动的转换。

### <a name="qualifying-conversions-for-programmatically-retrieving-the-campaign-id"></a>限定转换以编程方式检索市场活动 ID

若要在以编程方式检索与应用相关联的市场活动 ID 时限定为转换，必须满足以下条件：

* 在运行 **Windows 10 版本 1607 或更高版本**的设备上：*具有或没有已识别的 Microsoft 帐户*的客户单击包含自定义市场活动 ID 的应用 URL，然后重定向到该应用的 Windows 应用商店页面。 客户在单击 URL 后的同一 Windows 应用商店页面查看期间购买该应用。

* 在运行 **Windows 10 版本 1511 或更高版本**的设备上：*具有已识别的 Microsoft 帐户*的客户单击包含自定义市场活动 ID 的应用 URL，然后重定向到该应用的 Windows 应用商店页面。 客户在单击 URL 后的同一 Windows 应用商店页面查看期间购买该应用。 在这些版本的 Windows 10 中，用户必须具有已识别的 Microsoft 帐户，购买才能在以编程方式检索市场活动 ID 时限定为转换。

>**注意**&nbsp;&nbsp;如果客户离开该页面，然后 24 小时内返回到页面（在同一台计算机或设备上或在其他计算机或设备上，只要他们拥有 Microsoft 帐户），并购买该应用，这将在[通道和转换报告](channels-and-conversions-report.md) 中限定为转换。 但是，如果你以编程方式检索市场活动 ID，则将不会限定为转换。

## <a name="embed-a-custom-campaign-id-to-your-apps-windows-store-page-url"></a>将自定义市场活动 ID 嵌入应用的 Windows应用商店页面 URL

若要为应用创建带有自定义市场活动 ID 的 Windows 应用商店页面 URL：

1.  为自定义市场活动创建 ID 字符串。 此字符串最多可包含 100 个字符，尽管我们建议你定义容易识别的简短市场活动 ID。

  >**注意**&nbsp;&nbsp;在通道和转换报告中，市场活动 ID 字符串可能对其他开发人员可见。 当客户单击你的市场活动 ID 进入应用商店，但却在同一会话内购买了其他开发人员的应用时会发生这种情况。 在这种情况下，虽然你的市场活动 ID 名称可能对另一开发人员可见，但是该开发人员只能看到因首次单击市场活动 ID 而产生的自身应用转换数量。 他们不会看到通过单击市场活动 ID 购买你的应用的用户数量相关数据。

2.  为应用获取 HTML 或协议格式的 Windows 应用商店页面 URL。

    * 如果你希望客户在浏览器中导航到应用的 Windows 应用商店页面，请使用 HTTP 格式（如果已安装 Windows 应用商店应用，则此 URL 还会将 Windows 应用商店应用启动到应用一览）。 此 URL 格式为 **`https://www.microsoft.com/store/apps/*your app name*/*your app ID*`**。 例如，Skype 的 HTTP URL 是 `https://www.microsoft.com/store/apps/skype/9wzdncrfj364`。 HTML 格式 URL 在[开发人员中心仪表板的**应用标识**页](link-to-your-app.md) 上提供。 注意，在运行 Windows 7 和更高版本的计算机和平板电脑上以及运行 Windows Phone 8 和更高版本的手机上，HTTP 格式 URL 可用于在浏览器中导航到 Windows 应用商店。

    * 如果你要从在已安装 Windows 应用商店应用的设备或计算机上运行的其他 Windows 应用推广你的应用，并且希望客户在该 Windows 应用商店应用中打开到你的应用的页面，请使用协议格式。 此 URL 格式为 **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**。 例如，Skype 的协议 URL 是 `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`。

3.  将以下字符串附加到应用的 URL 的末尾：

    * 对于 HTTP 格式 URL，请附加 **`?cid=*my custom campaign ID*`**。 例如，如果 Skype 引入一个带有值 **custom\_campaign** 的市场活动 ID，则包含该市场活动 ID 的新 HTTP URL 将是：`https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`。

    * 对于协议格式 URL，请附加 **`&cid=*my custom campaign ID*`**。 例如，如果 Skype 引入一个带有值 **custom\_campaign** 的市场活动 ID，则包含该市场活动 ID 的新协议 URL 将是：`ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`。

<span id="programmatically" />
## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>以编程方式检索某个应用的自定义市场活动 ID

如果你的应用是 UWP 应用，则可以使用 Windows SDK 中的 API 以编程方式检索与你的应用相关联的自定义市场活动 ID。 这些 API 使许多分析和盈利方案成为可能。 例如，你可以查明当前用户是否在通过你的 Facebook 市场活动发现了你的应用后获取了该应用，然后对应用体验进行相应的自定义。 或者，如果你使用的是第三方应用营销提供商，则可以将数据发送回该提供商。

仅当客户单击带有嵌入的市场活动 ID 的 URL、重定向到应用的 Windows 应用商店页面，然后在没有离开该页面的情况下购买应用时，这些 API 才将返回一个市场活动 ID 字符串。 使用这些 API 时，如果用户离开该页面，然后稍后返回并购买应用，这不会[限定为转换](#conversions)。

根据你的应用所针对的 Windows 10 版本，你可以使用不同的 API：

* Windows 10 版本 1607 或更高版本：使用 **Windows.Services.Store** 命名空间中的 [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 类。 在使用此 API 时，无论用户是否具有已识别的 Microsoft 帐户，你都可以检索任何[限定购买](#conversions) 的自定义市场活动 ID。
* Windows 10 版本 1511 或更早版本：使用 **Windows.ApplicationModel.Store** 命名空间中的 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx) 类。 在使用此 API 时，只有在用户具有已识别的 Microsoft 帐户的情况下，你才能检索[限定购买](#conversions) 的自定义市场活动 ID。

>**注意**&nbsp;&nbsp;虽然 **Windows.ApplicationModel.Store** 命名空间可用于所有版本的 Windows 10，但是如果你的应用针对 Windows 10 版本 1607 或更高版本，我们建议你使用 **Windows.Services.Store** 命名空间中的 API。 有关这些命名空间之间的差异的详细信息，请参阅[应用内购买和试用](../monetize/in-app-purchases-and-trials.md#choose-namespace)。 以下代码示例介绍如何构建代码以在同一项目中使用两个 API。

### <a name="code-example"></a>代码示例

以下代码示例介绍如何检索自定义市场活动 ID。 通过使用[版本自适应代码](../debug-test-perf/version-adaptive-code.md)，此示例将使用 **Windows.Services.Store** 和 **Windows.ApplicationModel.Store** 命名空间中的两组 API。 按照此过程，你的代码可以在任何版本的 Windows 10 上运行。 若要使用此代码，你的项目的目标操作系统版本必须为 **Windows 10 周年版本（10.0；内部版本 14394）**或更高版本，尽管最低操作系统版本可能为早期版本。

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

2. 接下来，针对当前用户具有已识别的 Microsoft 帐户的情况，它将尝试获取自定义市场活动 ID。 为此，该代码会获取一个表示当前应用 SKU 的 [**StoreSku**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreSku) 对象，然后它将访问 [**CampaignId**](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata#Windows_Services_Store_StoreCollectionData_CampaignId) 属性，以检索市场活动 ID（如果可用）。

2. 然后，针对当前用户没有已识别的 Microsoft 帐户的情况，该代码将尝试检索市场活动 ID。 在此情况下，市场活动 ID 嵌入在应用许可证中。 该代码使用 [**GetAppLicenseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_GetAppLicenseAsync) 方法检索许可证，然后分析许可证的 JSON 内容以获取名为 *customPolicyField1* 的键的值。 此值包含市场活动 ID。

3. 如果 **Windows.Services.Store** 命名空间中的 [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 类不可用，则该代码将回退，使用 **Windows.ApplicationModel.Store** 命名空间中的 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.getapppurchasecampaignidasync.aspx) 方法检索自定义市场活动 ID（此命名空间适用于所有版本的 Windows 10，包括版本 1511 及更早版本）。 注意，在使用此方法时，只有在用户具有已识别的 Microsoft 帐户的情况下，你才能检索[限定购买](#conversions) 的自定义市场活动 ID。

  >**注意**&nbsp;&nbsp;**Windows.ApplicationModel.Store** 命名空间包括 [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator)，这是在你将应用提交到应用商店之前模拟应用商店操作以测试代码的特殊类。 此类从[名为 Windows.StoreProxy.xml 的本地文件](../monetize/in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#using-the-windowsstoreproxyxml-file-with-currentappsimulator) 中检索数据。 上一个代码示例介绍如何在项目内的调试和非调试代码中包括或使用 **CurrentApp** 和 **CurrentAppSimulator**。 若要在调试环境中测试此代码，请将 **AppPurchaseCampaignId** 元素添加到开发计算机上的 WindowsStoreProxy.xml 文件中，如以下示例所示。 当你运行应用时，[**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator#Windows_ApplicationModel_Store_CurrentAppSimulator_GetAppPurchaseCampaignIdAsync) 方法将始终返回此值。

  ``` xml
  <CurrentApp>
      ...
      <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
  </CurrentApp>
  ```

  >**注意**&nbsp;&nbsp;**Windows.Services.Store** 命名空间不提供可用于在测试期间模拟许可证信息的类。 必须将应用发布到应用商店，并将该应用下载到开发设备，才能将其许可证用于测试。 有关详细信息，请参阅[应用内购买和试用](../monetize/in-app-purchases-and-trials.md#testing)。

## <a name="test-your-custom-campaign"></a>测试你的自定义市场活动

推广自定义市场活动 URL 前，我们建议你通过执行以下操作测试自定义市场活动：

1.  在你用于测试的计算机或设备上登录 Microsoft 帐户。
2.  单击你的自定义市场活动 URL。 请确保 Windows 应用商店正确加载你的应用，然后关闭 Windows 应用商店应用或浏览器页面。
3.  再多次单击该 URL，在每次访问应用的页面后关闭 Windows 应用商店应用或浏览器页面。 在其中一次访问你的应用页面时，请购买你的应用以生成转换。 计算你单击 URL 的总次数。
4. 确认[通道和转换报告](channels-and-conversions-report.md) 中是否出现了预期的页面查看和转换，并测试应用代码以确认代码是否可以成功检索市场活动 ID。

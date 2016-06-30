---
author: jnHs
Description: "除了创建将在 Windows 应用中运行的应用的广告市场活动外，你还可以使用其他渠道推广你的应用。"
title: "创建自定义应用促销活动"
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
translationtype: Human Translation
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: b6fa63727ac636d37c52e2f3247c52efb1f8b763

---

# 创建自定义应用促销活动



除了创建将在 Windows 应用中运行的[应用的广告市场活动](create-an-ad-campaign-for-your-app.md)外，你还可以使用其他渠道推广你的应用。 例如，你可以使用第三方应用营销提供商推广你的应用，或者可以在社交媒体站点上发布指向你的应用的链接。 这些活动被称为*自定义市场活动*。

如果你为应用举行自定义市场活动，你可以通过为每个自定义市场活动创建不同的 Windows 应用商店应用 URL（其中每个 URL 包含不同的市场活动 ID）来跟踪每个市场活动的相对表现。 当运行 Windows 10 的客户单击包含市场活动 ID 的 URL 时，Microsoft 会将该单击操作与相应的自定义市场活动相关联，并向你提供此数据。

有两种主要类型的数据与自定义市场活动相关联：应用的页面查看和*转换*。 转换是由用户从通过自定义市场活动推广的 URL 单击应用的 Windows 应用商店页面所导致的应用安装。 有关转换的详细信息，请参阅本主题中的[了解应用安装如何限定为转换](#understanding-how-app-installs-qualify-as-conversions)。

你可以通过以下方式检索自定义市场活动表现数据：

-   如果你的应用是通用 Windows 平台 (UWP) 应用，则它可以使用 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 方法以编程方式检索导致转换的自定义市场活动 ID。
-   你可以从开发人员中心仪表板上的[通道和转换报告](channels-and-conversions-report.md)中查看有关应用或 IAP 的页面查看和转换的数据。

> **重要提示** 将仅为运行 Windows 10 的客户跟踪此数据。 使用其他操作系统的客户仍可以跟踪你的应用一览的链接，但有关这些客户的活动的数据将不包括在内。

 

## 自定义市场活动方案示例


假设有一位游戏开发人员已完成一款新游戏，并希望向她的现有游戏玩家推广该游戏。 她在其 Facebook 页面上发布新游戏发布的通知，其中包括指向该游戏的 Windows 应用商店页面的链接。 她的许多玩家也在 Twitter 上关注她，所以她也发布了该通知的推文，并附带指向该游戏的 Windows 应用商店页面的链接。

为了跟踪其中每个推广渠道是否成功，开发人员为该游戏的 Windows 应用商店页面创建了两个 URL 变体。

-   她将发布到其 Facebook 页面的 URL 包含自定义市场活动 ID `my-facebook-campaign`。
-   她将发布到 Twitter 的 URL 包含自定义市场活动 ID `my-twitter-campaign`。

当她的 Facebook 和 Twitter 关注者单击这些 URL 时，Microsoft 将跟踪每次单击，并将其与相应的自定义市场活动相关联。 后续的合格游戏购置行为和任何应用内产品 (IAP) 的购买将与自定义市场活动相关联并报告为转换。

## 了解应用安装如何限定为转换


*转换*是由客户从通过自定义市场活动推广的 URL 单击应用的 Windows 应用商店页面所导致的应用安装。 限定为开发人员中心仪表板上的[通道和转换报告](channels-and-conversions-report.md)中的转换与限定为[以编程方式检索市场活动 ID](#programmatically) 的转换具有不同的条件。

若要限定为[通道和转换报告](channels-and-conversions-report.md)中的转换，必须满足以下条件：

-   具有已识别的 Microsoft 帐户的客户单击包含自定义市场活动 ID 的应用 URL，然后重定向到该应用的 Windows 应用商店页面。
-   同一名客户（由相同的 Microsoft 帐户标识）在首次单击具有自定义市场活动 ID 的 Windows 应用商店 URL 后 24 小时内安装该应用。 即使客户单击了自定义市场活动 ID 的 Windows 应用商店 URL，但在另一台计算机或设备上安装该应用，这仍然限定为转换。
    > **注意** 对于计为自定义市场活动转换的应用安装，该应用中的任何 IAP 购买也计为同一自定义市场活动的转换。

     

若要在以编程方式检索与应用相关联的市场活动 ID 时限定为转换，必须满足以下条件：

-   具有已识别的 Microsoft 帐户的客户单击包含自定义市场活动 ID 的应用 URL，然后重定向到该应用的 Windows 应用商店页面。
-   客户在单击 URL 后的同一 Windows 应用商店页面查看期间安装该应用。 如果用户离开页面，然后在 24 小时内返回到该页面（无论在同一台计算机或设备上还是不同的计算机或设备上）并安装应用，这将限定为[通道和转换报告](channels-and-conversions-report.md)的转换，但如果你以编程方式检索市场活动 ID，则这不会限定为转换。

## 将自定义市场活动 ID 嵌入应用的 Windows应用商店页面 URL


若要为应用创建带有自定义市场活动 ID 的 Windows 应用商店页面 URL：

1.  为自定义市场活动创建 ID 字符串。 此字符串最多可包含 100 个字符，尽管我们建议你定义容易识别的简短市场活动 ID。
2.  为应用获取 HTML 或协议格式的 Windows 应用商店页面 URL。 HTML 格式 URL 在[开发人员仪表板的“应用标识”****页](link-to-your-app.md)上提供。
    -   如果你希望客户在浏览器中导航到应用的 Windows 应用商店页面，请使用 HTTP 格式（如果已安装 Windows 应用商店应用，则此 URL 还会将 Windows 应用商店应用启动到应用一览）。 此 URL 格式为 **`https://www.microsoft.com/store/apps/*your app name*/*your app ID*`**。 例如，Skype 的 HTTP URL 是 `https://www.microsoft.com/store/apps/skype/9wzdncrfj364`。
        > **注意** 在运行 Windows 7 和更高版本的计算机和平板电脑上以及运行 Windows Phone 8 和更高版本的手机上，HTTP 格式 URL 可用于在浏览器中导航到 Windows 应用商店。
如果你要从在已安装 Windows 应用商店应用的设备或计算机上运行的其他 Windows 应用推广你的应用，并且希望客户在该 Windows 应用商店应用中打开到你的应用的页面，请使用协议格式。 此 URL 格式为 **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**。 例如，Skype 的协议 URL 是 `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`。
3.  将以下字符串附加到应用的 URL 的末尾：
    -   对于 HTTP 格式 URL，请附加 **`?cid=*my custom campaign ID*`**。 例如，如果 Skype 引入一个带有值 **custom\_campaign** 的市场活动 ID，则包含该市场活动 ID 的新 HTTP URL 将是：`https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`。
    -   对于协议格式 URL，请附加 **`&cid=*my custom campaign ID*`**。 例如，如果 Skype 引入一个带有值 **custom\_campaign** 的市场活动 ID，则包含该市场活动 ID 的新协议 URL 将是：`ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`。

## 以编程方式检索某个应用的自定义市场活动 ID


如果你的应用是 UWP 应用，则可以使用 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 方法以编程方式检索与你的应用相关联的自定义市场活动 ID。 此方法使许多分析和盈利方案成为可能。 例如，你可以查明当前用户是否在通过你的 Facebook 市场活动发现了你的应用后获取了该应用，然后对应用体验进行相应的自定义。 或者，如果你使用的是第三方应用营销提供商，则可以将数据发送回该提供商。

> **注意** 仅当客户单击带有嵌入的市场活动 ID 的 URL、重定向到应用的 Windows 应用商店应用页面，然后在没有离开该页面的情况下安装应用时，[**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 方法将返回一个市场活动 ID 字符串。 使用 **GetAppPurchaseCampaignIdAsync** 时，如果用户离开该页面，然后稍后返回并安装应用，这不会限定为转换。 有关详细信息，请参阅本主题中的[了解转换](#conversions)。

 

以下示例演示如何使用 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 方法检索该应用的自定义市场活动 ID。 如果该应用未作为成功转换的一部分进行安装，则此方法返回空白字符串。

``` csharp
string campaignId = await CurrentApp.GetAppPurchaseCampaignIdAsync();
```

``` cpp
HString campaignId;
HRESULT hr = CurrentApp::GetAppPurchaseCampaignIdAsync(campaignId.GetAddressOf());
```

[
            **GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 方法从 Windows 应用商店访问数据。 在使用此方法时，请使用以下指南：

-   将此方法调用包装在异步操作中以允许调用完成。
-   如果你的应用尚未发布到 Windows 应用商店并且你要测试自定义市场活动，请使用 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 类而不是 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 类的 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) 方法。 请按照以下指南进行操作：
    -   将 **AppPurchaseCampaignId** 元素添加到 WindowsStoreProxy.xml 文件，如以下示例所示。 将元素的值分配到自定义市场活动 ID。 当你运行应用时，[**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) 方法将始终返回此值。 有关 WindowsStoreProxy.xml 文件的详细信息，请参阅 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 的文档。

    ```        XML
    <CurrentApp>
        ....
        <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
    </CurrentApp>
    ```
    
    -   若要在使用 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 和 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 之间轻松切换，我们建议你将以下声明添加到你的代码以定义 **Store** 别名，然后使用 **Store** 别名限定你的 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) 调用。

    ```        CSharp
    #if DEBUG
            using Store = Windows.ApplicationMode.Store.CurrentAppSimulator;
            #else
            using Store = Windows.ApplicationMode.Store.CurrentApp;
            #endif   
    ```

## 测试你的自定义市场活动


推广自定义市场活动 URL 前，我们建议你通过执行以下操作测试自定义市场活动：

1.  在你用于测试的计算机或设备上登录 Microsoft 帐户。
2.  单击你的自定义市场活动 URL。 请确保 Windows 应用商店正确加载你的应用，然后关闭 Windows 应用商店应用或浏览器页面。
3.  再多次单击该 URL，在每次访问应用的页面后关闭 Windows 应用商店应用或浏览器页面。 在其中一次访问你的应用页面时，请安装你的应用以生成转换。 计算你单击 URL 的总次数。
4.  如果你要在应用中以编程方式检索自定义市场活动 ID，请使用 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 类而不是 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 类的 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) 方法测试此操作。

 

 







<!--HONumber=Jun16_HO4-->



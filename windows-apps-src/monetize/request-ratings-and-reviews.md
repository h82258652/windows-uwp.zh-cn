---
Description: Learn about several ways you can programmatically enable customers to rate and review your app.
title: 请求应用的评分和评价
ms.date: 01/22/2019
ms.topic: article
keywords: windows 10, uwp, 评分, 评价
ms.localizationpriority: medium
ms.openlocfilehash: b167f4cc40ee72e6405436bacee28f2f20b4623c
ms.sourcegitcommit: 7a1899358cd5ce9d2f9fa1bd174a123740f98e7a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2019
ms.locfileid: "9042633"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>请求应用的评分和评价

你可以在你的通用 Windows 平台 (UWP) 应用中添加代码以编程方式提示你的客户对应用进行评分或评价。 你可以采用下面几种方法来实现：
* 您可以直接在应用上下文中显示评分和评价对话框。
* 你可以在 Microsoft Store 中以编程方式打开你的应用的评级和评论页面。

当你准备好分析评分和评价数据时，你可以在合作伙伴中心中查看数据或使用 Microsoft Store 分析 API 以编程方式检索此数据。

> [!IMPORTANT]
> 在添加你的应用中的评分函数时，所有评论必须都发送到应用商店的评分机制，而不考虑星级评分所选的用户。 如果你从用户收集反馈或评论，它必须清除它不相关的应用评分或评价的应用商店中，但是直接发送到应用开发人员。 请参阅开发人员行为准则与[Fraudulent 或恶意活动](https://docs.microsoft.com/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities)相关的详细信息。

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>在应用中显示评分和评价对话框

若要以编程方式显示一个对话框，要求你的应用进行评分和提交评价客户在应用中，调用[Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store)命名空间[RequestRateAndReviewAppAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestrateandreviewappasync)方法。 

> [!IMPORTANT]
> 显示评分和评价对话框的请求必须在应用中的 UI 线程上调用。

```csharp
using Windows.ApplicationModel.Store;

private StoreContext _storeContext;

public async Task Initialize()
{
    if (App.IsMultiUserApp) // pseudo-code
    {
        IReadOnlyList<User> users = await User.FindAllAsync();
        User firstUser = users[0];
        _storeContext = StoreContext.GetForUser(firstUser);
    }
    else
    {
        _storeContext = StoreContext.GetDefault();
    }
}

private async Task PromptUserToRateApp()
{
    // Check if we’ve recently prompted user to review, we don’t want to bother user too often and only between version changes
    if (HaveWePromptedUserInPastThreeMonths())  // pseudo-code
    {
        return;
    }

    StoreRateAndReviewResult result = await 
        _storeContext.RequestRateAndReviewAppAsync();

    // Check status
    switch (result.Status)
    { 
        case StoreRateAndReviewStatus.Succeeded:
            // Was this an updated review or a new review, if Updated is false it means it was a users first time reviewing
            if (result.UpdatedExistingRatingOrReview)
            {
                // This was an updated review thank user
                ThankUserForReview(); // pseudo-code
            }
            else
            {
                // This was a new review, thank user for reviewing and give some free in app tokens
                ThankUserForReviewAndGrantTokens(); // pseudo-code
            }
            // Keep track that we prompted user and don’t do it again for a while
            SetUserHasBeenPrompted(); // pseudo-code
            break;

        case StoreRateAndReviewStatus.CanceledByUser:
            // Keep track that we prompted user and don’t prompt again for a while
            SetUserHasBeenPrompted(); // pseudo-code

            break;

        case StoreRateAndReviewStatus.NetworkError:
            // User is probably not connected, so we’ll try again, but keep track so we don’t try too often
            SetUserHasBeenPromptedButHadNetworkError(); // pseudo-code

            break;

        // Something else went wrong
        case StoreRateAndReviewStatus.OtherError:
        default:
            // Log error, passing in ExtendedJsonData however it will be empty for now
            LogError(result.ExtendedError, result.ExtendedJsonData); // pseudo-code
            break;
    }
}
```

**RequestRateAndReviewAppAsync**方法在 Windows 10 版本 1809 中, 引入，它仅用于在项目中面向**Windows 10 2018 年 10 月更新 (10.0;版本 17763）** 或更高版本的 Visual Studio。

### <a name="response-data-for-the-rating-and-review-request"></a>评分和评价请求的响应数据

提交此显示评分和评价对话框的请求后， [StoreRateAndReviewResult](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult)类的[ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult.extendedjsondata)属性将包含 JSON 格式的字符串，它指示请求是否成功。

下面的示例演示在客户成功提交评分或评价之后此请求的返回值。

```json
{ 
  "status": "success", 
  "data": {
    "updated": false
  },
  "errorDetails": "Success"
}
```

下面的示例演示在客户选择不提交评分或评价之后此请求的返回值。

```json
{ 
  "status": "aborted", 
  "errorDetails": "Navigation was unsuccessful"
}
```

下表以 JSON 格式的数据字符串描述这些字段。

| 字段          | 描述                                                                                                                                   |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| *状态*       | 一个字符串，指示客户是否成功提交评分或评价。 受支持的值为 **success** 和 **aborted**。 |
| *数据*         | 一个包含一个名为*已更新*的布尔值的对象。 此值指示客户是否已更新现有评分或评价。 *数据*对象仅包含在成功的响应。 |
| *errorDetails* | 一个包含请求的错误详细信息的字符串。                                                                                     |

## <a name="launch-the-rating-and-review-page-for-your-app-in-the-store"></a>在 Microsoft Store 中对你的应用启动评分和评价页面。

如果你想要以程序方式在 Microsoft Store 中对你的应用打开评分和评价页面，可像此代码示例中演示的那样，使用 ```ms-windows-store://review``` URI 架构的 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) 方法。

```csharp
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://review/?ProductId=9WZDNCRFHVJL"));
```

有关详细信息，请参阅[启动 Microsoft Store 应用](../launch-resume/launch-store-app.md)。

## <a name="analyze-your-ratings-and-reviews-data"></a>分析你的评分和评价数据

要分析你的客户对应用的评分和评价数据，你有多个选择：
* 你可以使用合作伙伴中心中的[评价](../publish/reviews-report.md)报告以查看你的客户的评分和评价。 还可以下载该报告以便脱机查看。
* 你可以使用 Microsoft Store 分析 API 中的 [Get app ratings](get-app-ratings.md) 和 [Get app reviews](get-app-reviews.md) 方法以编程方式获取 JSON 格式的客户评分和评价。

## <a name="related-topics"></a>相关主题

* [向 Microsoft Store 发送请求](send-requests-to-the-store.md)
* [打开 Microsoft Store 应用](../launch-resume/launch-store-app.md)
* [评价报告](../publish/reviews-report.md)

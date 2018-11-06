---
author: Xansky
Description: Learn about several ways you can programmatically enable customers to rate and review your app.
title: 请求应用的评分和评价
ms.author: mhopkins
ms.date: 06/15/2018
ms.topic: article
keywords: windows 10, uwp, 评分, 评价
ms.localizationpriority: medium
ms.openlocfilehash: c00e69ed7d5057db4f835f3d91320806067d86e1
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2018
ms.locfileid: "6050452"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>请求应用的评分和评价

你可以在你的通用 Windows 平台 (UWP) 应用中添加代码以编程方式提示你的客户对应用进行评分或评价。 你可以采用下面几种方法来实现：
* 您可以直接在应用上下文中显示评分和评价对话框。
* 你可以在 Microsoft Store 中以编程方式打开你的应用的评级和评论页面。

当你准备好分析评分和评价数据时，你可以在合作伙伴中心中查看数据，或使用 Microsoft Store 分析 API 以编程方式检索此数据。

> [!IMPORTANT]
> 在添加你的应用内评分函数时，所有评论必须将用户都发送到应用商店的评分机制，而不考虑星级评分所选。 如果你从用户收集反馈或评论，它必须清除它不相关的应用评分或评价的应用商店中，但直接发送到应用开发人员。 请参阅开发人员行为准则[Fraudulent 或恶意活动](https://docs.microsoft.com/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities)相关的详细信息。

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>在应用中显示评分和评价对话框

要以编程方式在应用中显示对话框，以要求客户对应用进行评分和提交评价，请调用 [SendRequestAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync) method in the [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空间。 如此代码示例所示，将整数 16 传递到 *requestKind*参数，将一个空字符串传递到 *parametersAsJson* 参数。 此示例需要来自 Newtonsoft 的 [Json.NET](http://www.newtonsoft.com/json) 库，并且需要对 **Windows.Services.Store**、**System.Threading.Tasks** 和 **Newtonsoft.Json.Linq** 命名空间使用语句。

> [!IMPORTANT]
> 显示评分和评价对话框的请求必须在应用中的 UI 线程上调用。

```csharp
public async Task<bool> ShowRatingReviewDialog()
{
    StoreSendRequestResult result = await StoreRequestHelper.SendRequestAsync(
        StoreContext.GetDefault(), 16, String.Empty);

    if (result.ExtendedError == null)
    {
        JObject jsonObject = JObject.Parse(result.Response);
        if (jsonObject.SelectToken("status").ToString() == "success")
        {
            // The customer rated or reviewed the app.
            return true;
        }
    }

    // There was an error with the request, or the customer chose not to
    // rate or review the app.
    return false;
}
```

**SendRequestAsync** 方法使用基于整数的简单请求系统和基于 JSON 的数据参数向应用公开其他 Microsoft Store 操作。 当你将整数 16 传递到 * requestKind*参数时，可发出请求以显示评分和评价对话框，并向 Microsoft Store 发送相关数据。 此方法是在 Windows 10 版本 1607 中引入，它仅可用于面向 **Windows 10 周年纪念版（10.0；版本 14393）** 或 Visual Studio 更高版本的项目中。 有关此方法的一般概述，请参阅[向 Microsoft Store 发送请求](send-requests-to-the-store.md)。

### <a name="response-data-for-the-rating-and-review-request"></a>评分和评价请求的响应数据

提交此显示评分和评价对话框的请求之后，[StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult)  返回值的 [Response](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.Response) 属性将包含 JSON 格式的字符串，它指示请求是否成功。

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

|  字段  |  描述  |
|----------------------|---------------|
|  *状态*                   |  一个字符串，指示客户是否成功提交评分或评价。 受支持的值为 **success** 和 **aborted**。   |
|  *数据*                   |  一个包含一个名为*已更新*的布尔值的对象。 此值指示客户是否已更新现有评分或评价。 *数据*对象仅包含在成功的响应。   |
|  *errorDetails*                   |  一个包含请求的错误详细信息的字符串。 |

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

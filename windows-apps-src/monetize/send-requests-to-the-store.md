---
author: Xansky
Description: You can use the SendRequestAsync method to send requests to the Microsoft Store for operations that do not yet have an API available in the Windows SDK.
title: 向 Microsoft Store 发送请求
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, StoreRequestHelper, SendRequestAsync
ms.localizationpriority: medium
ms.openlocfilehash: 6463f6eee6d3f5ec82122cef532db8d0e9a26dc6
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "5520545"
---
# <a name="send-requests-to-the-microsoft-store"></a>向 Microsoft Store 发送请求

从 Windows 10 版本 1607 起，Windows SDK 在 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空间中为与 Microsoft Store 相关的操作（如应用内购买）提供 API。 但是，尽管支持应用商店的服务在操作系统发布过程中不断地更新、扩展和提高，但新的 API 通常只会在主要操作系统版本中添加到 Windows SDK。

我们提供的 [SendRequestAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync) 方法可以在发布新版本的 Windows SDK 之前灵活地为通用 Windows 平台 (UWP) 应用提供新的应用商店操作。 你可以使用此方法为最新版本的 Windows SDK 中尚未提供对应 API 的新操作向应用商店发送请求。

> [!NOTE]
> **SendRequestAsync** 方法仅可用于针对 Windows 10 版本 1607 或更高版本的应用。 此方法支持的有些请求仅在 Windows 10 版本 1607 之后的版本中受支持。

**SendRequestAsync** 是一种 [StoreRequestHelper](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper) 类的静态方法。 若要调用此方法，你必须将以下信息传递给此方法：
* [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 对象，用于提供与你想要为其执行操作的用户相关的信息。 有关此对象的更多信息，请参阅 [StoreContext 类入门](in-app-purchases-and-trials.md#get-started-with-the-storecontext-class)。
* 用于标识你想要向应用商店发送的请求的整数。
* 如果请求支持任何参数，你还可以传递 JSON 格式的字符串，它包含要与请求一起传递的参数。

以下示例展示了如何调用此方法。 本示例要求使用适用于 **Windows.Services.Store** 和 **System.Threading.Tasks** 命名空间的语句。

```csharp
public async Task<bool> AddUserToFlightGroup()
{
    StoreSendRequestResult result = await StoreRequestHelper.SendRequestAsync(
        StoreContext.GetDefault(), 8,
        "{ \"type\": \"AddToFlightGroup\", \"parameters\": \"{ \"flightGroupId\": \"your group ID\" }\" }");

    if (result.ExtendedError == null)
    {
        return true;
    }

    return false;
}
```

请参阅以下部分，了解与当前可通过 **SendRequestAsync** 方法发送的请求相关的信息。 如果添加了新请求的支持，我们将会更新此文章。

## <a name="request-for-in-app-ratings-and-reviews"></a>请求应用内评分和评价

你可以在应用中通过将请求整数 16 传递给 **SendRequestAsync** 方法，以编程方式启动对话框，请客户对应用进行评分和提交评价。 有关详细信息，请参阅[在应用中显示评分和评价对话框](request-ratings-and-reviews.md#show-a-rating-and-review-dialog-in-your-app)。

## <a name="requests-for-flight-group-scenarios"></a>适合于外部测试版组方案的请求

> [!IMPORTANT]
> 对于大多数开发人员帐户来说，本部分介绍的所有外部测试版组请求目前尚不可用。 除非你的开发人员帐户已由 Microsoft 专门预配，否则这些请求将会失败。

**SendRequestAsync** 方法支持一组外部测试版组方案的请求，例如为外部测试版组添加用户或设备。 若要提交这些请求，请将 *requestKind* 参数值 7 或 8 以及 JSON 格式的字符串一起传递给用于指示你想要与任何相关参数一起提交的请求的 *parametersAsJson* 参数。 这些 *requestKind* 值在以下方面不同。

|  请求类型值  |  描述  |
|----------------------|---------------|
|  7                   |  此请求将在当前设备的上下文中执行。 此值仅可用于 Windows 10 版本 1703 或更高版本。  |
|  8                   |  此请求将在当前已登录到应用商店的用户的上下文中执行。 此值可用于 Windows 10 版本 1607 或更高版本。  |

当前实施的是以下外部测试版组请求。

### <a name="retrieve-remote-variables-for-the-highest-ranked-flight-group"></a>检索排名最高的外部测试版组的远程变量

> [!IMPORTANT]
> 对于大多数开发人员帐户来说，此请求当前尚不可用。 除非你的开发人员帐户已由 Microsoft 专门预配，否则此请求将会失败。

此请求将为当前用户或设备检索排名最高的外部测试版组的远程变量。 若要发送此请求，请将以下信息传递至 **SendRequestAsync** 方法的 *requestKind* 和 *parametersAsJson* 参数。

|  参数  |  描述  |
|----------------------|---------------|
|  *requestKind*                   |  指定 7 以返回设备的最高排名外部测试版组，或者指定 8 以返回当前用户和设备的最高排名外部测试版组。 我们建议为 *requestKind* 参数使用值 8，因为此值将在成员中返回当前用户和设备的最高排名外部测试版组。  |
|  *parametersAsJson*                   |  传递 JSON 格式的字符串，它包含以下示例显示的数据。  |

以下示例显示了要传递至 *parametersAsJson* 的 JSON 数据格式。 必须为*类型*字段分配字符串 *GetRemoteVariables*。 在 Windows 开发人员中心仪表板中为你已在其中定义了远程变量的项目 ID 分配 *projectId* 字段。

```json
{ 
    "type": "GetRemoteVariables", 
    "parameters": "{ \"projectId\": \"your project ID\" }" 
}
```

提交此请求之后，[StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) 返回值的 [Response](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.Response) 属性将包含 JSON 格式的字符串及以下字段。

|  字段  |  描述  |
|----------------------|---------------|
|  *匿名*                   |  布尔值，其中 **true** 指示用户或设备身份不存在于请求中，**false** 指示用户或设备身份已存在于请求中。  |
|  *名称*                   |  包含设备或用户所在的最高排名外部测试版组名称的字符串。  |
|  *设置*                   |  键/值对的字典，包含开发人员为外部测试版组配置的远程变量的名称和值。  |

以下示例展示了此请求的返回值。

```json
{ 
  "anonymous": false, 
  "name": "Insider Slow",
  "settings":
  {
      "Audience": "Slow"
      ...
  }
}
```

### <a name="add-the-current-device-or-user-to-a-flight-group"></a>将当前设备或用户添加到外部测试版组

> [!IMPORTANT]
> 对于大多数开发人员帐户来说，此请求当前尚不可用。 除非你的开发人员帐户已由 Microsoft 专门设置，否则此请求将会失败。

若要发送此请求，请将以下信息传递至 **SendRequestAsync** 方法的 *requestKind* 和 *parametersAsJson* 参数。

|  参数  |  描述  |
|----------------------|---------------|
|  *requestKind*                   |  指定 7 以将设备添加到外部测试版组，或者指定 8 以将当前已登录到应用商店的用户添加到外部测试版组。  |
|  *parametersAsJson*                   |  传递 JSON 格式的字符串，它包含以下示例显示的数据。  |

以下示例显示了要传递至 *parametersAsJson* 的 JSON 数据格式。 必须为*类型*字段分配字符串 *AddToFlightGroup*。 为你想要向其中添加设备或用户的外部测试版组 ID 分配 *flightGroupId* 字段。

```json
{ 
    "type": "AddToFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

如果请求中没有错误，则 [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) 返回值的 [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.HttpStatusCode) 属性将包含响应代码。

### <a name="remove-the-current-device-or-user-from-a-flight-group"></a>从外部测试版组中删除当前设备或用户

> [!IMPORTANT]
> 对于大多数开发人员帐户来说，此请求当前尚不可用。 除非你的开发人员帐户已由 Microsoft 专门设置，否则此请求将会失败。

若要发送此请求，请将以下信息传递至 **SendRequestAsync** 方法的 *requestKind* 和 *parametersAsJson* 参数。

|  参数  |  描述  |
|----------------------|---------------|
|  *requestKind*                   |  指定 7 以从外部测试版组中删除设备，或者指定 8 以从外部测试版组中删除当前已登录到应用商店的用户。  |
|  *parametersAsJson*                   |  传递 JSON 格式的字符串，它包含以下示例显示的数据。  |

以下示例显示了要传递至 *parametersAsJson* 的 JSON 数据格式。 必须为*类型*字段分配字符串 *RemoveFromFlightGroup*。 为你想要从其中删除设备或用户的外部测试版组 ID 分配 *flightGroupId* 字段。

```json
{ 
    "type": "RemoveFromFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

如果请求中没有错误，则 [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) 返回值的 [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.HttpStatusCode) 属性将包含响应代码。

## <a name="related-topics"></a>相关主题

* [在应用中显示评分和评价对话框](request-ratings-and-reviews.md#show-a-rating-and-review-dialog-in-your-app)
* [SendRequestAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync)

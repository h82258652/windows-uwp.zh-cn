---
title: 对实时活动服务进行编程
author: KevinAsgari
description: 了解如何使用 C++ API 对 Xbox Live 实时活动服务进行编程。
ms.assetid: 98cdcb1f-41d8-43db-98fc-6647755d3b17
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 实时活动
ms.localizationpriority: low
ms.openlocfilehash: 77b2be18fdda6dbf173814787bde151f088086c2
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="programming-the-real-time-activity-service-using-c-apis"></a>使用 C++ API 对实时活动服务进行编程

本文包含以下部分

* 从 Xbox Live 连接到实时活动服务
* 与实时活动服务断开连接
* 创建统计数据
* 通过实时活动订阅统计数据
* 通过实时活动服务取消订阅统计数据
* 示例

## <a name="connecting-to-the-real-time-activity-service-from-xbox-live"></a>从 Xbox Live 连接到实时活动服务

应用程序必须从 Xbox Live 连接到实时活动 (RTA) 服务，才能获取事件信息。 本主题介绍如何建立此类连接。

> [!NOTE]
> 本主题中使用的示例指出了适用于单个用户的方法调用。 但是，你的作品必须为所有用户执行这些调用，才能与实时活动 (RTA) 服务建立连接或断开连接。

### <a name="connecting-to-the-real-time-activity-service"></a>连接到实时活动服务

```cpp
void Example_RealTimeActivity_ConnectAsync()
{
    // Get the list of users from the system, and create an Xbox Live context from the first.
    // This user's authentication token will be used for the service requests.

    // Note, only retrieve an XboxLiveContext for a given user *once*.  Otherwise you may encounter unpredictable behavior.
    std::shared_ptr<xbox_live_context> xboxLiveContext = std::make_shared<xbox_live_context>(User::Users->GetAt(0));

    xboxLiveContext->real_time_activity_service()->activate();
}
```

### <a name="creating-a-statistic"></a>创建统计数据

如果你是 XDK 开发人员，或者正在开发跨平台联机作品，则可以在 XDP 上创建统计数据。  如果你要创建在 Windows 10 上运行的纯 UWP 应用，则可在开发人员中心上创建统计数据。

#### <a name="xdk-developers"></a>XDK 开发人员

有关如何在 XDP 上创建统计数据的信息，请参阅 [XDP 文档](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_10_27_15_a.aspx#events)。  在创建了统计数据并定义了事件后，你将需要运行 [XCETool](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/atoc_xce_jun15.aspx)，以生成供你的应用程序使用的标头。  此标头包含你可以调用以发送修改统计数据的事件的函数。

#### <a name="uwp-developers"></a>UWP 开发人员

如果你要在 Windows 10 上进行非跨平台联机作品的 UWP 开发，则可以在开发人员中心上定义你的统计数据。  我们将很快提供有关如何执行此操作的文档。  与此同时，如果你有任何疑问，请联系你的 DAM。

### <a name="disconnecting-from-the-real-time-activity-service"></a>与实时活动服务断开连接

```cpp
void Example_RealTimeActivity_Disconnect()
{
    // Get the list of users from the system, and create an Xbox Live context from the first.
    // This user's authentication token will be used for the service requests.
    std::shared_ptr<xbox_live_context> xboxLiveContext = std::make_shared<xbox_live_context>(User::Users->GetAt(0));

    xboxLiveContext->real_time_activity_service()->deactivate();
}
```

## <a name="subscribing-to-a-statistic-from-the-real-time-activity"></a>通过实时活动订阅统计数据

应用程序可订阅实时活动 (RTA)，以在 Xbox 开发人员门户 (XDP) 中配置的统计数据发生更改时获取更新。

### <a name="subscribing-to-a-statistic-from-the-real-time-activity-service"></a>通过实时活动服务订阅统计数据

```cpp
void Example_RealTimeActivity_SubscribeToStatisticChangeAsync()
{
    // Obtain xboxLiveContext as shown in the connect function.  Then add a handler to be called on statistic changes.
    function_context statisticsChangeContext = xboxLiveContext->user_statistics_service().add_statistic_changed_handler(
        [](statistic_change_event_args args )
        {
            // Called on statistic change.  Inspect args to see which one.
            DebugPrint("%S %S", args.latest_statistic().statistic_name().c_str(), args.latest_statistic().value().c_str());
        }
    );

    // Call to subscribe to an individual statistic.  Once the subscription is complete, the handler will be called with the initial value of the statistic.
    auto statisticResults = xboxLiveContext->user_statistics_service().subscribe_to_statistic_change(
        User::Users->GetAt(0)->XboxUserId->Data(),
        L"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx",    // Get SCID from "Product Details" page in XDP
         L"YourStat"
        );

    std::shared_ptr<statistic_change_subscription> statisticsChangeSubscription;

    if(!statisticResults.err())
    {
        statisticsChangeSubscription = statisticResults.payload();
    }
    else
    {
        DebugPrint("Error calling SubscribeToStatistic: %S", statisticResults.err_message().c_str());
    }
}
```

## <a name="unsubscribing-from-a-statistic-from-the-real-time-activity-service"></a>通过实时活动服务取消订阅统计数据

应用程序可通过实时活动 (RTA) 服务订阅统计数据，以在统计数据发生更改时获取更新。 如果不再需要这些更新，则可以终止该订阅。本主题中的代码将会演示如何执行该操作。

### <a name="unsubscribing-from-a-real-time-services-statistic"></a>取消订阅实时服务统计数据

```cpp
void Example_RealTimeActivity_UnsubscribeFromStatisticChangeAsync()
{
    // statisticsChangeSubscription from the Example_RealTimeActivity_SubscribeToStatisticChangeAsync function.
    xboxLiveContext->user_statistics_service().unsubscribe_from_statistic_change(
        statisticsChangeSubscription
        );
}
```

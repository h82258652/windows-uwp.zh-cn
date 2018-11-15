---
author: TylerMSFT
title: 在后台无限期运行
description: 使用 extendedExecutionUnconstrained 功能可在后台无限期运行后台任务或扩展执行会话。
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: 后台任务，扩展执行，资源、 限制，后台任务
ms.author: twhitney
ms.date: 10/3/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: eb6e0735620c4a940d3414f22aaa4f09bb608424
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6674618"
---
# <a name="run-in-the-background-indefinitely"></a>在后台无限期运行

为了为用户提供最佳体验，Windows 对通用 Windows 平台 (UWP) 应用实施了资源限制。 前台应用获得了大部分内存和执行时间；后台应用获得的较少。 因此，会防止用户遇到前台应用性能不佳和电池严重消耗现象。

但是，编写供个人使用的 UWP 应用（即，不在 Microsoft Store 中发布的旁加载应用）的开发人员或编写企业 UWP 应用的开发人员可能希望使用设备上可用的所有资源，而不想受到任何后台或扩展执行的限制。 业务线和个人 UWP 应用程序可以使用 Windows 创意者更新（版本 1703）中的 API 来关闭限制。 请注意，如果应用使用这些 API，则无法将该应用放入 Microsoft Store 中。

## <a name="run-while-minimized"></a>最小化时运行

UWP 应用未在前台运行时将变为暂停状态。 在桌面上，用户最小化应用时会出现这种情况。 应用将使用扩展执行会话以在最小化时继续运行。 [使用扩展执行来推迟应用挂起](https://docs.microsoft.com/windows/uwp/launch-resume/run-minimized-with-extended-execution)中详细介绍了 Microsoft Store 接受的扩展执行 API。

如果你要开发一款不打算提交到 Microsoft Store 的应用，则可以将 [ExtendedExecutionForegroundSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) 与 `extendedExecutionUnconstrained` 受限功能配合使用，以便你的应用可以在最小化时继续运行，而不用考虑设备能耗状态。  

`extendedExecutionUnconstrained` 功能已作为受限功能添加到应用清单中。 有关受限功能的详细信息，请参阅[应用功能声明](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)。

_Package.appxmanifest_
```xml
<Package ...>
...
  <Capabilities>  
    <rescap:Capability Name="extendedExecutionUnconstrained"/>  
  </Capabilities>  
</Package>
```

当你使用 `extendedExecutionUnconstrained` 功能时，会使用 [ExtendedExecutionForegroundSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) 和 [ExtendedExecutionForegroundReason](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason)，而不是使用 [ExtendedExecutionSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionsession) 和 [ExtendedExecutionReason](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason)。 用于创建会话、设置成员以及异步请求扩展的相同模式仍然适用： 

```cs
var newSession = new ExtendedExecutionForegroundSession();  
newSession.Reason = ExtendedExecutionForegroundReason.Unconstrained;  
newSession.Description = "Long Running Processing";  
newSession.Revoked += SessionRevoked;  
ExtendedExecutionResult result = await newSession.RequestExtensionAsync();  
switch (result)  
{  
    case ExtendedExecutionResult.Allowed:  
        DoLongRunningWork();  
        break;  

    default:  
    case ExtendedExecutionResult.Denied:  
        DoShortRunningWork();  
        break;  
}
```

一旦应用进入前台，你就可以请求此扩展执行会话。 不受约束的扩展执行会话不受能耗配额或操作系统节电模式的限制。 只要存在对会话对象的引用，应用就将保持运行状态，而不会进入挂起状态。 如果用户关闭了该应用，则将吊销会话。

注册 **Revoked** 事件将使你的应用能够进行任何所需的清理工作。 处于挂起状态时，你可以利用 [ExtendedExecutionReason.SavingData](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason) 创建扩展执行会话，以在终止应用并将其从内存中删除之前保存用户数据。

## <a name="run-background-tasks-indefinitely"></a>无限期运行后台任务

在通用 Windows 平台上，后台任务是在没有任何形式用户界面的后台运行的进程。 在取消之前，后台任务一般最多可以运行二十五秒。 一些长时间运行的任务也会进行检查，以确保后台任务未处于空闲状态或未使用内存。 Windows 创意者更新（版本 1703）中引入了 [extendedBackgroundTaskTime](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) 受限功能，以删除这些限制。 **extendedBackgroundTaskTime** 功能已作为受限功能添加到应用清单文件中：

_Package.appxmanifest_
```xml
<Package ...>
   <Capabilities>  
       <rescap:Capability Name="extendedBackgroundTaskTime"/>  
   </Capabilities>  
</Package>
```

此功能将删除执行时间限制和空闲任务监视程序。 启动后台任务后，无论是通过触发器启动还是通过应用服务调用启动，一旦该任务对 **Run** 方法提供的 [BackgroundTaskInstance](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) 执行了延迟，就可以无限期运行。 如果将应用设置为**由 Windows 管理**，那么仍然可能会对它应用能耗配额，并且其后台任务在“节电模式”处于活动状态时将不会被激活。 可以使用操作系统设置对其进行更改。 [优化后台活动](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)中提供了详细信息。

通用 Windows 平台会监视后台任务执行情况，以确保电池使用时间长久并且拥有顺畅的前台应用体验。 但是，个人应用和企业业务线应用可以使用扩展执行和 **extendedBackgroundTaskTime** 功能，以创建只要需要就会运行而不考虑设备资源可用性的应用。

请注意，**extendedExecutionUnconstrained** 和 **extendedBackgroundTaskTime** 功能可能会替代 UWP 应用的默认策略，并且可能会导致电池严重消耗。 在使用这些功能之前，请先确认默认的扩展执行和后台任务时间策略是否未满足你的需求，并在电池受限条件下执行测试，以了解你的应用将对设备具有的影响。

## <a name="see-also"></a>另请参阅

[删除后台任务资源限制](https://docs.microsoft.com/windows/application-management/enterprise-background-activity-controls)

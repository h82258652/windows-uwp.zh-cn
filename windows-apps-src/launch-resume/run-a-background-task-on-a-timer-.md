---
author: mcleblanc
title: 在计时器上运行后台任务
description: 了解如何计划一次性后台任务，或运行定期后台任务。
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
---

# 在计时器上运行后台任务


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**TimeTrigger 类**](https://msdn.microsoft.com/library/windows/apps/br224843)
-   [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494)

了解如何计划一次性后台任务，或运行定期后台任务。

-   此示例假定你的后台任务需要定期运行，或在特定时间运行以支持你的应用。 如果你已调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)，后台任务将仅使用 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) 运行。
-   本主题假定你已创建一个后台任务类，其中包含用作后台任务入口点的 Run 方法。 若要快速生成后台任务，请参阅[创建和注册后台任务](create-and-register-a-background-task.md)。 有关条件和触发器的详细信息，请参阅[使用后台任务支持应用](support-your-app-with-background-tasks.md)。

## 创建时间触发器


-   创建新的 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)。 第二个参数 *OneShot* 指定后台任务是运行一次还是保持周期性运行。 如果 *OneShot* 设置为 true，则第一个参数 (*FreshnessTime*) 会指定在计划后台任务之前需等待的分钟数。 如果 *OneShot* 设置为 false，则 *FreshnessTime* 会指定后台任务的运行频率。

    通用 Windows 平台 (UWP) 应用的内置计时器以 15 分钟的间隔运行后台任务。

    -   如果 *FreshnessTime* 设置为 15 分钟并且 *OneShot* 为 true，则任务将从其注册之时起 0 至 15 分钟内运行一次该任务。

    -   如果 *FreshnessTime* 设置为 15 分钟并且 *OneShot* 为 false，则任务将从其注册之时起 0 至 15 分钟内每隔 15 分钟运行一次该任务。

    **注意** 如果 *FreshnessTime* 设置为少于 15 分钟，则在尝试注册后台任务时将引发异常。

     

    例如，该触发器将导致后台任务每小时运行一次：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
    > ```
    > ```cpp
    > TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
    > ```

## （可选）添加条件


-   如果需要，创建一个后台任务条件以控制任务何时运行。 防止后台任务在未满足条件之前运行的条件 - 有关详细信息，请参阅 [设置运行后台任务的条件](set-conditions-for-running-a-background-task.md)。

    在此示例中，条件设置为 **UserPresent**，以便在触发之后，在用户处于活动状态时才运行一次该任务。 有关可能条件的列表，请参阅 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)。

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemCondition userCondition = new SystemCondition(SystemConditionType.UserPresent);
    > ```
    > ```cpp
    > SystemCondition ^ userCondition = ref new SystemCondition(SystemConditionType::UserPresent)
    > ```

##  调用 RequestAccessAsync()


-   在尝试注册 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) 后台任务之前，调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494)。

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > BackgroundExecutionManager.RequestAccessAsync();
    > ```
    > ```cpp
    > BackgroundExecutionManager::RequestAccessAsync();
    > ```

## 注册后台任务


-   通过调用后台任务注册函数注册后台任务。 有关注册后台任务的详细信息，请参阅[注册后台任务](register-a-background-task.md)。

    下面的代码将注册后台任务：

    > > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > string taskName   = "Example hourly background task";
    > 
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > String ^ taskName   = "Example hourly background task";
    > 
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
    > ```
    
    > **注意** 后台任务注册参数在注册时进行验证。 如果有任何注册参数无效，则会返回一个错误。 确保你的应用能够流畅地处理后台任务注册失败的情况，否则，如果你的应用依赖于在尝试注册任务后具备有效注册对象，则它可能会崩溃。

   
## 备注

> **注意** 从 Windows 10 开始，用户无须再将你的应用添加到锁屏界面，即可利用后台任务。 有关后台任务触发器类型的指南，请参阅[使用后台任务支持应用](support-your-app-with-background-tasks.md)。

> **注意** 本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你面向 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。


## 相关主题


* [创建和注册后台任务](create-and-register-a-background-task.md)
* [在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md)
* [处理取消的后台任务](handle-a-cancelled-background-task.md)
* [监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)
* [注册后台任务](register-a-background-task.md)
* [使用后台任务响应系统事件](respond-to-system-events-with-background-tasks.md)
* [设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)
* [使用后台任务更新动态磁贴](update-a-live-tile-from-a-background-task.md)
* [使用维护触发器](use-a-maintenance-trigger.md)
* [后台任务指南](guidelines-for-background-tasks.md)

* [调试后台任务](debug-a-background-task.md)
* [如何在 Windows 应用商店应用中触发暂停、恢复和后台事件（在调试时）](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 





<!--HONumber=May16_HO2-->



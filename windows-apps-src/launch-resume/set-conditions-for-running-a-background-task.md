---
设置后台任务的运行条件
了解如何设置控制何时运行后台任务的条件。
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
---

# 设置后台任务的运行条件


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)
-   [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)

了解如何设置控制何时运行后台任务的条件。

有时，除了触发该任务的事件之外，后台任务还需要满足某些条件以便后台任务可以继续。 你可以在注册后台任务时指定由 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) 指定的一个或多个条件。 引发触发器之后将检查条件；后台任务将进入队列，但在满足所有所需条件之前不会运行。

对后台任务设置条件可阻止任务不必要地运行，从而节省电池电量和 CPU 运行时。 例如，如果你的后台任务在计时器上运行并要求 Internet 连接，请在注册该任务之前将 **InternetAvailable** 条件添加到 [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)。 通过在计时器时间过去以及 Internet 可用时让任务运行，有助于防止任务使用不必要的系统资源和电池寿命。

**注意** 通过多次调用同一 [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 上的 AddCondition 可组合多个条件。 注意不要添加冲突条件，如 **UserPresent** 和 **UserNotPresent**。

 

## 创建 SystemCondition 对象


本主题假定你的后台任务已与你的应用关联，且你的应用已包含用于创建名为 **taskBuilder** 的 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 对象的代码

添加该条件之前，创建一个代表该条件的 [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) 对象，该对象必须实际用于运行后台任务。 在构造函数中，通过提供一个 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) 枚举值指定必须满足的条件。

以下代码创建一个 [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) 对象，该对象将 Internet 可用性指定为条件要求：

> [!div class="tabbedCodeSnippets"]
> ```cs
> SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
> ```
> ```cpp
> SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
> ```

## 向你的后台任务中添加 SystemCondition 对象


要添加条件，请在 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 对象上调用 [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) 方法，并向其传递 [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) 对象。

下面的代码将使用 TaskBuilder 注册 InternetAvailable 后台任务条件：

> [!div class="tabbedCodeSnippets"]
> ```cs
> taskBuilder.AddCondition(internetCondition);
> ```
> ```cpp
> taskBuilder->AddCondition(internetCondition);
> ```

## 注册后台任务


现在，你便可以使用 [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772) 方法注册后台任务了，该任务在满足指定的条件之前不会启动。

以下代码注册该任务并存储所得到的 BackgroundTaskRegistration 对象：

> [!div class="tabbedCodeSnippets"]
> ```cs
> BackgroundTaskRegistration task = taskBuilder.Register();
> ```
> ```cpp
> BackgroundTaskRegistration ^ task = taskBuilder->Register();
> ```

> **注意** 通用 Windows 应用必须在注册任何后台触发器类型之前调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。

若要确保通用 Windows 应用在你发布更新后继续正常运行，必须在启动已经过更新的应用时调用 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)，然后调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。 有关详细信息，请参阅[后台任务指南](guidelines-for-background-tasks.md)。

> **注意** 后台任务注册参数在注册时进行验证。 如果有任何注册参数无效，则会返回一个错误。 确保你的应用能够流畅地处理后台任务注册失败的情况，否则，如果你的应用依赖于在尝试注册任务后具备有效注册对象，则它可能会崩溃。

## 在后台任务上放置多个条件

若要添加多个条件，你的应用需要多次调用 [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) 方法。 任务注册生效之前，必须进行这些调用。

> **注意** 小心不要向后台任务中添加冲突的条件。
 

以下代码段在创建和注册后台任务的上下文中显示了多个条件：

> [!div class="tabbedCodeSnippets"]
```cs
> // 
> // Set up the background task.
> // 
> 
> TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
> 
> var recurringTaskBuilder = new BackgroundTaskBuilder();
> 
> recurringTaskBuilder.Name           = "Hourly background task";
> recurringTaskBuilder.TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
> recurringTaskBuilder.SetTrigger(hourlyTrigger);
> 
> // 
> // Begin adding conditions.
> // 
> 
> SystemCondition userCondition     = new SystemCondition(SystemConditionType.UserPresent);
> SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
> 
> recurringTaskBuilder.AddCondition(userCondition);
> recurringTaskBuilder.AddCondition(internetCondition);
> 
> // 
> // Done adding conditions, now register the background task.
> // 
> 
> BackgroundTaskRegistration task = recurringTaskBuilder.Register();
> ```
> ```cpp
> // 
> // Set up the background task.
> // 
> 
> TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
> 
> auto recurringTaskBuilder = ref new BackgroundTaskBuilder();
> 
> recurringTaskBuilder->Name           = "Hourly background task";
> recurringTaskBuilder->TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
> recurringTaskBuilder->SetTrigger(hourlyTrigger);
> 
> // 
> // Begin adding conditions.
> // 
> 
> SystemCondition ^ userCondition     = ref new SystemCondition(SystemConditionType::UserPresent);
> SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
> 
> recurringTaskBuilder->AddCondition(userCondition);
> recurringTaskBuilder->AddCondition(internetCondition);
> 
> // 
> // Done adding conditions, now register the background task.
> // 
> 
> BackgroundTaskRegistration ^ task = recurringTaskBuilder->Register();
> ```

## Remarks


> **Note**  Chose the right conditions for your background task so that it only runs when it's needed, and doesn't run when it won't work. See [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) for descriptions of the different background task conditions.

> **Note**  This article is for Windows 10 developers writing Universal Windows Platform (UWP) apps. If you’re developing for Windows 8.x or Windows Phone 8.x, see the [archived documentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Related topics


****

* [Create and register a background task](create-and-register-a-background-task.md)
* [Declare background tasks in the application manifest](declare-background-tasks-in-the-application-manifest.md)
* [Handle a cancelled background task](handle-a-cancelled-background-task.md)
* [Monitor background task progress and completion](monitor-background-task-progress-and-completion.md)
* [Register a background task](register-a-background-task.md)
* [Respond to system events with background tasks](respond-to-system-events-with-background-tasks.md)
* [Update a live tile from a background task](update-a-live-tile-from-a-background-task.md)
* [Use a maintenance trigger](use-a-maintenance-trigger.md)
* [Run a background task on a timer](run-a-background-task-on-a-timer-.md)
* [Guidelines for background tasks](guidelines-for-background-tasks.md)

****

* [Debug a background task](debug-a-background-task.md)
* [How to trigger suspend, resume, and background events in Windows Store apps (when debugging)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 





<!--HONumber=Mar16_HO1-->



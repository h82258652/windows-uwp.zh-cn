---
author: TylerMSFT
title: "注册后台任务"
description: "了解如何创建可以重复使用以安全注册大部分后台任务的函数。"
ms.assetid: 8B1CADC5-F630-48B8-B3CE-5AB62E3DFB0D
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: acee438ae29b568bec20ff1225e8e801934e6c50

---

# 注册后台任务


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**BackgroundTaskRegistration 类**](https://msdn.microsoft.com/library/windows/apps/br224786)
-   [**BackgroundTaskBuilder 类**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**SystemCondition 类**](https://msdn.microsoft.com/library/windows/apps/br224834)

了解如何创建可以重复使用以安全注册大部分后台任务的函数。

本主题假定你已拥有需要注册的后台任务。 （请参阅[创建和注册后台任务](create-and-register-a-background-task.md)以获取有关如何编写后台任务的信息）。

本主题指导完成注册后台任务的实用工具函数。 此实用工具函数在注册任务前首先多次检查现有注册以避免多次注册产生的错误，并且该函数可以将系统条件应用于后台任务。 本操作实例包括此实用工具函数的正常运行的完整示例。

**注意**  

通用 Windows 应用必须在注册任何后台触发器类型之前调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。

若要确保通用 Windows 应用在你发布更新后继续正常运行，必须在启动已经过更新的应用时调用 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)，然后调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。 有关详细信息，请参阅[后台任务指南](guidelines-for-background-tasks.md)。

## 定义方法签名并返回类型


此方法包含任务入口点、任务名称、预构建的后台任务触发器以及后台任务的 [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)（可选）。 此方法返回 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) 对象。

> [!div class="tabbedCodeSnippets"]
> ```cs
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint,
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     
>     // We'll add code to this function in subsequent steps.
>
> }
> ```
> ```cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>     
>     // We'll add code to this function in subsequent steps.
>
> }
> ```

## [!div class="tabbedCodeSnippets"]


检查现有注册 检查任务是否已注册。

请务必检查此项，因为如果任务已多次注册，则将在该任务触发时运行多次，这可占用过多的 CPU 并可能导致意外行为。 你可以通过查询 [**BackgroundTaskRegistration.AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787) 属性并在结果上迭代来检查现有注册。

> 检查每个实例的名称 - 如果该名称与正注册的任务的名称匹配，则跳出循环并设置标志变量，以便你的代码可以在下一步中选择不同的路径。 **注意** 使用应用唯一的后台任务名称。

确保每个后台任务都具有唯一的名称。

> [!div class="tabbedCodeSnippets"]
> ```cs
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint,
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
>
>         if (cur.Value.Name == name)
>         {
>             //
>             // The task is already registered.
>             //
>
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>     
>     // We'll register the task in the next step.
> }
> ```
> ```cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>     
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>     
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>         
>         if(cur->Name == name)
>         {
>             //
>             // The task is registered.
>             //
>             
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>         
>         hascur = iter->MoveNext();
>     }
>     
>     // We'll register the task in the next step.
> }
> ```

## 以下代码使用我们在上一步中创建的 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) 注册后台任务：


[!div class="tabbedCodeSnippets"] 注册后台任务（或返回现有注册）

检查在现有后台任务注册的列表中是否找到该任务。 如果有，则返回该任务的实例。 否则，使用新 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 对象注册任务。

> 此代码应检查条件参数是否为空，如果不为空，则将条件添加到注册对象。 返回 [**BackgroundTaskBuilder.Register**](https://msdn.microsoft.com/library/windows/apps/br224772) 方法返回的 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)。 **注意** 后台任务注册参数在注册时进行验证。

如果有任何注册参数无效，则会返回一个错误。

> [!div class="tabbedCodeSnippets"]
> ```cs
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint,
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
>
>         if (cur.Value.Name == taskName)
>         {
>             //
>             // The task is already registered.
>             //
>
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>     
>     //
>     // Register the background task.
>     //
>
>     var builder = new BackgroundTaskBuilder();
>
>     builder.Name = name;
>     builder.TaskEntryPoint = taskEntryPoint;
>     builder.SetTrigger(trigger);
>
>     if (condition != null)
>     {
>
>         builder.AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration task = builder.Register();
>
>     return task;
> }
> ```
> ```cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>
>     //
>     // Check for existing registrations of this background task.
>     //
>     
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>     
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>         
>         if(cur->Name == name)
>         {
>             //
>             // The task is registered.
>             //
>             
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>         
>         hascur = iter->MoveNext();
>     }
>     
>     //
>     // Register the background task.
>     //
>
>     auto builder = ref new BackgroundTaskBuilder();
>
>     builder->Name = name;
>     builder->TaskEntryPoint = taskEntryPoint;
>     builder->SetTrigger(trigger);
>
>     if (condition != nullptr) {
>         
>         builder->AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration ^ task = builder->Register();
>
>     return task;
> }
> ```

## 确保你的应用能够流畅地处理后台任务注册失败的情况，否则，如果你的应用依赖于在尝试注册任务后具备有效注册对象，则它可能会崩溃。


以下示例可能返回现有任务，也可能添加注册后台任务的代码（如果有，包含可选系统条件）： [!div class="tabbedCodeSnippets"]

> [!div class="tabbedCodeSnippets"]
> ```cs
> //
> // Register a background task with the specified taskEntryPoint, name, trigger,
> // and condition (optional).
> //
> // taskEntryPoint: Task entry point for the background task.
> // taskName: A name for the background task.
> // trigger: The trigger for the background task.
> // condition: Optional parameter. A conditional event that must be true for the task to fire.
> //
> public static BackgroundTaskRegistration RegisterBackgroundTask(string taskEntryPoint,
>                                                                 string taskName,
>                                                                 IBackgroundTrigger trigger,
>                                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
>
>         if (cur.Value.Name == taskName)
>         {
>             //
>             // The task is already registered.
>             //
>
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>     
>     //
>     // Register the background task.
>     //
>
>     var builder = new BackgroundTaskBuilder();
>
>     builder.Name = taskName;
>     builder.TaskEntryPoint = taskEntryPoint;
>     builder.SetTrigger(trigger);
>
>     if (condition != null)
>     {
>
>         builder.AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration task = builder.Register();
>
>     return task;
> }
> ```
> ```cpp
> //
> // Register a background task with the specified taskEntryPoint, name, trigger,
> // and condition (optional).
> //
> // taskEntryPoint: Task entry point for the background task.
> // taskName: A name for the background task.
> // trigger: The trigger for the background task.
> // condition: Optional parameter. A conditional event that must be true for the task to fire.
> //
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(Platform::String ^ taskEntryPoint,
>                                                              Platform::String ^ taskName,
>                                                              IBackgroundTrigger ^ trigger,
>                                                              IBackgroundCondition ^ condition)
> {
>
>     //
>     // Check for existing registrations of this background task.
>     //
>     
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>     
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>         
>         if(cur->Name == name)
>         {
>             //
>             // The task is registered.
>             //
>             
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>         
>         hascur = iter->MoveNext();
>     }
>
>
>     //
>     // Register the background task.
>     //
>
>     auto builder = ref new BackgroundTaskBuilder();
>
>     builder->Name = name;
>     builder->TaskEntryPoint = taskEntryPoint;
>     builder->SetTrigger(trigger);
>
>     if (condition != nullptr) {
>         
>         builder->AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration ^ task = builder->Register();
>
>     return task;
> }
> ```

> 完整的后台任务注册实用工具函数 此示例展示完整的后台任务注册函数。

 
## 此函数可用于注册大部分后台任务，但不能注册网络后台任务。


****

* [[!div class="tabbedCodeSnippets"]](create-and-register-a-background-task.md)
* [**注意** 本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。](declare-background-tasks-in-the-application-manifest.md)
* [如果你面向 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。](handle-a-cancelled-background-task.md)
* [相关主题](monitor-background-task-progress-and-completion.md)
* [创建和注册后台任务](respond-to-system-events-with-background-tasks.md)
* [在应用程序清单中声明后台任务](set-conditions-for-running-a-background-task.md)
* [处理取消的后台任务](update-a-live-tile-from-a-background-task.md)
* [监视后台任务进度和完成](use-a-maintenance-trigger.md)
* [使用后台任务响应系统事件](run-a-background-task-on-a-timer-.md)
* [设置后台任务的运行条件](guidelines-for-background-tasks.md)

****

* [使用后台任务更新动态磁贴](debug-a-background-task.md)
* [使用维护触发器](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO5-->



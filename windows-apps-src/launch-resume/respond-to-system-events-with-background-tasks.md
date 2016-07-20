---
author: TylerMSFT
title: "使用后台任务响应系统事件"
description: "了解如何创建响应 SystemTrigger 事件的后台任务。"
ms.assetid: 43C21FEA-28B9-401D-80BE-A61B71F01A89
translationtype: Human Translation
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: f6845dce428f5e22ec68744293b1668da52002bf

---

# 使用后台任务响应系统事件


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838)

了解如何创建响应 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) 事件的后台任务。

本主题假定你已经为你的应用编写了一个后台任务类，并且该任务需要运行以响应系统触发的某个事件（如 Internet 变为可用或用户登录）。 此主题重点介绍 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) 类。 有关编写后台任务类的详细信息可以在[创建和注册后台任务](create-and-register-a-background-task.md)中找到。

## 创建 SystemTrigger 对象


-   在应用代码中，创建一个新的 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) 对象。 第一个参数 *triggerType* 指定了将激活此后台任务的系统事件触发器的类型。 有关事件类型的列表，请参阅 [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839)。

    第二个参数 *OneShot* 指定后台任务是否将在下次发生系统事件并触发后台任务时，或在每次系统事件发生时运行一次，直至任务注销为止。

    以下代码指定当 Internet 变为可用时运行后台任务：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemTrigger internetTrigger = new SystemTrigger(SystemTriggerType.InternetAvailable, false);
    > ```
    > ```cpp
    > SystemTrigger ^ internetTrigger = ref new SystemTrigger(SystemTriggerType::InternetAvailable, false);
    > ```

## 注册后台任务


-   通过调用后台任务注册函数注册后台任务。 有关注册后台任务的详细信息，请参阅[注册后台任务](register-a-background-task.md)。

    以下代码将注册后台任务：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > string taskName   = "Internet-based background task";
    >
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > String ^ taskName   = "Internet-based background task";
    >
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
    > ```

    > **注意** 通用 Windows 应用必须在注册任何后台触发器类型之前调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。

    若要确保通用 Windows 应用在你发布更新后继续正常运行，必须在启动已经过更新的应用时调用 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)，然后调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。 有关详细信息，请参阅[后台任务指南](guidelines-for-background-tasks.md)。

    > **注意** 后台任务注册参数在注册时进行验证。 如果有任何注册参数无效，则会返回一个错误。 确保你的应用能够流畅地处理后台任务注册失败的情况，否则，如果你的应用依赖于在尝试注册任务后具备有效注册对象，则它可能会崩溃。

     

## 备注


若要实际查看后台任务注册，请下载[后台任务示例](http://go.microsoft.com/fwlink/p/?LinkId=618666)。

可运行后台任务以响应 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) 和 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) 事件，但你仍需要 [在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md)。 还必须在注册任何后台任务之前调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。

应用可以注册响应 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)、[**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) 和 [**NetworkOperatorNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/br224831) 事件的后台任务，从而使这些事件可以提供与用户的实时通信，即使该应用不在前台也是如此。 有关详细信息，请参阅[使用后台任务支持应用](support-your-app-with-background-tasks.md)。

> **注意** 本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你面向 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 
## 相关主题


****

* [创建和注册后台任务](create-and-register-a-background-task.md)
* [在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md)
* [处理取消的后台任务](handle-a-cancelled-background-task.md)
* [监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)
* [注册后台任务](register-a-background-task.md)
* [设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)
* [使用后台任务更新动态磁贴](update-a-live-tile-from-a-background-task.md)
* [使用维护触发器](use-a-maintenance-trigger.md)
* [在计时器上运行后台任务](run-a-background-task-on-a-timer-.md)
* [后台任务指南](guidelines-for-background-tasks.md)

****

* [调试后台任务](debug-a-background-task.md)
* [如何在 Windows 应用商店应用中触发暂停、恢复和后台事件（在调试时）](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO5-->



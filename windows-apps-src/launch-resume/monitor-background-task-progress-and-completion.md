---
author: TylerMSFT
title: "监视后台任务进度和完成"
description: "了解应用如何识别后台任务报告的进度和完成情况。"
ms.assetid: 17544FD7-A336-4254-97DC-2BF8994FF9B2
translationtype: Human Translation
ms.sourcegitcommit: b877ec7a02082cbfeb7cdfd6c66490ec608d9a50
ms.openlocfilehash: 0488e47c35b2f7c8a8db2b2aca4527c4c3b67d28

---

# 监视后台任务进度和完成情况


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)
-   [**BackgroundTaskProgressEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224785)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

了解应用如何识别在单独进程中运行的后台任务报告的进度和完成情况。 （对于单一进程后台任务，可以设置共享变量来表示进度和完成情况。）

 后台任务从应用中分离，并且单独运行，但你可以通过应用代码监视后台任务进度和完成情况。 若要进行该操作，应用订阅已向系统注册的后台任务事件。

-   本主题假定你拥有一个注册后台任务的应用。 若要快速生成后台任务，请参阅[创建和注册后台任务](create-and-register-a-background-task.md)。 有关条件和触发器的详细信息，请参阅[使用后台任务支持应用](support-your-app-with-background-tasks.md)。

## 创建一个事件处理程序以处理完成的后台任务

1.  创建一个事件处理程序函数以处理完成的后台任务。 该代码需遵循特定的足迹，即获取 [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224803) 对象和 [**BackgroundTaskCompletedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224778) 对象。

    对 OnCompleted 后台任务事件处理程序方法使用以下足迹：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >  private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    >  {
    >      // TODO: Add code that deals with background task completion.
    >  }
    > ```
    > ```cpp
    >  auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >  {
    >      // TODO: Add code that deals with background task completion.
    >  };
    > ```

2.  向处理后台任务完成的事件处理程序中添加代码。

    例如，[后台任务示例](http://go.microsoft.com/fwlink/p/?LinkId=618666)更新 UI。

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    >     {
    >         UpdateUI();
    >     }
    > ```
    > ```cpp
    >     auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >     {    
    >         UpdateUI();
    >     };
    > ```

## 创建一个事件处理程序函数以处理后台任务进度。

1.  创建一个事件处理程序函数以处理完成的后台任务。 该代码需遵循特定的足迹，即获取 [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224803) 对象和 [**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782) 对象：

    对 OnProgress 后台任务事件处理程序方法使用以下足迹：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
    >     {
    >         // TODO: Add code that deals with background task progress.
    >     }
    > ```
    > ```cpp
    >     auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    >     {
    >         // TODO: Add code that deals with background task progress.
    >     };
    > ```

2.  向处理后台任务完成的事件处理程序中添加代码。

    例如，[后台任务示例](http://go.microsoft.com/fwlink/p/?LinkId=618666)使用通过 *args* 参数传递的进度状态更新 UI：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
    >     {
    >         var progress = "Progress: " + args.Progress + "%";
    >         BackgroundTaskSample.SampleBackgroundTaskProgress = progress;
    >
    >         UpdateUI();
    >     }
    > ```
    > ```cpp
    >     auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    >     {
    >         auto progress = "Progress: " + args->Progress + "%";
    >         BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    >
    >         UpdateUI();
    >     };
    > ```

## 使用新的和现有的后台任务注册事件处理程序函数


1.  当应用首次注册后台任务时，应用应该注册以在任务运行（同时应用仍然在前台处于活动状态）时接收它的进度和完成更新。

    例如，[后台任务示例](http://go.microsoft.com/fwlink/p/?LinkId=618666)在它触发的每个后台任务上调用以下函数：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     private void AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration task)
    >     {
    >         task.Progress += new BackgroundTaskProgressEventHandler(OnProgress);
    >         task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
    >     }
    > ```
    > ```cpp
    >     void SampleBackgroundTask::AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration^ task)
    >     {
    >         auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    >         {
    >             auto progress = "Progress: " + args->Progress + "%";
    >             BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    >             UpdateUI();
    >         };
    >
    >         task->Progress += ref new BackgroundTaskProgressEventHandler(progress);
    >         
    >
    >         auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >         {
    >             UpdateUI();
    >         };
    >
    >         task->Completed += ref new BackgroundTaskCompletedEventHandler(completed);
    >     }
    > ```

2.  当应用启动或导航到后台任务状态相关的新页面时，它应用获取档期已注册的后台任务列表并将它们与进度和完成事件处理程序函数关联。 应用程序当前已注册的后台任务列表位于 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786).[**AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787) 属性中。

    例如，[后台任务示例](http://go.microsoft.com/fwlink/p/?LinkId=618666)在导航到 SampleBackgroundTask 页面时使用以下代码附加事件处理程序：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     protected override void OnNavigatedTo(NavigationEventArgs e)
    >     {
    >         foreach (var task in BackgroundTaskRegistration.AllTasks)
    >         {
    >             if (task.Value.Name == BackgroundTaskSample.SampleBackgroundTaskName)
    >             {
    >                 AttachProgressAndCompletedHandlers(task.Value);
    >                 BackgroundTaskSample.UpdateBackgroundTaskStatus(BackgroundTaskSample.SampleBackgroundTaskName, true);
    >             }
    >         }
    >
    >         UpdateUI();
    >     }
    > ```
    > ```cpp
    >     void SampleBackgroundTask::OnNavigatedTo(NavigationEventArgs^ e)
    >     {
    >         // A pointer back to the main page.  This is needed if you want to call methods in MainPage such
    >         // as NotifyUser()
    >         rootPage = MainPage::Current;
    >
    >         //
    >         // Attach progress and completed handlers to any existing tasks.
    >         //
    >         auto iter = BackgroundTaskRegistration::AllTasks->First();
    >         auto hascur = iter->HasCurrent;
    >         while (hascur)
    >         {
    >             auto cur = iter->Current->Value;
    >
    >             if (cur->Name == SampleBackgroundTaskName)
    >             {
    >                 AttachProgressAndCompletedHandlers(cur);
    >                 break;
    >             }
    >
    >             hascur = iter->MoveNext();
    >         }
    >
    >         UpdateUI();
    >     }
    > ```

## 相关主题

* [创建和注册后台任务](create-and-register-a-background-task.md)
* [在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md)
* [处理取消的后台任务](handle-a-cancelled-background-task.md)
* [注册后台任务](register-a-background-task.md)
* [使用后台任务响应系统事件](respond-to-system-events-with-background-tasks.md)
* [设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)
* [使用后台任务更新动态磁贴](update-a-live-tile-from-a-background-task.md)
* [使用维护触发器](use-a-maintenance-trigger.md)
* [在计时器上运行后台任务](run-a-background-task-on-a-timer-.md)
* [后台任务指南](guidelines-for-background-tasks.md)
* [调试后台任务](debug-a-background-task.md)
* [如何在 Windows 应用商店应用中触发暂停、恢复和后台事件（在调试时）](http://go.microsoft.com/fwlink/p/?linkid=254345)



<!--HONumber=Aug16_HO3-->



---
title: 处理取消的后台任务
description: 了解如何创建一个后台任务，该任务识别取消请求并停止工作，向使用永久性存储的应用报告取消。
ms.assetid: B7E23072-F7B0-4567-985B-737DD2A8728E
---

# 处理取消的后台任务

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**重要的 API**

-   [**BackgroundTaskCanceledEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224775)
-   [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797)
-   [**ApplicationData.Current**](https://msdn.microsoft.com/library/windows/apps/br241619)

了解如何创建一个后台任务，该任务识别取消请求并停止工作，向使用永久性存储的应用报告取消。

> **注意** 对于除台式机以外的所有设备系列，如果设备内存不足，后台任务可能会终止。 如果没有呈现内存不足异常，或者应用没有处理该异常，则后台任务将在没有警告且不引发 OnCanceled 事件的情况下终止。 这有助于确保前台中应用的用户体验。 应该将后台任务设计为处理此方案。

本主题假定你已创建一个后台任务类，其中包含用作后台任务入口点的 Run 方法。 若要快速生成后台任务，请参阅[创建和注册后台任务](create-and-register-a-background-task.md)。 有关条件和触发器的详细信息，请参阅[使用后台任务支持应用](support-your-app-with-background-tasks.md)。

## 使用 OnCanceled 方法识别取消请求

编写一个用于处理取消事件的方法。

创建一个名为 OnCanceled 的方法，该方法具有以下足迹。 该方法是 Windows 运行时在针对后台任务进行取消请求时调用的入口点。

OnCanceled 方法需要具有以下占用：

> [!div class="tabbedCodeSnippets"]
> ```cs
>    private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
>    {
>        // TODO: Add code to notify the background task that it is cancelled.
>    }
> ```
> ```cpp
>    void ExampleBackgroundTask::OnCanceled(IBackgroundTaskInstance^ taskInstance, BackgroundTaskCancellationReason reason)
>    {
>        // TODO: Add code to notify the background task that it is cancelled.
>    }
> ```

将一个名为 **\_CancelRequested** 的标志变量添加到后台任务类。 此变量将用于指示何时发出取消请求。

> [!div class="tabbedCodeSnippets"]
> ```cs
>   volatile bool _CancelRequested = false;
> ```
> ```cpp
>   private:
>     volatile bool CancelRequested;
> ```

在步骤 1 中创建的 OnCanceled 方法中，将标志变量 **\_CancelRequested** 设置为 **true**。

完整[后台任务示例]( http://go.microsoft.com/fwlink/p/?linkid=227509) OnCanceled 方法将 **\_CancelRequested** 设置为 **true** 并编写可能有用的调试输出：

> [!div class="tabbedCodeSnippets"]
> ```cs
>     private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
>     {
>         //
>         // Indicate that the background task is canceled.
>         //
> 
>         _cancelRequested = true;
> 
>         Debug.WriteLine("Background " + sender.Task.Name + " Cancel Requested...");
>     }
> ```
> ```cpp
>     void SampleBackgroundTask::OnCanceled(IBackgroundTaskInstance^ taskInstance, BackgroundTaskCancellationReason reason)
>     {
>         //
>         // Indicate that the background task is canceled.
>         //
> 
>         CancelRequested = true;
>     }
> ```

在后台任务的 Run 方法中，在开始工作之前注册 OnCanceled 事件处理程序方法。 例如，使用以下代码行：

> [!div class="tabbedCodeSnippets"]
> ```cs
>     taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);
> ```
> ```cpp
>     taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &SampleBackgroundTask::OnCanceled);
> ```

## 通过退出 Run 方法来处理取消


当收到取消请求时，Run 方法需要通过识别 **\_cancelRequested** 何时设置为 **true** 来停止工作并退出。

修改后台任务的代码以在它工作时检查该标志变量。 如果 **\_cancelRequested** 设置为 true，则停止工作。

[后台任务示例](http://go.microsoft.com/fwlink/p/?LinkId=618666)包含一个检查，该检查在后台任务取消时停止定期计时器回调：

> [!div class="tabbedCodeSnippets"]
> ```cs
>     if ((_cancelRequested == false) &amp;&amp; (_progress &lt; 100))
>     {
>         _progress += 10;
>         _taskInstance.Progress = _progress;
>     }
>     else
>     {
>         _periodicTimer.Cancel();
> 
>         // TODO: Record whether the task completed or was cancelled.
>     }
> ```
> ```cpp
>     if ((CancelRequested == false) &amp;&amp; (Progress &lt; 100))
>     {
>         Progress += 10;
>         TaskInstance->Progress = Progress;
>     }
>     else
>     {
>         PeriodicTimer->Cancel();
> 
>         // TODO: Record whether the task completed or was cancelled.
>     }
> ```

> **注意** 上面所示的代码示例使用用于记录后台任务进度的 [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797).[**Progress**](https://msdn.microsoft.com/library/windows/apps/br224800) 属性。 使用 [**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782) 类将进度报告回应用。

修改 Run 方法以便停止工作后，它记录该任务是已完成还是已取消。

[后台任务示例](http://go.microsoft.com/fwlink/p/?LinkId=618666)将状态记录在 LocalSettings 中：

> [!div class="tabbedCodeSnippets"]
> ```cs
>     if ((_cancelRequested == false) &amp;&amp; (_progress &lt; 100))
>     {
>         _progress += 10;
>         _taskInstance.Progress = _progress;
>     }
>     else
>     {
>         _periodicTimer.Cancel();
> 
>         var settings = ApplicationData.Current.LocalSettings;
>         var key = _taskInstance.Task.TaskId.ToString();
> 
>         //
>         // Write to LocalSettings to indicate that this background task ran.
>         //
> 
>         if (_cancelRequested)
>         {
>             settings.Values[key] = "Canceled";
>         }
>         else
>         {
>             settings.Values[key] = "Completed";
>         }
>         
>         Debug.WriteLine("Background " + _taskInstance.Task.Name + (_cancelRequested ? " Canceled" : " Completed"));
>         
>         //
>         // Indicate that the background task has completed.
>         //
> 
>         _deferral.Complete();
>     }
> ```
> ```cpp
>     if ((CancelRequested == false) &amp;&amp; (Progress &lt; 100))
>     {
>         Progress += 10;
>         TaskInstance->Progress = Progress;
>     }
>     else
>     {
>         PeriodicTimer->Cancel();
>         
>         //
>         // Write to LocalSettings to indicate that this background task ran.
>         //
>         
>         auto settings = ApplicationData::Current->LocalSettings;
>         auto key = TaskInstance->Task->Name;
>         settings->Values->Insert(key, (Progress &lt; 100) ? "Canceled" : "Completed");
>         
>         //
>         // Indicate that the background task has completed.
>         //
>         
>         Deferral->Complete();
>     }
> ```

## 备注

你可以下载[后台任务示例](http://go.microsoft.com/fwlink/p/?LinkId=618666)以在方法上下文中查看这些代码示例。

为了便于说明，示例代码只显示[后台任务示例](http://go.microsoft.com/fwlink/p/?LinkId=618666)的部分 Run 方法（以及回调计时器）。

## Run 方法示例

下面显示了不同上下文的[后台任务示例](http://go.microsoft.com/fwlink/p/?LinkId=618666)中的完整 Run 方法和计时器回调代码：

> [!div class="tabbedCodeSnippets"]
> ```cs
> //
> // The Run method is the entry point of a background task.
> //
> public void Run(IBackgroundTaskInstance taskInstance)
> {
>     Debug.WriteLine("Background " + taskInstance.Task.Name + " Starting...");
> 
>     //
>     // Query BackgroundWorkCost
>     // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
>     // of work in the background task and return immediately.
>     //
>     var cost = BackgroundWorkCost.CurrentBackgroundWorkCost;
>     var settings = ApplicationData.Current.LocalSettings;
>     settings.Values["BackgroundWorkCost"] = cost.ToString();
> 
>     //
>     // Associate a cancellation handler with the background task.
>     //
>     taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);
> 
>     //
>     // Get the deferral object from the task instance, and take a reference to the taskInstance;
>     //
>     _deferral = taskInstance.GetDeferral();
>     _taskInstance = taskInstance;
> 
>     _periodicTimer = ThreadPoolTimer.CreatePeriodicTimer(new TimerElapsedHandler(PeriodicTimerCallback), TimeSpan.FromSeconds(1));
> }
> 
> //
> // Simulate the background task activity.
> //
> private void PeriodicTimerCallback(ThreadPoolTimer timer)
> {
>     if ((_cancelRequested == false) &amp;&amp; (_progress < 100))
>     {
>         _progress += 10;
>         _taskInstance.Progress = _progress;
>     }
>     else
>     {
>         _periodicTimer.Cancel();
> 
>         var settings = ApplicationData.Current.LocalSettings;
>         var key = _taskInstance.Task.Name;
> 
>         //
>         // Write to LocalSettings to indicate that this background task ran.
>         //
>         settings.Values[key] = (_progress < 100) ? "Canceled with reason: " + _cancelReason.ToString() : "Completed";
>         Debug.WriteLine("Background " + _taskInstance.Task.Name + settings.Values[key]);
> 
>         //
>         // Indicate that the background task has completed.
>         //
>         _deferral.Complete();
>     }
> }
> ```
> ```cpp
> void SampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
> {
>     //
>     // Query BackgroundWorkCost
>     // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
>     // of work in the background task and return immediately.
>     //
>     auto cost = BackgroundWorkCost::CurrentBackgroundWorkCost;
>     auto settings = ApplicationData::Current->LocalSettings;
>     settings->Values->Insert("BackgroundWorkCost", cost.ToString());
> 
>     //
>     // Associate a cancellation handler with the background task.
>     //
>     taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &amp;SampleBackgroundTask::OnCanceled);
> 
>     //
>     // Get the deferral object from the task instance, and take a reference to the taskInstance.
>     //
>     TaskDeferral = taskInstance->GetDeferral();
>     TaskInstance = taskInstance;
> 
>     auto timerDelegate = [this](ThreadPoolTimer^ timer)
>     {
>         if ((CancelRequested == false) &amp;&amp;
>             (Progress < 100))
>         {
>             Progress += 10;
>             TaskInstance->Progress = Progress;
>         }
>         else
>         {
>             PeriodicTimer->Cancel();
> 
>             //
>             // Write to LocalSettings to indicate that this background task ran.
>             //
>             auto settings = ApplicationData::Current->LocalSettings;
>             auto key = TaskInstance->Task->Name;
>             settings->Values->Insert(key, (Progress < 100) ? "Canceled with reason: " + CancelReason.ToString() : "Completed");
> 
>             //
>             // Indicate that the background task has completed.
>             //
>             TaskDeferral->Complete();
>         }
>     };
> 
>     TimeSpan period;
>     period.Duration = 1000 * 10000; // 1 second
>     PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(ref new TimerElapsedHandler(timerDelegate), period);
> }
> ```

> **注意** 本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你要针对 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。

## 相关主题

* [创建和注册后台任务](create-and-register-a-background-task.md)
* [在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md)
* [后台任务指南](guidelines-for-background-tasks.md)
* [监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)
* [注册后台任务](register-a-background-task.md)
* [使用后台任务响应系统事件](respond-to-system-events-with-background-tasks.md)
* [在计时器上运行后台任务](run-a-background-task-on-a-timer-.md)
* [设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)
* [使用后台任务更新动态磁贴](update-a-live-tile-from-a-background-task.md)
* [使用维护触发器](use-a-maintenance-trigger.md)

* [调试后台任务](debug-a-background-task.md)
* [如何在 Windows 应用商店应用中触发暂停、恢复和后台事件（在调试时）](http://go.microsoft.com/fwlink/p/?linkid=254345)





<!--HONumber=Mar16_HO1-->



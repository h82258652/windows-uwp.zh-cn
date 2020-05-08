---
title: 处理取消的后台任务
description: 了解如何创建一个后台任务，该任务识别取消请求并停止工作，向使用永久性存储的应用报告取消。
ms.assetid: B7E23072-F7B0-4567-985B-737DD2A8728E
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10，uwp，后台任务
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: d2b6ba88587f4f536d4fe6fc2750a520166fde18
ms.sourcegitcommit: 2571af6bf781a464a4beb5f1aca84ae7c850f8f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82606346"
---
# <a name="handle-a-cancelled-background-task"></a>处理取消的后台任务

**重要的 API**

-   [**BackgroundTaskCanceledEventHandler**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskcanceledeventhandler)
-   [**IBackgroundTaskInstance**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance)
-   [**ApplicationData.Current**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current)

了解如何创建一个后台任务，该任务识别取消请求、停止工作，并向使用永久性存储的应用报告取消。

本主题假定您已创建一个后台任务类，其中包括用作后台任务入口点的**Run**方法。 若要快速生成后台任务，请参阅[创建和注册进程外后台任务](create-and-register-a-background-task.md)或[创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)。 有关条件和触发器的更多深入信息，请参阅[使用后台任务支持应用](support-your-app-with-background-tasks.md)。

本主题也适用于进程内后台任务。 而不是**运行**方法，而是替换**OnBackgroundActivated**。 进程内后台任务不需要你使用永久性存储发送取消信号，因为你可以使用应用状态传达取消（如果后台任务与前台应用在同一进程中运行）。

## <a name="use-the-oncanceled-method-to-recognize-cancellation-requests"></a>使用 OnCanceled 方法识别取消请求

编写一个用于处理取消事件的方法。

> [!NOTE]
> 对于除台式机以外的所有设备系列，如果设备内存不足，后台任务可能会终止。 如果未出现 "内存不足" 异常，或者应用程序未对其进行处理，则将终止后台任务而不发出警告，并且不会引发 OnCanceled 事件。 这有助于确保前台中应用的用户体验。 应该将后台任务设计为处理此情形。

创建一个名为 **OnCanceled** 的方法，如下所示。 该方法是 Windows 运行时在针对后台任务进行取消请求时调用的入口点。

```csharp
private void OnCanceled(
    IBackgroundTaskInstance sender,
    BackgroundTaskCancellationReason reason)
{
    // TODO: Add code to notify the background task that it is cancelled.
}
```

```cppwinrt
void ExampleBackgroundTask::OnCanceled(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance,
    Windows::ApplicationModel::Background::BackgroundTaskCancellationReason reason)
{
    // TODO: Add code to notify the background task that it is cancelled.
}
```

```cpp
void ExampleBackgroundTask::OnCanceled(
    IBackgroundTaskInstance^ taskInstance,
    BackgroundTaskCancellationReason reason)
{
    // TODO: Add code to notify the background task that it is cancelled.
}
```

将名** \_为 CancelRequested**的标志变量添加到后台任务类中。 此变量将用于指示何时发出取消请求。

```csharp
volatile bool _CancelRequested = false;
```

```cppwinrt
private:
    volatile bool m_cancelRequested;
```

```cpp
private:
    volatile bool CancelRequested;
```

在步骤1中创建的**OnCanceled**方法中，将 "标志变量** \_CancelRequested** " 设置为 " **true**"。

完整的[后台任务示例]( https://code.msdn.microsoft.com/windowsapps/Background-Task-Sample-9209ade9) **OnCanceled**方法将** \_CancelRequested**设置为**true**并写入可能有用的调试输出。

```csharp
private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
{
    // Indicate that the background task is canceled.
    _cancelRequested = true;

    Debug.WriteLine("Background " + sender.Task.Name + " Cancel Requested...");
}
```

```cppwinrt
void ExampleBackgroundTask::OnCanceled(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance,
    Windows::ApplicationModel::Background::BackgroundTaskCancellationReason reason)
{
    // Indicate that the background task is canceled.
    m_cancelRequested = true;
}
```

```cpp
void ExampleBackgroundTask::OnCanceled(IBackgroundTaskInstance^ taskInstance, BackgroundTaskCancellationReason reason)
{
    // Indicate that the background task is canceled.
    CancelRequested = true;
}
```

在后台任务的**Run**方法中，在开始工作前注册**OnCanceled**事件处理程序方法。 在进程内后台任务中，执行此注册操作可能是应用程序初始化的一部分。 例如，使用下面的代码行。

```csharp
taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);
```

```cppwinrt
taskInstance.Canceled({ this, &ExampleBackgroundTask::OnCanceled });
```

```cpp
taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &ExampleBackgroundTask::OnCanceled);
```

## <a name="handle-cancellation-by-exiting-your-background-task"></a>通过退出后台任务处理取消

收到取消请求后，执行后台工作的方法需要通过识别** \_cancelRequested**设置为**true**来停止工作和退出。 对于进程内后台任务，这意味着从**OnBackgroundActivated**方法返回。 对于进程外后台任务，这意味着从**Run**方法返回。

修改后台任务类的代码以在它工作时检查该标志变量。 如果** \_cancelRequested**设置为 true，则停止工作继续。

[后台任务示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)包含一个检查，该检查在后台任务被取消时停止定期计时器回调。

```csharp
if ((_cancelRequested == false) && (_progress < 100))
{
    _progress += 10;
    _taskInstance.Progress = _progress;
}
else
{
    _periodicTimer.Cancel();
    // TODO: Record whether the task completed or was cancelled.
}
```

```cppwinrt
if (!m_cancelRequested && m_progress < 100)
{
    m_progress += 10;
    m_taskInstance.Progress(m_progress);
}
else
{
    m_periodicTimer.Cancel();
    // TODO: Record whether the task completed or was cancelled.
}
```

```cpp
if ((CancelRequested == false) && (Progress < 100))
{
    Progress += 10;
    TaskInstance->Progress = Progress;
}
else
{
    PeriodicTimer->Cancel();
    // TODO: Record whether the task completed or was cancelled.
}
```

> [!NOTE]
> 上面所示的代码示例使用[**IBackgroundTaskInstance**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance)。用于记录后台任务进度的[**进度**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.progress)属性。 使用 [**BackgroundTaskProgressEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskProgressEventArgs) 类将进度报告回应用。

修改**Run**方法，以便在工作停止后记录该任务是已完成还是已取消。 此步骤适用于进程外后台任务，因为你需要一种取消后台任务后在进程之间通信的方法。 对于进程内后台任务，可以仅与应用程序共享状态，以指示该任务已取消。

[后台任务示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)在 LocalSettings 中记录状态。

```csharp
if ((_cancelRequested == false) && (_progress < 100))
{
    _progress += 10;
    _taskInstance.Progress = _progress;
}
else
{
    _periodicTimer.Cancel();

    var settings = ApplicationData.Current.LocalSettings;
    var key = _taskInstance.Task.TaskId.ToString();

    // Write to LocalSettings to indicate that this background task ran.
    if (_cancelRequested)
    {
        settings.Values[key] = "Canceled";
    }
    else
    {
        settings.Values[key] = "Completed";
    }
        
    Debug.WriteLine("Background " + _taskInstance.Task.Name + (_cancelRequested ? " Canceled" : " Completed"));
        
    // Indicate that the background task has completed.
    _deferral.Complete();
}
```

```cppwinrt
if (!m_cancelRequested && m_progress < 100)
{
    m_progress += 10;
    m_taskInstance.Progress(m_progress);
}
else
{
    m_periodicTimer.Cancel();

    // Write to LocalSettings to indicate that this background task ran.
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings() };
    auto key{ m_taskInstance.Task().Name() };
    settings.Values().Insert(key, (m_progress < 100) ? winrt::box_value(L"Canceled") : winrt::box_value(L"Completed"));

    // Indicate that the background task has completed.
    m_deferral.Complete();
}
```

```cpp
if ((CancelRequested == false) && (Progress < 100))
{
    Progress += 10;
    TaskInstance->Progress = Progress;
}
else
{
    PeriodicTimer->Cancel();
        
    // Write to LocalSettings to indicate that this background task ran.
    auto settings = ApplicationData::Current->LocalSettings;
    auto key = TaskInstance->Task->Name;
    settings->Values->Insert(key, (Progress < 100) ? "Canceled" : "Completed");
        
    // Indicate that the background task has completed.
    Deferral->Complete();
}
```

## <a name="remarks"></a>备注

你可以下载[后台任务示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)以在方法上下文中查看这些代码示例。

出于说明目的，示例代码只显示了[后台任务示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)中的**Run**方法（和回调计时器）的一部分。

## <a name="run-method-example"></a>Run 方法示例

下面显示了[后台任务示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)中的完整**Run**方法和计时器回调代码，以获取上下文。

```csharp
// The Run method is the entry point of a background task.
public void Run(IBackgroundTaskInstance taskInstance)
{
    Debug.WriteLine("Background " + taskInstance.Task.Name + " Starting...");

    // Query BackgroundWorkCost
    // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
    // of work in the background task and return immediately.
    var cost = BackgroundWorkCost.CurrentBackgroundWorkCost;
    var settings = ApplicationData.Current.LocalSettings;
    settings.Values["BackgroundWorkCost"] = cost.ToString();

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

    // Get the deferral object from the task instance, and take a reference to the taskInstance;
    _deferral = taskInstance.GetDeferral();
    _taskInstance = taskInstance;

    _periodicTimer = ThreadPoolTimer.CreatePeriodicTimer(new TimerElapsedHandler(PeriodicTimerCallback), TimeSpan.FromSeconds(1));
}

// Simulate the background task activity.
private void PeriodicTimerCallback(ThreadPoolTimer timer)
{
    if ((_cancelRequested == false) && (_progress < 100))
    {
        _progress += 10;
        _taskInstance.Progress = _progress;
    }
    else
    {
        _periodicTimer.Cancel();

        var settings = ApplicationData.Current.LocalSettings;
        var key = _taskInstance.Task.Name;

        // Write to LocalSettings to indicate that this background task ran.
        settings.Values[key] = (_progress < 100) ? "Canceled with reason: " + _cancelReason.ToString() : "Completed";
        Debug.WriteLine("Background " + _taskInstance.Task.Name + settings.Values[key]);

        // Indicate that the background task has completed.
        _deferral.Complete();
    }
}
```

```cppwinrt
void ExampleBackgroundTask::Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
{
    // Query BackgroundWorkCost
    // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
    // of work in the background task and return immediately.
    auto cost{ Windows::ApplicationModel::Background::BackgroundWorkCost::CurrentBackgroundWorkCost() };
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings() };
    std::wstring costAsString{ L"Low" };
    if (cost == Windows::ApplicationModel::Background::BackgroundWorkCostValue::Medium) costAsString = L"Medium";
    else if (cost == Windows::ApplicationModel::Background::BackgroundWorkCostValue::High) costAsString = L"High";
    settings.Values().Insert(L"BackgroundWorkCost", winrt::box_value(costAsString));

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled({ this, &ExampleBackgroundTask::OnCanceled });

    // Get the deferral object from the task instance, and take a reference to the taskInstance.
    m_deferral = taskInstance.GetDeferral();
    m_taskInstance = taskInstance;

    Windows::Foundation::TimeSpan period{ std::chrono::seconds{1} };
    m_periodicTimer = Windows::System::Threading::ThreadPoolTimer::CreatePeriodicTimer([this](Windows::System::Threading::ThreadPoolTimer timer)
    {
        if (!m_cancelRequested && m_progress < 100)
        {
            m_progress += 10;
            m_taskInstance.Progress(m_progress);
        }
        else
        {
            m_periodicTimer.Cancel();

            // Write to LocalSettings to indicate that this background task ran.
            auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings() };
            auto key{ m_taskInstance.Task().Name() };
            settings.Values().Insert(key, (m_progress < 100) ? winrt::box_value(L"Canceled") : winrt::box_value(L"Completed"));

            // Indicate that the background task has completed.
            m_deferral.Complete();
        }
    }, period);
}
```

```cpp
void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
    // Query BackgroundWorkCost
    // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
    // of work in the background task and return immediately.
    auto cost = BackgroundWorkCost::CurrentBackgroundWorkCost;
    auto settings = ApplicationData::Current->LocalSettings;
    settings->Values->Insert("BackgroundWorkCost", cost.ToString());

    // Associate a cancellation handler with the background task.
    taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &ExampleBackgroundTask::OnCanceled);

    // Get the deferral object from the task instance, and take a reference to the taskInstance.
    TaskDeferral = taskInstance->GetDeferral();
    TaskInstance = taskInstance;

    auto timerDelegate = [this](ThreadPoolTimer^ timer)
    {
        if ((CancelRequested == false) &&
            (Progress < 100))
        {
            Progress += 10;
            TaskInstance->Progress = Progress;
        }
        else
        {
            PeriodicTimer->Cancel();

            // Write to LocalSettings to indicate that this background task ran.
            auto settings = ApplicationData::Current->LocalSettings;
            auto key = TaskInstance->Task->Name;
            settings->Values->Insert(key, (Progress < 100) ? "Canceled with reason: " + CancelReason.ToString() : "Completed");

            // Indicate that the background task has completed.
            TaskDeferral->Complete();
        }
    };

    TimeSpan period;
    period.Duration = 1000 * 10000; // 1 second
    PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(ref new TimerElapsedHandler(timerDelegate), period);
}
```

## <a name="related-topics"></a>相关主题

- [创建并注册进程内后台任务](create-and-register-an-inproc-background-task.md)。
- [创建和注册进程外后台任务](create-and-register-a-background-task.md)
- [在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md)
- [后台任务指南](guidelines-for-background-tasks.md)
- [监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)
- [注册后台任务](register-a-background-task.md)
- [使用后台任务响应系统事件](respond-to-system-events-with-background-tasks.md)
- [在计时器上运行后台任务](run-a-background-task-on-a-timer-.md)
- [设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)
- [使用后台任务更新动态磁贴](update-a-live-tile-from-a-background-task.md)
- [使用维护触发器](use-a-maintenance-trigger.md)
- [调试后台任务](debug-a-background-task.md)
- [如何在 UWP 应用中触发暂停、恢复和后台事件（在调试时）](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)

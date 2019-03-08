---
title: 从应用中触发后台任务
description: 介绍了如何在应用程序中触发后台任务
ms.date: 07/06/2018
ms.topic: article
keywords: 后台任务触发器，后台任务
ms.localizationpriority: medium
ms.openlocfilehash: 02e4bf3d7977c9bdd675f264a37e608a5082ef4c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608092"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>从应用中触发后台任务

了解如何使用 [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) 从应用中激活后台任务。

有关如何创建应用程序的一个触发器的示例，请参阅此[示例](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs)。

本主题假定你有一个你想要从应用程序激活的后台任务。 如果你还没有后台任务，[BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs) 中有一个示例后台任务。 或者，请按照[创建和注册进程外后台任务](create-and-register-a-background-task.md)中的步骤创建一个任务。

## <a name="why-use-an-application-trigger"></a>为什么使用应用程序触发器

使用 **ApplicationTrigger** 在与前台应用不同的进程中运行代码。 如果你的应用有工作要在后台完成，则 **ApplicationTrigger** 是适合的，即使用户关闭前台应用也是如此。 如果应用关闭时应暂停后台工作，或应该将后台工作与前台进程状态绑定，则应使用[扩展执行](run-minimized-with-extended-execution.md)。

## <a name="create-an-application-trigger"></a>创建应用程序触发器

创建新的 [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)。 你可以像下面的代码段一样，将它存储在一个字段中。 这是为方便起见，这样我们稍后在想要对触发器发送信号时就不必创建新实例。 但你可以使用任何 **ApplicationTrigger** 实例来向触发器发送信号。

```csharp
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
_AppTrigger = new ApplicationTrigger();
```

```cppwinrt
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
Windows::ApplicationModel::Background::ApplicationTrigger _AppTrigger;
```

```cpp
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
ApplicationTrigger ^ _AppTrigger = ref new ApplicationTrigger();
```

## <a name="optional-add-a-condition"></a>（可选）添加条件

你可以创建一个后台任务条件以控制任务何时运行。 条件会阻止后台任务运行，直到条件满足为止。 有关详细信息，请参阅[设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)。

在此示例中的条件设置为**InternetAvailable**以便一次触发，该任务仅运行一次都可以访问 internet。 有关可能条件的列表，请参阅 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)。

```csharp
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable)
```

有关条件和后台触发器类型的更多深入信息，请参阅[使用后台任务支持应用](support-your-app-with-background-tasks.md)。

##  <a name="call-requestaccessasync"></a>调用 RequestAccessAsync()

注册 **ApplicationTrigger** 后台任务前，应调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494) 以确定用户允许的后台活动级别，因为用户可能为你的应用禁用了后台活动。 有关用户可用来控制后台活动设置的方法的详细信息，请参阅[优化后台活动](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)。

```csharp
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
   // Depending on the value of requestStatus, provide an appropriate response
   // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>注册后台任务

通过调用后台任务注册函数注册后台任务。 有关注册后台任务的详细信息以及下面示例代码中的 **RegisterBackgroundTask()** 方法定义，请参阅[注册后台任务](register-a-background-task.md)。

如果你正在考虑使用应用程序触发器延长前台进程的生命周期，请考虑使用[扩展执行](run-minimized-with-extended-execution.md)。 应用程序触发器用于创建单独托管的进程以便执行工作。 下面代码片段注册进程外后台触发器。

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Example application trigger";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Example application trigger" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Example application trigger";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition);
```

后台任务注册参数在注册时验证。 如果有任何注册参数无效，则会返回一个错误。 确保你的应用能够流畅地处理后台任务注册失败的情况，否则，如果你的应用依赖于在尝试注册任务后具备有效注册对象，则它可能会崩溃。

## <a name="trigger-the-background-task"></a>触发后台任务

触发后台任务之前，请使用 [BackgroundTaskRegistration](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) 来验证注册了后台任务。 在应用启动时就是验证所有后台任务都已注册的好时机。

通过调用 [ApplicationTrigger.RequestAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtrigger) 触发后台任务。 任何 **ApplicationTrigger** 实例都可以。

请注意，**ApplicationTrigger.RequestAsync** 不能从后台任务本身调用，或应用处于后台运行状态时也不能调用（请参阅 [应用生命周期](app-lifecycle.md) 了解有关应用程序状态的详细信息)。
如果用户已设置了阻止应用执行后台活动的能量或隐私策略，它可能会返回 [DisabledByPolicy](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult)。
同样，只有一个 AppTrigger 可以运行一次。 如果你尝试运行 AppTrigger 时已有另一个触发器已经在运行，则将返回函数 [CurrentlyRunning](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult)。

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>管理后台任务的资源

使用 [BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) 确定用户是否已决定应限制你应用的后台活动。 注意电池使用情况，并且仅当有必要完成用户想要执行的操作时再在后台运行应用。 有关用户可用来控制后台活动设置的方法的详细信息，请参阅[优化后台活动](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)。  

- 内存：优化应用的内存和能源使用是确保操作系统将允许你运行的后台任务的关键。 使用[内存管理 API](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) 查看你的后台任务所使用的内存大小。 当有其他应用在前台运行时，你的后台任务所使用的内存越多，操作系统就越难保持其运行。 用户最终控制你的应用可以执行的所有后台活动，并且可以看到你的应用对电池使用情况的影响。  
- CPU 时间：后台任务都受其获取基于触发器类型的时钟使用时间量。 应用程序触发器触发的后台任务限制为只能运行约 10 分钟。

有关适用于后台任务的资源限制，请参阅[使用后台任务支持应用](support-your-app-with-background-tasks.md)。

## <a name="remarks"></a>备注

从 Windows 10 开始，它不再需要将您的应用程序添加到锁定屏幕，以利用后台任务的用户。

如果你已先调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)，后台任务将仅使用 **ApplicationTrigger** 运行。

## <a name="related-topics"></a>相关主题

* [后台任务的指导原则](guidelines-for-background-tasks.md)
* [后台任务的代码示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)。
* [创建并注册进程外后台任务](create-and-register-a-background-task.md)
* [调试后台任务](debug-a-background-task.md)
* [声明应用程序清单中的后台任务](declare-background-tasks-in-the-application-manifest.md)
* [如果您的应用程序移到背景的可用内存](reduce-memory-usage.md)
* [处理已取消的后台任务](handle-a-cancelled-background-task.md)
* [如何在触发挂起、 继续和后台 UWP 应用中的事件 （在调试）](https://go.microsoft.com/fwlink/p/?linkid=254345)
* [监视器后台任务进度和完成](monitor-background-task-progress-and-completion.md)
* [推迟使用扩展执行的应用程序暂挂](run-minimized-with-extended-execution.md)
* [注册后台任务](register-a-background-task.md)
* [响应通过后台任务的系统事件](respond-to-system-events-with-background-tasks.md)
* [设置运行后台任务的条件](set-conditions-for-running-a-background-task.md)
* [更新动态磁贴通过后台任务](update-a-live-tile-from-a-background-task.md)
* [使用维护触发器](use-a-maintenance-trigger.md)

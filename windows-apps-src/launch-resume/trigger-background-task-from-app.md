---
author: TylerMSFT
title: 从应用中触发后台任务
description: 介绍了如何触发后台任务从应用程序中
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: 后台任务触发器，后台任务
ms.localizationpriority: medium
ms.openlocfilehash: 5ccd171f53795ef71830ffb022d0468facb3ac4f
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2018
ms.locfileid: "4533164"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>从应用中触发后台任务

了解如何使用 [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) 从应用中激活后台任务。

有关如何创建应用程序触发器的示例，请参阅此[示例](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs)。

本主题假定你已在你想要激活从你的应用程序的后台任务。 如果你尚未获得后台任务，则示例后台任务在[BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs)。 或者，按照中[创建和注册进程外后台任务](create-and-register-a-background-task.md)来创建一个步骤。

## <a name="why-use-an-application-trigger"></a>为什么要使用的应用程序触发器

使用**ApplicationTrigger**从前台应用在单独进程中运行代码。 如果你的应用具有必须要完成后台-即使用户关闭前台应用的工作**ApplicationTrigger**适合使用。 如果应终止后台工作，当应用关闭时，或[扩展执行](run-minimized-with-extended-execution.md)应改为使用，则应绑定到的前台进程，状态。

## <a name="create-an-application-trigger"></a>创建应用程序触发器

创建新[ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)。 你可以将其存储在字段如下面的代码片段中完成。 这是为了方便起见，以便我们无需更高版本创建一个新实例，当我们想要发送信号触发器。 但你可以使用任何**ApplicationTrigger**实例发送信号触发器。

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

你可以创建一个后台任务条件以控制任务何时运行。 条件阻止后台任务在满足条件之前运行。 有关详细信息，请参阅[设置运行后台任务的条件](set-conditions-for-running-a-background-task.md)。

在此示例中，以便在触发，条件设置为**InternetAvailable** ，仅运行一次任务都可以访问 internet。 有关可能条件的列表，请参阅 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)。

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

有关条件和后台触发器类型的更多深入信息，请参阅[支持你的应用使用后台任务](support-your-app-with-background-tasks.md)。

##  <a name="call-requestaccessasync"></a>调用 RequestAccessAsync()

注册**ApplicationTrigger**后台任务之前，调用[**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494)来确定用户允许，因为用户可能已禁用你的应用的后台活动的后台活动的级别。 请参阅有关方式用户详细信息的[优化后台活动](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)可以控制后台活动的设置。

```csharp
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
   // Depending on the value of requestStatus, provide an appropriate response
   // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>注册后台任务

通过调用后台任务注册函数注册后台任务。 注册后台任务，并查看下面的示例代码中的**RegisterBackgroundTask()** 方法定义的详细信息，请参阅[注册后台任务](register-a-background-task.md)。

如果你正在考虑使用应用程序触发器来扩展你的前台进程的生命周期，请考虑改为使用[扩展执行](run-minimized-with-extended-execution.md)。 应用程序触发器旨在用于创建单独宿主的进程中执行工作。 以下代码片段注册进程外后台触发器。

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

后台任务注册参数在注册时验证。 如果有任何注册参数无效，则会返回一个错误。 确保你的应用能够流畅地处理后台任务注册失败的情况，否则，如果你的应用依赖于在尝试注册任务后具备有效注册对象，它可能会崩溃。

## <a name="trigger-the-background-task"></a>触发后台任务

触发后台任务之前，请使用[BackgroundTaskRegistration](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration)验证已注册后台任务。 验证所有后台任务已注册的好时机是在应用启动期间。

通过调用[ApplicationTrigger.RequestAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtrigger)触发后台任务。 任何**ApplicationTrigger**实例将执行操作。

请注意，不能从后台任务本身，或当应用在后台运行状态时调用**ApplicationTrigger.RequestAsync** （有关应用程序状态的详细信息，请参阅[应用生命周期](app-lifecycle.md)）。
如果用户已设置执行后台活动阻止应用的能耗或隐私策略，它可能返回[DisabledByPolicy](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult) 。
此外，只有一个 AppTrigger 可以运行一次。 如果你尝试运行 AppTrigger 另一台已运行时，该函数将返回[CurrentlyRunning](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult)。

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>管理你的后台任务的资源

使用 [BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) 确定用户是否已决定应限制你应用的后台活动。 注意电池使用情况，并且仅当有必要完成用户想要执行的操作时再在后台运行应用。 请参阅有关方式用户详细信息的[优化后台活动](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)可以控制后台活动的设置。  

- 内存： 调整你的应用的内存和能耗使用是确保操作系统将允许你的后台任务运行的关键。 使用的[内存管理 Api](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx)以查看你的后台任务正在使用的内存量。 更多内存你的后台任务使用，让它保持运行另一个应用位于前台时，操作系统越难。 用户最终控制你的应用可以执行的所有后台活动，并且可以看到你的应用对电池使用情况的影响。  
- CPU 时间： 后台任务受限制的基于触发器类型时，他们获取的时钟时间量。 通过应用程序触发器触发后台任务仅限于大约 10 分钟。

有关适用于后台任务的资源限制，请参阅[使用后台任务支持应用](support-your-app-with-background-tasks.md)。

## <a name="remarks"></a>备注

从 Windows 10 开始，它不再需要为用户将你的应用添加到锁屏界面上，即可利用后台任务。

如果你已先调用[**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)使用**ApplicationTrigger**仅运行后台任务。

## <a name="related-topics"></a>相关主题

* [后台任务指南](guidelines-for-background-tasks.md)
* [后台任务代码示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)。
* [创建和注册进程外后台任务](create-and-register-a-background-task.md)
* [调试后台任务](debug-a-background-task.md)
* [在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md)
* [在将应用移动到后台时释放内存](reduce-memory-usage.md)
* [处理取消的后台任务](handle-a-cancelled-background-task.md)
* [如何在 UWP 应用中触发暂停、恢复和后台事件（在调试时）](http://go.microsoft.com/fwlink/p/?linkid=254345)
* [监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)
* [使用扩展执行来推迟应用挂起](run-minimized-with-extended-execution.md)
* [注册后台任务](register-a-background-task.md)
* [使用后台任务响应系统事件](respond-to-system-events-with-background-tasks.md)
* [设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)
* [使用后台任务更新动态磁贴](update-a-live-tile-from-a-background-task.md)
* [使用维护触发器](use-a-maintenance-trigger.md)

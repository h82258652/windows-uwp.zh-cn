---
author: TylerMSFT
title: 从应用中触发后台任务
description: 介绍如何触发背景任务从应用程序中
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: 背景任务触发器，后台任务
ms.localizationpriority: medium
ms.openlocfilehash: 5ccd171f53795ef71830ffb022d0468facb3ac4f
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "2867026"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>从应用中触发后台任务

了解如何使用 [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) 从应用中激活后台任务。

有关如何创建应用程序触发器的示例，请参阅本[示例](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs)。

本主题假定您具有您要从您的应用程序激活后台任务。 如果您没有背景任务，但没有示例背景任务存储在[BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs)上。 或者，按照[创建和注册的进程外背景任务](create-and-register-a-background-task.md)创建一个权限级别中的步骤。

## <a name="why-use-an-application-trigger"></a>为什么使用应用程序触发器

使用**ApplicationTrigger**从前景应用程序在一个单独的进程中运行代码。 **ApplicationTrigger**适合您的应用程序是否需要在后台-完成即使用户关闭前景色应用程序的工作。 如果应中止后台工作时应用程序已关闭，或然后[扩展执行](run-minimized-with-extended-execution.md)应改为使用，应限制到的前景色进程的状态。

## <a name="create-an-application-trigger"></a>创建应用程序触发器

创建新[ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)。 您无法将其存储在字段中，完成以下片段中。 这是为方便起见，以便我们不需要时我们希望信号触发器更高版本创建的新实例。 但您可以使用任何**ApplicationTrigger**实例信号触发器。

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

任务运行时，您可以创建控件的背景任务条件。 条件防止后台任务运行，直到满足的条件。 有关详细信息，请参阅[设置运行背景任务的条件](set-conditions-for-running-a-background-task.md)。

条件设置为**InternetAvailable**以便一次触发本示例中，在该任务只运行后都可以访问 internet。 有关可能条件的列表，请参阅 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)。

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

在条件和类型的背景触发器的更多深入信息，请参阅[支持您的应用程序与背景任务](support-your-app-with-background-tasks.md)。

##  <a name="call-requestaccessasync"></a>调用 RequestAccessAsync()

在注册之前**ApplicationTrigger**后台任务，调用[**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494)来确定用户允许，因为用户可能已禁用您的应用程序的后台活动的后台活动级别。 请参阅[优化后台活动](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)的详细信息的方式用户可以控制后台活动的设置。

```csharp
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
   // Depending on the value of requestStatus, provide an appropriate response
   // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>注册后台任务

通过调用后台任务注册函数注册后台任务。 注册后台任务，并查看下面的代码示例中的**RegisterBackgroundTask()** 方法的定义的详细信息，请参阅[注册背景任务](register-a-background-task.md)。

如果您正在考虑使用应用程序触发器扩展前景进程的生存期，可以考虑使用[扩展执行](run-minimized-with-extended-execution.md)。 应用程序触发器创建单独寄宿的过程旨在开展工作。 下面的代码段注册进程外背景触发。

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

## <a name="trigger-the-background-task"></a>触发背景任务

触发背景任务之前，请使用[BackgroundTaskRegistration](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration)验证背景任务已注册。 若要验证已注册的所有背景任务的良机期间应用程序启动。

通过调用[ApplicationTrigger.RequestAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtrigger)触发后台任务。 任何**ApplicationTrigger**实例将执行操作。

请注意，不能从背景任务本身，或在后台运行状态应用程序时调用**ApplicationTrigger.RequestAsync** （有关应用程序状态的详细信息，请参阅[应用程序生命周期](app-lifecycle.md)）。
如果用户已将阻止执行后台活动应用程序的能源或隐私策略设置，它可能返回[DisabledByPolicy](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult) 。
此外，只有一个 AppTrigger 可以运行一次。 如果您尝试运行 AppTrigger，另一个已运行时，该函数将返回[CurrentlyRunning](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult)。

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>管理背景任务的资源

使用 [BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) 确定用户是否已决定应限制你应用的后台活动。 注意电池使用情况，并且仅当有必要完成用户想要执行的操作时再在后台运行应用。 请参阅[优化后台活动](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)的详细信息的方式用户可以控制后台活动的设置。  

- 内存： 调整您的应用程序的内存和能源使用是确保操作系统将允许您要运行的背景任务关键。 使用的[内存管理 Api](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx)以查看使用后台任务的内存量。 更多内存后台任务使用，以使其在前台另一个应用程序时运行的操作系统是更加复杂。 用户最终控制你的应用可以执行的所有后台活动，并且可以看到你的应用对电池使用情况的影响。  
- CPU 时间： 后台任务受到它们获取基于触发器类型时钟使用率时间量的限制。 由应用程序触发器触发的后台任务被限制为 10 分钟。

有关适用于后台任务的资源限制，请参阅[使用后台任务支持应用](support-your-app-with-background-tasks.md)。

## <a name="remarks"></a>备注

从 Windows 10 开始，它不再需要将您的应用程序添加到锁定屏幕上，以便利用后台任务的用户。

后台任务才会运行使用**ApplicationTrigger** ，如果您必须首先调用[**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) 。

## <a name="related-topics"></a>相关主题

* [后台任务指南](guidelines-for-background-tasks.md)
* [背景任务代码示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
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

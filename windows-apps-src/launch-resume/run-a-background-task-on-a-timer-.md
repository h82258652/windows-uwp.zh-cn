---
title: 在计时器上运行后台任务
description: 了解如何计划一次性后台任务，或运行周期性后台任务。
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10，uwp，后台任务
ms.localizationpriority: medium
ms.openlocfilehash: b0d3c9401ff71475e379b2959a1f0cdc03fe8d8b
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260437"
---
# <a name="run-a-background-task-on-a-timer"></a>在计时器上运行后台任务

了解如何使用 [**TimeTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.TimeTrigger) 计划一次性后台任务或运行定期后台任务。

请参阅**后台激活示例**中的[方案 4](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundActivation)，查看如何实现本主题中介绍的时间触发后台任务。

本主题假定你有需要定期运行或在某一特定时间运行的后台任务。 如果你还没有后台任务，[BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs) 中有一个示例后台任务。 或者也可以按照[创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)或[创建和注册进程外后台任务](create-and-register-a-background-task.md)中的步骤创建一个。

## <a name="create-a-time-trigger"></a>创建时间触发器

创建新的 [**TimeTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.TimeTrigger)。 第二个参数 *OneShot* 指定后台任务是仅运行一次还是保持周期性运行。 如果 *OneShot* 设置为 true，则第一个参数 (*FreshnessTime*) 会指定在计划后台任务之前需等待的分钟数。 如果 *OneShot* 设置为 false，*FreshnessTime* 会指定后台任务的运行频率。

面向桌面或移动设备系列的通用 Windows 平台 (UWP) 应用的内置计时器以 15 分钟的间隔运行后台任务。 （计时器以 15 分钟的间隔运行，因此系统只需每 15 分钟唤醒一次来唤醒具有已请求 TimerTriggers 的应用，这可以省电。）

- 如果 *FreshnessTime* 设置为 15 分钟并且 *OneShot* 为 true，则任务将计划从其注册之时起 15 至 30 分钟内运行一次该任务。 如果设置为 25 分钟并且 *OneShot* 为 true，则任务将计划从其注册之时起 25 至 40 分钟内运行一次该任务。

- 如果 *FreshnessTime* 设置为 15 分钟并且 *OneShot* 为 false，则任务将计划从其注册之时起 15 至 30 分钟内每隔 15 分钟运行一次该任务。 如果设置为 n 分钟并且 *OneShot* 为 false，则任务将计划从其注册之时起 n 至 n + 15 分钟内每隔 n 分钟运行一次该任务。

> [!NOTE]
> 如果*FreshnessTime*设置为不到15分钟，则在尝试注册后台任务时会引发异常。

例如，此触发器将导致后台任务每小时运行一次。

```cs
TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
```

```cppwinrt
Windows::ApplicationModel::Background::TimeTrigger hourlyTrigger{ 60, false };
```

```cpp
TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
```

## <a name="optional-add-a-condition"></a>（可选）添加条件

你可以创建一个后台任务条件以控制任务何时运行。 条件会阻止后台任务运行，直到条件满足为止。 有关详细信息，请参阅[设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)。

在此示例中，条件设置为 **UserPresent**，以便在触发之后，在用户处于活动状态时才运行一次该任务。 有关可能条件的列表，请参阅 [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)。

```cs
SystemCondition userCondition = new SystemCondition(SystemConditionType.UserPresent);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition userCondition{
    Windows::ApplicationModel::Background::SystemConditionType::UserPresent };
```

```cpp
SystemCondition ^ userCondition = ref new SystemCondition(SystemConditionType::UserPresent);
```

有关条件和后台触发器类型的更多深入信息，请参阅[使用后台任务支持应用](support-your-app-with-background-tasks.md)。

##  <a name="call-requestaccessasync"></a>调用 RequestAccessAsync()

注册 **ApplicationTrigger** 后台任务前，应调用 [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 以确定用户允许的后台活动级别，因为用户可能为你的应用禁用了后台活动。 有关用户可用来控制后台活动设置的方法的详细信息，请参阅[优化后台活动](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)。

```cs
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
    // Depending on the value of requestStatus, provide an appropriate response
    // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>注册后台任务

通过调用后台任务注册函数注册后台任务。 有关注册后台任务的详细信息以及下面示例代码中的 **RegisterBackgroundTask()** 方法定义，请参阅[注册后台任务](register-a-background-task.md)。

> [!IMPORTANT]
> 对于在应用程序相同的进程中运行的后台任务，请不要设置 `entryPoint`。 对于在应用程序的独立进程中运行的后台任务，将 `entryPoint` 设置为命名空间 "."，并将包含后台任务实现的类的名称设置为。

```cs
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Example hourly background task";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Example hourly background task" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Example hourly background task";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
```

后台任务注册参数在注册时验证。 如果有任何注册参数无效，则会返回一个错误。 确保你的应用能够流畅地处理后台任务注册失败的情况，否则，如果你的应用依赖于在尝试注册任务后具备有效注册对象，则它可能会崩溃。

## <a name="manage-resources-for-your-background-task"></a>管理后台任务的资源

使用 [BackgroundExecutionManager.RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager) 确定用户是否已决定应限制你应用的后台活动。 注意电池使用情况，并且仅当有必要完成用户想要执行的操作时再在后台运行应用。 有关用户可用来控制后台活动设置的方法的详细信息，请参阅[优化后台活动](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)。

- 内存：调整应用内存和能耗使用情况是确保操作系统允许后台任务运行的关键。 使用[内存管理 API](https://docs.microsoft.com/uwp/api/windows.system.memorymanager) 查看你的后台任务所使用的内存大小。 当有其他应用在前台运行时，你的后台任务所使用的内存越多，操作系统就越难保持其运行。 用户最终控制你的应用可以执行的所有后台活动，并且可以看到你的应用对电池使用情况的影响。  
- CPU 时间：后台任务受其基于触发器类型获取的时钟时间使用的限制。

有关适用于后台任务的资源限制，请参阅[使用后台任务支持应用](support-your-app-with-background-tasks.md)。

## <a name="remarks"></a>备注

从 Windows 10 开始，用户不再需要将你的应用程序添加到锁定屏幕，就可以使用后台任务。

如果你先调用RequestAccessAsync[ **，后台任务将仅使用** TimeTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 运行。

## <a name="related-topics"></a>相关主题

* [后台任务指南](guidelines-for-background-tasks.md)
* [后台任务代码示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)
* [创建和注册进程外后台任务](create-and-register-a-background-task.md)
* [调试后台任务](debug-a-background-task.md)
* [在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md)
* [在将应用移动到后台时释放内存](reduce-memory-usage.md)
* [处理取消的后台任务](handle-a-cancelled-background-task.md)
* [如何在 UWP 应用中触发挂起、继续和后台事件（调试时）](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)
* [监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)
* [使用扩展执行来推迟应用挂起](run-minimized-with-extended-execution.md)
* [注册后台任务](register-a-background-task.md)
* [使用后台任务响应系统事件](respond-to-system-events-with-background-tasks.md)
* [设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)
* [使用后台任务更新动态磁贴](update-a-live-tile-from-a-background-task.md)
* [使用维护触发器](use-a-maintenance-trigger.md)

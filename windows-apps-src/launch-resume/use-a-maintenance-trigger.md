---
author: TylerMSFT
title: 使用维护触发器
description: 了解如何在插入设备的情况下使用 MaintenanceTrigger 类在后台运行轻型代码。
ms.assetid: 727D9D84-6C1D-4DF3-B3B0-2204EA4D76DD
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 08bf867b6690a84f89b61cac9942b8ad6c27cd99
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2018
ms.locfileid: "7441429"
---
# <a name="use-a-maintenance-trigger"></a>使用维护触发器

**重要的 API**

- [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)
- [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
- [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)

了解如何在插入设备的情况下使用 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) 类在后台运行轻型代码。

## <a name="create-a-maintenance-trigger-object"></a>创建一个维护触发器对象

本示例假定在插入设备时你已具有可以在后台运行的轻型代码以增强你的应用。 此主题重点介绍 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)，它类似于 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839)。

有关编写后台任务类的详细信息可以在[创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)或[创建和注册进程外后台任务](create-and-register-a-background-task.md)中找到。

创建新的 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) 对象。 第二个参数 *OneShot* 指定维护任务是只运行一次还是继续定期运行。 如果 *OneShot* 设置为 true，则第一个参数 (*FreshnessTime*) 会指定在计划后台任务之前需等待的分钟数。 如果 *OneShot* 设置为 false，则 *FreshnessTime* 会指定后台任务运行的频率。

> [!NOTE]
> 如果*FreshnessTime*设置为少于 15 分钟，在尝试注册后台任务时，会引发异常。

此示例代码创建运行一小时一次的触发器。

```csharp
uint waitIntervalMinutes = 60;
MaintenanceTrigger taskTrigger = new MaintenanceTrigger(waitIntervalMinutes, false);
```

```cppwinrt
uint32_t waitIntervalMinutes{ 60 };
Windows::ApplicationModel::Background::MaintenanceTrigger taskTrigger{ waitIntervalMinutes, false };
```

```cpp
unsigned int waitIntervalMinutes = 60;
MaintenanceTrigger ^ taskTrigger = ref new MaintenanceTrigger(waitIntervalMinutes, false);
```

## <a name="optional-add-a-condition"></a>（可选）添加条件

- 如果需要，创建一个后台任务条件以控制任务何时运行。 防止后台任务在未满足条件之前运行的条件，有关详细信息，请参阅[设置运行后台任务的条件](set-conditions-for-running-a-background-task.md)

在该示例中，条件设置为 **InternetAvailable** 以便在 Internet 可用时（或者变为可用时）运行维护。 有关可能的后台任务条件列表，请参阅 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)。

以下代码向维护任务生成器中添加一个条件：

```csharp
SystemCondition exampleCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition exampleCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ exampleCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
```

## <a name="register-the-background-task"></a>注册后台任务

- 通过调用后台任务注册函数注册后台任务。 有关注册后台任务的详细信息，请参阅[注册后台任务](register-a-background-task.md)。

以下代码将注册维护任务。 请注意，假设后台任务将在独立于应用的进程中运行，因为它指定 `entryPoint`。 如果后台任务将在应用的同一个进程中运行，不要指定 `entryPoint`。

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Maintenance background task example";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Maintenance background task example" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Maintenance background task example";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
```

> [!NOTE]
> 对于除台式机以外的所有设备系列，如果设备内存不足，后台任务可能会终止。 如果没有呈现内存不足异常，或者应用没有处理该异常，则后台任务将在没有警告且不引发 OnCanceled 事件的情况下终止。 这有助于确保前台中应用的用户体验。 应该将后台任务设计为处理此情形。

> [!NOTE]
> 通用 Windows 平台应用必须在注册任何后台触发器类型之前调用[**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) 。

若要确保通用 Windows 应用在你发布应用的更新后继续正常运行，则必须在启动已经过更新的应用时调用 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)，然后调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。 有关详细信息，请参阅[后台任务指南](guidelines-for-background-tasks.md)。

> [!NOTE]
> 后台任务注册参数在注册时进行验证。 如果有任何注册参数无效，则会返回一个错误。 确保你的应用能够流畅地处理后台任务注册失败的情况，否则，如果你的应用依赖于在尝试注册任务后具备有效注册对象，它可能会崩溃。

## <a name="related-topics"></a>相关主题

* [创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)。
* [创建和注册进程外后台任务](create-and-register-a-background-task.md)
* [在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md)
* [处理取消的后台任务](handle-a-cancelled-background-task.md)
* [监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)
* [注册后台任务](register-a-background-task.md)
* [使用后台任务响应系统事件](respond-to-system-events-with-background-tasks.md)
* [设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)
* [使用后台任务更新动态磁贴](update-a-live-tile-from-a-background-task.md)
* [在计时器上运行后台任务](run-a-background-task-on-a-timer-.md)
* [后台任务指南](guidelines-for-background-tasks.md)
* [调试后台任务](debug-a-background-task.md)
* [如何在 UWP 应用中触发暂停、恢复和后台事件（在调试时）](http://go.microsoft.com/fwlink/p/?linkid=254345)

---
title: 设置后台任务的运行条件
description: 了解如何设置控制何时运行后台任务的条件。
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10，uwp，后台任务
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 618c8891551d851c27414968be76fb465eb89bf0
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260421"
---
# <a name="set-conditions-for-running-a-background-task"></a>设置后台任务的运行条件

**重要的 API**

- [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition)
- [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)
- [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)

了解如何设置控制何时运行后台任务的条件。

有时，后台任务还需要满足某些条件，才可以使后台任务继续进行。 你可以在注册后台任务时指定由 [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType) 指定的一个或多个条件。 引发触发器之后将检查条件。 后台任务将进入队列，但必须先满足所有所需条件才会运行。

对后台任务设置条件可阻止任务不必要地运行，从而节省电池电量和 CPU。 例如，如果你的后台任务在计时器上运行并要求 Internet 连接，请在注册该任务之前将 **InternetAvailable** 条件添加到 [**TaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)。 仅当计时器时间过去*以及* Internet 可用时运行后台任务，这有助于防止任务不必要地使用系统资源和电池使用时间。

还可以通过在同一TaskBuilder[**上多次调用**AddCondition](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 组合多个条件。 注意不要添加冲突条件，如 **UserPresent** 和 **UserNotPresent**。

## <a name="create-a-systemcondition-object"></a>创建 SystemCondition 对象

本主题假定你的后台任务已与你的应用关联，并且你的应用已包含用于创建名为 [taskBuilder**的**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)BackgroundTaskBuilder 对象的代码  如果需要先创建一个后台任务，请参阅[创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)或[创建和注册进程外后台任务](create-and-register-a-background-task.md)。

本主题适用于在进程外运行的后台任务以及在前台应用所在的同一进程中运行的那些后台任务。

添加该条件之前，创建一个代表该条件的 [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition) 对象，该对象必须实际用于运行后台任务。 在构造函数中，通过提供一个 [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType) 枚举值指定必须满足的条件。

以下代码创建一个 [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition) 对象，该对象将指定 **InternetAvailable** 条件：

```csharp
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
```

## <a name="add-the-systemcondition-object-to-your-background-task"></a>向你的后台任务中添加 SystemCondition 对象

若要添加条件，请在 [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.addcondition) 对象上调用 [**AddCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 方法，并向其传递 [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition) 对象。

以下代码使用 **taskBuilder** 添加 **InternetAvailable** 条件。

```csharp
taskBuilder.AddCondition(internetCondition);
```

```cppwinrt
taskBuilder.AddCondition(internetCondition);
```

```cpp
taskBuilder->AddCondition(internetCondition);
```

## <a name="register-your-background-task"></a>注册后台任务

现在即可使用 [**Register**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register) 方法注册后台任务，该后台任务必须先满足指定的条件才能启动。

以下代码注册该任务并存储所生成的 BackgroundTaskRegistration 对象：

```csharp
BackgroundTaskRegistration task = taskBuilder.Register();
```

```cppwinrt
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ recurringTaskBuilder.Register() };
```

```cpp
BackgroundTaskRegistration ^ task = taskBuilder->Register();
```

> [!NOTE]
> 通用 Windows 应用必须在注册任何后台触发器类型之前调用 [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)。

若要确保通用 Windows 应用在你发布更新后继续正常运行，必须在启动已经过更新的应用时调用 [**RemoveAccess**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess)，然后调用 [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)。 有关详细信息，请参阅[后台任务指南](guidelines-for-background-tasks.md)。

> [!NOTE]
> 后台任务注册参数在注册时验证。 如果有任何注册参数无效，则会返回一个错误。 确保你的应用能够流畅地处理后台任务注册失败的情况，否则，如果你的应用依赖于在尝试注册任务后具备有效注册对象，则它可能会崩溃。

## <a name="place-multiple-conditions-on-your-background-task"></a>在后台任务上放置多个条件

若要添加多个条件，你的应用需要多次调用 [**AddCondition**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.addcondition) 方法。 任务注册生效之前，必须进行这些调用。

> [!NOTE]
> 请注意不要将冲突的条件添加到后台任务。

以下代码片段显示了在创建和注册后台任务的上下文中出现的多个条件。

```csharp
// Set up the background task.
TimeTrigger hourlyTrigger = new TimeTrigger(60, false);

var recurringTaskBuilder = new BackgroundTaskBuilder();

recurringTaskBuilder.Name           = "Hourly background task";
recurringTaskBuilder.TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
recurringTaskBuilder.SetTrigger(hourlyTrigger);

// Begin adding conditions.
SystemCondition userCondition     = new SystemCondition(SystemConditionType.UserPresent);
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);

recurringTaskBuilder.AddCondition(userCondition);
recurringTaskBuilder.AddCondition(internetCondition);

// Done adding conditions, now register the background task.

BackgroundTaskRegistration task = recurringTaskBuilder.Register();
```

```cppwinrt
// Set up the background task.
Windows::ApplicationModel::Background::TimeTrigger hourlyTrigger{ 60, false };

Windows::ApplicationModel::Background::BackgroundTaskBuilder recurringTaskBuilder;

recurringTaskBuilder.Name(L"Hourly background task");
recurringTaskBuilder.TaskEntryPoint(L"Tasks.ExampleBackgroundTaskClass");
recurringTaskBuilder.SetTrigger(hourlyTrigger);

// Begin adding conditions.
Windows::ApplicationModel::Background::SystemCondition userCondition{
    Windows::ApplicationModel::Background::SystemConditionType::UserPresent };
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };

recurringTaskBuilder.AddCondition(userCondition);
recurringTaskBuilder.AddCondition(internetCondition);

// Done adding conditions, now register the background task.
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ recurringTaskBuilder.Register() };
```

```cpp
// Set up the background task.
TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);

auto recurringTaskBuilder = ref new BackgroundTaskBuilder();

recurringTaskBuilder->Name           = "Hourly background task";
recurringTaskBuilder->TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
recurringTaskBuilder->SetTrigger(hourlyTrigger);

// Begin adding conditions.
SystemCondition ^ userCondition     = ref new SystemCondition(SystemConditionType::UserPresent);
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);

recurringTaskBuilder->AddCondition(userCondition);
recurringTaskBuilder->AddCondition(internetCondition);

// Done adding conditions, now register the background task.
BackgroundTaskRegistration ^ task = recurringTaskBuilder->Register();
```

## <a name="remarks"></a>备注

> [!NOTE]
> 选择后台任务的条件，使其仅在需要时才运行，不应在不运行的情况下运行。 有关不同后台任务条件的描述，请参阅 [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)。

## <a name="related-topics"></a>相关主题

* [创建和注册进程外后台任务](create-and-register-a-background-task.md)
* [创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)
* [在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md)
* [处理取消的后台任务](handle-a-cancelled-background-task.md)
* [监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)
* [注册后台任务](register-a-background-task.md)
* [使用后台任务响应系统事件](respond-to-system-events-with-background-tasks.md)
* [使用后台任务更新动态磁贴](update-a-live-tile-from-a-background-task.md)
* [使用维护触发器](use-a-maintenance-trigger.md)
* [在计时器上运行后台任务](run-a-background-task-on-a-timer-.md)
* [后台任务指南](guidelines-for-background-tasks.md)
* [调试后台任务](debug-a-background-task.md)
* [如何在 UWP 应用中触发挂起、继续和后台事件（调试时）](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)

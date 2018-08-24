---
author: TylerMSFT
title: 设置后台任务的运行条件
description: 了解如何设置控制何时运行后台任务的条件。
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，背景任务
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 556a787eb1e92e4c8adb7457235afb45c02df2dc
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "2830446"
---
# <a name="set-conditions-for-running-a-background-task"></a>设置后台任务的运行条件

**重要的 API**

- [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)
- [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)
- [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)

了解如何设置控制何时运行后台任务的条件。

有时，后台任务需要某些条件满足背景任务成功。 你可以在注册后台任务时指定由 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) 指定的一个或多个条件。 在触发触发器后，将检查条件。 然后将排队后台任务，但它将无法运行直到满足所有必需的条件。

后台任务上放置条件将电池寿命和 CPU 保存通过防止任务不必要地运行。 例如，如果你的后台任务在计时器上运行并要求 Internet 连接，请在注册该任务之前将 **InternetAvailable** 条件添加到 [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)。 仅当计时器时间过去*以及* Internet 可用时运行后台任务，这有助于防止任务不必要地使用系统资源和电池使用时间。

还有可能要通过调用**AddCondition**多次相同[**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)组合多个条件。 注意不要添加冲突条件，如 **UserPresent** 和 **UserNotPresent**。

## <a name="create-a-systemcondition-object"></a>创建 SystemCondition 对象

本主题假定你的后台任务已与你的应用关联，并且你的应用已包含用于创建名为 **taskBuilder** 的 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 对象的代码  如果需要先创建一个后台任务，请参阅[创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)或[创建和注册进程外后台任务](create-and-register-a-background-task.md)。

本主题适用于在进程外运行的后台任务以及在前台应用所在的同一进程中运行的那些后台任务。

添加条件之前, 创建[**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)对象来表示必须有效背景任务运行的条件。 在构造函数中，指定[**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)枚举值，必须满足的条件。

下面的代码创建一个[**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)对象，指定**InternetAvailable**条件：

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

若要添加条件，请在 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224769) 对象上调用 [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224768) 方法，并向其传递 [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) 对象。

下面的代码使用**taskBuilder**可以添加**InternetAvailable**条件。

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

现在您可以采用[**注册**](https://msdn.microsoft.com/library/windows/apps/br224772)方法时，注册后台任务和直到满足指定的条件，将不会启动的后台任务。

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
> 通用 Windows 应用必须在注册任何后台触发器类型之前调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。

若要确保通用 Windows 应用在你发布更新后继续正常运行，必须在启动已经过更新的应用时调用 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)，然后调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。 有关详细信息，请参阅[后台任务指南](guidelines-for-background-tasks.md)。

> [!NOTE]
> 后台任务注册参数在注册时进行验证。 如果有任何注册参数无效，则会返回一个错误。 确保你的应用能够流畅地处理后台任务注册失败的情况，否则，如果你的应用依赖于在尝试注册任务后具备有效注册对象，则它可能会崩溃。

## <a name="place-multiple-conditions-on-your-background-task"></a>在后台任务上放置多个条件

若要添加多个条件，你的应用需要多次调用 [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) 方法。 任务注册生效之前，必须进行这些调用。

> [!NOTE]
> 小心不要向后台任务中添加冲突的条件。

下面的代码段创建并注册后台任务的上下文中显示多个条件。

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
> 以便仅运行它需要时，它不应时不运行时，请选择背景任务的条件。 有关不同后台任务条件的描述，请参阅 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)。

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
* [如何在 UWP 应用中触发暂停、恢复和后台事件（在调试时）](http://go.microsoft.com/fwlink/p/?linkid=254345)

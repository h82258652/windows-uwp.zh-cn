---
title: 分组后台任务注册
description: 作为分组的一部分注册/注销后台任务，以隔离这些注册。
ms.date: 04/05/2017
ms.topic: article
keywords: Windows 10, 后台任务
ms.openlocfilehash: a70c814e5e35359746076c5418d1f1d973e61773
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623852"
---
# <a name="group-background-task-registration"></a>分组后台任务注册

**重要的 Api**

[BackgroundTaskRegistrationGroup 类](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup)

现在后台任务可以成组注册，可将其视为逻辑上的命名空间。 这种隔离有助于确保应用或不同库的组件不会干扰彼此的后台任务注册。

当应用和它使用的框架（或库）使用相同名称来注册后台任务时，该应用可能无意中删除该框架的后台任务注册。 应用作者可能使用 [BackgroundTaskRegistration.AllTasks](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks) 注销所有已注册的后台任务，因此可能会意外删除框架和库后台任务注册。  通过分组，可以隔离后台任务注册，因此不会出现这种情况。

## <a name="features-of-groups"></a>组的功能

* GUID 可以唯一标识组。 它们还可以有一个关联的友好名称字符串，在调试时更易于读取。
* 多个后台任务可在一个组中注册。
* 在一个组中注册的后台任务不会出现在 [BackgroundTaskRegistration.AllTasks](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks) 中。 因此，当前使用 **BackgroundTaskRegistration.AllTasks** 注销任务的应用不会无意中注销在组中注册的后台任务。 请参阅[取消注册组中的后台任务](#unregister-background-tasks-in-a-group)下若要了解如何取消注册已注册为组的一部分的所有后台触发器。
* 每个后台任务注册都将具有 Group 属性，以确定与之关联的组。
* 使用一组注册过程中后台任务将导致激活可通过[BackgroundTaskRegistrationGroup.BackgroundActivated](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup.BackgroundActivated)而不是事件[Application.OnBackgroundActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onbackgroundactivated#Windows_UI_Xaml_Application_OnBackgroundActivated_Windows_ApplicationModel_Activation_BackgroundActivatedEventArgs_).

## <a name="register-a-background-task-in-a-group"></a>在一个组中注册后台任务

下面显示如何在组中注册后台任务（在此示例中，通过时区更改来触发）。

```csharp
private const string groupFriendlyName = "myGroup";
private const string groupId = "3F2504E0-4F89-41D3-9A0C-0305E82C3301";
private const string myTaskName = "My Background Trigger";

public static void RegisterBackgroundTaskInGroup()
{
   BackgroundTaskRegistrationGroup group = BackgroundTaskRegistration.GetTaskGroup(groupId);
   bool isTaskRegistered = false;

   // See if this task already belongs to a group
   if (group != null)
   {
       foreach (var taskKeyValue in group.AllTasks)
       {
           if (taskKeyValue.Value.Name == myTaskName)
           {
               isTaskRegistered = true;
               break;
           }
       }
   }

   // If the background task is not in a group, register it
   if (!isTaskRegistered)
   {
       if (group == null)
       {
           group = new BackgroundTaskRegistrationGroup(groupId, groupFriendlyName);
       }

       var builder = new BackgroundTaskBuilder();
       builder.Name = myTaskName;
       builder.TaskGroup = group; // we specify the group, here
       builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));

       // Because builder.TaskEntryPoint is not specified, OnBackgroundActivated() will be raised when the background task is triggered
       BackgroundTaskRegistration task = builder.Register();
   }
}
```

## <a name="unregister-background-tasks-in-a-group"></a>在组中注销后台任务

下面演示如何注销在组中注册的后台任务。
由于在某个组中注册的后台任务不会出现在 [BackgroundTaskRegistration.AllTasks](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks) 中，因此你必须循环访问组、查找向每个组注册的后台任务并将其注销。

```csharp
private static void UnRegisterAllTasks()
{
    // Unregister tasks that are part of a group
    foreach (var groupKeyValue in BackgroundTaskRegistration.AllTaskGroups)
    {
        foreach (var groupedTask in groupKeyValue.Value.AllTasks)
        {
            groupedTask.Value.Unregister(true); // passing true to cancel currently running instances of this background task
        }
    }

    // Unregister tasks that aren't part of a group
    foreach(var taskKeyValue in BackgroundTaskRegistration.AllTasks)
    {
        taskKeyValue.Value.Unregister(true); // passing true to cancel currently running instances of this background task
    }
}
```

## <a name="register-persistent-events"></a>注册永久性事件

在将后台任务注册组与进程内后台任务结合使用时，后台激活将指向组的事件，而不是 Application 或 CoreApplication 对象中的事件。 这样便可通过应用中的多个组件来处理激活，而不是将所有激活代码路径都放置在 Application 对象中。 下面演示如何注册组的后台激活事件。 首先检查 [BackgroundTaskRegistration.GetTaskGroup](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.gettaskgroup) 以确定该组是否已注册。 如果没有，则用你的 id 和友好名称创建新组。 然后向组中的 BackgroundActivated 事件注册事件处理程序。

```csharp
void RegisterPersistentEvent()
{
    var group = BackgroundTaskRegistration.GetTaskGroup(groupId);
    if (group == null)
    {
        group = new BackgroundTaskRegistrationGroup(groupId, groupFriendlyName);
    }

    group.BackgroundActivated += MyEventHandler;
}
```

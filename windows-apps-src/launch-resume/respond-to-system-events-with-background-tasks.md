---
title: 使用后台任务响应系统事件
description: 了解如何创建响应 SystemTrigger 事件的后台任务。
ms.assetid: 43C21FEA-28B9-401D-80BE-A61B71F01A89
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10，uwp，后台任务
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 4b704a83fbcf948f2c9377334831ca8948fc0e1a
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260448"
---
# <a name="respond-to-system-events-with-background-tasks"></a>使用后台任务响应系统事件

**重要的 API**

- [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
- [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
- [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTrigger)

了解如何创建响应 [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) 事件的后台任务。

本主题假定你已经为你的应用编写了一个后台任务类，并且需要运行该任务以响应系统触发的某个事件（如 Internet 可用性发生改变或用户登录时）。 此主题重点介绍 [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) 类。 有关编写后台任务类的详细信息可以在[创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)或[创建和注册进程外后台任务](create-and-register-a-background-task.md)中找到。

## <a name="create-a-systemtrigger-object"></a>创建 SystemTrigger 对象

在应用代码中，创建一个新的 [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTrigger) 对象。 第一个参数 *triggerType* 指定了将激活此后台任务的系统事件触发器的类型。 有关事件类型的列表，请参阅 [**SystemTriggerType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)。

第二个参数 *OneShot* 指定后台任务是否在下次发生系统事件时，或在每次发生系统事件时仅运行一次，直至任务注销为止。

以下代码指定当 Internet 变为可用时运行后台任务：

```csharp
SystemTrigger internetTrigger = new SystemTrigger(SystemTriggerType.InternetAvailable, false);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemTrigger internetTrigger{
    Windows::ApplicationModel::Background::SystemTriggerType::InternetAvailable, false};
```

```cpp
SystemTrigger ^ internetTrigger = ref new SystemTrigger(SystemTriggerType::InternetAvailable, false);
```

## <a name="register-the-background-task"></a>注册后台任务

通过调用后台任务注册函数注册后台任务。 有关注册后台任务的详细信息，请参阅[注册后台任务](register-a-background-task.md)。

以下代码将为在进程外运行的后台进程注册后台任务。 如果要调用在与主机应用相同的进程中运行的后台任务，请不要设置 `entrypoint`：

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass"; // Namespace name, '.', and the name of the class containing the background task
string taskName   = "Internet-based background task";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" }; // don't set for in-process background tasks.
std::wstring taskName{ L"Internet-based background task" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass"; // don't set for in-process background tasks
String ^ taskName   = "Internet-based background task";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
```

> [!NOTE]
> 通用 Windows 平台应用必须先调用[**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) ，然后才能注册任何后台触发器类型。

若要确保通用 Windows 应用在你发布更新后继续正常运行，必须在启动已经过更新的应用时调用 [**RemoveAccess**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess)，然后调用 [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)。 有关详细信息，请参阅[后台任务指南](guidelines-for-background-tasks.md)。

> [!NOTE]
> 后台任务注册参数在注册时验证。 如果有任何注册参数无效，则会返回一个错误。 确保你的应用能够流畅地处理后台任务注册失败的情况，否则，如果你的应用依赖于在尝试注册任务后具备有效注册对象，则它可能会崩溃。
 
## <a name="remarks"></a>备注

若要实际查看后台任务注册，请下载[后台任务示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)。

可运行后台任务以响应 [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTrigger) 和 [**MaintenanceTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) 事件，但你仍需要 [在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md)。 还必须在注册任何后台任务之前调用 [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)。

应用可以注册响应 [**TimeTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.TimeTrigger)、[**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger) 和 [**NetworkOperatorNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.NetworkOperatorNotificationTrigger) 事件的后台任务，从而使这些事件可以提供与用户的实时通信，即使该应用不在前台也是如此。 有关详细信息，请参阅[使用后台任务支持应用](support-your-app-with-background-tasks.md)。

## <a name="related-topics"></a>相关主题

* [创建和注册进程外后台任务](create-and-register-a-background-task.md)
* [创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)
* [在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md)
* [处理取消的后台任务](handle-a-cancelled-background-task.md)
* [监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)
* [注册后台任务](register-a-background-task.md)
* [设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)
* [使用后台任务更新动态磁贴](update-a-live-tile-from-a-background-task.md)
* [使用维护触发器](use-a-maintenance-trigger.md)
* [在计时器上运行后台任务](run-a-background-task-on-a-timer-.md)
* [后台任务指南](guidelines-for-background-tasks.md)
* [调试后台任务](debug-a-background-task.md)
* [如何在 UWP 应用中触发挂起、继续和后台事件（调试时）](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)

---
title: 创建和注册进程内后台任务
description: 创建和注册在前台应用所在的同一进程中运行的进程内任务。
ms.date: 11/03/2017
ms.topic: article
keywords: windows 10，uwp，后台任务
ms.assetid: d99de93b-e33b-45a9-b19f-31417f1e9354
ms.localizationpriority: medium
ms.openlocfilehash: 2a59fe6056661289726fdaa6c2dd26e90d5e3fad
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8742939"
---
# <a name="create-and-register-an-in-process-background-task"></a>创建和注册进程内后台任务

**重要的 API**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

本主题演示了如何创建和注册在你的应用所在的同一进程中运行的后台任务。

与进程外后台任务相比，进程内后台任务更易于实现。 但是，它们不够灵活。 如果在进程内后台任务中运行的代码崩溃，这将降低你的应用的性能。 另请注意，[DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx)、[DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) 和 **IoTStartupTask** 无法与进程内模型一起使用。 也无法在你的应用程序内激活 VoIP 后台任务。 这些触发器和任务仍支持使用进程外后台任务模型。

请注意，如果后台活动的运行时间超过执行时间限制，即使在应用的前台进程内运行，后台活动也会终止。 出于某些目的，复原将工作分离到可在单独进程中运行的后台任务仍然有用。 对于不需要与前台应用程序通信的工作，最好是使后台任务作为独立于前台应用程序的任务工作。

## <a name="fundamentals"></a>基础

进程内模型可通过应用在前台运行还是在后台运行的改进通知增强应用程序生命周期。 对于这些过渡，Application 对象提供了两个新事件：[**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 和 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground)。 基于你的应用程序的可见性状态，这些事件适合应用程序生命周期。在[应用生命周期](app-lifecycle.md)中阅读有关这些事件的详细信息，以及它们如何影响应用程序生命周期。

在较高级别上，处理 **EnteredBackground** 事件以运行将在应用在后台运行时执行的代码，处理 **LeavingBackground** 以了解应用何时移至前台。

## <a name="register-your-background-task-trigger"></a>注册你的后台任务触发器

进程内后台活动的注册方式与进程外后台活动大致相同。 所有后台触发器均从使用 [BackgroundTaskBuilder](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundtaskbuilder.aspx?f=255&MSPPError=-2147217396) 注册开始。 借助生成器，可轻松通过在一个位置设置所有所需值来注册后台任务。

> [!div class="tabbedCodeSnippets"]
> ```cs
> var builder = new BackgroundTaskBuilder();
> builder.Name = "My Background Trigger";
> builder.SetTrigger(new TimeTrigger(15, true));
> // Do not set builder.TaskEntryPoint for in-process background tasks
> // Here we register the task and work will start based on the time trigger.
> BackgroundTaskRegistration task = builder.Register();
> ```

> [!NOTE]
> 通用 Windows 应用必须在注册任何后台触发器类型之前调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。
> 若要确保通用 Windows 应用在你发布更新后继续正常运行，必须在启动已经过更新的应用时调用 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)，然后调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。 有关详细信息，请参阅[后台任务指南](guidelines-for-background-tasks.md)。

对于进程内后台活动，不要设置 `TaskEntryPoint.`。将其保留为空可启用默认入口点，这是 Application 对象上称为 [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) 的一种新的受保护方法。

注册触发器后，将基于在 [SetTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundtaskbuilder.settrigger.aspx) 方法中设置的触发器类型引发它。 在上面的示例中，使用 [TimeTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.timetrigger.aspx)，这将在注册之后的十五分钟引发。

## <a name="add-a-condition-to-control-when-your-task-will-run-optional"></a>添加条件控制任务何时运行（可选）

在触发器事件发生后，你可以添加条件控制任务何时运行。 例如，如果你不希望在用户存在前运行任务，请使用条件 **UserPresent**。 有关可能条件的列表，请参阅 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)。

以下示例代码指定需要用户存在的条件：

> [!div class="tabbedCodeSnippets"]
> ```cs
> builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
> ```

## <a name="place-your-background-activity-code-in-onbackgroundactivated"></a>将你的后台活动代码放置在 OnBackgroundActivated() 中

将你的后台活动代码放在[OnBackgroundActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx)它触发时响应后台触发器。 就像[IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396)可以视为**OnBackgroundActivated** 。 该方法具有[BackgroundActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.backgroundactivatedeventargs.aspx)参数，其中包含**Run**方法提供的所有内容。 例如，在 App.xaml.cs 中：

``` cs
using Windows.ApplicationModel.Background;

...

sealed partial class App : Application
{
  ...

  protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
  {
      base.OnBackgroundActivated(args);
      IBackgroundTaskInstance taskInstance = args.TaskInstance;
      DoYourBackgroundWork(taskInstance);  
  }
}
```

有关的更丰富的**OnBackgroundActivated**示例，请参阅[转换为与其主机应用相同的进程中运行的应用服务](convert-app-service-in-process.md)。

## <a name="handle-background-task-progress-and-completion"></a>处理后台任务进度和完成

可使用用于多进程后台任务的相同方式监视任务进度和完成（请参阅[监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)），但你可能会发现，通过使用变量跟踪你的应用中的进度或完成状态，可更轻松地跟踪它们。 这是在你的应用所在的同一进程中运行后台活动代码的优势之一。

## <a name="handle-background-task-cancellation"></a>处理后台任务取消

使用进程外后台任务所使用的相同方式取消进程内后台任务（请参阅[处理取消的后台任务](handle-a-cancelled-background-task.md)）。 请注意，必须在 **BackgroundActivated** 事件处理程序退出后再取消，否则整个进程都将终止。 如果你的前台应用在取消后台任务时意外关闭，请验证你的处理程序是否在取消之前就已退出。

## <a name="the-manifest"></a>清单

与进程外后台任务不同，不需要将后台任务信息添加到程序包清单，即可运行进程内后台任务。

## <a name="summary-and-next-steps"></a>摘要和后续步骤

你现在应该了解如何编写进程内后台任务的基础知识。

有关 API 引用、后台任务概念指南以及编写使用后台任务的应用的更多详细说明，请参阅以下相关主题。

## <a name="related-topics"></a>相关主题

**详细的后台任务说明主题**

* [将进程外后台任务转换为进程内后台任务](convert-out-of-process-background-task.md)
* [创建和注册进程外后台任务](create-and-register-a-background-task.md)
* [在后台播放媒体](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [使用后台任务响应系统事件](respond-to-system-events-with-background-tasks.md)
* [注册后台任务](register-a-background-task.md)
* [设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)
* [使用维护触发器](use-a-maintenance-trigger.md)
* [处理取消的后台任务](handle-a-cancelled-background-task.md)
* [监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)
* [在计时器上运行后台任务](run-a-background-task-on-a-timer-.md)

**后台任务指南**

* [后台任务指南](guidelines-for-background-tasks.md)
* [调试后台任务](debug-a-background-task.md)
* [如何在 UWP 应用中触发暂停、恢复和后台事件（在调试时）](http://go.microsoft.com/fwlink/p/?linkid=254345)

**后台任务 API 引用**

* [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

---
author: TylerMSFT
title: "后台任务指南"
description: "确保你的应用满足运行后台任务的要求。"
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
translationtype: Human Translation
ms.sourcegitcommit: 04cb13ce355b3983a55b7f7f253e6b22a7cebece
ms.openlocfilehash: 04e8776852a68d2e15c0a08732da48756f403d15

---

# 后台任务指南

\[ 已针对 Windows10 上的 UWP 应用更新。 有关 Windows8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

确保你的应用满足运行后台任务的要求。

## 后台任务指南

在开发后台任务时以及发布应用之前，考虑以下指南。

如果你使用后台任务在后台播放媒体，请参阅[在后台播放媒体](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/background-audio)，了解有关 Windows10 版本 1607 中使此操作更加简单的改进信息。


  **进程内与进程外后台任务：**Windows10 版本 1607 引入了[进程内后台任务](create-and-register-an-inproc-background-task.md)，使你能够在与前台应用相同的进程中运行后台代码。 当决定执行进程内后台任务还是执行进程外后台任务时，请考虑以下因素：

|注意事项 | 影响 |
|--------------|--------|
|恢复能力   | 如果你的后台进程在另一个进程中运行，后台进程崩溃时不会关闭你的前台应用程序。 此外，如果后台活动运行时间超过了执行时间限制，就可能会终止，即使在你的应用中运行也是如此。 当前台进程和后台进程不需要相互通信时，将后台工作分离成一个独立于前台应用的任务可能是更好的选择（因为进程内后台任务的主要优势之一就是它们不需要进程之间相互通信）。 |
|简易性    | 进程内后台任务不需要跨进程通信，并且编写起来没有那么复杂。  |
|可用的触发器 | 进程内后台任务不支持以下触发器：[DeviceUseTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396)、[DeviceServicingTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) 和 **IoTStartupTask**。 |
|VoIP | 进程内后台任务不支持在应用程序中激活 VoIP 后台任务。 |  

**CPU 配额：**后台任务受其基于触发器类型获取的时钟时间使用的限制。 大多数触发器限制为 30 秒的时钟时间使用，而另一些触发器在完成耗时任务时最多可以运行 10 分钟。 为了延长电池使用时间并为前台应用提供最佳用户体验，后台任务应该是轻量级任务。 有关适用于后台任务的资源限制，请参阅[使用后台任务支持应用](support-your-app-with-background-tasks.md)。

**管理后台任务：**你的应用应该获取已注册后台任务的列表，注册进度和完成处理程序以及相应地处理这些事件。 你的后台任务类应该报告进度、取消以及完成。 有关详细信息，请参阅[处理取消的后台任务](handle-a-cancelled-background-task.md)和[监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)。

**使用 [BackgroundTaskDeferral](https://msdn.microsoft.com/library/windows/apps/hh700499)：**如果后台任务类运行异步代码，则确保使用延迟。 否则，当使用 [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 方法（或针对进程内后台任务使用 [OnBackgroundActivated](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) 方法）时，你的后台任务可能会提前终止。 有关详细信息，请参阅[创建和注册进程外后台任务](create-and-register-an-outofproc-background-task.md)。

或者也可以请求一个延迟，并使用 **async/await** 来完成异步方法调用。 在 **await** 方法调用之后关闭延迟。

**更新应用清单：**对于在进程外运行的后台任务，请在应用程序清单中声明每个后台任务及其使用的触发器类型。 否则，你的应用将不能在运行时注册后台任务。

在与前台应用相同的进程中运行的后台任务不需要在应用程序清单中声明自己。 有关在清单中声明在进程外运行的后台任务的详细信息，请参阅[在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md)。

**准备应用更新：**如果你的应用将更新，请创建和注册一个 **ServicingComplete** 后台任务（请参阅 [SystemTriggerType](https://msdn.microsoft.com/library/windows/apps/br224839)），以便取消注册适用于前一版本应用的后台任务，并注册适用于新版本应用的后台任务。 此时也非常适合在前台运行的上下文之外执行所需的应用更新。

**请求执行后台任务：**

> 
  **重要提示** 从 Windows10 开始，应用不必满足置于锁屏界面上的先决条件也可以运行后台任务。

通用 Windows 平台 (UWP) 应用无需固定到锁屏界面，即可运行所有受支持的任务类型。 但是，应用必须在注册任何类型的后台任务之前调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。 如果用户在设备设置中显式拒绝了应用的后台任务权限，此方法将返回 [**BackgroundAccessStatus.Denied**](https://msdn.microsoft.com/library/windows/apps/hh700439)。
## 后台任务清单

*既适用于进程内后台任务，也适用于进程外后台任务*

-   将后台任务与正确的触发器关联。
-   添加条件以帮助确保你的后台任务成功运行。
-   处理后台任务进度、完成以及取消。
-   在应用启动期间重新注册后台任务。 这可以确保在应用首次启动时注册它们。 它还提供了一种检测用户是否已禁用应用的后台执行功能的方法（如果事件注册失败）。
-   检查是否存在后台任务注册错误。 如果适用，请尝试使用不同参数值来再次注册后台任务。
-   对于除台式机以外的所有设备系列，如果设备内存不足，后台任务可能会终止。 如果没有呈现内存不足异常，或者应用没有处理该异常，则后台任务将在没有警告且不引发 OnCanceled 事件的情况下终止。 这有助于确保前台中应用的用户体验。 应该将后台任务设计为处理此情形。

*仅适用于进程外后台任务*

-   在 Windows 运行时组件中创建后台任务。
-   不要通过后台任务显示 UI，但 Toast、磁贴以及锁屏提醒更新除外。
-   在 [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 方法中，为每个异步方法调用请求延迟并在完成该方法时关闭它们。 或者，通过 **async/await** 使用一个延迟。
-   使用永久性存储在后台任务与应用之间共享数据。
-   在应用程序清单中声明每个后台任务及其使用的触发器类型。 确保入口点和触发器类型正确。
-   不要在清单中指定 Executable 元素，除非使用的是应在与应用相同的上下文中运行的触发器（例如 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)）。

*仅适用于进程内后台任务*

- 当取消任务时，请确保 `BackgroundActivated` 事件处理程序退出后再取消，否则整个进程都将终止。
-   编写生存时间较短的后台任务。 后台任务限制为在 30 秒的时钟时间内使用。
-   不要依赖后台任务中的用户交互。

## Windows：支持锁屏界面的应用的后台任务清单

当开发可以使用锁屏的应用的后台任务时，请遵循此指南。 遵循[锁屏磁贴指南和清单](https://msdn.microsoft.com/library/windows/apps/hh465403)中的指南。

-   在将你的应用作为可锁屏应用进行开发之前，确保该应用位于锁屏上。 有关详细信息，请参阅[锁屏概述](https://msdn.microsoft.com/library/windows/apps/hh779720)。

-   确保你的应用在未置于锁屏界面上时也能工作。

-   包括使用 [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543)、[**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) 或 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) 注册并在应用清单中声明的后台任务。 确保入口点和触发器类型正确。 这是认证所需的内容，它使得用户能够将应用置于锁屏界面上。

**注意**  
本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows10 开发人员。 如果你要针对 Windows8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。

## 相关主题

* [创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)。
* [创建和注册进程外后台任务](create-and-register-an-outofproc-background-task.md)
* [在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md)
* [在后台播放媒体](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/background-audio)
* [处理取消的后台任务](handle-a-cancelled-background-task.md)
* [监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)
* [注册后台任务](register-a-background-task.md)
* [使用后台任务响应系统事件](respond-to-system-events-with-background-tasks.md)
* [设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)
* [使用后台任务更新动态磁贴](update-a-live-tile-from-a-background-task.md)
* [使用维护触发器](use-a-maintenance-trigger.md)
* [在计时器上运行后台任务](run-a-background-task-on-a-timer-.md)
* [调试后台任务](debug-a-background-task.md)
* [如何在 Windows 应用商店应用中触发暂停、恢复和后台事件（在调试时）](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Nov16_HO1-->



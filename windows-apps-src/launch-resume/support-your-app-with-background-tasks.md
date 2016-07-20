---
author: TylerMSFT
title: "使用后台任务支持应用"
description: "本部分中的主题展示如何通过响应具有后台任务的触发器在后台运行你自己的轻型代码。"
ms.assetid: EFF7CBFB-D309-4ACB-A2A5-28E19D447E32
translationtype: Human Translation
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 38942aa2a274828cc36677a93d0923beb03060dc

---

# 使用后台任务支持应用


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本部分中的主题展示如何通过响应具有后台任务的触发器在后台运行你自己的轻型代码。 后台任务是操作系统在后台运行的轻型类。 你可以使用后台任务在应用暂停或未运行时提供功能。 也可以将后台任务用于实时通信应用，例如 VOIP、邮件和 IM。

后台任务是实现 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 接口的单独的类。 通过使用 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 类注册后台任务。 注册后台任务时，类名称将用于指定入口点。

若要快速开始使用后台任务，请参阅[创建和注册后台任务](create-and-register-a-background-task.md)。

**提示** 从 Windows 10 开始，你不再需要将应用放置在锁屏界面上即可注册后台任务。

 

## 系统事件的后台任务


应用可以通过使用 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) 类注册后台任务来响应系统生成的事件。 应用可以使用以下任意系统事件触发器（在 [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839) 中定义）

| 触发器名称                     | 描述                                                                                                    |
|----------------------------------|----------------------------------------------------------------------------------------------------------------|
| **InternetAvailable**            | Internet 变为可用。                                                                                |
| **NetworkStateChange**           | 网络更改，如开销或连接性发生更改。                                              |
| **OnlineIdConnectedStateChange** | 与帐户更改关联的联机 ID。                                                                 |
| **SmsReceived**                  | 安装的移动宽带设备收到一条新短信。                                         |
| **TimeZoneChange**               | 设备上的时区发生更改（例如，当系统为夏令时调整时钟时）。 |

 

有关详细信息，请参阅[使用后台任务响应系统事件](respond-to-system-events-with-background-tasks.md)。

## 后台任务的条件


你可以控制后台任务何时运行，通过添加条件，甚至可以在任务触发后进行控制。 在触发后，后台任务将不再运行，直至所有条件均符合为止。 可以使用以下条件（由 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) 枚举表示）。

| 条件名称           | 描述                       |
|--------------------------|-----------------------------------|
| **InternetAvailable**    | Internet 必须可用。   |
| **InternetNotAvailable** | Internet 必须不可用。 |
| **SessionConnected**     | Internet 必须连接。    |
| **SessionDisconnected**  | 该会话必须断开连接。 |
| **UserNotPresent**       | 用户必须离开。            |
| **UserPresent**          | 用户必须存在。         |

 

有关详细信息，请参阅[设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)。

## 应用程序清单要求


在应用可以成功注册后台任务前，必须在应用程序清单中声明该任务。 有关详细信息，请参阅[在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md)。

## 后台任务


以下实时触发器可用于在后台运行轻型自定义代码：

**控制通道：**后台任务通过使用 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) 可以使连接保持活动状态，在控制通道上接收消息。 如果你的应用正在侦听套接字，你可以使用套接字代理而不是 **ControlChannelTrigger**。 有关使用套接字代理的详细信息，请参阅 [SocketActivityTrigger](https://msdn.microsoft.com/library/windows/apps/dn806009)。 **ControlChannelTrigger** 在 Windows Phone 上不受支持。

**计时器：**后台任务运行的频率可以为每 15 分钟一次，并且可以通过使用 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) 将它们设置在特定时间运行。 有关详细信息，请参阅[通过计时器运行后台任务](run-a-background-task-on-a-timer-.md)。

**推送通知：**后台任务响应 [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) 以接收原始推送通知。

**注意**  

通用 Windows 应用必须在注册任何后台触发器类型之前调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。

若要确保通用 Windows 应用在你发布更新后继续正常运行，必须在启动已经过更新的应用时调用 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)，然后调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。 有关详细信息，请参阅[后台任务指南](guidelines-for-background-tasks.md)。

## 系统事件触发器


> **注意** [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839) 枚举包括以下系统事件触发器。

| 触发器名称            | 描述                                                       |
|-------------------------|-------------------------------------------------------------------|
| **UserPresent**         | 后台任务在用户出现后触发。   |
| **UserAway**            | 后台任务在用户离开后触发。    |
| **ControlChannelReset** | 后台任务在控件通道初始化后触发。 |
| **SessionConnected**    | 后台任务在会话连接后触发。   |

 

以下系统事件触发器使识别用户何时将应用移到或移出锁屏界面成为可能。

| 触发器名称                     | 描述                                  |
|----------------------------------|----------------------------------------------|
| **LockScreenApplicationAdded**   | 向锁屏中添加应用磁贴。     |
| **LockScreenApplicationRemoved** | 从锁屏中删除应用磁贴。 |

 
## 后台任务资源限制


后台任务时轻型的。 使后台执行最少可确保前台应用的最佳用户体验以及最佳电池寿命。 通过对后台任务应用资源限制来实施该操作：

-   后台任务限制为在 30 秒的时钟时间内使用。

## 其他后台任务资源约束


### 内存约束

由于对于内存较低的设备的资源约束，后台任务可能具有内存限制，该限制决定了后台任务可以使用的内存上限。 如果后台任务尝试执行超过此限制的操作，则该操作将失败，并可能生成内存不足的异常情况，但该任务可以处理此情况。 如果该任务无法处理内存不足的异常情况，或者尝试的操作的性质导致无法生成内存不足的异常情况，任务将立即终止。 你可以使用 [**MemoryManager**](https://msdn.microsoft.com/library/windows/apps/dn633831) PAI 查询当前内存使用量和限制，以发现上限（若有），并监视正在进行的后台任务内存使用量。

### 内存较低的设备上带有后台任务的应用在每台设备上的限制

在内存受限的设备上，对于可以安装在一台设备上并在任何给定时间使用后台任务的应用数量有所限制。 如果超出该数量，将无法调用注册所有后台任务时所需的 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。

### 节电模式

除非你豁免你的应用，以便它可以在节电模式打开时仍可以运行后台任务和接收推送通知，否则当节电模式功能启用时，如果设备未连接到外部电源且电量低于指定剩余电量，它将阻止后台任务运行。 这不会阻止你注册后台任务。

## 后台任务资源保证实时通信


为了防止资源配额干涉实时通信功能，后台任务使用 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) 和 [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) 接收用于每次运行任务所保证的 CPU 资源配额。 资源配额如上所述，并为这些后台任务保持不变。

你的应用不必采用任何不同的方式即可获取 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) 和 [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) 后台任务的保证资源配额。 系统始终将这些后台任务视为关键后台任务。

## 维护触发


维护任务仅在设备插入到交流电源时才会运行。 有关详细信息，请参阅[使用维护触发器](use-a-maintenance-trigger.md)。

## 传感器和设备的后台任务


你的应用可以使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 类从后台任务访问传感器和外围设备。 你可以针对运行时间较长的操作（例如数据同步或监视）使用此触发器。 与用于系统事件的任务不同，**DeviceUseTrigger** 任务仅在你的应用在前台运行且在其上无法设置任何条件时触发。

某些关键设备操作（如长时间运行的固件更新）无法通过 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 执行。 此类操作仅可以在电脑上通过使用 [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297315) 的特权应用执行。 *特权应用*是指由设备制造商授权执行这些操作的应用。 设备元数据用于指定已指派哪个应用（如果有的话）作为设备的特权应用。 有关详细信息，请参阅 [Windows 应用商店设备应用的设备同步和更新](http://go.microsoft.com/fwlink/p/?LinkId=306619)。

## 管理后台任务


后台任务可以使用事件和本地存储向你的应用报告进度、完成和取消。 你的应用还可以捕捉由后台任务引发的异常，并在应用更新过程中管理后台任务注册。 有关详细信息，请参阅：

[处理取消的后台任务](handle-a-cancelled-background-task.md)

[监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)

**注意**  
本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你要针对 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## 相关主题


**在 Windows 10 中使用多任务的概念指南**

* [启动、恢复和多任务](index.md)

**相关的后台任务指南**

* [从后台任务访问传感器和设备](access-sensors-and-devices-from-a-background-task.md)
* [后台任务指南](guidelines-for-background-tasks.md)
* [创建和注册后台任务](create-and-register-a-background-task.md)
* [调试后台任务](debug-a-background-task.md)
* [在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md)
* [处理取消的后台任务](handle-a-cancelled-background-task.md)
* [监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)
* [注册后台任务](register-a-background-task.md)
* [使用后台任务响应系统事件](respond-to-system-events-with-background-tasks.md)
* [在计时器上运行后台任务](run-a-background-task-on-a-timer-.md)
* [设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)
* [使用后台任务更新动态磁贴](update-a-live-tile-from-a-background-task.md)
* [使用维护触发器](use-a-maintenance-trigger.md)
* [如何在 Windows 应用商店应用中触发暂停、恢复和后台事件（在调试时）](http://go.microsoft.com/fwlink/p/?linkid=254345)
* [Windows 应用商店设备应用的设备同步和更新](http://go.microsoft.com/fwlink/p/?LinkId=306619)

 

 



<!--HONumber=Jun16_HO5-->



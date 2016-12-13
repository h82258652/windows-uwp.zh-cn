---
author: PatrickFarley
ms.assetid: 24351dad-2ee3-462a-ae78-2752bb3374c2
title: "利用电池节省功能"
description: "创建 UWP 应用，这些应用与系统一起以电池高效方式使用后台任务。"
translationtype: Human Translation
ms.sourcegitcommit: 73b19e54b863693aece045e5b653bc0583a676bb
ms.openlocfilehash: 854ec43d075f8adc1f875d3b9e5e2d818434edb9

---

# <a name="optimize-background-activity"></a>优化后台活动

通用 Windows 应用应跨所有设备系列一致地执行。 在电池供电的设备上，电源消耗在应用的用户整体体验中是一个关键因素。 全天的电池使用时间对每个用户都是想要的功能，但它要求设备上安装的所有软件（包括你自己的软件）都可节电。 

后台任务行为可以说是应用的总能耗成本的最大因素。 后台任务是已在系统上注册为无需打开应用即可运行的任何程序活动。 请参阅[创建和注册进程外后台任务](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-an-outofproc-background-task)以获取详细信息。

## <a name="background-activity-allowance"></a>后台活动余量

在 Windows 10 版本 1607 中，用户可以在“设置”应用的“电池”部分查看其“按应用查看电池使用情况”。 此处，他们将看到应用列表和每个应用已使用的电池使用时间百分比（占自上次冲电以来已使用的电池使用时间量的百分比）。 

![按应用查看电池使用情况](images/battery-usage-by-app.png)

对于此列表上的 UWP 应用，用户对系统如何处理其后台活动有一些控制。 后台活动可指定为“始终允许”、“通过 Windows 管理”（默认设置），或“从不允许”（下面是更多详细信息）。 使用从 [**BackgroundExecutionManager.RequestAccessAsync()**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync.aspx) 方法返回的**BackgroundAccessStatus** 枚举值，查看应用具有哪些后台活动余量。

![后台任务权限](images/background-task-permissions.png)

这表示如果你的应用不实现负责的后台活动管理，用户可能完全拒绝应用的后台权限，这对任何一方都是不想看到的。

## <a name="work-with-the-battery-saver-feature"></a>使用节电模式功能
节电模式是一种系统级功能，用户可以在“设置”中配置。 当电池电量低于用户定义的阈值时，它将关闭所有应用的所有后台活动，*除了*已设置为“始终允许”的应用的后台活动。

如果你的应用标记为“通过 Windows 管理”，并在打开节电模式的情况下调用 **BackgroundExecutionManager.RequestAccessAsync()** 来注册后台活动，它将返回 **DeniedSubjectToSystemPolicy** 值。 通过通知用户给定的后台任务将不会运行，直到节电模式关闭并且他们重新注册系统，你的应用应该会处理此情况。 如果后台任务已注册为运行，并且节电模式在触发时处于打开状态，则该任务将不会运行并且不会通知用户。 若要减少发生这种情况的几率，较好的做法是对应用编程，使其在每次打开时重新注册后台任务。

尽管后台活动管理是节电模式功能的主要用途，但你的应用可以进行其他调整，以进一步在节电模式打开时节省能源。 通过引用 [**PowerManager.PowerSavingMode**](https://msdn.microsoft.com/library/windows/apps/windows.phone.system.power.powermanager.powersavingmode.aspx) 属性，检查应用中的节电模式状态。 它是一个枚举值：**PowerSavingMode.Off** 或 **PowerSavingMode.On**。 在节电模式打开的情况下，应用可以减少其使用的动画、停止位置轮询，或延迟同步和备份。 

## <a name="further-optimize-background-tasks"></a>进一步优化后台任务
以下是注册后台任务时可以采取的其他步骤，以使它们更省电。

使用维护触发器。 可以使用 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.maintenancetrigger.aspx) 对象而不是 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.systemtrigger.aspx) 对象来确定后台任务的启动时间。 当设备连接到交流电源且允许它们运行较长时间时，使用维护触发器的任务才会运行。 有关说明，请参阅[使用维护触发器](https://msdn.microsoft.com/windows/uwp/launch-resume/use-a-maintenance-trigger)。

使用 **BackgroundWorkCostNotHigh** 系统条件类型。 必须满足系统条件才能使后台任务运行（请参阅[设置运行后台任务的条件](https://msdn.microsoft.com/windows/uwp/launch-resume/set-conditions-for-running-a-background-task)以获取详细信息)。 后台工作成本是表示运行后台任务的*相对*能量影响的度量方法。 将设备插入到交流电源时运行的任务会标记为“低”（少/不影响电池）。 当设备使用电池电源且屏幕关闭时运行的任务会标记为“高”，因为可能当时在设备上运行的程序活动很少，因此后台任务有更高的相对成本。 当设备使用电池电源且屏幕“打开”时运行的任务会标记为“中等”，因为可能已有某些程序活动在运行，而且后台任务将增加一点能源成本。 **BackgroundWorkCostNotHigh** 系统条件只会延迟你的任务运行，直到屏幕打开或设备连接到交流电源。

## <a name="test-battery-efficiency"></a>测试电池效率

请确保针对任何高能耗方案在真实设备上测试应用。 它是在许多不同设备上、节电模式处于打开和关闭状态，以及在各种网络强度的环境中测试应用的一个好方法。

## <a name="related-topics"></a>相关主题

* [创建和注册进程外后台任务](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-an-outofproc-background-task)  
* [规划性能](https://msdn.microsoft.com/windows/uwp/debug-test-perf/planning-and-measuring-performance)  




<!--HONumber=Dec16_HO1-->



---
ms.assetid: 24351dad-2ee3-462a-ae78-2752bb3374c2
title: 优化后台活动
description: 创建与系统兼容的 UWP 应用，以便以低电池能耗使用后台任务。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 71a56bc23b4b727d5be2ed35fb77afae03f0689c
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7691038"
---
# <a name="optimize-background-activity"></a>优化后台活动

通用 Windows 应用应跨所有设备系列一致地执行。 在电池供电的设备上，电源消耗在应用的用户整体体验中是一个关键因素。 全天的电池使用时间对每个用户都是想要的功能，但它要求设备上安装的所有软件（包括你自己的软件）都可节电。 

后台任务行为可以说是应用的总能耗成本的最重要因素。 后台任务是已在系统上注册为无需打开应用即可运行的任何程序活动。 有关详细信息，请参阅[创建和注册进程外后台任务](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)。

## <a name="background-activity-permissions"></a>后台活动权限

在运行 Windows 10 版本 1607 或更高版本的桌面和移动设备上，用户可以在“设置”应用的“电池”部分中查看其“应用的电池使用情况”。 此处，他们将看到应用列表和每个应用已使用的电池使用时间百分比（占自上次冲电以来已使用的电池使用时间量的百分比）。 对于此列表上的 UWP 应用，用户可以选择应用以打开与后台活动相关的控件。

![应用的电池使用情况](images/battery-usage-by-app.png)

### <a name="background-permissions-on-mobile"></a>移动设备上的后台权限

在移动设备上，用户将看到为该应用指定后台任务权限的单选按钮的列表。 后台活动可以设置为“始终允许”、“从不允许”或“由 Windows 管理”（这意味着应用的后台活动由系统按照一些因素进行控制）。 

![后台任务权限单选按钮](images/background-task-permissions.png)

### <a name="background-permissions-on-desktop"></a>桌面设备上的后台权限

在桌面设备上，“由 Windows 管理”设置显示为切换开关（默认情况下设置为**打开**）。 如果用户切换到**关闭**，则会显示一个复选框，可以用于手动定义后台活动权限。 选中该框时，允许应用在任何时候运行后台任务。 取消选中该框时，会禁用后台活动。

![后台任务权限打开](images/background-task-permissions-on.png)

![后台任务权限关闭](images/background-task-permissions-off.png)

在应用中，可以使用 [**BackgroundExecutionManager.RequestAccessAsync()**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync.aspx) 方法调用返回的 [**BackgroundAccessStatus**](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.background.backgroundaccessstatus) 枚举值来确定其当前后台活动权限设置。

这表示如果你的应用不实现负责的后台活动管理，用户可能完全拒绝应用的后台权限，这对任何一方都是不想看到的。 如果应用被拒绝了在后台运行的权限，但是需要后台活动才能为用户完成操作，则可以通知用户并引导其进入“设置”应用。 这可以通过向“后台应用”或“电池使用详细信息”页面[启动“设置”应用](https://docs.microsoft.com/en-us/windows/uwp/launch-resume/launch-settings-app)来完成。

## <a name="work-with-the-battery-saver-feature"></a>使用节电模式功能
节电模式是一种系统级功能，用户可以在“设置”中配置。 当电池电量低于用户定义的阈值时，它将关闭所有应用的所有后台活动，*除了*已设置为“始终允许”的应用的后台活动。

通过引用 [**PowerManager.EnergySaverStatus**](https://docs.microsoft.com/en-us/uwp/api/windows.system.power.energysaverstatus) 属性，在应用中检查节电模式状态。 它是枚举值：**EnergySaverStatus.Disabled**、**EnergySaverStatus.Off** 或 **EnergySaverStatus.On**。 如果应用需要后台活动并且未设置为“始终允许”，则应通过向用户通知给定后台任务在节电模式关闭之前不会运行，来处理 **EnergySaverStatus.On**。 尽管后台活动管理是节电模式功能的主要用途，但你的应用可以进行其他调整，以进一步在节电模式打开时节省能源。  在节电模式打开的情况下，应用可以减少其使用的动画、停止位置轮询，或延迟同步和备份。 

## <a name="further-optimize-background-tasks"></a>进一步优化后台任务
以下是注册后台任务时可以采取的其他步骤，以使它们更省电。

### <a name="use-a-maintenance-trigger"></a>使用维护触发器 
可以使用 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.maintenancetrigger.aspx) 对象而不是 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.systemtrigger.aspx) 对象来确定后台任务的启动时间。 当设备连接到交流电源且允许它们运行较长时间时，使用维护触发器的任务才会运行。 有关说明，请参阅[使用维护触发器](https://msdn.microsoft.com/windows/uwp/launch-resume/use-a-maintenance-trigger)。

### <a name="use-the-backgroundworkcostnothigh-system-condition-type"></a>使用 **BackgroundWorkCostNotHigh** 系统条件类型
必须满足系统条件才能使后台任务运行（请参阅[设置运行后台任务的条件](https://msdn.microsoft.com/windows/uwp/launch-resume/set-conditions-for-running-a-background-task)以获取详细信息）。 后台工作成本是表示运行后台任务的*相对*能量影响的度量方法。 将设备插入到交流电源时运行的任务会标记为**低**（少/不影响电池）。 当设备使用电池电源且屏幕关闭时运行的任务会标记为**高**，因为可能当时在设备上运行的程序活动很少，因此后台任务有更高的相对成本。 当设备使用电池电源且屏幕*打开*时运行的任务会标记为**中等**，因为可能已有某些程序活动在运行，而且后台任务将增加一点能源成本。 **BackgroundWorkCostNotHigh** 系统条件只会延迟你的任务运行，直到屏幕打开或设备连接到交流电源。

## <a name="test-battery-efficiency"></a>测试电池效率

请确保针对任何高能耗方案在真实设备上测试应用。 它是在许多不同设备上、节电模式处于打开和关闭状态，以及在各种网络强度的环境中测试应用的一个好方法。

## <a name="related-topics"></a>相关主题

* [创建和注册进程外后台任务](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)  
* [规划性能](https://msdn.microsoft.com/windows/uwp/debug-test-perf/planning-and-measuring-performance)  


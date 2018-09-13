---
author: muhsinking
title: 从后台任务访问传感器和设备
description: DeviceUseTrigger 允许你的通用 Windows 应用访问后台中的传感器和外围设备，即使在前台应用暂停时也是如此。
ms.assetid: B540200D-9FF2-49AF-A224-50877705156B
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，后台任务
ms.localizationpriority: medium
ms.openlocfilehash: 99f853da53302d4080bfa9462da0ec524e8d2064
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3960165"
---
# <a name="access-sensors-and-devices-from-a-background-task"></a>从后台任务访问传感器和设备




[**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 允许你的通用 Windows 应用访问后台中的传感器和外围设备，即使在前台应用暂停时也是如此。 例如，根据应用运行所在的位置，它可以使用后台任务将数据与设备或监视器传感器同步。 为了帮助延长电池使用时间并确保相应的用户同意，使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 需遵循本主题中所述的策略。

若要访问后台中的传感器或外围设备，请创建一个使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 的后台任务。 有关显示如何在电脑上实现此目的的示例，请参阅[自定义 USB 设备示例](http://go.microsoft.com/fwlink/p/?LinkId=301975 )。 有关在手机上实现此目的的示例，请参阅[后台传感器示例](http://go.microsoft.com/fwlink/p/?LinkId=393307)。

> [!Important]
> **DeviceUseTrigger** 不能用于进程内的后台任务。 本主题中的信息仅适用于进程外运行的后台任务。

## <a name="device-background-task-overview"></a>设备后台任务概述

当你的应用不再对用户可见时，Windows 会暂停或终止应用以回收内存和 CPU 资源。 这允许其他应用在前台运行并减少电池消耗。 出现这种情况时，如未获得后台任务的帮助，正在进行的数据事件将丢失。 Windows 提供了后台任务触发器 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)，让你的应用从后台安全地在设备和传感器上执行长时间运行的同步和监视操作，即使你的应用暂停也是如此。 有关应用生命周期的详细信息，请参阅[启动、恢复和后台任务](index.md)。 有关后台任务的详细信息，请参阅[使用后台任务支持应用](support-your-app-with-background-tasks.md)。

**注意**  在通用 Windows 应用中，在后台同步设备需要用户批准你的应用执行后台同步。 设备还必须通过活动 I/O 连接到电脑或与电脑配对，最长允许执行 10 分钟的后台活动。 本主题后面将介绍有关策略实施的更多详细信息。

### <a name="limitation-critical-device-operations"></a>限制：关键设备操作

某些关键设备操作（如长时间运行的固件更新）无法通过 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 执行。 此类操作仅可以在电脑上通过使用 [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297315) 的特权应用执行。 *特权应用*是指由设备制造商授权执行这些操作的应用。 设备元数据用于指定已指派哪个应用（如果有）作为设备的特权应用。 有关详细信息，请参阅 [Microsoft Store 设备应用的设备同步和更新](http://go.microsoft.com/fwlink/p/?LinkId=306619)。

## <a name="protocolsapis-supported-in-a-deviceusetrigger-background-task"></a>DeviceUseTrigger 后台任务中支持的协议/API

使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 的后台任务使你的应用可以跨许多协议/API 通信，其中大部分协议/API 不受系统触发的后台任务支持。 通用 Windows 应用支持以下内容。

| 协议         | 通用 Windows 应用中的 DeviceUseTrigger                                                                                                                                                    |
|------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| USB              | ![支持此协议。](images/ap-tools.png)                                                                                                                                            |
| HID              | ![支持此协议。](images/ap-tools.png)                                                                                                                                            |
| 蓝牙 RFCOMM | ![支持此协议。](images/ap-tools.png)                                                                                                                                            |
| 蓝牙 GATT   | ![支持此协议。](images/ap-tools.png)                                                                                                                                            |
| MTP              | ![支持此协议。](images/ap-tools.png)                                                                                                                                            |
| 有线网络    | ![支持此协议。](images/ap-tools.png)                                                                                                                                            |
| WLAN 网络    | ![支持此协议。](images/ap-tools.png)                                                                                                                                            |
| IDeviceIOControl | ![DeviceServicingTrigger 支持 IDeviceIOControl](images/ap-tools.png)                                                                                                                       |
| 传感器 API      | ![DeviceServicingTrigger 支持通用传感器 API](images/ap-tools.png)（仅限于[通用设备系列](https://msdn.microsoft.com/library/windows/apps/dn894631)中的传感器） |

## <a name="registering-background-tasks-in-the-app-package-manifest"></a>在应用包清单中注册后台任务

你的应用将在运行后台任务期间以代码的形式执行同步和更新操作。 此代码嵌入在可实现 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 的 Windows 运行时类中（或 JavaScript 应用的专用 JavaScript 页中）。 若要使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 后台任务，你的应用必须在前台应用的应用部件清单文件中声明该任务，这一点与系统触发的后台任务类似。

在应用包清单文件的这一示例中，**DeviceLibrary.SyncContent** 是使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 的后台任务的入口点。

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" EntryPoint="DeviceLibrary.SyncContent">
    <BackgroundTasks>
      <m2:Task Type="deviceUse" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

## <a name="introduction-to-using-deviceusetrigger"></a>使用 DeviceUseTrigger 简介

若要使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)，请执行以下基本步骤。 有关后台任务的详细信息，请参阅[使用后台任务支持应用](support-your-app-with-background-tasks.md)。

1.  你的应用在应用清单中注册其后台任务，并在实现 IBackgroundTask 的 Windows 运行时类或 JavaScript 应用的专用 JavaScript 页中嵌入后台任务代码。
2.  当你的应用启动时，它将创建并配置类型为 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 的触发器对象，然后存储触发器实例以供将来使用。
3.  你的应用检查先前是否已经注册该后台任务，如未注册，则针对该触发器注册后台任务。 请注意，不允许你的应用为与此触发器关联的任务设置条件。
4.  当你的应用需要触发后台任务时，它必须先调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)，才能检查该应用是否能够请求后台任务。
5.  如果你的应用可以请求后台任务，它将在设备触发器对象上调用 [**RequestAsync**](https://msdn.microsoft.com/library/windows/apps/dn297341) 激活方法。
6.  与其他系统后台任务不同，你的后台任务不会受到限制（无 CPU 时间配额），但运行优先级将会降低，从而保证前台应用的及时响应。
7.  然后，在开始执行该后台任务之前，Windows 将根据触发器类型验证是否符合必要的策略，包括是否就该操作请求用户同意。
8.  Windows 监控系统条件和任务运行情况，并在必要时（不再符合所需条件）取消该任务。
9.  当后台任务报告进度或完成时，你的应用将通过该注册任务的进度事件和完成事件接收这些事件。

**重要提示**  
使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 时，请考虑以下几个要点：

-   首次在 Windows8.1 和 Windows Phone 8.1 中引入了通过编程方式触发可使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 的后台任务功能。

-   Windows 会强制执行某些策略，从而确保获得用户同意才能更新电脑上的外围设备。

-   此外还会强制执行其他策略，其目的是为了在同步和更新外围设备时维持用户的电池使用寿命。

-   当不再符合某些策略要求（包括最长后台时间（时钟时间）量时，Windows 可能会取消使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 的后台任务。 使用这些后台任务与外围设备交互时考虑这些策略要求很重要。

**提示**  若要了解这些后台任务的工作原理，请下载相关示例。 有关显示如何在电脑上实现此目的的示例，请参阅[自定义 USB 设备示例](http://go.microsoft.com/fwlink/p/?LinkId=301975 )。 有关在手机上实现此目的的示例，请参阅[后台传感器示例](http://go.microsoft.com/fwlink/p/?LinkId=393307)。
 
## <a name="frequency-and-foreground-restrictions"></a>频率和前台限制

应用启动操作频率方面没有相关限制，但应用每次只能运行一项 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 后台任务操作（这不会对其他类型的后台任务造成影响），并且只有当你的应用位于前台时才能启动后台任务。 当你的应用不在前台时，将无法启动使用 **DeviceUseTrigger** 的后台任务。 在前一项后台任务完成之前，应用无法启动另一项 **DeviceUseTrigger** 后台任务。

## <a name="device-restrictions"></a>设备限制

当每个应用仅限于注册并运行一项 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 后台任务时，设备（在其上运行你的应用）可能允许多个应用注册并运行 **DeviceUseTrigger** 后台任务。 对于所有应用，**DeviceUseTrigger** 后台任务的总数量可能存在限制，具体取决于设备。 这有助于在资源受限的设备上维持电池寿命。 有关详细信息，请参阅下表。

在单个 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 后台任务中，你的应用可以访问无限数量的外围设备或传感器，仅受在上表中列出的受支持的 API 和协议限制。

## <a name="background-task-policies"></a>后台任务策略

当应用使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 后台任务时，Windows 将强制执行策略。 如果不符合这些策略要求，可能会取消该后台任务。 使用此类型的后台任务与设备或传感器交互时考虑这些策略要求很重要。

### <a name="task-initiation-policies"></a>任务启动策略

下表指示哪些任务启动策略适用于通用 Windows 应用。

| 策略 | 通用 Windows 应用中的 DeviceUseTrigger |
|--------|---------------------------------------------|
| 当触发后台任务时，你的应用在前台运行。 | ![策略适用](images/ap-tools.png) |
| 该设备已连接到系统（或在无线设备覆盖范围内）。 | ![策略适用](images/ap-tools.png) |
| 使用受支持的设备外围 API（适用于 USB、HID、蓝牙和传感器等的 Windows 运行时 API）的应用可以访问该设备。 如果你的应用无法访问设备或传感器，则拒绝其访问该后台任务。 | ![策略适用](images/ap-tools.png) |
| 在应用包部件清单中注册该应用提供的后台任务入口点。 | ![策略适用](images/ap-tools.png) |
| 每个应用只有一项 [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/dn297337) 后台任务正在运行。 | ![策略适用](images/ap-tools.png) |
| [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/dn297337) 后台任务的最大数量还未达到设备（在其上运行你的应用）上限。 | **桌面设备系列**：可以注册无限数量的任务，并且可以并行运行。 **移动设备系列**：在 512 MB 设备上只能注册 1 个任务；否则，可以注册 2 个任务，并且可以并行运行。 |
| 当使用受支持的 API/协议时，你的应用可以从单个 [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/dn297337) 后台任务中访问的最大数量的外围设备或传感器。 | 无限制 |
| 当屏幕锁定时，你的后台任务每分钟消耗 400 毫秒的 CPU 时间（假设 1GHz CPU），或者当屏幕未锁定时，每五分钟消耗该时间。 未能满足此策略可能导致任务被取消。 | ![策略适用](images/ap-tools.png) |

### <a name="runtime-policy-checks"></a>运行时策略检查

当你的任务在后台运行时，Windows 将强制执行以下运行时策略要求。 如果不能达到任意一项运行时要求，Windows 将取消你的设备后台任务。

下表指示哪些运行时策略适用于通用 Windows 应用。

| 策略检查 | 通用 Windows 应用中的 DeviceUseTrigger |
|--------------|:-------------------------------------------:|
| 该设备已连接到系统（或在无线设备覆盖范围内）。 | ![策略检查适用](images/ap-tools.png) |
| 任务不断对设备执行常规 I/O （每隔 5 秒执行 1 次 I/O）。 | ![策略检查适用](images/ap-tools.png) |
| 应用未取消任务。 | ![策略检查适用](images/ap-tools.png) |
| 时钟时间限制 - 你的应用任务可以在后台运行的时间总量。 | **桌面设备系列**：10 分钟。 **移动设备系列**：无时间限制。 若要节省资源，不要一次执行多于 1 个或 2 个任务。 |
| 应用未退出。 | ![策略检查适用](images/ap-tools.png) |

## <a name="best-practices"></a>最佳做法

以下是针对使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 后台任务的应用的最佳做法。

### <a name="programming-a-background-task"></a>为后台任务编程

从你的应用使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 后台任务，可确保在用户切换应用以及你的前台应用被 Windows 暂停的情况下，从你的前台应用启动的所有同步或监视操作都会在后台继续运行。 我们建议你按照这个整体模型注册、触发及注销后台任务：

1.  调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) 以检查该应用是否能够请求后台任务。 必须先完成此操作，然后再注册后台任务。

2.  先注册后台任务，然后再请求触发。

3.  将进度事件处理程序和完成事件处理程序连接到你的触发器。 当你的应用从挂起状态恢复时，Windows 将为该应用提供可用于确定后台任务状态的任何排队等候处理的进度事件和完成事件。

4.  触发 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 后台任务时，关闭所有打开的设备或传感器对象，以便你的后台任务能够自由打开和使用这些设备或传感器。

5.  注册触发器。

6.  仔细考虑从后台任务访问设备或传感器对电池产生的影响。 例如，传感器运行的报告间隔太频繁可能会导致任务频繁运行，这会快速消耗手机的电池。

7.  完成后台任务后，请注销。

8.  从你的后台任务类注册取消事件。 注册取消事件后，当 Windows 或你的前台应用取消后台任务时，即可使用后台任务代码完全停止你运行的后台任务。

9.  在应用退出时（并非挂起），注销并取消所有运行中的任务（如果你的应用不再需要这些任务）。 在资源受限的系统（例如低内存手机）上，这将允许其他应用使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 后台任务。

    -   退出你的应用时，注销并取消所有运行中的任务。

    -   退出你的应用时，将取消你的后台任务并断开所有现有事件处理程序与现有后台任务的连接。 这样你就无需确定你的后台任务的状态。 通过对注销和取消所有后台任务，你即可利用取消代码完全停止后台任务。

### <a name="cancelling-a-background-task"></a>取消后台任务

若要从你的前台应用取消在后台运行的任务，请使用你的应用中所使用的 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) 对象上的 Unregister 方法，注册 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 后台任务。 使用 **BackgroundTaskRegistration** 上的 [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229869) 方法注销你的后台任务，将会导致后台任务基础结构取消你的后台任务。

此外，[**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229869) 方法使用布尔值 true 或 false 指示是否应在任务未完成的情况下取消当前运行的后台任务实例。 有关详细信息，请参阅 **Unregister** 的 API 参考。

除了 [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229869)，你的应用还需要调用 [**BackgroundTaskDeferral.Complete**](https://msdn.microsoft.com/library/windows/apps/hh700504)。 这会通知系统与后台任务关联的异步操作已完成。

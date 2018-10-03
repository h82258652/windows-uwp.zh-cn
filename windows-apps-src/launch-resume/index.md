---
author: TylerMSFT
title: 启动、恢复和后台任务
description: 本节介绍在启动、暂停、恢复和终止通用 Windows 平台 (UWP) 应用时会发生什么情况。
ms.assetid: 75011D52-1511-4ECF-9DF6-52CBBDB15BD7
ms.author: twhitney
ms.date: 10/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，后台任务，应用服务连接的设备，远程系统
ms.localizationpriority: medium
ms.openlocfilehash: d4aa5a4f379e0791e9da7db4ecd2a27c09cf0a3a
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/02/2018
ms.locfileid: "4262032"
---
# <a name="launching-resuming-and-background-tasks"></a>启动、恢复和后台任务


此部分包含有关以下内容的信息：

- 在启动、暂停、恢复和终止通用 Windows 平台 (UWP) 应用时会发生什么情况。
- 如何通过使用 URI 或文件激活启动应用。
- 如何使用应用服务，从而允许通用 Windows 平台 (UWP) 应用与其他应用共享数据和功能。
- 如何使用后台任务，从而允许 UWP 应用在应用自身不在前台时也能执行工作。
- 如何发现连接的设备、在其他设备上启动应用，以及与远程设备上的应用服务通信，以便你可以创建跨设备流动的用户体验。
- 如何选择正确的技术来扩展你的应用并实现应用组件化。
- 如何为应用添加和配置初始屏幕。
- 如何编写，以便通过用户可从 Microsoft Store 中安装的程序包来扩展应用。

## <a name="the-app-lifecycle"></a>应用生命周期

本部分详细介绍 Windows 10 通用 Windows 平台 (UWP) 应用的生命周期，从其激活时直到其关闭。

| 主题 | 描述 |
|-------|-------------|
| [应用生命周期](app-lifecycle.md)               | 了解有关 UWP 应用的生命周期，以及在 Windows 启动、暂停和恢复你的应用时会发生什么情况。 |
| [处理应用预启动](handle-app-prelaunch.md) | 了解如何处理应用预启动。                                                                              |
| [处理应用激活](activate-an-app.md)     | 了解如何处理应用激活。                                                                             |
| [处理应用暂停](suspend-an-app.md)         | 了解当系统暂停你的应用时如何保存重要的应用程序数据。                                 |
| [处理应用恢复](resume-an-app.md)           | 了解当系统恢复你的应用时如何刷新显示的内容。                                        |
| [在将应用移动到后台时释放内存](reduce-memory-usage.md) | 了解如何降低应用处于后台状态时所使用的内存量，以防止该应用终止。|
| [使用扩展执行来推迟应用挂起](run-minimized-with-extended-execution.md) | 了解最小化时如何使用扩展执行让应用保持运行 |

## <a name="launch-apps"></a>启动应用

| 主题 | 描述 |
|-------|-------------|
| [创建通用 Windows 平台控制台应用](console-uwp.md) | 了解如何编写在控制台窗口中运行的通用 Windows 平台应用。 |
| [创建多实例 UWP 应用](multi-instance-uwp.md) | 了解如何编写多实例通用 Windows 平台应用。 |

[使用 URI 启动应用](launch-app-with-uri.md)部分详细介绍了如何使用统一资源标识符 (URI) 来启动应用。

| 主题 | 描述 |
|-------|-------------|
| [启动 URI 的默认应用](launch-default-app.md) | 了解如何启动统一资源标识符 (URI) 的默认应用。 URI 允许你启动其他应用以执行特定任务。 本主题还提供许多内置于 Windows 的 URI 方案的概述。 |
| [处理 URI 激活](handle-uri-activation.md) | 了解如何将应用注册为统一资源标识符 (URI) 方案名称的默认处理程序。 |
| [针对结果启动应用](how-to-launch-an-app-for-results.md) | 了解如何从其他应用启动某个应用，以及在这两者之间交换数据。 这就是针对结果启动应用。 |
| [使用 ms-tonepicker URI 方案选择并保存音调](launch-ringtone-picker.md) | 本主题介绍了 ms-tonepicker URI 方案，以及如何使用它显示音调选取器，以便选择音调、保存音调和获取音调的友好名称。 |
| [启动 Windows 设置应用](launch-settings-app.md) | 了解如何从你的应用启动 Windows 设置应用。 本主题介绍了 ms-settings URI 方案。 使用此 URI 方案将 Windows 设置应用启动到特定设置页面。 |
| [打开 Microsoft Store 应用](launch-store-app.md) | 本主题介绍了 ms-windows-store URI 方案。 应用可以使用此 URI 方案将 UWP 应用启动到 Microsoft Store 中的特定页面。 |
| [启动 Windows 地图应用](launch-maps-app.md) | 了解如何从你的应用启动 Windows 地图应用。 |
| [启动“人脉”应用](launch-people-apps.md) | 本主题介绍了 ms-people URI 方案。 你的应用可以使用此 URI 方案来针对特定操作启动“人脉”应用。 |
| [支持使用应用 URI 处理程序的 Web 到应用链接](web-to-app-linking.md) | 通过使用应用 URI 处理程序推动用户与应用的互动。 |

[通过文件激活启动应用](launch-app-from-file.md)部分详细介绍如何将应用设置为在打开特定类型的文件时启动。

| 主题 | 描述 |
|-------|-------------|
| [启动文件的默认应用](launch-the-default-app-for-a-file.md) | 了解如何启动文件的默认应用。 |
| [处理文件激活](handle-file-activation.md) | 了解如何注册你的应用以成为某个文件类型的默认处理程序。 |

请参阅下面与启动应用相关的其他主题。

| 主题 | 描述 |
|-------|-------------|
| [即便跨设备，也继续用户活动](useractivities.md) | 通过在用户离开的位置启动应用，即使跨设备也可以让用户重新使用应用。 |
| [借助自动播放功能自动启动](auto-launching-with-autoplay.md) | 可以使用自动播放功能在用户将设备连接到其电脑时，将应用作为一个选项提供。 这包括非卷设备（如相机或媒体播放器）或卷设备（如 U 盘、SD 卡或 DVD）。 |
| [保留的文件和 URI 方案名称](reserved-uri-scheme-names.md) | 本主题将列出不可用于应用的保留文件和 URI 方案名称。 |

## <a name="app-services-and-extensions"></a>应用服务和扩展

[应用服务和扩展](app-services.md)部分介绍如何将应用服务集成到你的 UWP 应用，从而允许跨应用共享数据和功能。

| 主题 | 描述 |
|-------|-------------|
| [创建和使用应用服务](how-to-create-and-consume-an-app-service.md) | 了解如何编写可以向其他 UWP 应用提供服务的通用 Windows 平台 (UWP) 应用，以及如何使用这些服务。 |
| [将应用服务转换为与其主机应用在同一个进程中运行](convert-app-service-in-process.md) | 将在单独的后台进程中运行的应用服务代码转换为在与应用服务提供程序相同的进程中运行的代码。 |
| [使用应用服务、扩展和包扩展应用](extend-your-app-with-services-extensions-packages.md) | 确定使用哪种技术来扩展和组件化应用，并简要概述每种技术。 |
| [创建和使用应用扩展](how-to-create-an-extension.md) | 编写并托管通用 Windows 平台 (UWP) 应用扩展，以便通过用户可从 Microsoft Store 中安装的程序包来扩展应用。 |

## <a name="background-tasks"></a>后台任务

[后台任务](support-your-app-with-background-tasks.md)部分显示了如何使在后台运行的轻型代码响应触发器。

| 主题 | 描述 |
|-------|-------------|
| [后台任务指南](guidelines-for-background-tasks.md)                                       | 确保应用满足运行后台任务的要求。 |
| [从后台任务访问传感器和设备](access-sensors-and-devices-from-a-background-task.md)   | 通过 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)，通用 Windows 应用可访问后台中的传感器和外围备，即使在前台应用暂停时也是如此。 |
| [创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)       | 创建和注册在前台应用所在的同一进程中运行的后台任务。 |
| [创建和注册进程外后台任务](create-and-register-a-background-task.md)           | 创建和注册一个与你的应用不在同一个进程中运行的后台任务，然后将它注册为在应用不在前台运行时运行。 |
| [移植为进程内后台任务进程外后台任务](convert-out-of-process-background-task.md) | 了解如何将移植到在前台应用所在的同一进程中运行的进程内后台任务进程外后台任务。|
| [调试后台任务](debug-a-background-task.md)                                                       | 了解如何调试后台任务，其中包括后台任务激活和调试 Windows 事件日志中的跟踪。 |
| [在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md) | 通过在应用清单中将后台任务声明为扩展，以实现对后台任务的使用。 |
| [分组后台任务注册](group-background-tasks.md)                                             | 通过分组隔离后台任务注册。 |
| [处理取消的后台任务](handle-a-cancelled-background-task.md)                                 | 介绍如何创建一个后台任务，该任务识别取消请求并停止工作，向使用永久性存储的应用报告取消。 |
| [监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)       | 了解应用可以识别后台任务进度和完成的方式。 |
| [优化后台活动](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) |了解如何减少后台中使用的能量并与后台活动的用户设置进行交互。 |
| [注册后台任务](register-a-background-task.md)                                                 | 了解如何创建可以重新使用以安全注册大部分后台任务的函数。 |
| [使用后台任务响应系统事件](respond-to-system-events-with-background-tasks.md)         | 了解如何创建响应 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) 事件的后台任务。 |
| [在计时器上运行后台任务](run-a-background-task-on-a-timer-.md)                                    | 了解如何计划一次性后台任务，或运行周期性后台任务。 |
| [在后台无限期运行](run-in-the-background-indefinetly.md)                                    | 使用可在后台无限期运行后台任务或扩展执行会话的功能。 |
| [从应用中触发后台任务](trigger-background-task-from-app.md) | 了解如何使用 [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) 从应用中激活后台任务。|
| [设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)             | 了解如何设置控制何时运行后台任务的条件。 |
| [在后台传输数据](https://msdn.microsoft.com/library/windows/apps/mt280377)                 | 使用后台传输 API 以便在后台复制文件。 |
| [使用后台任务更新动态磁贴](update-a-live-tile-from-a-background-task.md)                   | 使用后台任务，以最新内容更新应用的动态磁贴。 |
| [使用维护触发器](use-a-maintenance-trigger.md)                                                   | 了解如何在插入设备的情况下使用 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) 类在后台运行轻型代码。 |

## <a name="remote-systems"></a>远程系统

[连接的应用和设备（项目“Rome”）](connected-apps-and-devices.md)介绍如何使用远程系统平台发现远程设备、在远程设备上启动应用，以及与远程设备上的应用服务通信。

| 主题 | 描述 |
|-------|-------------|
| [发现远程设备](discover-remote-devices.md)  | 了解如何发现可以连接的设备。 |
| [启动远程设备上的应用](launch-a-remote-app.md) | 了解如何启动远程设备上的应用。  |
| [与远程应用服务通信](communicate-with-a-remote-app-service.md) | 了解如何与远程设备上的应用交互。 |
| [通过远程会话连接设备](remote-sessions.md) | 通过在远程会话中加入多个设备来创建跨多个设备的共享体验。 |

## <a name="splash-screens"></a>初始屏幕

[初始屏幕](splash-screens.md)部分介绍如何设置和配置应用的初始屏幕。

| 主题 | 描述 |
|-------|-------------|
| [添加初始屏幕](add-a-splash-screen.md) | 设置你的应用的初始屏幕图像和背景色。 |
| [延长显示初始屏幕的时间](create-a-customized-splash-screen.md) | 通过为你的应用创建延长的初始屏幕，延长显示初始屏幕的时间。 此延长的屏幕将模仿你的应用启动时显示的初始屏幕，并且可以进行自定义。 |

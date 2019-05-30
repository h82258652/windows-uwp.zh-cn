---
title: 将进程外后台任务移植到进程内后台任务
description: 移植到在前台应用程序进程中运行的进程在后台任务进程外后台任务。
ms.date: 09/19/2018
ms.topic: article
keywords: windows 10、 uwp、 后台任务、 应用服务
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: 42aaa5600b30924acc84aa61c2a15dbb2a320c10
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366261"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>将进程外后台任务移植到进程内后台任务

移植您的进程外 (OOP) 的后台活动以进程内活动的最简单方法是将您[IBackgroundTask.Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run?f=255&MSPPError=-2147217396)方法在应用程序的代码，并启动从[OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated). 此处所述的技术不是使用进程内的后台任务; 从 OOP 后台任务创建填充码它的有关重写 （或移植） 到进程内版本的 OOP 版本。

如果应用具有多个后台任务，[后台激活示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation)可显示如何使用 `BackgroundActivatedEventArgs.TaskInstance.Task.Name` 确定启动的任务。

如果当前在后台进程和前台进程之间通信，可删除该状态管理和通信代码。

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>无法转换的后台任务和触发器类型

* 进程内后台任务不支持激活 VoIP 后台任务。
* 进程内的后台任务不支持以下触发器：[DeviceUseTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396)， [DeviceServicingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceservicingtrigger)和**IoTStartupTask**

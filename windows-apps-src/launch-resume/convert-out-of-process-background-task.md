---
author: TylerMSFT
title: 将进程外后台任务移植到进程内后台任务
description: 进程外后台任务移植到在前台应用进程中运行的进程内后台任务。
ms.author: twhitney
ms.date: 09/19/2018
ms.topic: article
keywords: windows 10，uwp，后台任务，应用服务
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: 47008fd7ba0b7724aa8fbdc2dd6cbd55288faea0
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5971308"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>将进程外后台任务移植到进程内后台任务

端口用于进程内活动在进程外 (OOP) 后台活动的最简单方法是将应用程序中，你[IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396)方法代码，并从[OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated)启动它。 有关为进程内后台任务; 从 OOP 后台任务创建填充的不是正在此处所述的技术它的有关重写 （或移植） 到进程内版本 OOP 版本。

如果应用具有多个后台任务，[后台激活示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation)可显示如何使用 `BackgroundActivatedEventArgs.TaskInstance.Task.Name` 确定启动的任务。

如果当前在后台进程和前台进程之间通信，可删除该状态管理和通信代码。

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>无法转换的后台任务和触发器类型

* 进程内后台任务不支持激活 VoIP 后台任务。
* 进程内后台任务不支持以下触发器：[DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396)、[DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) 和 **IoTStartupTask**。

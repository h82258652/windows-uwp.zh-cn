---
author: TylerMSFT
title: 将进程外后台任务转换为进程内后台任务
description: 将进程外后台任务转换为在前台应用进程中运行的进程内后台任务。
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10 uwp、 后台任务、 应用程序服务
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: 1144443f943f134991d050dea1457f252eaaf36d
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "2829723"
---
# <a name="convert-an-out-of-process-background-task-to-an-in-process-background-task"></a>将进程外后台任务转换为进程内后台任务

将进程外后台活动转换为进程内活动的最简单方法是将 [IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) 方法代码加入应用程序，并从 [OnBackgroundActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) 中启动它。

如果应用具有多个后台任务，[后台激活示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation)可显示如何使用 `BackgroundActivatedEventArgs.TaskInstance.Task.Name` 确定启动的任务。

如果当前在后台进程和前台进程之间通信，可删除该状态管理和通信代码。

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>无法转换的后台任务和触发器类型

* 进程内后台任务不支持激活 VoIP 后台任务。
* 进程内后台任务不支持以下触发器：[DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396)、[DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) 和 **IoTStartupTask**。

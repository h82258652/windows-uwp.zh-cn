---
author: TylerMSFT
title: "将多进程后台任务转换为单进程后台任务"
description: "将在独立进程中运行的后台任务转换为在前台应用进程中运行的后台任务。"
translationtype: Human Translation
ms.sourcegitcommit: 2c34ca40d3c930254500477ab5a2e41e5206d823
ms.openlocfilehash: e342667347cf3b89a5aa193495cbf7195263b276

---

# 将多进程后台任务转换为单进程后台任务

将多进程后台活动转换为单进程后台活动的最简单方法是将 [IBackgroundTask.Run](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) 方法代码加入应用程序，并从 [OnBackgroundActivated](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) 中启动它。

如果应用具有多个后台任务，[后台激活示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation)可显示如何使用 `BackgroundActivatedEventArgs.TaskInstance.Task.Name` 确定启动的任务。

如果当前在后台进程和前台进程之间通信，可删除该状态管理和通信代码。

## 无法转换的后台任务和触发器类型

* 单进程后台任务不支持激活 VoIP 后台任务。
* 单进程后台任务不支持以下触发器：[DeviceUseTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396)、[DeviceServicingTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) 和 **IoTStartupTask**



<!--HONumber=Aug16_HO3-->



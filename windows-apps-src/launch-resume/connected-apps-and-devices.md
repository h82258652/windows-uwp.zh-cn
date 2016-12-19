---
author: TylerMSFT
title: Connected apps and devices (Project "Rome")
description: "本节介绍如何使用远程系统平台发现远程设备、在远程设备上启动应用，以及与远程设备上的应用服务通信。"
translationtype: Human Translation
ms.sourcegitcommit: 871599217e6da0eb0febd140323e99db7d1258cb
ms.openlocfilehash: 2dbce34aeaf4933eee22e1e8ef40c48e633f6f7e

---

# <a name="connected-apps-and-devices-project-rome"></a>连接的应用和设备（项目“Rome”）

本节介绍如何使用 Project“Rome”跨设备和平台连接应用。 了解如何发现远程设备、在远程设备上启动应用，以及与远程设备上的应用服务通信。

大多数用户拥有多台设备，并且通常在一台设备上开始某项活动，而在其他设备上结束该活动。 为了满足此要求，应用需要横跨设备和平台。

Windows 10 版本 1607 中引入了[远程系统 API](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.System.RemoteSystems)，可使你编写允许用户在一台设备上开始某项任务而在另一台设备上结束该任务的应用。 将集中处理该任务，用户可在最方便的设备上执行工作。 例如，在车上你可能会收听手机上的收音机，但当回到家中，你可能会想将播放转到连接家庭立体声系统的 Xbox One。

还可将项目“Rome”用于伴侣设备或远程控制方案。 使用应用消息处理 API 在两台设备之间创建应用通道，以发送和接收自定义消息。 例如，可为手机编写控制电视播放的应用，或者编写提供关于在其他应用上观看的电视节目人物信息的伴侣应用。  

设备可通过蓝牙和无线在近处连接，也可通过云远程连接，并且通过使用设备的用户的 Microsoft 帐户连接。

请参阅[远程系统示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems )获取如何发现远程系统、在远程系统上启动应用，以及使用应用服务在运行在两个系统上的应用之间发送消息的示例。

| 主题 | 描述 |
|-------|-------------|
| [发现远程设备](discover-remote-devices.md)  | 了解如何发现可以连接的设备。 |
| [启动远程设备上的应用](launch-a-remote-app.md) | 了解如何启动远程设备上的应用。  |
| [与远程应用服务通信](communicate-with-a-remote-app-service.md) | 了解如何与远程设备上的应用交互。 |



<!--HONumber=Dec16_HO1-->



---
title: 耳机
description: 使用 Windows.Gaming.Input 耳机 API 检测耳机、捕获播放器语音并播放音频。
ms.assetid: 021CCA26-D339-4C8B-B084-0D499BD83ABE
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 耳机
ms.localizationpriority: medium
ms.openlocfilehash: b3de68cc59c9928a52eba5caeb840e9e825eecf0
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8464077"
---
# <a name="headset"></a>耳机

此页介绍使用 [Windows.Gaming.Input.Headset][headset] 和通用 Windows 平台 (UWP) 的相关 API 进行耳机编程的基本知识。

在本页中，你将了解如下内容：
* 如何访问连接到输入或导航设备的耳机
* 如何检测是否已连接或断开某个耳机


## <a name="headset-overview"></a>耳机概述

耳机是音频捕获和播放设备，最常用于与联机游戏中的玩家沟通，但也可在玩游戏时使用或用于其他有创意的用途。 在 Windows 10 和 Xbox UWP 应用中，耳机受 [Windows.Gaming.Input][] 命名空间支持。


## <a name="detect-and-track-headsets"></a>检测和跟踪耳机

耳机通过系统进行管理，因此，你不必进行创建或初始化。 系统提供通过与耳机相连的输入设备对耳机进行访问的权限和对事件的访问权限，以在连接或断开耳机时进行通知。

### <a name="igamecontrollerheadset"></a>IGameController.Headset

[Windows.Gaming.Input][] 命名空间中的所有输入设备都实现了 [IGameController][] 界面，此界面将 [Headset][igamecontroller.headset] 属性定义为当前与设备相连的耳机。

### <a name="connecting-and-disconnecting-headsets"></a>连接和断开耳机。

连接或断开耳机时，会引发 [HeadsetConnected][igamecontroller.headsetconnected] 和 [HeadsetDisconnected][igamecontroller.headsetdisconnected] 事件。 你可以为这些事件注册处理程序以跟踪输入设备当前是否连接了耳机。

以下示例显示如何为 `HeadsetConnected` 事件注册处理程序。

```cpp
auto inputDevice = myGamepads[0]; // or arcade stick, racing wheel

inputDevice.HeadsetConnected += ref new TypedEventHandler<IGameController^, Headset^>(IGameController^ device, Headset^ headset)
{
    // enable headset capture and playback on this device
}
```

以下示例显示如何为 `HeadsetDisconnected` 事件注册处理程序。

```cpp
auto inputDevice = myGamepads[0]; // or arcade stick, racing wheel

inputDevice.HeadsetDisconnected += ref new TypedEventHandler<IGameController^, Headset^>(IGameController^ device, Headset^ headset)
{
    // disable headset capture and playback on this device
}
```

## <a name="using-the-headset"></a>使用耳机

[Headset][] 类由两个表示 XAudio 终结点 ID 的字符串组成--一个用于音频捕获（从耳机麦克风录制），一个用于音频呈现（通过耳机播放）。

此处不讨论使用 XAudio 的详细信息，有关详细信息，请参阅 [XAudio2 编程指南](https://msdn.microsoft.com/library/windows/desktop/ee415737.aspx)和 [XAudio2 API 参考](https://msdn.microsoft.com/library/windows/desktop/ee415899.aspx).


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[igamecontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[igamecontroller.headset]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headset.aspx
[igamecontroller.headsetconnected]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headsetconnected.aspx
[igamecontroller.headsetdisconnected]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headsetdisconnected.aspx
[耳机]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.headset.aspx

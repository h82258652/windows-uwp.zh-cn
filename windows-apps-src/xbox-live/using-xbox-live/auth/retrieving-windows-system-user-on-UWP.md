---
title: 在 UWP 上检索 Windows 系统用户
author: KevinAsgari
description: 了解如何在通用 Windows 平台 (UWP) 游戏中检索 Windows 系统用户。
ms.author: kevinasg
ms.date: 06/07/2017
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, 系统用户
ms.localizationpriority: medium
ms.openlocfilehash: bd783c4e1614cc558f80472765a77138d1abf409
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2018
ms.locfileid: "5969522"
---
# <a name="retrieving-the-windows-system-user-in-a-universal-windows-platform-uwp-title"></a>在通用 Windows 平台 (UWP) 作品中检索 Windows 系统用户

## <a name="windowssystemuser"></a>Windows.System.User

一个 [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) 对象代表一个本地 Windows 用户。 在 Xbox 主机上，允许在单个交互会话中同时登录多个 windows 用户，如果你的应用为多用户应用程序，则需要使用 Windows.System.User 对象构建 XboxLiveUser 才能访问 Live 服务。 在诸如电脑或收集之类的其他 windows 平台上，在一台设备或一个交互会话上只允许一个 windows 用户，因此无需传递 Windows.System.User 来构建 XboxLiveUser。

## <a name="ways-to-retrieve-windows-system-user"></a>检索 Windows 系统用户的方式

* **使用 [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) 类中的静态方法。**

  [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) 类可提供一组静态方法，以帮助检索 Windows.System.User 对象。 例如，你可以通过调用 FindAllAsync 来获取所有活动的 windows 用户。

* **[UserPicker](https://docs.microsoft.com/en-us/uwp/api/windows.system.userpicker)**

  [Windows.System.UserPicker](https://docs.microsoft.com/en-us/uwp/api/windows.system.userpicker) 类提供用于启动用户选取器 UI 和在多用户情况下选择 windows 系统用户的方法。

* **[IGameController](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.igamecontroller)**

  [Windows.Gaming.Input.IGameController](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.igamecontroller) 是所有控制器设备（游戏板、赛车方向盘、飞行杆等）的核心接口。 你可以通过调用用户属性获取与游戏控制器关联的 Windows.System.User 对象。  

  以下是 windows [ArcadeStick](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.arcadestick)、[FlightStick](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.flightstick)、[Gamepad](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.gamepad)、[RacingWheel](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.racingwheel) 使用的双游戏控制器。

* **[UserDeviceAssociation](https://docs.microsoft.com/en-us/uwp/api/windows.system.userdeviceassociation)**

  你可以通过调用静态方法 FindUserFromDeviceId 来查找与设备 ID 关联的用户。你可以经常通过 Windows 输入事件来获取设备 ID，如 [Windows.UI.Xaml.Input.KeyRoutedEventArgs](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs)、[Windows.UI.Core.KeyEventArgs](https://docs.microsoft.com/en-us/uwp/api/windows.ui.core.keyeventargs)

---
author: mithom
title: "游戏输入"
description: "本节演示了如何使用游戏板和通用 Windows 平台 (UWP) 游戏的其他输入设备。"
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 游戏, 输入"
ms.openlocfilehash: ee6017648974d5283f59708550092f26d88388ce
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="input-for-games"></a>游戏输入

本部分描述了可在 Windows 10 和 Xbox One 的通用 Windows 平台 (UWP) 游戏中使用的不同类型的输入设备，演示了它们的基本用法，并推荐了在游戏中实现有效输入编程的模式和技术。

> **注意**    存在可在 UWP 游戏中使用的其他类型的输入设备，例如，可能特定于流派或特定于游戏的自定义输入设备。 本部分不讨论此类设备及其编程。 有关用于加快自定义输入设备的接口的信息，请参阅 [Windows.Gaming.Input.Custom][] 命名空间。

## <a name="gaming-input-devices"></a>游戏输入设备

游戏输入设备在 Windows 10 和 Xbox One 的 UWP 游戏和应用中受 [Windows.Gaming.Input][] 命名空间支持。

### <a name="gamepads"></a>游戏板

游戏板是 Xbox One 上的标准输入设备，不喜欢用键盘和鼠标的 Windows 游戏玩家通常会选择这些设备。 它们可提供各种数字和模拟控件使其适用于几乎任何类型的游戏，还可以通过嵌入震动控件提供触觉反馈。

有关如何在 UWP 游戏中使用游戏板的信息，请参阅[游戏板和震动](gamepad-and-vibration.md)。

### <a name="arcade-sticks"></a>街机摇杆

街机摇杆是对于重现独立街机的感觉而言尤为重要的全数字输入设备，也是进行肉搏战和其他街机风格游戏的完美输入设备。

有关如何在 UWP 游戏中使用街机摇杆的信息，请参阅[街机摇杆](arcade-stick.md)。

### <a name="racing-wheels"></a>赛车方向盘

赛车方向盘是与真实的赛车驾驶舱外观类似的输入设备，对以汽车或卡车为特征的赛车游戏而言也是完美的输入设备。 很多赛车方向盘都配备有真实的力回馈--即，它们可以将实际用力应用于控制轴（如方向盘），而不只是简单的震动。

有关如何在 UWP 游戏中使用赛车方向盘的信息，请参阅[赛车方向盘和力回馈](racing-wheel-and-force-feedback.md)。

### <a name="ui-navigation-controller"></a>UI 导航控制器

UI 导航控制器是为提供 UI 导航命令的常用词汇而存在的逻辑输入设备，这些命令可在不同的游戏和物理输入设备间推行一致的用户体验。 游戏的用户界面应使用 UINavigationController 界面，而非特定于设备的界面。

有关如何在 UWP 游戏中使用 UI 导航控制器的信息，请参阅 [UI 导航控制器](ui-navigation-controller.md)。

### <a name="headsets"></a>耳机

耳机是音频捕获和播放设备，在通过其输入设备连接时与特定用户关联。 耳机通常在联机游戏中用于语音聊天，但也可以在联机和脱机游戏中用于加强沉浸或提供游戏玩法功能。

有关如何在 UWP 游戏中使用耳机的信息，请参阅[耳机](headset.md)。

### <a name="users"></a>用户

每个输入设备及其连接的耳机都可以与特定用户关联，以将其身份与游戏玩法相关联。 物理输入设备中的输入与其逻辑 UI 导航控制器中的输入也是通过用户身份这一方式关联的。

有关如何管理用户及其输入设备的信息，请参阅[跟踪用户及其设备](input-practices-for-games.md#tracking-users-and-their-devices)。

## <a name="see-also"></a>另请参阅
[Windows.Gaming.Input.Custom][]


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.Custom]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.gaming.input.custom.aspx

---
author: Karl-Bridge-Microsoft
Description: Optimize your app for input from Xbox gamepad and remote control.
title: 游戏板和遥控器交互
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 022724064ad1e7f5551b6676bf256ca5cf6e4b8e
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2019
ms.locfileid: "9048554"
---
# <a name="gamepad-and-remote-control-interactions"></a>游戏板和遥控器交互

![键盘和游戏板图像](images/keyboard/keyboard-gamepad.jpg)

***游戏板、 遥控器和键盘之间共享常见的交互模式***

确保你的应用适用于游戏板和遥控器是针对 10 英尺体验进行优化中最重要的步骤。 可以进行多个特定于游戏板和遥控器的改进，以优化用户操作在某种程度上受限的设备上的用户交互体验。

通用 Windows 平台 (UWP) 应用现在支持游戏板和遥控器输入。 

游戏板和遥控器是 Xbox 和电视体验的主要输入设备。 

UWP 应用应针对这些输入设备类型进行优化，就像针对电脑上的键盘和鼠标以及手机或平板电脑上的触摸输入进行优化一样。 

确保你的应用适用于这些输入设备是针对 Xbox 和电视进行优化时最重要的步骤。

你现在可以为电脑上的 UWP 应用插入并使用游戏板，这使验证工作变得轻松。

若要使你的 UWP 应用在使用游戏板或遥控器时提供成功且愉快的用户体验，你应考虑以下事项：

* [硬件按钮](../devices/designing-for-tv.md#hardware-buttons) - 手柄和遥控器提供截然不同的按钮和配置。

* [XY 焦点导航和交互](../devices/designing-for-tv.md#xy-focus-navigation-and-interaction) - XY 焦点导航使用户可以在应用 UI 中四处导航。

* [鼠标模式](../devices/designing-for-tv.md#mouse-mode) - 当 XY 焦点导航有所不足时，鼠标模式可使你的应用模拟鼠标体验。

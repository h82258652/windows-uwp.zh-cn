---
author: mijacobs
Description: TODO
title: 手柄和遥控器交互
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3713d74edd93f437726c04dd68b604cb8a22da8f
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
ms.locfileid: "1392426"
---
# <a name="gamepad-and-remote-control-interactions"></a>游戏板和遥控器交互

通用 Windows 平台 (UWP) 应用现在支持游戏板和遥控器输入。 游戏板和遥控器是 Xbox 和电视体验的主要输入设备。 UWP 应用应针对这些输入设备类型进行优化，就像针对电脑上的键盘和鼠标以及手机或平板电脑上的触摸输入进行优化一样。 确保你的应用适用于这些输入设备是针对 Xbox 和电视进行优化时最重要的步骤。
你现在可以为电脑上的 UWP 应用插入并使用游戏板，这使验证工作变得轻松。

若要使你的 UWP 应用在使用游戏板或遥控器时提供成功且愉快的用户体验，你应考虑以下事项：

* [硬件按钮](../devices/designing-for-tv.md#hardware-buttons) - 手柄和遥控器提供截然不同的按钮和配置。

* [XY 焦点导航和交互](../devices/designing-for-tv.md#xy-focus-navigation-and-interaction) - XY 焦点导航使用户可以在应用 UI 中四处导航。

* [鼠标模式](../devices/designing-for-tv.md#mouse-mode) - 当 XY 焦点导航有所不足时，鼠标模式可使你的应用模拟鼠标体验。

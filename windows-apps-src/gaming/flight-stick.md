---
author: eliotcowley
title: 飞行杆
description: 使用 Windows.Gaming.Input 飞行杆 API 可从飞行杆读取输入。
ms.assetid: DC633F6B-FDC9-4D6E-8401-305861F31192
ms.author: wdg-dev-content
ms.date: 03/06/2017
ms.topic: article
keywords: Windows 10, uwp, 游戏, 输入, 飞行杆
ms.localizationpriority: medium
ms.openlocfilehash: ebe7695b3f16271f3adedae658c0d62d38d7c078
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6027056"
---
# <a name="flight-stick"></a>飞行杆

此页介绍使用 [Windows.Gaming.Input.FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) 和通用 Windows 平台 (UWP) 的相关 API 进行 Xbox One 认证飞行杆编程的基本知识。

在本页中，你将了解如下内容：

* 如何收集相连飞行杆及其用户的列表
* 如何检测是否已添加或删除某飞行杆
* 如何读取一个或多个飞行杆的输入
* 如何像 UI 导航设备一样使用飞行杆

## <a name="overview"></a>概述

飞行杆是游戏输入设备，对于重现飞机或宇宙飞船驾驶舱中的飞行杆感觉特别重要。 它们是用于快速精确地控制飞行的完美输入设备。 飞行杆在 Windows 10 和 Xbox One UWP 应用中受 [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) 命名空间支持。

Xbox One 飞行杆配有以下控件：

* 一个能够滚动、倾斜和旋转的可扭曲模拟游戏杆
* 一个模拟油门
* 两个射击按钮
* 一个 8 向数字顶帽控制器
* **视图**和**菜单**按钮

> [!NOTE]
> **视图**和**菜单**按钮用于支持 UI 导航而不是游戏命令，因此无法如同游戏杆按钮一样方便地访问。

### <a name="ui-navigation"></a>UI 导航

为了减轻对用户界面导航提供不同输入设备支持的负担并促进游戏和设备之间的一致性，大多_物理_输入设备同时会充当单独的被称为 [UI 导航控制器](ui-navigation-controller.md)的_逻辑_输入设备。 UI 导航控制器可跨各种输入设备提供通用的 UI 导航命令词汇。

作为 UI 导航控制器，飞行杆将导航命令的[必需集](ui-navigation-controller.md#required-set)映射到游戏杆以及**视图**、**菜单**、**主射击**和**辅助射击**按钮。

| 导航命令 | 飞行杆输入                  |
| ------------------:| ----------------------------------- |
|                 向上 | 游戏杆向上                         |
|               向下 | 游戏杆向下                       |
|               向左 | 游戏杆向左                       |
|              向右 | 游戏杆向右                      |
|               视图 | **视图**按钮                     |
|               菜单 | **菜单**按钮                     |
|             接受 | **主射击**按钮              |
|             取消 | **辅助射击**按钮            |

飞行杆不会映射导航命令的任何[可选集](ui-navigation-controller.md#optional-set)。

## <a name="detect-and-track-flight-sticks"></a>检测和跟踪飞行杆

检测与跟踪飞行杆的工作原理与检测与跟踪游戏板完全相同，除非使用 [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) 类而不是 [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad) 类。 有关详细信息，请参阅[游戏板和振动](gamepad-and-vibration.md)。

<!-- Flight sticks are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected flight sticks and events to notify you when a flight stick is added or removed.

### The flight stick list

The [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) class provides a static property, [FlightSticks](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightSticks), which is a read-only list of flight sticks that are currently connected. Because you might only be interested in some of the connected flight sticks, we recommend that you maintain your own collection instead of accessing them through the `FlightSticks` property.

The following example copies all connected flight sticks into a new collection:

```cpp
auto myFlightSticks = ref new Vector<FlightStick^>();

for (auto flightStick : FlightStick::FlightSticks)
{
    // This code assumes that you're interested in all flight sticks.
    myFlightSticks->Append(flightStick);
}
```

### Adding and removing flight sticks

When a flight stick is added or removed, the [FlightStickAdded](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickAdded) and [FlightStickRemoved](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickRemoved) events are raised. You can register handlers for these events to keep track of the flight sticks that are currently connected.

The following example starts tracking a flight stick that's been added:

```cpp
FlightStick::FlightStickAdded += 
    ref new EventHandler<FlightStick^>([] (Platform::Object^, FlightStick^ args)
{
    // This code assumes that you're interested in all new flight sticks.
    myFlightSticks->Append(args);
});
```

The following example stops tracking a flight stick that's been removed:

```cpp
FlightStick::FlightStickRemoved += 
    ref new EventHandler<FlightStick^>([] (Platform::Object^, FlightStick^ args)
{
    unsigned int indexRemoved;

    if (myFlightSticks->IndexOf(args, &indexRemoved))
    {
        myFlightSticks->RemoveAt(indexRemoved);
    }
});
```

### Users and headsets

Each flight stick can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-flight-stick"></a>读取飞行杆

确定感兴趣的飞行杆之后，便可以从飞行杆收集输入了。 但是，与你可能已习惯的某些其他输入类型不同，飞行杆不会通过触发事件来表达状态更改。 相反，你需要通过对它们进行“轮询”__ 来定期读取其当前状态。

### <a name="polling-the-flight-stick"></a>轮询飞行杆

轮询会捕获飞行杆在某个精确时间点的快照。 此输入收集方法对于大多数游戏都非常适合，因为其逻辑通常按确定的循环运行，而不是受事件驱动。 而且，相对于随着时间逐个收集许多单个输入解释游戏命令，从一次性收集的输入解释游戏命令，通常会更为简单。

可通过调用 [FlightStick.GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick.GetCurrentReading) 来轮询飞行杆。 此函数返回 [FlightStickReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading)，其中包含飞行杆的状态。

下面是轮询飞行杆来获取其当前状态的示例：

```cpp
auto flightStick = myFlightSticks->GetAt(0);
FlightStickReading reading = flightStick->GetCurrentReading();
```

除了飞行杆状态之外，每个读数还包括一个时间戳，准确指出检索状态的时间。 该时间戳对于关联之前读数的时间或者游戏模拟的时间非常有用。

### <a name="reading-the-joystick-and-throttle-input"></a>读取游戏杆和油门输入

游戏杆在 X、Y 和 Z 轴（分别对应滚动、倾斜和旋转）上提供介于 -1.0 与 1.0 之间的模拟读数。 对于滚动，值 -1.0 对应于游戏杆最左位置，而值 1.0 对应于最右位置。 对于倾斜，值 -1.0 对应于游戏杆杆最下面的位置，而值 1.0 对应于最上面的位置。 对于旋转，值 -1.0 对应于逆时针反向到头的旋转位置，而值 1.0 对应于顺时针方向到头的旋转位置。

在所有轴中，当游戏杆处于中心位置时，值大约是 0.0，不过精确值有所不同是正常情况，即使是在后续读数之间。 本部分后面讨论了缓解此差异的策略。

游戏杆滚动值从 [FlightStickReading.Roll](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Roll) 属性进行读取，倾斜值从 [FlightStickReading.Pitch](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Pitch) 属性进行读取，而旋转值从 [FlightStickReading.Yaw](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Yaw) 属性进行读取：

```cpp
// Each variable will contain a value between -1.0 and 1.0.
float roll = reading.Roll;
float pitch = reading.Pitch;
float yaw = reading.Yaw;
```

读取游戏杆的值时，你会注意到，当游戏杆处于中心位置静止时，它们不会稳定地生成中性读数 0.0；而是每次移动游戏杆并返回到中心位置时，才会生成不同的接近 0.0 的值。 要减小这些误差，你可以使用小“死区”__（一系列被忽略的接近理想中心位置的值）。

使用死区的一种方法是，确定游戏杆被移动远离中心的距离，并忽略比你选择的某些距离更近的读数。 你可以使用勾股定理粗略计算这一距离，该值并不精确，因为游戏杆读数实际上是极值，不是平面值。 这会生成一个径向死区。

下面示例演示如何使用勾股定理计算基本径向死区：

```cpp
// Choose a deadzone. Readings inside this radius are ignored.
const float deadzoneRadius = 0.1f;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem: For a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
float oppositeSquared = pitch * pitch;
float adjacentSquared = roll * roll;

// Accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) < deadzoneSquared)
{
    // Input accepted, process it.
}
```

### <a name="reading-the-buttons-and-hat-switch"></a>读取按钮和顶帽控制器

每个飞行杆的两个射击按钮可提供指示它是按下（向下）还是释放（向上）的数字读数。 为了提高效率，按钮读数不以单独的布尔值表示；而是全部打包到一个由 [FlightStickButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickbuttons) 枚举表示的单独位域中。 此外，8 向顶帽控制器提供打包到由 [GameControllerSwitchPosition](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerswitchposition) 枚举表示的单独位域中的方向。

> [!NOTE]
> 飞行杆配备了用于 UI 导航的其他按钮，例如**视图**和**菜单**按钮。 这些按钮不是 `FlightStickButtons` 枚举的一部分，只能作为 UI 导航设备通过访问飞行杆进行读取。 有关详细信息，请参阅 [UI 导航控制器](ui-navigation-controller.md)。

按钮值从 [FlightStickReading.Buttons](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Buttons) 属性进行读取。 因为此属性是位域，所以使用按位掩码隔离你感兴趣的按钮值。 设置相应位时按钮为按下（向下）；否则，按钮为释放（向上）。

以下示例确定是否按下**主射击**按钮：

```cpp
if (FlightStickButtons::FirePrimary == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is pressed.
}
```

以下示例确定是否释放**主射击**按钮：

```cpp
if (FlightStickButtons::None == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is released (not pressed).
}
```

有时你可能需要确定：何时将按钮从按下转换为释放或从释放转换为按下，是按下还是释放多个按钮，或者是否按特定方式安排一组按钮（按下一些按钮，释放一些按钮）。 有关如何检测这些条件的详细信息，请参阅[检测按钮转换](input-practices-for-games.md#detecting-button-transitions)和[检测复杂按钮安排](input-practices-for-games.md#detecting-complex-button-arrangements)。

顶帽控制器值从 [FlightStickReading.HatSwitch](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.HatSwitch) 属性进行读取。 因为此属性也是位域，所以再次使用按位掩码隔离顶帽控制器的位置。

以下示例确定顶帽控制器是否处于向上位置：

```cpp
if (GameControllerSwitchPosition::Up == (reading.HatSwitch & GameControllerSwitchPosition::Up))
{
    // The hat switch is in the up position.
}
```

以下示例确定顶帽控制器是否处于中心位置：

```cpp
if (GameControllerSwitchPosition::Center == (reading.HatSwitch & GameControllerSwitchPosition::Center))
{
    // The hat switch is in the center position.
}
```

<!--## Run the InputInterfacingUWP sample

The [InputInterfacingUWP sample _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) demonstrates how to use flight sticks and different kinds of input devices in tandem, as well as how these input devices behave as UI navigation controllers.-->

## <a name="see-also"></a>另请参阅

* [Windows.Gaming.Input.UINavigationController 类](https://docs.microsoft.com/uwp/api/windows.gaming.input.uinavigationcontroller)
* [Windows.Gaming.Input.IGameController 接口](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [游戏输入实践](input-practices-for-games.md)
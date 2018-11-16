---
author: eliotcowley
title: 原始游戏控制器
description: 使用 Windows.Gaming.Input 原始游戏控制器 API 可从几乎所有类型的游戏控制器读取输入。
ms.assetid: 2A466C16-1F51-4D8D-AD13-704B6D3C7BEC
ms.author: wdg-dev-content
ms.date: 03/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 输入, 原始游戏控制器
ms.localizationpriority: medium
ms.openlocfilehash: c57db3f9604e20d0dc83d6c3cf2ced87b1f5dcc1
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "6995260"
---
# <a name="raw-game-controller"></a>原始游戏控制器

本页介绍使用 [Windows.Gaming.Input.RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) 和通用 Windows 平台 (UWP) 的相关 API 进行几乎任何类型的游戏控制器编程的基础知识。

在本页中，你将了解如下内容：

* 如何收集相连原始游戏控制器及其用户的列表
* 如何检是否已添加或删除某个原始游戏控制器
* 如何获取原始游戏控制器的功能
* 如何从原始游戏控制器读取输入

## <a name="overview"></a>概述

原始游戏控制器是游戏控制器的通用表示形式，具有许多不同种类的常见游戏控制器上的输入。 这些输入作为未命名按钮、开关和轴的简单数组进行公开。 使用原始游戏控制器，可以使客户能够创建自定义输入映射（无论使用何种类型的控制器）。

[RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) 类实际上适用于其他输入类（[ArcadeStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick)、[FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) 等）不满足需求时方案 &mdash; 如果你需要更加通用的内容（预计客户会使用许多不同类型的游戏控制器），则此类适合你。

## <a name="detect-and-track-raw-game-controllers"></a>检测并跟踪原始游戏控制器

检测与跟踪原始游戏控制器的工作原理与检测与跟踪游戏板完全相同，除非使用 [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) 类而不是 [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad) 类。 有关详细信息，请参阅[游戏板和振动](gamepad-and-vibration.md)。

<!-- Raw game controllers are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected raw game controllers and events to notify you when a raw game controller is added or removed.

### The raw game controller list

The [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) class provides a static property, [RawGameControllers](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllers), which is a read-only list of raw game controllers that are currently connected. Because you might only be interested in some of the connected raw game controllers, we recommend that you maintain your own collection instead of accessing them through the **RawGameControllers** property.

The following example copies all connected raw game controllers into a new collection:

```cpp
auto myRawGameControllers = ref new Vector<RawGameController^>();

for (auto rawGameController : RawGameController::RawGameControllers)
{
    // This code assumes that you're interested in all raw game controllers.
    myRawGameControllers->Append(rawGameController);
}
```

### Adding and removing raw game controllers

When a raw game controller is added or removed, the [RawGameControllerAdded](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerAdded) and [RawGameControllerRemoved](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerRemoved) events are raised. You can register handlers for these events to keep track of the raw game controllers that are currently connected.

The following example starts tracking a raw game controller that's been added:

```cpp
RawGameController::RawGameControllerAdded += 
    ref new EventHandler<RawGameController^>(
        [] (Platform::Object^, RawGameController^ args)
{
    // This code assumes that you're interested in all new raw game controllers.
    myRawGameControllers->Append(args);
});
```

The following example stops tracking a raw game controller that's been removed:

```cpp
RawGameController::RawGameControllerRemoved +=
    ref new EventHandler<RawGameController^>(
        [] (Platform::Object^, RawGameController^ args)
{
    unsigned int indexRemoved;

    if (myRawGameControllers->IndexOf(args, &indexRemoved))
    {
        myRawGameControllers->RemoveAt(indexRemoved);
    }
});
```

### Users and headsets

Each raw game controller can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="get-the-capabilities-of-a-raw-game-controller"></a>获取原始游戏控制器的功能

确定感兴趣的原始游戏控制器之后，可以收集有关控制器的功能的信息。 可以使用 [RawGameController.ButtonCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.ButtonCount) 获取原始游戏控制器上的按钮数、使用 [RawGameController.AxisCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.AxisCount) 获取模拟轴数以及使用 [RawGameController.SwitchCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.SwitchCount) 获取开关数。 此外，可以使用 [RawGameController.GetSwitchKind](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_) 获取开关的类型。

下面的示例获取原始游戏控制器的输入计数：

```cpp
auto rawGameController = myRawGameControllers->GetAt(0);
int buttonCount = rawGameController->ButtonCount;
int axisCount = rawGameController->AxisCount;
int switchCount = rawGameController->SwitchCount;
```

下面的示例确定每个开关的类型：

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    GameControllerSwitchKind mySwitch = rawGameController->GetSwitchKind(i);
}
```

## <a name="reading-the-raw-game-controller"></a>读取原始游戏控制器

了解原始游戏控制器上的输入数之后，便准备好从它收集输入。 不过，与你可能已习惯的某些其他输入类型不同，原始游戏控制器不会通过引发事件来传达状态的更改。 相反，你需要通过对它进行“轮询”__ 来定期读取其当前状态。

### <a name="polling-the-raw-game-controller"></a>轮询原始游戏控制器

轮询会捕获原始游戏控制器在某个精确时间点的快照。 此输入收集方法对于大多数游戏都非常适合，因为其逻辑通常按确定的循环运行，而不是受事件驱动。 而且，相对于随着时间逐个收集许多单个输入解释游戏命令，从一次性收集的输入解释游戏命令，通常会更为简单。

可通过调用 [RawGameController.GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetCurrentReading_System_Boolean___Windows_Gaming_Input_GameControllerSwitchPosition___System_Double___) 来轮询原始游戏控制器。 此函数填充包含原始游戏控制器状态的按钮、开关和轴数组。

下面是轮询原始游戏控制器来获取其当前状态的示例：

```cpp
Platform::Array<bool>^ currentButtonReading =
    ref new Platform::Array<bool>(buttonCount);

Platform::Array<GameControllerSwitchPosition>^ currentSwitchReading =
    ref new Platform::Array<GameControllerSwitchPosition>(switchCount);

Platform::Array<double>^ currentAxisReading = ref new Platform::Array<double>(axisCount);

rawGameController->GetCurrentReading(
    currentButtonReading,
    currentSwitchReading,
    currentAxisReading);
```

在不同类型的控制器间，不保证每个数组中的哪个位置会容纳哪个输入值，因此需要使用方法 [RawGameController.GetButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) 和 [RawGameController.GetSwitchKind](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_) 检查输入的具体内容。

**GetButtonLabel** 会告知在实际按钮上打印的文本或符号，而不是按钮的功能 &mdash; 因此，它最好在你要向玩家提供有关哪些按钮执行哪些功能的提示时，用作 UI 辅助。 **GetSwitchKind** 会告知开关的类型（即，它具有的位置数），但不告知名称。

没有标准化方法可用于获取轴或开关的标签，因此需要自己测试这些对象以确定输入的具体内容。

如果要支持特定控制器，则可以获取 [RawGameController.HardwareProductId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId) 和 [RawGameController.HardwareVendorId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId) 并检查它们是否与该控制器匹配。 每个数组中的每个输入的位置对于具有相同 **HardwareProductId** 和 **HardwareVendorId** 的每个控制器是相同的，因此不必担心逻辑可能在相同类型的不同控制器间不一致。

除了原始游戏控制器状态之外，每个读数还返回一个时间戳，确切指出检索状态的时间。 该时间戳对于关联之前读数的时间或者游戏模拟的时间非常有用。

### <a name="reading-the-buttons-and-switches"></a>读取按钮和开关

每个原始游戏控制器的按钮都提供指示它是按下（向下）还是释放（向上）的数字读数。 按钮读数表示为单个数组中的各个布尔值。 可以将 [RawGameController.GetButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) 与数组中的布尔值索引结合使用来查找每个按钮的标签。 每个值都表示为 [GameControllerButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel)。

以下示例确定是否按下 **XboxA** 按钮：

```cpp
for (uint32_t i = 0; i < buttonCount; i++)
{
    if (currentButtonReading[i])
    {
        GameControllerButtonLabel buttonLabel = rawGameController->GetButtonLabel(i);

        if (buttonLabel == GameControllerButtonLabel::XboxA)
        {
            // XboxA is pressed.
        }
    }
}
```

有时你可能需要确定：何时将按钮从按下转换为释放或从释放转换为按下，是按下还是释放多个按钮，或者是否按特定方式安排一组按钮（按下一些按钮，释放一些按钮）。 有关如何检测这些条件的详细信息，请参阅[检测按钮转换](input-practices-for-games.md#detecting-button-transitions)和[检测复杂按钮安排](input-practices-for-games.md#detecting-complex-button-arrangements)。

开关值作为 [GameControllerSwitchPosition](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerswitchposition) 的数组进行提供。 由于此属性为位域，因此使用按位掩码隔离开关的方向。

以下示例确定每个开关是否处于向上位置：

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    if (GameControllerSwitchPosition::Up ==
        (currentSwitchReading[i] & GameControllerSwitchPosition::Up))
    {
        // The switch is in the up position.
    }
}
```

### <a name="reading-the-analog-inputs-sticks-triggers-throttles-and-so-on"></a>读取模拟输入（摇杆、触发器、油门等）

模拟轴提供介于 0.0 与 1.0 之间的读数。 这包括摇杆上的每个维度，如用于标准摇杆 X 和 Y，甚至是用于飞行杆的 X、Y 和 Z 轴（分别是滚动、倾斜和旋转）。

值可以表示模拟触发器、油门或任何其他类型的模拟输入。 这些值不随标签一起提供，因此建议使用各种输入设备测试代码，以确保它们正确符合假设。

在所有轴中，当摇杆位于中心位置时，值接近于 0.5，但即使在后续读数之间，精确的值通常也会有所差异；本节后面会讨论减小此误差的策略。

下面的示例演示如何从 Xbox 控制器读取模拟值：

```cpp
// Xbox controllers have 6 axes: 2 for each stick and one for each trigger.
float leftStickX = currentAxisReading[0];
float leftStickY = currentAxisReading[1];
float rightStickX = currentAxisReading[2];
float rightStickY = currentAxisReading[3];
float leftTrigger = currentAxisReading[4];
float rightTrigger = currentAxisReading[5];
```

读取摇杆值时，你会注意到，当摇杆处于中心位置静止时，它们不会稳定地生成中性读数 0.5；而是每次移动摇杆并返回到中心位置时，才会生成不同的接近 0.5 的值。 要减小这些误差，你可以使用小“死区”__（一系列被忽略的接近理想中心位置的值）。

使用死区的一种方法是，确定摇杆被移动远离中心的距离，并忽略比你选择的某些距离更近的读数。 你可以使用勾股定理粗略计算这一距离，该值并不精确，因为摇杆读数实际上是极值，不是平面值。 这会生成一个径向死区。

下面示例演示如何使用勾股定理计算基本径向死区：

```cpp
// Choose a deadzone. Readings inside this radius are ignored.
const float deadzoneRadius = 0.1f;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem: For a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
float oppositeSquared = leftStickY * leftStickY;
float adjacentSquared = leftStickX * leftStickX;

// Accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) < deadzoneSquared)
{
    // Input accepted, process it.
}
```

<!--## Run the RawGameControllerUWP sample

The [RawGameControllerUWP sample (GitHub)](TODO: Link) demonstrates how to use raw game controllers. TODO: More information-->

## <a name="see-also"></a>另请参阅

* [游戏输入](input-for-games.md)
* [游戏输入实践](input-practices-for-games.md)
* [Windows.Gaming.Input 命名空间](https://docs.microsoft.com/uwp/api/windows.gaming.input)
* [Windows.Gaming.Input.RawGameController 类](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller)
* [Windows.Gaming.Input.IGameController 接口](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Windows.Gaming.Input.IGameControllerBatteryInfo 接口](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo)
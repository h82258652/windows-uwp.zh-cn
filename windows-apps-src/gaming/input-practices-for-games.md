---
author: eliotcowley
title: 游戏输入实践
description: 了解有效使用输入设备的模式和技术。
ms.assetid: CBAD3345-3333-4924-B6D8-705279F52676
ms.author: elcowle
ms.date: 11/20/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 输入
ms.localizationpriority: medium
ms.openlocfilehash: ed0d611c761315e42decb89e1a5a5ad84f4b067a
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7307205"
---
# <a name="input-practices-for-games"></a>游戏输入实践

本页描述了有效使用通用 Windows 平台 (UWP) 游戏中的输入设备的模式和技术。

在本页中，你将了解如下内容：

* 如何跟踪玩家和玩家当前正在使用的输入和导航设备
* 如何检测按钮转换（按下到释放，释放到按下）
* 如何使用一个测试检测复杂的按钮排列

## <a name="choosing-an-input-device-class"></a>选择输入设备类

可使用许多不同类型的输入 API，如 [ArcadeStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick)、[FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) 和 [Gamepad](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad)。 如何确定要用于游戏的 API？

应选择可提供最适合于游戏的输入的 API。 例如，如果在创建 2D 平台游戏，则可能只需使用 **Gamepad** 类，不会为可通过其他类提供的额外功能而费心。 这会将游戏限制为仅支持游戏板，并提供可适用于许多不同游戏板而无需额外代码的一致接口。

另一方面，对于复杂的飞行和赛车模拟，可能要枚举所有 [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) 对象作为基线，以确保它们支持发烧友玩家可能拥有的任何专业设备，包括诸如仍由单个玩家所使用的单独踏板或油门等设备。 

在其中可以使用输入类的 **FromGameController** 方法（如 [Gamepad.FromGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.fromgamecontroller)）查看每个设备是否具有更精选的视图。 例如，如果设备也是**游戏板**，则可能要调整按钮映射 UI 以反映这种情况，并提供某些合理的默认按钮映射以进行选择。 （这与仅使用 **RawGameController** 时需要玩家手动配置游戏板的情况相反。） 

或者，可以查看 **RawGameController** 的供应商 ID (VID) 和产品 ID (PID)（分别使用 [HardwareVendorId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId) 和 [HardwareProductId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId)），并为常见设备提供建议的按钮映射，同时仍通过玩家的手动映射来保持与将来推出的未知设备兼容。

## <a name="keeping-track-of-connected-controllers"></a>跟踪连接的控制器

虽然每个控制器类型均包含连接控制器的列表（如 [Gamepad.Gamepads](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.Gamepads)），但最好维护你自己的控制器列表。 请参阅[游戏板列表](gamepad-and-vibration.md#the-gamepads-list)了解详细信息（每个控制器类型均有一个有关其自己主题的名称类似的部分）。

但是，在玩家拔掉控制器或插入新控制器时会发生什么？ 你需要处理这些事件，并相应更新列表。 请参阅[添加和删除游戏板](gamepad-and-vibration.md#adding-and-removing-gamepads)了解详细信息（同样，每个控制器类型均有一个有关其自己主题的名称类似的部分）。

由于添加和删除的事件异步引发，因此在处理控制器列表时可能会收到错误结果。 因此，不论何时访问控制器列表，都应在列表周围放置锁定，以便一次只有一个线程可以访问它。 这可以通过[并发运行时](https://docs.microsoft.com/cpp/parallel/concrt/concurrency-runtime)实现，特别是 **&lt;ppl.h&gt;** 中的 [critical_section 类](https://docs.microsoft.com/cpp/parallel/concrt/reference/critical-section-class)。

另一个需要考虑的事项是连接的控制器列表最初是空的，需要一到两秒时间填充。 因此，如果你仅在 Start 方法中分配当前游戏板，它将为 **null**！

若要解决此问题，你应该有“刷新”主游戏板的方法（单人游戏中；多人游戏需要更复杂的解决方案）。 然后，应在添加控制器和删除控制器事件处理程序中，或使用更新方法来调用此方法。

以下方法只返回列表中的第一个游戏板（或者如果列表为空返回 **nullptr**）。 然后，当你在任何时间执行任何操作时，只需记住检查 **nullptr** 即可。 它由你在未连接控制器时是要阻止游戏（例如，通过暂停游戏）还是让游戏继续来决定，同时将忽略输入。

```cpp
#include <ppl.h>

using namespace Platform::Collections;
using namespace Windows::Gaming::Input;
using namespace concurrency;

Vector<Gamepad^>^ m_myGamepads = ref new Vector<Gamepad^>();

Gamepad^ GetFirstGamepad()
{
    Gamepad^ gamepad = nullptr;
    critical_section::scoped_lock{ m_lock };

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(0);
    }

    return gamepad;
}
```

组织到一起，下面是如何处理通过游戏板输入的示例：

```cpp
#include <algorithm>
#include <ppl.h>

using namespace Platform::Collections;
using namespace Windows::Foundation;
using namespace Windows::Gaming::Input;
using namespace concurrency;

static Vector<Gamepad^>^ m_myGamepads = ref new Vector<Gamepad^>();
static Gamepad^          m_gamepad = nullptr;
static critical_section  m_lock{};

void Start()
{
    // Register for gamepad added and removed events.
    Gamepad::GamepadAdded += ref new EventHandler<Gamepad^>(&OnGamepadAdded);
    Gamepad::GamepadRemoved += ref new EventHandler<Gamepad^>(&OnGamepadRemoved);

    // Add connected gamepads to m_myGamepads.
    for (auto gamepad : Gamepad::Gamepads)
    {
        OnGamepadAdded(nullptr, gamepad);
    }
}

void Update()
{
    // Update the current gamepad if necessary.
    if (m_gamepad == nullptr)
    {
        auto gamepad = GetFirstGamepad();

        if (m_gamepad != gamepad)
        {
            m_gamepad = gamepad;
        }
    }

    if (m_gamepad != nullptr)
    {
        // Gather gamepad reading.
    }
}

// Get the first gamepad in the list.
Gamepad^ GetFirstGamepad()
{
    Gamepad^ gamepad = nullptr;
    critical_section::scoped_lock{ m_lock };

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(0);
    }

    return gamepad;
}

void OnGamepadAdded(Platform::Object^ sender, Gamepad^ args)
{
    // Check if the just-added gamepad is already in m_myGamepads; if it isn't, 
    // add it.
    critical_section::scoped_lock lock{ m_lock };
    auto it = std::find(begin(m_myGamepads), end(m_myGamepads), args);

    if (it == end(m_myGamepads))
    {
        m_myGamepads->Append(args);
    }
}

void OnGamepadRemoved(Platform::Object^ sender, Gamepad^ args)
{
    // Remove the gamepad that was just disconnected from m_myGamepads.
    unsigned int indexRemoved;
    critical_section::scoped_lock lock{ m_lock };

    if (m_myGamepads->IndexOf(args, &indexRemoved))
    {
        if (m_gamepad == m_myGamepads->GetAt(indexRemoved))
        {
            m_gamepad = nullptr;
        }

        m_myGamepads->RemoveAt(indexRemoved);
    }
}
```

## <a name="tracking-users-and-their-devices"></a>跟踪用户及其设备

所有输入设备都与一位[用户](https://docs.microsoft.com/uwp/api/windows.system.user)关联，以便可以将其身份与其游戏玩法、成就、设置更改和其他活动关联。 用户可以随意登录或注销，而且，其他用户在之前的用户注销后登录到与系统保持相连的设备也很常见。用户登录或注销时会引发 [IGameController.UserChanged](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller.UserChanged) 事件。 你可以为此事件注册事件处理程序，以跟踪玩家和他们正在使用的设备。

输入设备与其相应的 [UI 导航控制器](ui-navigation-controller.md)也是通过用户身份这一方式关联的。

因此，应该使用设备类（继承自 [IGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller) 接口）的 [User](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller.User) 属性跟踪和关联玩家。

[UserGamepadPairingUWP (GitHub)](
https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/UserGamepadPairingUWP) 示例演示了如何跟踪用户和他们正在使用的设备。

## <a name="detecting-button-transitions"></a>检测按钮转换

有时你可能需要了解何时第一次按下或释放按钮；即，按钮状态从释放转换为按下或从按下转化为释放的准确时间。 要确定准确时间，你需要记住之前的设备读数并将当前的读数与之比较，以查看发生了哪些变化。

以下示例演示了记住之前读数的基本方法；游戏板如下所示，但街机摇杆、赛车方向盘和其他输入设备类型的原则相同。

```cpp
Gamepad gamepad;
GamepadReading newReading();
GamepadReading oldReading();

// Called at the start of the game.
void Game::Start()
{
    gamepad = Gamepad::Gamepads[0];
}

// Game::Loop represents one iteration of a typical game loop
void Game::Loop()
{
    // move previous newReading into oldReading before getting next newReading
    oldReading = newReading, newReading = gamepad.GetCurrentReading();

    // process device readings using buttonJustPressed/buttonJustReleased (see below)
}
```

执行任何操作之前，`Game::Loop` 会将 `newReading` 的现有值（之前的循环迭代中的游戏板读数）移动到 `oldReading` 中，然后用当前迭代的全新游戏板读数填充 `newReading`。 可以为你提供检测按钮转换所需的信息。

以下示例演示了检测按钮转换的基本方法：

```cpp
bool ButtonJustPressed(const GamepadButtons selection)
{
    bool newSelectionPressed = (selection == (newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (oldReading.Buttons & selection));

    return newSelectionPressed && !oldSelectionPressed;
}

bool ButtonJustReleased(GamepadButtons selection)
{
    bool newSelectionReleased =
        (GamepadButtons.None == (newReading.Buttons & selection));

    bool oldSelectionReleased =
        (GamepadButtons.None == (oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

这两个功能先从 `newReading` 和 `oldReading` 中派生按钮选择的布尔状态，然后执行布尔逻辑以确定是否已出现目标转换。 仅当新读数包含目标状态（分别为按下或释放）*且*旧读数不包含目标状态时，这些功能返回 **true**，否则返回 **false**。

## <a name="detecting-complex-button-arrangements"></a>检测复杂的按钮排列

每个输入设备按钮均提供指示是按下（向下）还是释放（向上）的数字读数。 为了提高效率，按钮读数不以单独的布尔值表示；而是全部打包到一个由特定于设备的枚举（例如 [GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons)）表示的位域中。 若要读取特定按钮，使用按位掩码隔离你感兴趣的值。 设置相应位时按钮为按下（向下）；否则，按钮为释放（向上）。

回想确定按下或释放按钮的方式；游戏板如下所示，但街机摇杆、赛车方向盘和其他输入设备类型的原则相同。

```cpp
GamepadReading reading = gamepad.GetCurrentReading();

// Determines whether gamepad button A is pressed.
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // The A button is pressed.
}

// Determines whether gamepad button A is released.
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // The A button is released (not pressed).
}
```

如你所见，确定单个按钮的状态很简单，但有时你可能需要确定：是按下还是释放多个按钮，或者是否按特定方式安排一组按钮（按下一些按钮，释放一些按钮）。 测试多个按钮比测试单个按钮更复杂，特别是可能存在混合按钮状态，但是对于这些适用于单个和多个相似的按钮测试有一个简单的公式。

以下示例确定是否同时按下游戏板按钮 A 和 B：

```cpp
if ((GamepadButtons::A | GamepadButtons::B) == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both pressed.
}
```

以下示例确定是否同时释放游戏板按钮 A 和 B：

```cpp
if ((GamepadButtons::None == (reading.Buttons & GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both released (not pressed).
}
```

以下示例确定是否在释放按钮 B 时按下按钮 A：

```cpp
if (GamepadButtons::A == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

这五个示例共同拥有的公式是：由相等运算符左侧的表达式指定要测试的按钮排列，而由右侧的掩码表达式选择要考虑的按钮。

通过重新编写之前的示例，以下示例更清楚地演示了此公式：

```cpp
auto buttonArrangement = GamepadButtons::A;
auto buttonSelection = (reading.Buttons & (GamepadButtons::A | GamepadButtons::B));

if (buttonArrangement == buttonSelection)
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

此公式可应用于在任意状态排列中测试任何按钮数。

## <a name="get-the-state-of-the-battery"></a>获取电池状态

对于实现 [IGameControllerBatteryInfo](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo) 界面的任何游戏控制器，你可以调用控制器实例上的 [TryGetBatteryReport](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo.TryGetBatteryReport) 来获取提供控制器中电池信息的 [BatteryReport](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport) 对象。 你可以获取诸如电池正在充电的速度 ([ChargeRateInMilliwatts](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.ChargeRateInMilliwatts))、新电池的估计能量容量 ([DesignCapacityInMilliwattHours](https://docs.microsoft.com/en-us/uwp/api/windows.devices.power.batteryreport.DesignCapacityInMilliwattHours)) 以及当前电池的完整充电电量 ([FullChargeCapacityInMilliwattHours](https://docs.microsoft.com/en-us/uwp/api/windows.devices.power.batteryreport.FullChargeCapacityInMilliwattHours)) 这样的属性。

对于支持详细电池报告的游戏控制器，你可以获取有关电池的这一信息及更多信息，在[获取电池信息](../devices-sensors/get-battery-info.md)中作了详细介绍。 但是，大部分游戏控制器不支持该电池报告级别，而是使用成本较低的硬件。 对于这些控制器，你需要记住以下注意事项：

* **ChargeRateInMilliwatts** 和 **DesignCapacityInMilliwattHours** 将始终为 **NULL**。

* 你可以通过计算 [RemainingCapacityInMilliwattHours](https://docs.microsoft.com/en-us/uwp/api/windows.devices.power.batteryreport.RemainingCapacityInMilliwattHours) / **FullChargeCapacityInMilliwattHours** 来获取电池电量百分比。 你应该忽略这些属性的值，而仅处理计算得出的百分比。

* 上一条中所述的百分比始终是以下值之一：

    * 100%（满）
    * 70%（中）
    * 40%（低）
    * 10%（临界）

如果你的代码根据剩余的电池使用时间百分比执行某些操作（如绘制 UI），请确保它与上面的值相符。 例如，如果你想要警告玩家控制器的电池电量不足，则在到达 10% 时执行此操作。

## <a name="see-also"></a>另请参阅

* [Windows.System.User 类](https://docs.microsoft.com/uwp/api/windows.system.user)
* [Windows.Gaming.Input.IGameController 接口](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Windows.Gaming.Input.GamepadButtons 枚举](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons)

---
author: mithom
title: "游戏输入实践"
description: "了解有效使用输入设备的模式和技术。"
ms.assetid: CBAD3345-3333-4924-B6D8-705279F52676
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 游戏, 输入"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 15d56a27ad914b258bb19b80b3498510d01105cd
ms.lasthandoff: 02/07/2017

---

# <a name="input-practices-for-games"></a>游戏输入实践

本页描述了有效使用通用 Windows 平台 (UWP) 游戏中的输入设备的模式和技术。

在本页中，你将了解如下内容：
* 如何跟踪玩家和玩家当前正在使用的输入和导航设备
* 如何检测按钮转换（按下到释放，释放到按下）
* 如何使用一个测试检测复杂的按钮排列

## <a name="tracking-users-and-their-devices"></a>跟踪用户及其设备

所有输入设备都与一位[用户][Windows.System.User]关联，以便可以将其身份与其游戏玩法、成就、设置更改和其他活动关联。 用户可以随意登录或注销，而且，其他用户在之前的用户注销后登录到与系统保持相连的设备也很常见。 用户登录或注销时会引发 [IGameController.UserChanged][] 事件。 你可以为此事件注册事件处理程序，以跟踪玩家和他们正在使用的设备。

输入设备与其相应的 UI 导航控制器也是通过用户身份这一方式关联的。

因此，应该使用设备类（继承自 [IGameController][] 接口）的 [User][igamecontroller.user] 属性跟踪和关联玩家。

[UserGamepadPairingUWP _(github)_](
https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/UserGamepadPairingUWP) 示例演示了如何跟踪用户和他们正在使用的设备。

## <a name="detecting-button-transitions"></a>检测按钮转换

有时你可能需要了解何时第一次按下或释放按钮；即，按钮状态从释放转换为按下或从按下转化为释放的准确时间。 要确定准确时间，你需要记住之前的设备读数并将当前的读数与之比较，以查看发生了哪些变化。

以下示例演示了记住之前读数的基本方法；游戏板如下所示，但街机摇杆、赛车方向盘和 UI 导航按钮的原则相同

```cpp
GamepadReading newReading();
GamepadReading oldReading();

// Game::Loop represents one iteration of a typical game loop
void Game::Loop()
{
    // move previous newReading into oldReading before getting next newReading
    oldReading = newReading, newReading = gamepad.CurrentReading();

    // process device readings using buttonJustPressed/buttonJustReleased
}
```

执行任何操作之前，`Game::Loop` 会将 `newReading` 的现有值（之前的循环迭代中的游戏板读数）移动到 `oldReading` 中，然后用当前迭代的全新游戏板读数填充 `newReading`。 可以为你提供检测按钮转换所需的信息。

以下示例演示了检测按钮转换的基本方法。

```cpp
bool buttonJustPressed(const gamepadButtons selection)
{
    bool newSelectionPressed = (selection == (newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (oldReading.Buttons & selection));

    return newSelectionPressed && !oldSelectionPressed;
}

bool buttonJustReleased(gamepadButtons selection)
{
    bool newSelectionReleased = (gamepadButtons.None == (newReading.Buttons & selection));
    bool oldSelectionReleased = (gamepadButtons.None == (oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

这两个功能先从 `newReading` 和 `oldReading` 中派生按钮选择的布尔状态，然后执行布尔逻辑以确定是否已出现目标转换。 仅当新读数包含目标状态（分别为按下或释放）*且*旧读数不包含目标状态时，这些功能返回 _true_，否则返回 _false_。


## <a name="detecting-complex-button-arrangements"></a>检测复杂的按钮排列

每个输入设备按钮均提供指示是按下（向下）还是释放（向上）的数字读数。 为了提高效率，按钮读数不以单独的布尔值表示；而是全部打包到一个由特定于设备的枚举（例如 [GamepadButtons][]）表示的位域中。 若要读取特定按钮，使用按位掩码隔离你感兴趣的值。 设置相应位时按钮为按下（向下）；否则，按钮为释放（向上）。

回想确定按下或释放按钮的方式；游戏板如下所示，但街机摇杆、赛车方向盘和 UI 导航按钮的原则相同。

```cpp
// determines whether gamepad button A is pressed
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // button A is pressed
}

// determines whether gamepad button A is released
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // button A is released (not pressed)
}
```

如你所见，确定单个按钮的状态很简单，但有时你可能需要确定：是按下还是释放多个按钮，或者是否按特定方式安排一组按钮（按下一些按钮，释放一些按钮）。 测试多个按钮比测试单个按钮更复杂，特别是可能存在混合按钮状态，但是对于这些适用于单个和多个相似的按钮测试有一个简单的公式。

以下示例确定是否同时按下游戏板按钮 A 和 B。

```cpp
if ((GamepadButtons::A | GamepadButtons::B) == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // buttons A and B are pressed
}
```

以下示例确定是否同时释放游戏板按钮 A 和 B。

```cpp
if ((GamepadButtons::None == (reading.Buttons & GamepadButtons::A | GamepadButtons::B))
{
    // buttons A and B are released (not pressed)
}
```

以下示例确定是否在释放按钮 B 时按下按钮 A。

```cpp
if (GamepadButtons::A == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // button A is pressed and button B is released (button B is not pressed)
}
```

这五个示例共同拥有的公式是：由相等运算符左侧的表达式指定要测试的按钮排列，而由右侧的掩码表达式选择要考虑的按钮。

通过重新编写之前的示例，以下示例更清楚地演示了此公式。

```cpp
auto buttonArrangement = GamepadButtons::A;
auto buttonSelection = (reading.Buttons & (GamepadButtons::A | GamepadButtons::B));

if (buttonArrangement == buttonSelection)
{
    // button A is pressed and button B is released (button B is not pressed)
}
```

此公式可应用于在任意状态排列中测试任何按钮数。



[Windows.System.User]: https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx
[igamecontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[igamecontroller.user]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.user.aspx
[igamecontroller.userchanged]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.userchanged.aspx
[gamepadbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadbuttons.aspx


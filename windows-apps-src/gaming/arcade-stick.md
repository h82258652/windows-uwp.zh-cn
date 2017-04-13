---
author: mithom
title: "街机摇杆"
description: "使用 Windows.Gaming.Input 街机摇杆 API 检测并读取街机摇杆。"
ms.assetid: 2E52232F-3014-4C8C-B39D-FAC478BA3E01
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 游戏, 街机摇杆, 输入"
ms.openlocfilehash: b0411dcf1fd75ec7dc31d29a39e95f5c26073953
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="arcade-stick"></a>街机摇杆

此页介绍使用 [Windows.Gaming.Input.ArcadeStick][arcadestick] 和通用 Windows 平台 (UWP) 的相关 API 进行 Xbox One 街机摇杆编程的基本知识。

在本页中，你将了解如下内容：
* 如何收集相连街机摇杆及其用户的列表
* 如何检测是否已添加或删除某街机摇杆
* 如何读取一个或多个街机摇杆的输入
* 街机摇杆如何像导航设备一样运行


## <a name="arcade-stick-overview"></a>街机摇杆概述

要重现单独街机的感觉并实现其高精度数字控制，街机摇杆是尤为重要的输入设备。 街机摇杆对于肉搏战和其他街机风格的游戏来说是完美的输入设备，并适合所有能正常使用全数字控件的游戏。 街机摇杆在 Windows 10 和 Xbox One UWP 应用中受 [Windows.Gaming.Input][] 命名空间支持。

Xbox One 街机摇杆配备了一个 8 向数字游戏杆，六个**操作**按钮和两个**特殊**按钮；它们都是全数字输入设备，不支持模拟控件或震动。 Xbox One 街机摇杆还配备了用于支持 UI 导航的**视图**和**操作**按钮，但它们并不打算支持游戏命令，也不能作为游戏杆按钮很容易进行访问。

### <a name="ui-navigation"></a>UI 导航

为了减轻支持很多适用于用户界面导航的不同输入设备的负担并促进游戏和设备之间的一致性，大多数_物理_输入设备同时充当单独的称为 [UI 导航控制器](ui-navigation-controller.md) 的_逻辑_输入设备。 UI 导航控制器可跨各种输入设备提供通用的 UI 导航命令词汇。

作为 UI 导航控制器，街机摇杆将导航命令[必需组](ui-navigation-controller.md#required-set) 映射为游戏杆和**视图**、**菜单**、**操作 1** 和**操作 2** 按钮。

| 导航命令 | 街机摇杆输入  |
| ------------------:| ------------------- |
|                 向上 | 上摇杆            |
|               向下 | 下摇杆          |
|               向左 | 左摇杆          |
|              向右 | 右摇杆         |
|               视图 | “视图”按钮         |
|               菜单 | “菜单”按钮         |
|             接受 | “操作 1”按钮     |
|             取消 | “操作 2”按钮     |

街机摇杆不会映射导航命令的任何 [可选组](ui-navigation-controller.md#optional-set)。


## <a name="detect-and-track-arcade-sticks"></a>检测和跟踪街机摇杆

街机摇杆是由系统进行管理的，因此，你不必进行创建或初始化它们。 系统会为你提供连接的街机摇杆和事件的列表，以在添加或删除街机摇杆时通知你。

### <a name="the-arcade-sticks-list"></a>街机摇杆列表

[ArcadeStick][] 类会提供一个静态属性 [ArcadeSticks][]，该属性是当前连接的街机摇杆的只读列表。 因为你可能只对某些连接的街机摇杆感兴趣，所以建议你保留自己的集合，而不要通过 `ArcadeSticks` 属性来进行访问。

下面是将所有连接的街机摇杆复制到一个新集合的示例。
```cpp
auto myArcadeSticks = ref new Vector<ArcadeStick^>();

for (auto arcadestick : ArcadeStick::ArcadeSticks)
{
    // This code assumes that you're interested in all arcade sticks.
    myArcadeSticks->Append(arcadestick);
}
```

### <a name="adding-and-removing-arcade-sticks"></a>添加和删除街机摇杆

添加或删除街机摇杆时，会触发 [ArcadeStickAdded][] 和 [ArcadeStickRemoved][] 事件。 你可以为这些事件注册处理程序以便跟踪当前连接的街机摇杆。

下面是开始跟踪已经添加的街机摇杆的示例。
```cpp
ArcadeStick::ArcadeStickAdded += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    // This code assumes that you're interested in all new arcade sticks.
    myArcadeSticks->Append(args);
}
```

下面是停止跟踪已删除街机摇杆的示例。
```cpp
ArcadeStick::ArcadeStickRemoved += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    unsigned int indexRemoved;

    if(myArcadeSticks->IndexOf(args, &indexRemoved))
    {
        myArcadeSticks->RemoveAt(indexRemoved);
    }
}
```

### <a name="users-and-headsets"></a>用户和耳机

每个街机摇杆可以关联一个用户帐户以便将其身份与其游戏关联，并且可以附带一个耳机以方便使用语音聊天或游戏内功能。 若要了解有关如何关联用户帐户和使用耳机的详细信息，请参阅 [跟踪用户及其设备](input-practices-for-games.md#tracking-users-and-their-devices) 和 [耳机](headset.md)。


## <a name="reading-the-arcade-stick"></a>读取街机摇杆

确定感兴趣的街机摇杆之后，便可以从街机摇杆收集输入了。 但是，与你可能已习惯的某些其他输入类型不同，街机摇杆不会通过触发事件来表达状态更改。 相反，你需要通过对它们进行“轮询”__来定期读取其当前状态。

### <a name="polling-the-arcade-stick"></a>轮询街机摇杆

轮询会捕获街机摇杆在某个精确时间点的快照。 此输入收集方法对于大多数游戏都非常适合，因为其逻辑通常按确定的循环运行，而不是受事件驱动；而且，相对于随着时间逐个收集许多单个输入解释游戏命令，从一次性收集的输入解释游戏命令，通常会更为简单。

你通过调用 [GetCurrentReading][] 来轮询街机摇杆；此功能会返回包含街机摇杆状态的 [ArcadeStickReading][]。

下面是轮询街机摇杆来获取其当前状态的示例。
```cpp
auto arcadestick = myArcadeSticks[0];

ArcadeStickReading reading = arcadestick->GetCurrentReading();
```

除了街机摇杆状态之外，每个读数还包括一个时间戳，准确指出检索状态的时间。 该时间戳对于关联之前读数的时间或者游戏模拟的时间非常有用。

### <a name="reading-the-buttons"></a>读取按钮

每个街机摇杆按钮（游戏杆的四个方向、六个**操作**按钮和两个**特殊**按钮）均有一个数字读数，指出是按下（向下）还是释放（向上）。 为了提高效率，按钮读数不以单独的布尔值表示；而是全部打包到一个由 [ArcadeStickButtons][] 枚举表示的单独位域中。

> **注意**    街机摇杆配备了用于 UI 导航的其他按钮，例如**视图**和**菜单**按钮。 这些按钮不是 `ArcadeStickButtons` 枚举的一部分，只能作为 UI 导航设备通过访问街机摇杆进行读取。 有关详细信息，请参阅 [UI 导航设备](ui-navigation-controller.md)。

从 [ArcadeStickReading][] 结构的 `Buttons` 属性中读取按钮值。 因为此属性为位域，所以使用位域掩码隔离你感兴趣的按钮值。 设置相应位时按钮为按下（向下）；否则，按钮为释放（向上）。

以下示例确定是否按下了“操作 1”按钮。
```cpp
if (ArcadeStickButtons::Action1 == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is pressed
}
```

以下示例确定是否释放了“操作 1”按钮。
```cpp
if (ArcadeStickButtons::None == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is released (not pressed)
}
```

有时你可能需要确定：何时将按钮从按下转换为释放或从释放转换为按下，是按下还是释放多个按钮，或者是否按特定方式安排一组按钮（按下一些按钮，释放一些按钮）。 有关如何检测这些状态的详细信息，请参阅 [检测按钮转换](input-practices-for-games.md#detecting-button-transitions) 和 [检测复杂按钮安排](input-practices-for-games.md#detecting-complex-button-arrangements)。

## <a name="run-the-inputinterfacing-sample"></a>运行 InputInterfacing 示例

[InputInterfacingUWP 示例 _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) 演示了如何将街机摇杆和不同类型的输入设备配合使用，以及这些输入设备如何像 UI 导航控制器那样运行。


## <a name="see-also"></a>另请参阅
[Windows.Gaming.Input.UINavigationController][]
[Windows.Gaming.Input.IGameController][]

[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[arcadestick]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.aspx
[arcadesticks]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadesticks.aspx
[arcadestickadded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadestickadded.aspx
[arcadestickremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadestickremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.getcurrentreading.aspx
[arcadestickreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestickreading.aspx
[arcadestickbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestickbuttons.aspx

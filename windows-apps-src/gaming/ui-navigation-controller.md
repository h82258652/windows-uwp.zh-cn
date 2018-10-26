---
author: eliotcowley
title: UI 导航控制器
description: 使用 Windows.Gaming.Input UI 导航控制器 API 来检测和读取各种适用于 UI 导航的输入设备。
ms.assetid: 5A14926D-8C2E-4DE8-AAFB-BEEB9BFE91A5
ms.author: elcowle
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, ui, 导航
ms.localizationpriority: medium
ms.openlocfilehash: a0ec2790f6dddf93959a8c826602c0ac622a7d23
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5593481"
---
# <a name="ui-navigation-controller"></a>UI 导航控制器

本页介绍使用 [Windows.Gaming.Input.UINavigationController][uinavigationcontroller] 和通用 Windows 平台 (UWP) 的相关 API 进行 UI 导航设备编程的基础知识。

在本页中，你将了解如下内容：
* 如何收集相连 UI 导航设备及其用户的列表
* 如何检是否已添加或删除某个测导航设备
* 如何从一个或多个 UI 导航设备读取输入
* 如何像导航设备一样使用游戏板和街机摇杆

## <a name="ui-navigation-controller-overview"></a>UI 导航控制器概述

几乎所有游戏都至少有一些与游戏玩法分开的用户界面，即使它只是游戏前的菜单或游戏中的对话框。 玩家需要能够使用他们选择的任何输入设备来导航此 UI，但这会使得开发人员必须增加对每种输入设备的特定支持，并且还可能引起游戏和输入设备之间的不一致，进而让玩家受到困扰。 出于这些原因，我们创建了 [UINavigationController][] API。

UI 导航控制器是_逻辑_输入设备，用于提供一套可由各种_物理_输入设备支持的常见 UI 导航命令的词汇。 _UI 导航控制器_只是以一种不同的方式来看待物理输入设备；我们用_导航设备_来指代被视为导航控制器的任何物理输入设备。 通过针对导航设备而不是特定输入设备进行编程，开发人员避免了支持不同输入设备的负担并且可在默认情况下实现一致性。

因为每种输入设备支持的控件数量和种类可能有很大差别，并且由于某些输入设备需要支持较丰富的导航命令集合，因此，导航控制器接口将命令词汇划分为包含最常见和最基本命令的_必需组_，以及包含方便但非必要命令的_可选组_。 所有导航设备都支持_必需组_中的每个命令，并且可能支持_可选组_中的全部命令或部分命令，或不支持其中的命令。

### <a name="required-set"></a>必需组

导航设备必须都支持_必需组_中的所有导航命令；这些命令包括方向（up、down、left 和 right）、view、menu、accept 和 cancel 命令。

方向命令用于单个 UI 元素之间的主要 [XY-聚焦导航](../design/devices/designing-for-tv.md#xy-focus-navigation-and-interaction)。 view 和 menu 命令分别用于显示游戏玩法信息（通常是瞬时的，有时是模态的）和用于在游戏玩法和菜单上下文之间进行切换。 accept 和 cancel 命令分别用于肯定（是）和否定（否）响应。

下表通过示例总结了这些命令及其预期用途。
| 命令 | 预期用途
| -------:| ---------------
|      向上 | XY-聚焦导航上
|    向下 | XY-聚焦导航下
|    向左 | XY-聚焦导航左
|   向右 | XY-聚焦导航右
|    视图 | 显示游戏玩法信息 _（记分牌、游戏统计信息、目标、世界地图或区域地图）_
|    菜单 | 主菜单/暂停 _（设置、状态、装备、背包、暂停）_
|  接受 | 肯定响应 _（接受、前进、确认、开始、是）_
|  取消 | 否定响应 _（拒绝、倒退、谢绝、停止、否）_


### <a name="optional-set"></a>可选组

导航设备还可能支持_可选组_中的全部命令或部分命令，或不支持其中的命令；这些命令包括分页（上、下、左和右）、滚动（上、下、左和右）和上下文 (上下文 1-4) 命令。

上下文命令明确用于特定于应用程序的命令和导航快捷方式。 分页和滚动命令分别用于在 UI 元素的页面或群组之间的快速导航，以及在 UI 元素内的细粒度导航。

下表总结了这些命令及其预期用途。
|     命令 | 预期用途
| -----------:| ------------
|      PageUp | 向上跳转（至上面的/前一个垂直页面或群组）
|    PageDown | 向下跳转（至下面的/下一个垂直页面或群组）
|    PageLeft | 向左跳转（至左面的/前一个水平页面或群组）
|   PageRight | 向右跳转（至右面的/下一个水平页面或群组）
|    ScrollUp | 向上滚动（在聚焦的 UI 元素或可滚动的组内）
|  ScrollDown | 向下滚动（在聚焦的 UI 元素或可滚动的组内）
|  ScrollLeft | 向左滚动（在聚焦的 UI 元素或可滚动的组内）
| ScrollRight | 向右滚动（在聚焦的 UI 元素或可滚动的组内）
|    Context1 | 主要上下文操作
|    Context2 | 次要上下文操作
|    Context3 | 第三上下文操作
|    Context4 | 第四上下文操作

> **注意**    虽然游戏可以自由地响应具有与其预期用途不同的实际功能的任何命令，但应当避免出人意料的行为。 特别是，如果你需要命令的预期用途，请不要更改其实际功能。可以尝试为最有意义的命令分配新颖的功能，并将相应的功能分配给对应的命令，如 PageUp/PageDown。 最后，考虑每种类型的输入设备支持哪些命令以及它们映射到的控件，以确保可以从每个设备访问关键命令。

## <a name="gamepad-arcade-stick-and-racing-wheel-navigation"></a>游戏板、街机摇杆和赛车方向盘导航

Windows.Gaming.Input 命名空间支持的所有输入设备都是 UI 导航设备。

下表总结了导航命令的_必需组_是如何映射到各种输入设备的。

| 导航命令 | 游戏板输入                       | 街机摇杆输入 | 赛车方向盘输入 |
| ------------------:| ----------------------------------- | ------------------ | ------------------ |
|                 向上 | 左操纵杆向上/方向键向上       | 上摇杆           | 方向键向上           |
|               向下 | 左操纵杆向下/方向键向下   | 下摇杆         | 方向键向下         |
|               向左 | 左操纵杆向左/方向键向左   | 左摇杆         | 方向键向左         |
|              向右 | 左操纵杆向右/方向键向右 | 右摇杆        | 方向键向右        |
|               视图 | “视图”按钮                         | “视图”按钮        | “视图”按钮        |
|               菜单 | “菜单”按钮                         | “菜单”按钮        | “菜单”按钮        |
|             接受 | A 按钮                            | 操作 1 按钮    | A 按钮           |
|             取消 | B 按钮                            | 操作 2 按钮    | B 按钮           |

下表总结了导航命令的_可选组_是如何映射到各种输入设备的。

| 导航命令 | 游戏板输入          | 街机摇杆输入 | 赛车方向盘输入    |
| ------------------:| ---------------------- | ------------------ | --------------------- |
|             PageUp | 左扳机键           | _不支持_    | _视情况而定_              |
|           PageDown | 右扳机键          | _不支持_    | _视情况而定_              |
|           PageLeft | 左缓冲键            | _不支持_    | _视情况而定_              |
|          PageRight | 右缓冲键           | _不支持_    | _视情况而定_              |
|           ScrollUp | 右操纵杆向上    | _不支持_    | _视情况而定_              |
|         ScrollDown | 右操纵杆向下  | _不支持_    | _视情况而定_              |
|         ScrollLeft | 右操纵杆向左  | _不支持_    | _视情况而定_              |
|        ScrollRight | 右操纵杆向右 | _不支持_    | _视情况而定_              |
|           Context1 | X 按钮               | _不支持_    | X 按钮（_常用_） |
|           Context2 | Y 按钮               | _不支持_    | Y 按钮（_常用_） |
|           Context3 | 左操纵杆按键  | _不支持_    | _视情况而定_              |
|           Context4 | 右操纵杆按键 | _不支持_    | _视情况而定_              |


## <a name="detect-and-track-ui-navigation-controllers"></a>检测并追踪 UI 导航控制器

尽管 UI 导航控制器是逻辑输入设备，但它们仍然是物理设备的表示，并且由系统以相同的方式管理。 你不必创建或初始化它们；系统会为你提供连接的 UI 导航控制器和事件的列表，以在添加或删除 UI 导航控制器时向你发送通知。

### <a name="the-ui-navigation-controllers-list"></a>UI 导航控制器列表

[UINavigationController][] 类提供一个静态属性 [UINavigationControllers][]，该属性是当前连接的 UI 导航设备的只读列表。 因为你可能只对某些连接的导航设备感兴趣，所以建议你保留自己的集合，而不要通过 `UINavigationControllers` 属性访问它们。

下面是将所有连接的 UI 导航控制器复制到一个新集合的示例。
```cpp
auto myNavigationControllers = ref new Vector<UINavigationController^>();

for (auto device : UINavigationController::UINavigationControllers)
{
    // This code assumes that you're interested in all navigation controllers.
    myNavigationControllers->Append(device);
}
```

### <a name="adding-and-removing-ui-navigation-controllers"></a>添加和删除 UI 导航控制器

当添加或删除 UI 导航控制器时，会触发 [UINavigationControllerAdded][] 和 [UINavigationControllerRemoved][] 事件。 你可以为这些事件注册事件处理程序，以跟踪当前连接的导航设备。

下面是开始跟踪已添加 UI 导航设备的示例。
```cpp
UINavigationController::UINavigationControllerAdded += ref new EventHandler<UINavigationController^>(Platform::Object^, UINavigationController^ args)
{
    // This code assumes that you're interested in all new navigation controllers.
    myNavigationControllers->Append(args);
}
```

下面是停止跟踪已删除街机摇杆的示例。
```cpp
UINavigationController::UINavigationControllerRemoved += ref new EventHandler<UINavigationController^>(Platform::Object^, UINavigationController^ args)
{
    unsigned int indexRemoved;

    if(myNavigationControllers->IndexOf(args, &indexRemoved))
    {
        myNavigationControllers->RemoveAt(indexRemoved);
    }
}
```

### <a name="users-and-headsets"></a>用户和耳机

每个导航设备都可以关联一个用户帐户，用以将其身份链接到他们的输入，而且还可以连一个耳机，方便使用语音聊天或游戏内功能。 若要了解有关如何关联用户帐户和使用耳机的详细信息，请参阅 [跟踪用户及其设备](input-practices-for-games.md#tracking-users-and-their-devices)和[耳机](headset.md)。

## <a name="reading-the-ui-navigation-controller"></a>读取 UI 导航控制器

确定感兴趣的 UI 导航设备之后，便可以从中收集输入了。 不过，与你可能已习惯的某些其他输入类型不同，导航设备不会通过触发事件来传达状态的更改。 相反，你需要通过对它们进行“轮询”__ 来定期读取其当前状态。

### <a name="polling-the-ui-navigation-controller"></a>轮询 UI 导航控制器

轮询会捕获导航设备在某个精确时间点的快照。 此输入收集方法对于大多数游戏都非常适合，因为其逻辑通常按确定的循环运行，而不是受事件驱动；而且，相对于随着时间逐个收集许多单个输入解释游戏命令，从一次性收集的输入解释游戏命令，通常会更为简单。

你可以通过调用 [UINavigationController.GetCurrentReading][getcurrentreading] 来轮询导航设备；此函数会返回包含导航设备状态的 [UINavigationReading][]。

```cpp
auto navigationController = myNavigationControllers[0];

UINavigationReading reading = navigationController->GetCurrentReading();
```

### <a name="reading-the-buttons"></a>读取按钮

每个 UI 导航按钮提供对应于其被按下（向下）还是释放（向上）的布尔读数。 为了提高效率，按钮读数不以单独的布尔值表示；而是全部打包到一个由 [RequiredUINavigationButtons][] 和 [OptionalUINavigationButtons][] 枚举表示的两个位域中。

属于_必需组_按钮的读数读取自 [UINavigationReading][] 结构的 `RequiredButtons` 属性；属于_可选组_按钮的读数读取自 `OptionalButtons` 属性。 由于这些属性都是位域，因此使用按位掩码来隔离你感兴趣的按钮值。 设置相应位时按钮为按下（向下）；否则，按钮为释放（向上）。

以下示例确定是否按下_必需组_中的“接受”按钮。
```cpp
if (RequiredUINavigationButtons::Accept == (reading.RequiredButtons & RequiredUINavigationButtons::Accept))
{
    // Accept is pressed
}
```

以下示例确定是否释放_必需组_中的“接受”按钮。
```cpp
if (RequiredUINavigationButtons::None == (reading.RequiredButtons & RequiredUINavigationButtons::Accept))
{
    // Accept is released (not pressed)
}
```

当读取_可选组_中按钮时，务必要使用 `OptionalButtons` 属性和 `OptionalUINavigationButtons` 枚举。

以下示例确定是否按下_可选组_中的“上下文 1”按钮。
```cpp
if (OptionalUINavigationButtons::Context1 == (reading.OptionalButtons & OptionalUINavigationButtons::Context1))
{
    // Context 1 is pressed
}
```

有时你可能需要确定：何时将按钮从按下转换为释放或从释放转换为按下，是按下还是释放多个按钮，或者是否按特定方式安排一组按钮（按下一些按钮，释放一些按钮）。 有关如何检测这些状态的详细信息，请参阅 [检测按钮转换](input-practices-for-games.md#detecting-button-transitions) 和 [检测复杂按钮安排](input-practices-for-games.md#detecting-complex-button-arrangements)。


## <a name="run-the-ui-navigation-controller-sample"></a>运行 UI 导航控制器示例

[InputInterfacingUWP 示例_ (github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/InputInterfacingUWP) 演示了不同的输入设备如何作为 UI 导航控制器。

## <a name="see-also"></a>另请参阅
[Windows.Gaming.Input.Gamepad][]
[Windows.Gaming.Input.ArcadeStick][]
[Windows.Gaming.Input.RacingWheel][]
[Windows.Gaming.Input.IGameController][]


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.Gamepad]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.aspx
[Windows.Gaming.Input.Arcadestick]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.aspx
[Windows.Gaming.Input.Racingwheel]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[uinavigationcontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[uinavigationcontrollers]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.uinavigationcontrollers.aspx
[uinavigationcontrolleradded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.uinavigationcontrolleradded.aspx
[uinavigationcontrollerremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.uinavigationcontrollerremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.getcurrentreading.aspx
[uinavigationreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationreading.aspx
[requireduinavigationbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.requireduinavigationbuttons.aspx
[optionaluinavigationbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.optionaluinavigationbuttons.aspx

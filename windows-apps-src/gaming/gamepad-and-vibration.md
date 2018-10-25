---
author: eliotcowley
title: 游戏板和振动
description: 使用 Windows.Gaming.Input 游戏板 API 检测、读取振动和脉冲命令并向游戏板发送这些命令。
ms.assetid: BB03BB8E-255F-4AE8-AC43-1E519CA860FE
ms.author: wdg-dev-content
ms.date: 09/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 游戏, 游戏板, 振动
ms.localizationpriority: medium
ms.openlocfilehash: 2bf78b43bb09f97c196858d7cc4fcdb1e71462fc
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5480496"
---
# <a name="gamepad-and-vibration"></a>游戏板和振动

此页介绍使用 [Windows.Gaming.Input.Gamepad][gamepad] 和通用 Windows 平台 (UWP) 的相关 API 进行 Xbox One 游戏板编程的基本知识。

在本页中，你将了解如下内容：

* 如何收集连接的游戏板及其用户的列表
* 如何检测是否已添加或删除某个游戏板
* 如何读取一个或多个游戏板的输入
* 如何发送振动和脉冲命令
* 如何像 UI 导航设备游戏板

## <a name="gamepad-overview"></a>游戏板概述

Xbox Wireless Controller 和 Xbox Wireless Controller S 等游戏板是通用游戏输入设备。 它们是 Xbox One 上的标准输入设备，不喜欢用键盘和鼠标的 Windows 游戏玩家通常会选择这些设备。 游戏板在 Windows 10 和 Xbox UWP 应用中受 [Windows.Gaming.Input][] 命名空间支持。

Xbox One 游戏板配备有方向板 （d-pad）;**A**、 **B**、 **X**、 **Y**、**视图**和**菜单**按钮;左和右操纵杆、 缓冲键和触发器;以及总共 4 个振动电机。 两个操纵杆会在 X 和 Y 轴提供两个模拟读数，并在向内按时还可以充当一个按钮。 每个扳机键会提供一个模拟读数，表示它距离返回拉取。

<!-- > [!NOTE]
> The Xbox Elite Wireless Controller is equipped with four additional **Paddle** buttons on its underside. These can be used to provide redundant access to game commands that are difficult to use together (such as the right thumbstick together with any of the **A**, **B**, **X**, or **Y** buttons) or to provide dedicated access to additional commands. -->

> [!NOTE]
> `Windows.Gaming.Input.Gamepad` 此外支持 Xbox 360 游戏板，具有与标准 Xbox One 游戏板相同的控件布局。

### <a name="vibration-and-impulse-triggers"></a>振动和脉冲扳机键

Xbox One 游戏板针对强烈和细微的游戏板振动提供了两个独立的电机，同时还提供了两个专用电机用于向每个扳机键提供剧烈振动（这一独特功能正是 Xbox One 游戏板扳机键被称为“脉冲扳机键”__ 的原因）。

> [!NOTE]
> Xbox 360 游戏板未配备_脉冲扳机键_。

有关详细信息，请参阅 [振动和脉冲扳机键概述](#vibration-and-impulse-triggers-overview)。

### <a name="thumbstick-deadzones"></a>操纵杆死区

理想状态下，中心位置闲置的操纵杆每次都会在 X 和 Y 轴生成相同的中性读数。 但是，由于机械作用力以及操纵杆的敏感性，中心位置的实际读数只能近似于理想的中性值，在后续的读数之间可能会有所不同。 出于此原因，你必须始终使用小_死区_&mdash;一系列被忽略的接近理想中心位置值&mdash;来弥补制造差异、 机械磨损或者其他游戏板问题。

较大的死区可提供一个简单的策略，用于将有意输入和无意输入区分开来。

有关详细信息，请参阅 [读取操纵杆](#reading-the-thumbsticks)。

### <a name="ui-navigation"></a>UI 导航

为了减轻对用户界面导航提供不同输入设备支持的负担并促进游戏和设备之间的一致性，大多_物理_输入设备同时会充当单独的被称为 [UI 导航控制器](ui-navigation-controller.md)的_逻辑_输入设备。 UI 导航控制器可跨各种输入设备提供通用的 UI 导航命令词汇。

作为 UI 导航控制器，游戏板将导航命令[所需的设置](ui-navigation-controller.md#required-set)映射为左的操纵杆、 方向键、**视图**、**菜单**、 **A**和**B**按钮。

| 导航命令 | 游戏板输入                       |
| ------------------:| ----------------------------------- |
|                 向上 | 左操纵杆向上/方向键向上       |
|               向下 | 左操纵杆向下/方向键向下   |
|               向左 | 左操纵杆向左/方向键向左   |
|              向右 | 左操纵杆向右/方向键向右 |
|               视图 | “视图”按钮                         |
|               菜单 | “菜单”按钮                         |
|             接受 | A 按钮                            |
|             取消 | B 按钮                            |

此外，游戏板将所有导航命令的[可选组](ui-navigation-controller.md#optional-set)映射到其余输入。

| 导航命令 | 游戏板输入          |
| ------------------:| ---------------------- |
|            Page Up | 左扳机键           |
|          Page Down | 右扳机键          |
|          Page Left | 左缓冲键            |
|         Page Right | 右缓冲键           |
|          Scroll Up | 右操纵杆向上    |
|        Scroll Down | 右操纵杆向下  |
|        Scroll Left | 右操纵杆向左  |
|       Scroll Right | 右操纵杆向右 |
|          Context 1 | X 按钮               |
|          Context 2 | Y 按钮               |
|          Context 3 | 左操纵杆按键  |
|          Context 4 | 右操纵杆按键 |

## <a name="detect-and-track-gamepads"></a>检测和跟踪游戏板

游戏板通过系统进行管理，因此，你不必进行创建或初始化。 系统会为你提供连接的游戏板和事件的列表，并在添加或删除游戏板时会为你发送通知。

### <a name="the-gamepads-list"></a>游戏板列表

[Gamepad][] 类会提供一个静态属性 [Gamepads][]，该属性是当前已连接游戏板的只读列表。 因为你可能只对某些连接的游戏板感兴趣，建议你保留自己的集合，而不是访问它们通过`Gamepads`属性。

下面是将所有已连接游戏板复制到一个新集合的示例。 请注意，因为将会在后台中的其他线程访问此集合 （在[GamepadAdded][]和[GamepadRemoved][]事件），你需要设置任何代码的读取或更新集合周围锁定。

```cpp
auto myGamepads = ref new Vector<Gamepad^>();
critical_section myLock{};

for (auto gamepad : Gamepad::Gamepads)
{
    // Check if the gamepad is already in myGamepads; if it isn't, add it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), gamepad);

    if (it == end(myGamepads))
    {
        // This code assumes that you're interested in all gamepads.
        myGamepads->Append(gamepad);
    }
}
```

```cs
private readonly object myLock = new object();
private List<Gamepad> myGamepads = new List<Gamepad>();
private Gamepad mainGamepad;

private void GetGamepads()
{
    lock (myLock)
    {
        foreach (var gamepad in Gamepad.Gamepads)
        {
            // Check if the gamepad is already in myGamepads; if it isn't, add it.
            bool gamepadInList = myGamepads.Contains(gamepad);

            if (!gamepadInList)
            {
                // This code assumes that you're interested in all gamepads.
                myGamepads.Add(gamepad);
            }
        }
    }   
}
```

### <a name="adding-and-removing-gamepads"></a>添加和删除游戏板

添加或删除游戏板时会引发的[GamepadAdded][]和[GamepadRemoved][]事件。 你可以为这些事件注册处理程序以跟踪当前连接的游戏板。

下面是开始跟踪已添加的游戏板的示例。

```cpp
Gamepad::GamepadAdded += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    // Check if the just-added gamepad is already in myGamepads; if it isn't, add
    // it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), args);

    if (it == end(myGamepads))
    {
        // This code assumes that you're interested in all new gamepads.
        myGamepads->Append(args);
    }
}
```

```cs
Gamepad.GamepadAdded += (object sender, Gamepad e) =>
{
    // Check if the just-added gamepad is already in myGamepads; if it isn't, add
    // it.
    lock (myLock)
    {
        bool gamepadInList = myGamepads.Contains(e);

        if (!gamepadInList)
        {
            myGamepads.Add(e);
        }
    }
};
```

下面的示例将停止跟踪已删除的游戏板。 你还需要处理时将其; 要跟踪的游戏板会发生什么情况例如，此代码仅跟踪来自一个游戏板输入，并且只需将其设置为`nullptr`删除的时间。 你需要检查每个帧，如果你的游戏板处于活动状态，以及哪些游戏板你正在收集来自输入控制器的连接和断开连接时的更新。

```cpp
Gamepad::GamepadRemoved += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;
    critical_section::scoped_lock lock{ myLock };

    if(myGamepads->IndexOf(args, &indexRemoved))
    {
        if (m_gamepad == myGamepads->GetAt(indexRemoved))
        {
            m_gamepad = nullptr;
        }

        myGamepads->RemoveAt(indexRemoved);
    }
}
```

```cs
Gamepad.GamepadRemoved += (object sender, Gamepad e) =>
{
    lock (myLock)
    {
        int indexRemoved = myGamepads.IndexOf(e);

        if (indexRemoved > -1)
        {
            if (mainGamepad == myGamepads[indexRemoved])
            {
                mainGamepad = null;
            }

            myGamepads.RemoveAt(indexRemoved);
        }
    }
};
```

有关详细信息，请参阅[适用于游戏的输入操作](input-practices-for-games.md)。

### <a name="users-and-headsets"></a>用户和耳机

每个游戏板都可以关联一个用户帐户，用以将其身份与游戏板关联，而且还可以连一个耳机，方便使用语音聊天或游戏内功能。 若要了解有关如何关联用户帐户和使用耳机的详细信息，请参阅 [跟踪用户及其设备](input-practices-for-games.md#tracking-users-and-their-devices) 和 [耳机](headset.md)。

## <a name="reading-the-gamepad"></a>读取游戏板

确定感兴趣的游戏板之后，便可以从游戏板收集输入了。 不过，与你可能已习惯的某些其他输入类型不同，游戏板不会通过触发事件来传达状态的更改。 相反，你需要通过对它们进行“轮询”__ 来定期读取其当前状态。

### <a name="polling-the-gamepad"></a>轮询游戏板

轮询会捕获导航设备在某个精确时间点的快照。 此输入收集方法对于大多数游戏都非常适合，因为其逻辑通常按确定的循环运行，而不是受事件驱动；而且，相对于随着时间逐个收集许多单个输入解释游戏命令，从一次性收集的输入解释游戏命令，通常会更为简单。

你可以通过调用 [GetCurrentReading][] 来轮询游戏板；此函数会返回包含游戏板状态的 [GamepadReading][]。

下面是轮询游戏板来获取其当前状态的示例。

```cpp
auto gamepad = myGamepads[0];

GamepadReading reading = gamepad->GetCurrentReading();
```

```cs
Gamepad gamepad = myGamepads[0];

GamepadReading reading = gamepad.GetCurrentReading();
```

除了游戏板状态之外，每个读数还包括一个时间戳，确切指出检索状态的时间。 该时间戳对于关联之前读数的时间或者游戏模拟的时间非常有用。

### <a name="reading-the-thumbsticks"></a>读取操纵杆

每个操纵杆会在 X 和 Y 轴提供一个介于 -1.0 和 +1.0 之间的模拟读数。 在 X 轴，值 -1.0 对应于操纵杆最左位置；值 +1.0 对应于最右的位置。 在 Y 轴，值 -1.0 对应于操纵杆最下面的位置；值 +1.0 对应于最上面的位置。 在这两个轴，值大约是 0.0，当摇杆处于中心位置，但它是正常的精确值有所不同，即使是在后续读数; 之间在本部分后面讨论减小此误差的策略。

左操纵杆 X 轴的值通过 [GamepadReading][] 结构的 `LeftThumbstickX` 属性读取，Y 轴的值通过 `LeftThumbstickY` 属性来读取。 右操纵杆 X 轴的值通过 `RightThumbstickX` 属性读取，Y 轴的值通过 `RightThumbstickY` 属性读取。

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0
float rightStickX = reading.RightThumbstickX; // returns a value between -1.0 and +1.0
float rightStickY = reading.RightThumbstickY; // returns a value between -1.0 and +1.0
```

```cs
double leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
double leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0
double rightStickX = reading.RightThumbstickX; // returns a value between -1.0 and +1.0
double rightStickY = reading.RightThumbstickY; // returns a value between -1.0 and +1.0
```

读取操纵杆的值时，你会注意到，当操纵杆处于中心位置闲置时，它们不会稳定地生成中性读数 0.0；而是每次移动操纵杆并返回到中心位置时，才会生成不同的接近 0.0 的值。 要减小这些误差，你可以使用小“死区”__（一系列被忽略的接近理想中心位置的值）。 使用死区的一种方法是，确定操纵杆被移动远离中心的距离，并忽略比你选择的某些距离更近的读数。 你可以大致计算距离&mdash;它它们并不精确，因为操纵杆读数实际上是极值，不是平面值&mdash;只需通过使用勾股定理计算。 这会生成一个径向死区。

下面示例演示如何使用勾股定理计算基本径向死区。

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0

// choose a deadzone -- readings inside this radius are ignored.
const float deadzoneRadius = 0.1;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem -- for a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
auto oppositeSquared = leftStickY * leftStickY;
auto adjacentSquared = leftStickX * leftStickX;

// accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) > deadzoneSquared)
{
    // input accepted, process it
}
```

```cs
double leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
double leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0

// choose a deadzone -- readings inside this radius are ignored.
const double deadzoneRadius = 0.1;
const double deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem -- for a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
double oppositeSquared = leftStickY * leftStickY;
double adjacentSquared = leftStickX * leftStickX;

// accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) > deadzoneSquared)
{
    // input accepted, process it
}
```

向内按时，每个操纵杆都可充当一个按钮；有关读取此输入的详细信息，请参阅 [读取按钮](#reading-the-buttons)。

### <a name="reading-the-triggers"></a>读取扳机键

扳机键表示为介于 0.0（完全释放）和 1.0（完全凹陷）之间的浮点值。 左扳机键的值通过 [GamepadReading][] 结构的 `LeftTrigger` 属性读取，右扳机键的值通过 `RightTrigger` 属性来读取。

```cpp
float leftTrigger  = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
float rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

```cs
double leftTrigger = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
double rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

### <a name="reading-the-buttons"></a>读取按钮

每个游戏板按钮&mdash;方向键、 向左和右缓冲键、 左右操纵杆按键、 **A**、 **B**、 **X**、 **Y**、**视图**和**菜单**的四个方向&mdash;提供阅读该数字指示它是按下 （向下） 还是释放 （向上）。 为了提高效率，按钮读数不以单独的布尔值; 表示相反，它们是全部打包到的单独位域[GamepadButtons][]枚举表示。

<!-- > [!NOTE]
> The Xbox Elite Wireless Controller is equipped with four additional **paddle** buttons on its underside. These buttons are also represented in the `GamepadButtons` enumeration and their values are read in the same way as the standard gamepad buttons. -->

这些按钮值通过 [GamepadReading][] 结构的 `Buttons` 属性读取。 由于此属性为位域，因此使用按位掩码隔离你感兴趣的按钮值。 设置相应位时按钮为按下（向下）；否则，按钮为释放（向上）。

以下示例确定是否按下 A 按钮。

```cpp
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // button A is pressed
}
```

```cs
if (GamepadButtons.A == (reading.Buttons & GamepadButtons.A))
{
    // button A is pressed
}
```

以下示例确定是否释放 A 按钮。

```cpp
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // button A is released
}
```

```cs
if (GamepadButtons.None == (reading.Buttons & GamepadButtons.A))
{
    // button A is released
}
```

有时你可能想要确定从一个按钮的过渡时按下为释放还是释放多个按钮是按下还是释放，或按特定方式安排一组按钮为按下，&mdash;按下，释放一些。 有关如何检测这些条件的详细信息，请参阅 [检测按钮转换](input-practices-for-games.md#detecting-button-transitions) 和 [检测复杂按钮安排](input-practices-for-games.md#detecting-complex-button-arrangements)。

## <a name="run-the-gamepad-input-sample"></a>运行游戏板输入示例

[GamepadUWP sample _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/GamepadUWP) 演示了如何连接到游戏板并读取其状态。

## <a name="vibration-and-impulse-triggers-overview"></a>振动和脉冲扳机键概述

游戏板内的振动电机用于向用户提供触觉反馈。 游戏可利用此功能打造更为出色的带入感，帮助传达状态信息（如遭受攻击），向重要对象发出邻近信号，或者用于其他有创意的用途。

Xbox One 游戏板共配备有四个独立的振动电机。 两个大型电机位于游戏板中;左的电机提供强烈的高幅振动，右电机提供较轻柔更巧妙的振动。 另外两个是小型电机，每个扳机键内一个，直接向用户的扳机键手指提供剧烈的突发性振动，Xbox One 游戏板的这一独特功能，正是其扳机键被称为“脉冲扳机键”__ 的原因。 通过将这些电机编排在一起，可以生成层次丰富的触觉。

## <a name="using-vibration-and-impulse"></a>使用振动和脉冲

游戏板振动通过 [Gamepad][] 类的 [Vibration][] 属性进行控制。 `Vibration` Vibration 是 [GamepadVibration][] 结构的实例，该结构由四个浮点值组成，每个值代表其中一个电机的强度。

尽管的成员`Gamepad.Vibration`属性可以直接修改，建议在初始化单独`GamepadVibration`实例与你需的值，然后将其复制到`Gamepad.Vibration`属性来一次性更改实际电机强度。

以下示例演示了如何一次性更改电机强度。

```cpp
// get the first gamepad
Gamepad^ gamepad = Gamepad::Gamepads->GetAt(0);

// create an instance of GamepadVibration
GamepadVibration vibration;

// ... set vibration levels on vibration struct here

// copy the GamepadVibration struct to the gamepad
gamepad.Vibration = vibration;
```

```cs
// get the first gamepad
Gamepad gamepad = Gamepad.Gamepads[0];

// create an instance of GamepadVibration
GamepadVibration vibration = new GamepadVibration();

// ... set vibration levels on vibration struct here

// copy the GamepadVibration struct to the gamepad
gamepad.Vibration = vibration;
```

### <a name="using-the-vibration-motors"></a>使用振动电机

左和右振动电机采用介于 0.0（无振动）和 1.0（最强振动）之间的浮点值。 左电机的强度通过 [GamepadVibration][] 结构的 `LeftMotor` 属性来设置，右电机的强度通过 `RightMotor` 属性来设置。

下面是设置振动电机强度并激活游戏板振动的示例。

```cpp
GamepadVibration vibration;
vibration.LeftMotor = 0.80;  // sets the intensity of the left motor to 80%
vibration.RightMotor = 0.25; // sets the intensity of the right motor to 25%
gamepad.Vibration = vibration;
```

```cs
GamepadVibration vibration = new GamepadVibration();
vibration.LeftMotor = 0.80;  // sets the intensity of the left motor to 80%
vibration.RightMotor = 0.25; // sets the intensity of the right motor to 25%
mainGamepad.Vibration = vibration;
```

切记，这两个电机不是完全相同的，所以将这些属性设置为相同的值并不会在一个电机中生成与另一个电机中相同的振动。 对于任何值，左的电机会比右演练的更强以较低频率振动&mdash;对于相同的值&mdash;生成更轻柔的振动，较高的频率。 即使是最大值，左电机也无法生成右电机的高频率，右电机也无法生成左电机的高动力。 因为电机通过游戏板刚性连接，所以即使电机具有不同的特性并且能够以不同的强度振动，游戏玩家仍然不能完全独立地体验振动。 相比完全相同的电机，这种布置可以产生更大范围、更丰富的感觉。

### <a name="using-the-impulse-triggers"></a>使用脉冲扳机键

每个脉冲扳机键电机采用介于 0.0（无振动）和 1.0（最强振动）之间的浮点值。 左扳机键电机的强度通过 [GamepadVibration][] 结构的 `LeftTrigger` 属性来设置，右扳机键的强度通过 `RightTrigger` 属性来设置。

下面是设置两个脉冲扳机键强度并激活它们的示例。

```cpp
GamepadVibration vibration;
vibration.LeftTrigger = 0.75;  // sets the intensity of the left trigger to 75%
vibration.RightTrigger = 0.50; // sets the intensity of the right trigger to 50%
gamepad.Vibration = vibration;
```

```cs
GamepadVibration vibration = new GamepadVibration();
vibration.LeftTrigger = 0.75;  // sets the intensity of the left trigger to 75%
vibration.RightTrigger = 0.50; // sets the intensity of the right trigger to 50%
mainGamepad.Vibration = vibration;
```

与其他两个电机不同，扳机键内的两个振动电机是完全相同的，所以对于相同的值它们会在任一电机中产生与另一个电机中相同的振动。 但是，因为这两个电机并未以任意方式刚性连接，所以游戏玩家可以独立体验振动。 这种布置能够同时向两个扳机键传送完全独立的感觉，相比游戏板中的电机，它们有助于传达更为特定的信息。

## <a name="run-the-gamepad-vibration-sample"></a>运行游戏板振动示例

[GamepadVibrationUWP sample _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/GamepadVibrationUWP) 演示了如何使用游戏板振动电机和脉冲扳机键产生各种效果。

## <a name="see-also"></a>另请参阅

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [游戏输入实践](input-practices-for-games.md)

[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[游戏板]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.aspx
[游戏板]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepads.aspx
[gamepadadded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepadadded.aspx
[gamepadremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepadremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.getcurrentreading.aspx
[振动]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.vibration.aspx
[gamepadreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadreading.aspx
[gamepadbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadbuttons.aspx
[gamepadvibration]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadvibration.aspx

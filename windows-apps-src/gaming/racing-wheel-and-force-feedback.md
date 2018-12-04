---
title: 赛车方向盘和力回馈
description: 使用 Windows.Gaming.Input 赛车方向盘 API 进行检测、确定功能、读取力回馈命令并将其发送给赛车方向盘。
ms.assetid: 6287D87F-6F2E-4B67-9E82-3D6E51CBAFF9
ms.date: 05/09/2018
ms.topic: article
keywords: windows 10, uwp, 游戏, 赛车方向盘, 力反馈
ms.localizationpriority: medium
ms.openlocfilehash: 90d12caca103648824ceb36a4ca4968754beb7f2
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8471595"
---
# <a name="racing-wheel-and-force-feedback"></a>赛车方向盘和力回馈

此页介绍使用 [Windows.Gaming.Input.RacingWheel][racingwheel] 和通用 Windows 平台 (UWP) 的相关 API 进行 Xbox One 赛车方向盘编程的基本知识。

在本页中，你将了解如下内容：

* 如何收集连接的赛车方向盘及其用户的列表
* 如何检测是否已添加或删除某个赛车方向盘
* 如何从一个或多个赛车方向盘读取输入
* 如何发送力回馈命令
* 如何像 UI 导航设备一样使用赛车方向盘

## <a name="racing-wheel-overview"></a>赛车方向盘概述

赛车方向盘是与真实的赛车驾驶舱外观类似的输入设备。 对于具有汽车或卡车的街机风格和模拟风格的赛车游戏而言，赛车方向盘是完美的输入设备。 赛车方向盘在 Windows 10 和 Xbox One UWP 应用中受 [Windows.Gaming.Input][] 命名空间支持。

Xbox One 赛车方向盘有多种价位，一般而言，价格越高，输入和力回馈功能的数量越多且越出色。 所有赛车方向盘均配备模拟方向盘、模拟油门和刹车控件，以及一些方向盘上的按钮。 某些赛车方向盘还配备模拟离合器和手刹控件、档位以及力回馈功能。 并非所有赛车方向盘都配备有相同的功能组，对某些功能的支持也可能有所不同。例如，方向盘可能支持不同范围的旋转，档位可能支持不同的档数。

### <a name="device-capabilities"></a>设备功能

不同的 Xbox One 赛车方向盘提供不同的可选设备功能组，并对这些功能提供不同级别的支持，在 [Windows.Gaming.Input][] API 支持的设备当中，单个种类的输入设备之间的这种差异级别是唯一的。 并且，你将会遇到的大多设备都将至少支持某些可选功能或者其他功能变体。 因此，单独确定每个连接的赛车方向盘的功能并支持适合游戏的各种功能非常重要。

有关详细信息，请参阅[确定赛车方向盘功能](#determining-racing-wheel-capabilities)。

### <a name="force-feedback"></a>力回馈

某些 Xbox One 赛车方向盘提供真实的力回馈，即它们可以将实际用力应用于控制轴（例如，它们的方向盘），而不只是简单的震动。 游戏利用此功能打造更出色的沉浸感（_模拟撞毁_、“路感”）并增加出色驾驶的挑战。

有关详细信息，请参阅[力回馈概述](#force-feedback-overview)。

### <a name="ui-navigation"></a>UI 导航

为了减轻对用户界面导航提供不同输入设备支持的负担并促进游戏和设备之间的一致性，大多_物理_输入设备同时会充当单独的被称为 [UI 导航控制器](ui-navigation-controller.md)的_逻辑_输入设备。 UI 导航控制器可跨各种输入设备提供通用的 UI 导航命令词汇。

鉴于他们对模拟控件的独特关注以及不同赛车方向盘之间的差异程度，他们通常配备有数字方向键、**视图**、**菜单**、**A**、**B**、**X** 和 **Y** 按钮，与[游戏板](gamepad-and-vibration.md)上的这些按钮相似。但这些按钮并不用于支持游戏中的命令，也不能像赛车方向盘那样可以随时访问。

作为 UI 导航控制器，赛车方向盘将导航命令的[必需组](ui-navigation-controller.md#required-set)映射为左操纵杆、方向键、**视图**、**菜单**、**A** 和 **B** 按钮。

| 导航命令 | 赛车方向盘输入 |
| ------------------:| ------------------ |
|                 向上 | 方向键向上           |
|               向下 | 方向键向下         |
|               向左 | 方向键向左         |
|              向右 | 方向键向右        |
|               视图 | “视图”按钮        |
|               菜单 | “菜单”按钮        |
|             接受 | A 按钮           |
|             取消 | B 按钮           |

此外，某些赛车方向盘可能还会将导航命令的某些[可选组](ui-navigation-controller.md#optional-set)映射为他们支持的其他输入，但是命令映射会因设备不同而有所差异。 也考虑支持这些命令，但是确保这些命令并非导航你游戏界面的必需命令。

| 导航命令 | 赛车方向盘输入    |
| ------------------:| --------------------- |
|            Page Up | _视情况而定_              |
|          Page Down | _视情况而定_              |
|          Page Left | _视情况而定_              |
|         Page Right | _视情况而定_              |
|          Scroll Up | _视情况而定_              |
|        Scroll Down | _视情况而定_              |
|        Scroll Left | _视情况而定_              |
|       Scroll Right | _视情况而定_              |
|          Context 1 | X 按钮（_通常情况_） |
|          Context 2 | Y 按钮（_通常情况_） |
|          Context 3 | _视情况而定_              |
|          Context 4 | _视情况而定_              |

## <a name="detect-and-track-racing-wheels"></a>检测和跟踪赛车方向盘

检测与跟踪赛车方向盘的工作原理与检测与跟踪游戏板完全相同，除了使用 [RacingWheel][] 类而不是 [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad) 类。 有关详细信息，请参阅[游戏板和振动](gamepad-and-vibration.md)。

<!-- Racing wheels are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected racing wheels and events to notify you when a racing wheel is added or removed.

### The racing wheels list

The [RacingWheel][] class provides a static property, [RacingWheels][], which is a read-only list of racing wheels that are currently connected. Because you might only be interested in some of the connected racing wheels, it's recommended that you maintain your own collection instead of accessing them through the `RacingWheels` property.

The following example copies all connected racing wheels into a new collection.
```cpp
auto myRacingWheels = ref new Vector<RacingWheel^>();

for (auto racingwheel : RacingWheel::RacingWheels)
{
    // This code assumes that you're interested in all racing wheels.
    myRacingWheels->Append(racingwheel);
}
```

### Adding and removing racing wheels

When a racing wheel is added or removed the [RacingWheelAdded][] and [RacingWheelRemoved][] events are raised. You can register handlers for these events to keep track of the racing wheels that are currently connected.

The following example starts tracking an racing wheels that's been added.
```cpp
RacingWheel::RacingWheelAdded += ref new EventHandler<RacingWheel^>(Platform::Object^, RacingWheel^ args)
{
    // This code assumes that you're interested in all new racing wheels.
    myRacingWheels->Append(args);
}
```

The following example stops tracking a racing wheel that's been removed.
```cpp
RacingWheel::RacingWheelRemoved += ref new EventHandler<RacingWheel^>(Platform::Object^, RacingWheel^ args)
{
    unsigned int indexRemoved;

    if(myRacingWheels->IndexOf(args, &indexRemoved))
    {
        myRacingWheels->RemoveAt(indexRemoved);
    }
}
```

### Users and headsets

Each racing wheel can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-racing-wheel"></a>读取赛车方向盘

确定感兴趣的赛车方向盘之后，便可以从赛车方向盘收集输入了。 不过，与你可能已习惯的某些其他输入类型不同，赛车方向盘不会通过触发事件来传达状态的更改。 相反，你需要通过对它们进行_轮询_ 来定期读取其当前状态。

### <a name="polling-the-racing-wheel"></a>轮询赛车方向盘

轮询会捕获赛车方向盘在某个精确时间点的快照。 此输入收集方法对于大多数游戏都非常适合，因为其逻辑通常按确定的循环运行，而不是受事件驱动；而且，相对于随着时间逐个收集许多单个输入解释游戏命令，从一次性收集的输入解释游戏命令，通常会更为简单。

通过调用 [GetCurrentReading][] 轮询赛车方向盘；此函数返回包含赛车方向盘状态的 [RacingWheelReading][]。

下面是轮询赛车方向盘来获取其当前状态的示例。

```cpp
auto racingwheel = myRacingWheels[0];

RacingWheelReading reading = racingwheel->GetCurrentReading();
```

除了赛车方向盘状态之外，每个读数还包括一个时间戳，确切指出检索状态的时间。 该时间戳对于关联之前读数的时间或者游戏模拟的时间非常有用。

### <a name="determining-racing-wheel-capabilities"></a>确定赛车方向盘功能

许多赛车方向盘控件是可选控件，或者即使位于必需控件中也只是支持不同的功能变体，所以你必须单独确定每个赛车方向盘的功能，然后才能处理每个赛车方向盘读数中收集的输入。

可选控件包括手刹、离合器和档位。你可以通过分别读取赛车方向盘的 [HasHandbrake][]、[HasClutch][] 和 [HasPatternShifter][] 属性来确定连接的赛车方向盘是否支持这些控件。 如果属性的值为 **true**，则支持该控件，否则不支持。

```cpp
if (racingwheel->HasHandbrake)
{
    // the handbrake is supported
}

if (racingwheel->HasClutch)
{
    // the clutch is supported
}

if (racingwheel->HasPatternShifter)
{
    // the pattern shifter is supported
}
```

此外，可能会有所不同的控件还有方向盘和档位。 方向盘会因实际方向盘可以支持的物理旋转程度而异，而档位会因其支持的前进档数量的不同而有所差异。 你可以读取赛车方向盘的 `MaxWheelAngle` 属性来确定实际方向盘支持的最大旋转角度，其值是顺时针方向支持的最大物理角度（正向旋转度数），同样，逆时针方向也支持该角度（负向旋转度数）。 你可以读取赛车方向盘的 `MaxPatternShifterGear` 属性来确定档位支持的最大前进档，其值是档位支持的最大前进档（含该档），也就是说，如果值为 4，则档位支持倒档、空挡、一档、二档、三档和四档。

```cpp
auto maxWheelDegrees = racingwheel->MaxWheelAngle;
auto maxShifterGears = racingwheel->MaxPatternShifterGear;
```

最后，某些赛车方向盘通过方向盘支持力回馈。 你可以读取赛车方向盘的 [WheelMotor][] 属性来确定连接的赛车方向盘是否支持力回馈。 如果 `WheelMotor` 不为 **null**，则支持力回馈，否则不支持力回馈。

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    // force feedback is supported
}
```

有关如何使用赛车方向盘支持的力回馈功能的信息，请参阅[力回馈概述](#force-feedback-overview)。

### <a name="reading-the-buttons"></a>读取按钮

每个赛车方向盘按钮（方向键的四个方向、**减档**和**加档**按钮，以及 16 个其他按钮）提供一个数字读数，指出其是按下（向下）还是释放（向上）。 为了提高效率，按钮读数不以单独的布尔值表示；而是全部打包到一个由 [RacingWheelButtons][] 枚举表示的单独位域中。

> [!NOTE]
> 赛车方向盘配备了用于 UI 导航的其他按钮，例如**视图**和**菜单**按钮。 这些按钮不是 `RacingWheelButtons` 枚举的一部分，只能作为 UI 导航设备通过访问赛车方向盘进行读取。 有关详细信息，请参阅 [UI 导航设备](ui-navigation-controller.md)。

从 [RacingWheelReading][] 结构的 `Buttons` 属性中读取按钮值。 由于此属性为位域，因此使用按位掩码隔离你感兴趣的按钮值。 设置相应位时按钮为按下（向下）；否则，按钮为释放（向上）。

以下示例确定是否按下**加档**按钮。

```cpp
if (RacingWheelButtons::NextGear == (reading.Buttons & RacingWheelButtons::NextGear))
{
    // Next Gear is pressed
}
```

以下示例确定是否释放“加档”按钮。

```cpp
if (RacingWheelButtons::None == (reading.Buttons & RacingWheelButtons::NextGear))
{
    // Next Gear is released (not pressed)
}
```

有时你可能需要确定：何时将按钮从按下转换为释放或从释放转换为按下，是按下还是释放多个按钮，或者是否按特定方式安排一组按钮（按下一些按钮，释放一些按钮）。 有关如何检测这些状态的详细信息，请参阅[检测按钮转换](input-practices-for-games.md#detecting-button-transitions) 和[检测复杂按钮安排](input-practices-for-games.md#detecting-complex-button-arrangements)。

### <a name="reading-the-wheel"></a>读取方向盘

方向盘是必需控件，提供介于 -1.0 和 +1.0 之间的模拟读数。 值 -1.0 对应于方向盘最左的位置；值 +1.0 对应于最右的位置。 从 [RacingWheelReading][] 结构的 `Wheel` 属性中读取方向盘的值。

```cpp
float wheel = reading.Wheel;  // returns a value between -1.0 and +1.0.
```

尽管对应于实际方向盘中不同物理旋转程度的方向盘读数具体取决于物理赛车方向盘所支持的旋转范围，但你通常也不想对方向盘读数进行刻度划分；支持更高旋转程度的方向盘提供的精确度也就更高。

### <a name="reading-the-throttle-and-brake"></a>读取油门和刹车

油门和刹车为必需控件，每个控件会提供一个介于 0.0（完全释放）和 1.0（完全按下）的模拟读数，用浮点值表示。 油门控件的值通过 [RacingWheelReading][] 结构的 `Throttle` 属性读取，刹车控件的值通过 `Brake` 属性来读取。

```cpp
float throttle = reading.Throttle;  // returns a value between 0.0 and 1.0
float brake    = reading.Brake;     // returns a value between 0.0 and 1.0
```

### <a name="reading-the-handbrake-and-clutch"></a>读取手刹和离合器

手刹和离合器为可选控件，每个控件会提供一个介于 0.0（完全释放）和 1.0（完全占用）的模拟读数，用浮点值表示。 手刹的值通过 [RacingWheelReading][] 结构的 `Handbrake` 属性读取，离合器控件的值通过 `Clutch` 属性来读取。

```cpp
float handbrake = 0.0;
float clutch = 0.0;

if(racingwheel->HasHandbrake)
{
    handbrake = reading.Handbrake;  // returns a value between 0.0 and 1.0
}

if(racingwheel->HasClutch)
{
    clutch = reading.Clutch;        // returns a value between 0.0 and 1.0
}
```

### <a name="reading-the-pattern-shifter"></a>读取档位

档位是可选控件，会提供一个介于 -1 和 [MaxPatternShifterGear][] 之间的数字读数，用带符号的整数值表示。 值 -1 或 0 分别对应于_倒档_ 和_空挡_，递增的正值对应于越来越高的前进挡（最高 [MaxPatternShifterGear][] 档，含该档）。 档位值通过 [RacingWheelReading][] 结构的 `PatternShifterGear` 属性读取。

```cpp
if (racingwheel->HasPatternShifter)
{
    gear = reading.PatternShifterGear;
}
```

> [!NOTE]
> 支持的档位位于必需的**减档**和**加档**按钮旁边（还会影响游戏玩家汽车的当前档位）。 统一这些输入（两者都存在）的简单策略就是，当游戏玩家将汽车选择为自动档时忽略档位（和离合器），以及当游戏玩家将汽车选择为手动档时忽略**加档**和**减档**按钮。这仅在赛车方向盘配备有档位控件时适用。 如果这对你的游戏不适用，你可以采用不同的统一策略。

## <a name="run-the-inputinterfacing-sample"></a>运行 InputInterfacing 示例

[InputInterfacingUWP 示例 _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) 演示了如何将赛车方向盘和不同类型的输入设备配合使用，以及这些输入设备如何像 UI 导航控制器那样运行。

## <a name="force-feedback-overview"></a>力回馈概述

很多赛车方向盘有力回馈功能，可提供更具沉浸感且更富有挑战性的驾驶体验。 支持力回馈的赛车方向盘通常配备有一个电机，它可以将用力沿单个轴（方向盘旋转轴）应用于方向盘。 力回馈在 Windows 10 和 Xbox One UWP 应用中受 [Windows.Gaming.Input.ForceFeedback][] 命名空间支持。

> [!NOTE]
> 力回馈 API 能够支持多个力轴，但是当前没有 Xbox One 赛车方向盘支持除方向盘旋转反馈轴以外的任何反馈轴。

## <a name="using-force-feedback"></a>使用力回馈

这些章节介绍了对 Xbox One 赛车方向盘力回馈效果进行编程的基本知识。 反馈利在使用效果时适用，它首先加载到力回馈设备中，然后以类似于声效的方式启动、暂停、恢复和停止。但是，你必需先确定赛车方向盘的回馈功能。

### <a name="determining-force-feedback-capabilities"></a>确定力回馈功能

你可以读取赛车方向盘的 [WheelMotor][] 属性来确定连接的赛车方向盘是否支持力回馈。 如果 `WheelMotor` 为 **null**，则不支持力回馈，否则支持力回馈。并且，你可以继续确定电机的特定反馈功能，例如它可以影响的轴。

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    auto axes = racingwheel->WheelMotor->SupportedAxes;

    if(ForceFeedbackEffectAxes::X == (axes & ForceFeedbackEffectAxes::X))
    {
        // Force can be applied through the X axis
    }

    if(ForceFeedbackEffectAxes::Y == (axes & ForceFeedbackEffectAxes::Y))
    {
        // Force can be applied through the Y axis
    }

    if(ForceFeedbackEffectAxes::Z == (axes & ForceFeedbackEffectAxes::Z))
    {
        // Force can be applied through the Z axis
    }
}
```

### <a name="loading-force-feedback-effects"></a>加载力回馈效果

力回馈效果被加载到回馈设备中，此设备在遇到游戏命令时自主“执行”。 提供了一些基本效果，可以通过实现 [IForceFeedbackEffect][] 接口的类创建自定义效果。

| 效果类         | 效果描述                                                                     |
| -------------------- | -------------------------------------------------------------------------------------- |
| ConditionForceEffect | 应用变力以响应设备内当前传感器的效果。 |
| ConstantForceEffect  | 沿矢量应用恒力的效果。                                  |
| PeriodicForceEffect  | 沿矢量应用由波形定义的变力的效果。           |
| RampForceEffect      | 沿矢量应用线性递增/递减的力的效果。          |

```cpp
using FFLoadEffectResult = ForceFeedback::ForceFeedbackLoadEffectResult;

auto effect = ref new Windows.Gaming::Input::ForceFeedback::ConstantForceEffect();
auto time = TimeSpan(10000);

effect->SetParameters(Windows::Foundation::Numerics::float3(1.0f, 0.0f, 0.0f), time);

// Here, we assume 'racingwheel' is valid and supports force feedback

IAsyncOperation<FFLoadEffectResult>^ request
    = racingwheel->WheelMotor->LoadEffectAsync(effect);

auto loadEffectTask = Concurrency::create_task(request);

loadEffectTask.then([this](FFLoadEffectResult result)
{
    if (FFLoadEffectResult::Succeeded == result)
    {
        // effect successfully loaded
    }
    else
    {
        // effect failed to load
    }
}).wait();
```

### <a name="using-force-feedback-effects"></a>使用力回馈效果

加载后，可以通过调用赛车方向盘 `WheelMotor` 属性上的函数来启动、暂停、恢复和停止效果，或者通过调用反馈效果本身上的函数来逐个启动、暂停、恢复和停止。 通常，你应在游戏开始前将所有想要使用的效果加载到回馈设备中，然后随着游戏的进行使用相应的 `SetParameters` 函数更新效果。

```cpp
if (ForceFeedbackEffectState::Running == effect->State)
{
    effect->Stop();
}
else
{
    effect->Start();
}
```

最后，你可以随时在特定赛车方向盘上异步启用、禁用或者重置整个力回馈系统。

## <a name="see-also"></a>另请参阅

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [游戏输入实践](input-practices-for-games.md)

[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[racingwheel]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.aspx
[racingwheels]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.racingwheels.aspx
[racingwheeladded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.racingwheeladded.aspx
[racingwheelremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.racingwheelremoved.aspx
[haspatternshifter]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.haspatternshifter.aspx
[hashandbrake]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.hashandbrake.aspx
[hasclutch]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.hasclutch.aspx
[maxpatternshiftergear]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.maxpatternshiftergear.aspx
[maxwheelangle]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.maxwheelangle.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.getcurrentreading.aspx
[wheelmotor]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.wheelmotor.aspx
[racingwheelreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheelreading.aspx
[racingwheelbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheelbuttons.aspx

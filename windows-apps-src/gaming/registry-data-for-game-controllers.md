---
author: eliotcowley
title: 游戏控制器注册表数据
description: 了解可以添加到电脑注册表中以使控制器能够在 UWP 游戏中使用的数据。
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.author: wdg-dev-content
ms.date: 06/25/2018
ms.topic: article
keywords: windows 10, uwp, 游戏, 输入, 注册表, 自定义
ms.localizationpriority: medium
ms.openlocfilehash: 4bbd4074c52514b9cb66fd6f2dd189421f61d5ee
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6032057"
---
# <a name="registry-data-for-game-controllers"></a>游戏控制器注册表数据

> [!NOTE]
> 本主题适用于与 Windows 10 兼容的游戏控制器的制造商，不适用于大多数开发人员。

利用 [Windows.Gaming.Input 命名空间](https://docs.microsoft.com/uwp/api/windows.gaming.input)，独立硬件供应商 (IHV) 可以将数据添加到电脑的注册表中，从而使其设备能够相应地显示为 [Gamepads](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad)、[RacingWheels](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheel)、[ArcadeSticks](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick)、[FlightSticks](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.flightstick) 和 [UINavigationControllers](https://docs.microsoft.com/uwp/api/windows.gaming.input.uinavigationcontroller)。 所有 IHV 都应该为其兼容的控制器添加此数据。 通过执行此操作，所有 UWP 游戏（以及任何使用 WinRT API 的桌面游戏）都将能够支持游戏控制器。

## <a name="mapping-scheme"></a>映射方案

系统将从注册表中的以下位置读出具有供应商 ID (VID) **VVVV**、产品 ID (PID) **PPPP**、用法页面 **UUUU** 和用法 ID **XXXX** 的设备的映射：

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\VVVVPPPPUUUUXXXX`

下表说明了设备根位置下的预期值：

<table>
    <tr>
        <th>名称</th>
        <th>类型</th>
        <th>是否必需？</th>
        <th>信息</th>
    </tr>
    <tr>
        <td>Disabled</td>
        <td>DWORD</td>
        <td>否</td>
        <td>
            <p>指示此特定设备应该处于禁用状态。</p>
            <ul>
                <li><b>0</b>：未禁用设备。</li>
                <li><b>1</b>：已禁用设备。</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>Description</td>
        <td>REG_SZ <td>否</td>
        <td>设备的简短说明。</td>
    </tr>
</table>

设备安装程序应该会（通过设置或 [INF 文件](https://docs.microsoft.com/windows-hardware/drivers/install/inf-files)）将此数据添加至注册表。

以下部分中详细说明了设备根位置下面的子项。

### <a name="gamepad"></a>Gamepad

下表列出了 **Gamepad** 子项下面的必需子项和可选子项：

<table>
    <tr>
        <th>子项</th>
        <th>是否必需？</th>
        <th>信息</th>
    </tr>
    <tr>
        <td>Menu</td>
        <td>是</td>
        <td rowspan="18" style="vertical-align: middle;">请参阅<a href="#button-mapping">按钮映射</a></td>
    </tr>
    <tr>
        <td>View</td>
        <td>是</td>
    </tr>
    <tr>
        <td>A</td>
        <td>是</td>
    </tr>
    <tr>
        <td>B</td>
        <td>是</td>
    </tr>
    <tr>
        <td>X</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Y</td>
        <td>是</td>
    </tr>
    <tr>
        <td>LeftShoulder</td>
        <td>是</td>
    </tr>
    <tr>
        <td>RightShoulder</td>
        <td>是</td>
    </tr>
    <tr>
        <td>LeftThumbstickButton</td>
        <td>是</td>
    </tr>
    <tr>
        <td>RightThumbstickButton</td>
        <td>是</td>
    </tr>
    <tr>
        <td>DPadUp</td>
        <td>是</td>
    </tr>
    <tr>
        <td>DPadDown</td>
        <td>是</td>
    </tr>
    <tr>
        <td>DPadLeft</td>
        <td>是</td>
    </tr>
    <tr>
        <td>DPadRight</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Paddle1</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Paddle2</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Paddle3</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Paddle4</td>
        <td>否</td>
    </tr>
    <tr>
        <td>LeftTrigger</td>
        <td>是</td>
        <td rowspan="6" style="vertical-align: middle;">请参阅<a href="#axis-mapping">轴映射</a></td>
    </tr>
    <tr>
        <td>RightTrigger</td>
        <td>是</td>
    </tr>
    <tr>
        <td>LeftThumbstickX</td>
        <td>是</td>
    </tr>
    <tr>
        <td>LeftThumbstickY</td>
        <td>是</td>
    </tr>
    <tr>
        <td>RightThumbstickX</td>
        <td>是</td>
    </tr>
    <tr>
        <td>RightThumbstickY</td>
        <td>是</td>
    </tr>
</table>

> [!NOTE]
> 如果你将游戏控制器添加为受支持的 **Gamepad**，那么我们强烈建议你也将其添加为受支持的 **UINavigationController**。

### <a name="racingwheel"></a>RacingWheel

下表列出了 **RacingWheel** 子项下面的必需子项和可选子项：

<table>
    <tr>
        <th>子项</th>
        <th>是否必需？</th>
        <th>信息</th>
    </tr>
    <tr>
        <td>PreviousGear</td>
        <td>是</td>
        <td rowspan="30" style="vertical-align: middle;">请参阅<a href="#button-mapping">按钮映射</a></td>
    </tr>
    <tr>
        <td>NextGear</td>
        <td>是</td>
    </tr>
    <tr>
        <td>DPadUp</td>
        <td>否</td>
    </tr>
    <tr>
        <td>DPadDown</td>
        <td>否</td>
    </tr>
    <tr>
        <td>DPadLeft</td>
        <td>否</td>
    </tr>
    <tr>
        <td>DPadRight</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button1</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button2</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button3</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button4</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button5</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button6</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button7</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button8</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button9</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button10</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button11</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button12</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button13</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button14</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button15</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button16</td>
        <td>否</td>
    </tr>
    <tr>
        <td>FirstGear</td>
        <td>否</td>
    </tr>
    <tr>
        <td>SecondGear</td>
        <td>否</td>
    </tr>
    <tr>
        <td>ThirdGear</td>
        <td>否</td>
    </tr>
    <tr>
        <td>FourthGear</td>
        <td>否</td>
    </tr>
    <tr>
        <td>FifthGear</td>
        <td>否</td>
    </tr>
    <tr>
        <td>SixthGear</td>
        <td>否</td>
    </tr>
    <tr>
        <td>SeventhGear</td>
        <td>否</td>
    </tr>
    <tr>
        <td>ReverseGear</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Wheel</td>
        <td>是</td>
        <td rowspan="5" style="vertical-align: middle;">请参阅<a href="#axis-mapping">轴映射</a></td>
    </tr>
    <tr>
        <td>Throttle</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Brake</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Clutch</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Handbrake</td>
        <td>否</td>
    </tr>
    <tr>
        <td>MaxWheelAngle</td>
        <td>是</td>
        <td>请参阅<a href="#properties-mapping">属性映射</a></td>
    </tr>
</table>

### <a name="arcadestick"></a>ArcadeStick

下表列出了 **ArcadeStick** 子项下面的必需子项和可选子项：

<table>
    <tr>
        <th>子项</th>
        <th>是否必需？</th>
        <th>信息</th>
    </tr>
    <tr>
        <td>Action1</td>
        <td>是</td>
        <td rowspan="12" style="vertical-align: middle;">请参阅<a href="#button-mapping">按钮映射</a></td>
    </tr>
    <tr>
        <td>Action2</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Action3</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Action4</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Action5</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Action6</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Special1</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Special2</td>
        <td>是</td>
    </tr>
    <tr>
        <td>StickUp</td>
        <td>是</td>
    </tr>
    <tr>
        <td>StickDown</td>
        <td>是</td>
    </tr>
    <tr>
        <td>StickLeft</td>
        <td>是</td>
    </tr>
    <tr>
        <td>StickRight</td>
        <td>是</td>
    </tr>
</table>

### <a name="flightstick"></a>FlightStick

下表列出了 **FlightStick** 子项下面的必需子项和可选子项：

<table>
    <tr>
        <th>子项</th>
        <th>是否必需？</th>
        <th>信息</th>
    </tr>
    <tr>
        <td>FirePrimary</td>
        <td>是</td>
        <td rowspan="2" style="vertical-align: middle;">请参阅<a href="#button-mapping">按钮映射</a></td>
    </tr>
    <tr>
        <td>FireSecondary</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Roll</td>
        <td>是</td>
        <td rowspan="4" style="vertical-align: middle;">请参阅<a href="#axis-mapping">轴映射</a></td>
    </tr>
    <tr>
        <td>Pitch</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Yaw</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Throttle</td>
        <td>是</td>
    </tr>
    <tr>
        <td>HatSwitch</td>
        <td>是</td>
        <td>请参阅<a href="#switch-mapping">开关映射</a></td>
    </tr>
</table>

### <a name="uinavigation"></a>UINavigation

下表列出了 **UINavigation** 子项下面的必需子项和可选子项：

<table>
    <tr>
        <th>子项</th>
        <th>是否必需？</th>
        <th>信息</th>
    </tr>
    <tr>
        <td>Menu</td>
        <td>是</td>
        <td rowspan="24" style="vertical-align: middle;">请参阅<a href="#button-mapping">按钮映射</a></td>
    </tr>
    <tr>
        <td>View</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Accept</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Cancel</td>
        <td>是</td>
    </tr>
    <tr>
        <td>PrimaryUp</td>
        <td>是</td>
    </tr>
    <tr>
        <td>PrimaryDown</td>
        <td>是</td>
    </tr>
    <tr>
        <td>PrimaryLeft</td>
        <td>是</td>
    </tr>
    <tr>
        <td>PrimaryRight</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Context1</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Context2</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Context3</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Context4</td>
        <td>否</td>
    </tr>
    <tr>
        <td>PageUp</td>
        <td>否</td>
    </tr>
    <tr>
        <td>PageDown</td>
        <td>否</td>
    </tr>
    <tr>
        <td>PageLeft</td>
        <td>否</td>
    </tr>
    <tr>
        <td>PageRight</td>
        <td>否</td>
    </tr>
    <tr>
        <td>ScrollUp</td>
        <td>否</td>
    </tr>
    <tr>
        <td>ScrollDown</td>
        <td>否</td>
    </tr>
    <tr>
        <td>ScrollLeft</td>
        <td>否</td>
    </tr>
    <tr>
        <td>ScrollRight</td>
        <td>否</td>
    </tr>
    <tr>
        <td>SecondaryUp</td>
        <td>否</td>
    </tr>
    <tr>
        <td>SecondaryDown</td>
        <td>否</td>
    </tr>
    <tr>
        <td>SecondaryLeft</td>
        <td>否</td>
    </tr>
    <tr>
        <td>SecondaryRight</td>
        <td>否</td>
    </tr>
</table>

有关 UI 导航控制器和上述命令的详细信息，请参阅 [UI 导航控制器](https://docs.microsoft.com/windows/uwp/gaming/ui-navigation-controller)。

## <a name="keys"></a>项

以下部分介绍 **Gamepad**、**RacingWheel**、**ArcadeStick**、**FlightStick** 和 **UINavigation** 项下面每个子项的内容。

### <a name="button-mapping"></a>按钮映射

下表列出了映射按钮所需的值。 例如，如果按游戏控制器上的 **DPadUp**，则 **DPadUp** 的映射应包含 **ButtonIndex** 值（**源**是 **Button**)。 如果需要从某个开关位置映射 **DPadUp**，则 **DPadUp** 映射应包含值 **SwitchIndex** 和 **SwitchPosition**（**源**是 **Switch**）。

<table>
    <tr>
        <th>源</th>
        <th>值名称</th>
        <th>值类型</th>
        <th>是否必需？</th>
        <th>值信息</th>
    </tr>
    <tr>
        <td>Button</td>
        <td>ButtonIndex</td>
        <td>DWORD</td>
        <td>是</td>
        <td><b>RawGameController</b> 按钮阵列中的索引。</td>
    </tr>
    <tr>
        <td rowspan="4" style="vertical-align: middle;">Axis</td>
        <td>AxisIndex</td>
        <td>DWORD</td>
        <td>是</td>
        <td><b>RawGameController</b> 轴阵列中的索引。</td>
    </tr>
    <tr>
        <td>Invert</td>
        <td>DWORD</td>
        <td>否</td>
        <td>指明在应用 <b>ThresholdPercent</b> 和 <b>DebouncePercent</b> 系数之前应该反转轴值。</td>
    </tr>
    <tr>
        <td>ThresholdPercent</td>
        <td>DWORD</td>
        <td>是</td>
        <td>指明轴位置，在此位置，映射的按钮值会在按下状态和释放状态之间进行转换。 值的有效范围是 0 至 100。 如果轴值大于或等于此值，则认为按下了按钮。</td>
    </tr>
    <tr>
        <td>DebouncePercent</td>
        <td>DWORD</td>
        <td>是</td>
        <td>
            <p>定义接近于 <b>ThresholdPercent</b> 值（用于防止对报告的按钮状态误操作）的窗口大小。 值的有效范围是 0 至 100。 只有在轴值超过了防止误动作窗口的上边界或下边界时，才会发生按钮状态转换。 例如，如果 <b>ThresholdPercent</b> 为 50，<b>DebouncePercent</b> 为 10，则会按完整范围轴值的 45% 和 55% 生成防止误动作边界。 直到轴值达到 55% 或更高，按钮才能转换成按下状态；直到轴值达到 45% 或更低，按钮才能转换回释放状态。</p>
            <p>计算的防止误动作窗口边界固定在 0% 到 100% 之间。 例如，阈值 5% 和防止误动作窗口值 20% 会导致防止误动作窗口边界下降为 0% 和 15%。 轴值 0% 和 100% 的按钮状态始终分别报告为释放和按下，与阈值和防止误动作值无关。</p>
        </td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Switch</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>是</td>
        <td><b>RawGameController</b> 开关阵列中的索引。</td>
    </tr>
    <tr>
        <td>SwitchPosition</td>
        <td>REG_SZ</td>
        <td>是</td>
        <td>
            <p>指示将导致映射的按钮报告自己正在被按下的开关位置。 位置值可以为以下字符串之一：</p>
            <ul>
                <li>Up</li> 
                <li>UpRight</li>
                <li>Right</li>
                <li>DownRight</li>
                <li>Down</li>
                <li>DownLeft</li>
                <li>Left</li>
                <li>UpLeft</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>IncludeAdjacent</td>
        <td>DWORD</td>
        <td>否</td>
        <td>指示相邻开关位置也将导致映射的按钮报告自己正在被按下。</td>
    </tr>
</table>

### <a name="axis-mapping"></a>轴映射

下表列出了映射轴所需的值：

<table>
    <tr>
        <th>源</th>
        <th>值名称</th>
        <th>值类型</th>
        <th>是否必需？</th>
        <th>值信息</th>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">Button</td>
        <td>MaxValueButtonIndex</td>
        <td>DWORD</td>
        <td>是</td>
        <td>
            <p>转换为所映射单向轴值的 <b>RawGameController</b> 按钮阵列中的索引。</p>
            <table>
                <tr>
                    <th>MaxButton</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>1.0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>MinValueButtonIndex</td>
        <td>DWORD</td>
        <td>否</td>
        <td>
            <p>指示映射的轴为双向。 <b>MaxButton</b> 和 <b>MinButton</b> 的值会合并到单个双向轴中，如下所示。</p>
            <table>
                <tr>
                    <th>MinButton</th>
                    <th>MaxButton</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>FALSE</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>TRUE</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>FALSE</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>TRUE</td>
                    <td>0.5</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">Axis</td>
        <td>AxisIndex</td>
        <td>DWORD</td>
        <td>是</td>
        <td><b>RawGameController</b> 轴阵列中的索引。</td>
    </tr>
    <tr>
        <td>Invert</td>
        <td>DWORD</td>
        <td>否</td>
        <td>指示在返回映射的轴值之前应该反转该值。</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Switch</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>是</td>
        <td><b>RawGameController</b> 开关阵列中的索引。
    </tr>
    <tr>
        <td>MaxValueSwitchPosition</td>
        <td>REG_SZ</td>
        <td>是</td>
        <td>
            <p>以下字符串之一：</p>
            <ul>
                <li>Up</li>
                <li>UpRight</li>
                <li>Right</li>
                <li>DownRight</li>
                <li>Down</li>
                <li>DownLeft</li>
                <li>Left</li>
                <li>UpLeft</li>
            </ul>
            <p>它指示导致将映射的轴值报告为 1.0 的开关位置。 <b>MaxValueSwitchPosition</b> 的相反方向被视为 0.0。 例如，如果 <b>MaxValueSwitchPosition</b> 是 <b>Up</b>，则轴值的转换如下所示：</p>
            <table>
                <tr>
                    <th>开关位置</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>Up</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>Center</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>Down</td>
                    <td>0.0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>IncludeAdjacent</td>
        <td>DWORD</td>
        <td>否</td>
        <td>
            <p>指示相邻开关位置也将导致映射的轴报告 1.0。 在以上示例中，如果设置了 <b>IncludeAdjacent</b>，则按如下方式完成轴转换：</p>
            <table>
                <tr>
                    <th>开关位置</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>Up</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>UpRight</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>UpLeft</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>Center</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>Down</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>0.0</td>
                </tr>
            </table>
        </td>
    </tr>
</table>

### <a name="switch-mapping"></a>开关映射

可以从 **RawGameController** 按钮阵列中的一组按钮或开关阵列中的索引映射开关位置。 不能从轴映射开关位置。

<table>
    <tr>
        <th>源</th>
        <th>值名称</th>
        <th>值类型</th>
        <th>值信息</th>
    </tr>
    <tr>
        <td rowspan="10" style="vertical-align: middle;">Button</td>
        <td>ButtonCount</td>
        <td>DWORD</td>
        <td>2、4 或 8</td>
    </tr>
    <tr>
        <td>SwitchKind</td>
        <td>REG_SZ</td>
        <td><b>TwoWay</b>、<b>FourWay</b> 或 <b>EightWay</b>
    </tr>
    <tr>
        <td>UpButtonIndex</td>
        <td>DWORD</td>
        <td rowspan="8" style="vertical-align: middle;">请参阅 <a href="#buttonindex-values">*ButtonIndex 值</a></td>
    </tr>
    <tr>
        <td>DownButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>LeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>RightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>UpRightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>DownRightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>DownLeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>UpLeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="9" style="vertical-align: middle;">Axis</td>
        <td>SwitchKind</td>
        <td>REG_SZ</td>
        <td><b>TwoWay</b>、<b>FourWay</b> 或 <b>EightWay</b></td>
    </tr>
    <tr>
        <td>XAxisIndex</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;"><b>YAxisIndex</b> 始终存在。 只有当 <b>SwitchKind</b> 是 <b>FourWay</b> 或 <b>EightWay</b> 时，才存在 <b>XAxisIndex</b>。</td>
    </tr>
    <tr>
        <td>YAxisIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDeadZonePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">指示大约在轴中心位置的死区的大小。</td>
    </tr>
    <tr>
        <td>YDeadZonePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDebouncePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">定义接近于死区上限和下限（用于防止对报告的开关状态误操作）的窗口大小。</td>
    </tr>
    <tr>
        <td>YDebouncePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XInvert</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">指示应该在应用死区和防止误动作窗口计算之前反转相应的轴值。</td>
    </tr>
    <tr>
        <td>YInvert</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Switch</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td><b>RawGameController</b> 开关阵列中的索引。
    </tr>
    <tr>
        <td>Invert</td>
        <td>DWORD</td>
        <td>指示开关以逆时针顺序而不是默认的顺时针顺序来报告其位置。</td>
    </tr>
    <tr>
        <td>PositionBias</td>
        <td>DWORD</td>
        <td>
            <p>将位置报告方式的起点移动指定的距离。 系统始终从原始起点按顺时针方向计算 <b>PositionBias</b>，并在颠倒值顺序之前对其进行应用。</p>
            <p>例如，通过设置 <b>Invert</b> 标志，并将 <b>PositionBias</b> 指定为 5，可以使一个按逆时针顺序报告以 <b>DownRight</b> 开头的位置的开关标准化：</p>
            <table>
                <tr>
                    <th>位置</th>
                    <th>报告的值</th>
                    <th>在 PositionBias 和 Invert 标志之后</th>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0</td>
                    <td>3</td>
                </tr>
                <tr>
                    <td>Right</td>
                    <td>1</td>
                    <td>2</td>
                </tr>
                <tr>
                    <td>UpRight</td>
                    <td>2</td>
                    <td>1</td>
                </tr>
                <tr>
                    <td>Up</td>
                    <td>3</td>
                    <td>0</td>
                </tr>
                <tr>
                    <td>UpLeft</td>
                    <td>4</td>
                    <td>7</td>
                </tr>
                <tr>
                    <td>Left</td>
                    <td>5</td>
                    <td>6</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>6</td>
                    <td>5</td>
                </tr>
                <tr>
                    <td>Down</td>
                    <td>7</td>
                    <td>4</td>
                </tr>
            </table>
    </tr>
</table>

#### <a name="buttonindex-values"></a>*ButtonIndex 值

\***RawGameController** 的按钮阵列中的 ButtonIndex 值索引：

<table>
    <tr>
        <th>ButtonCount</th>
        <th>SwitchKind</th>
        <th>RequiredMappings</th>
    </tr>
    <tr>
        <td>2</td>
        <td>TwoWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>4</td>
        <td>FourWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>4</td>
        <td>EightWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>8</td>
        <td>EightWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
                <li>UpRightButtonIndex</li>
                <li>DownRightButtonIndex</li>
                <li>DownLeftButtonIndex</li>
                <li>UpLeftButtonIndex</li>
            </ul>
        </td>
    </tr>
</table>

### <a name="properties-mapping"></a>属性映射

以下是不同映射类型的静态映射值。

<table>
    <tr>
        <th>映射</th>
        <th>值名称</th>
        <th>值类型</th>
        <th>值信息</th>
    </tr>
    <tr>
        <td>RacingWheel</td>
        <td>MaxWheelAngle</td>
        <td>DWORD</td>
        <td>指示滚轮沿一个方向支持的最大物理滚轮角度。 例如，如果滚轮可以在 -90 度至 90 度之间旋转，则应指定 90。</td>
    </tr>
</table>

## <a name="labels"></a>标签

标签应存在于设备根位置下的 **Labels** 项下面。 **Labels** 可能有以下 3 个子项：**Buttons**、**Axes** 和 **Switches**。

### <a name="button-labels"></a>按钮标签

**Buttons** 项将 **RawGameController** 按钮阵列中的每个按钮位置都映射到字符串。 每个字符串都在内部映射到相应的 [GameControllerButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel) 枚举值。 例如，如果游戏板有 10 个按钮，并且 **RawGameController** 分析出按钮并在按钮报告中呈现按钮的顺序如下所示：

```cpp
Menu,               // Index 0
View,               // Index 1
LeftStickButton,    // Index 2
RightStickButton,   // Index 3
LetterA,            // Index 4
LetterB,            // Index 5
LetterX,            // Index 6
LetterY,            // Index 7
LeftBumper,         // Index 8
RightBumper         // Index 9
```

则标签应该按此顺序显示在 **Buttons** 项下面：

<table>
    <tr>
        <th>名称</th>
        <th>值（类型：REG_SZ）</th>
    </tr>
    <tr>
        <td>Button0</td>
        <td>Menu</td>
    </tr>
    <tr>
        <td>Button1</td>
        <td>View</td>
    </tr>
    <tr>
        <td>Button2</td>
        <td>LeftStickButton</td>
    </tr>
    <tr>
        <td>Button3</td>
        <td>RightStickButton</td>
    </tr>
    <tr>
        <td>Button4</td>
        <td>LetterA</td>
    </tr>
    <tr>
        <td>Button5</td>
        <td>LetterB</td>
    </tr>
    <tr>
        <td>Button6</td>
        <td>LetterX</td>
    </tr>
    <tr>
        <td>Button7</td>
        <td>LetterY</td>
    </tr>
    <tr>
        <td>Button8</td>
        <td>LeftBumper</td>
    </tr>
    <tr>
        <td>Button9</td>
        <td>RightBumper</td>
    </tr>
</table>

### <a name="axis-labels"></a>轴标签

就像按钮标签中的情况一样，**Axes** 项会将 **RawGameController** 轴阵列中的每个轴位置映射到 [GameControllerButtonLabel 枚举](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel)中列出的其中一个标签。 请参阅[按钮标签](#button-labels)中的示例。

### <a name="switch-labels"></a>开关标签

**Switches** 项将开关位置映射到标签。 值遵循此命名约定：若要标出其索引在 **RawGameController** 开关阵列中为 *x* 的开关的位置，请将以下值添加到 **Switches** 子项下面： 

* SwitchxUp 
* SwitchxUpRight 
* SwitchxRight
* SwitchxDownRight
* SwitchxDown
* SwitchxDownLeft
* SwitchxUpLeft
* SwitchxLeft

下表显示了一个 4 向开关的开关位置示例标签集，该开关显示在 **RawGameController** 中的索引 0 处： 

<table>
    <tr>
        <th>名称</th>
        <th>值（类型：REG_SZ）</th>
    </tr>
    <tr>
        <td>Switch0Up</td>
        <td>XboxUp</td>
    </tr>
    <tr>
        <td>Switch0Right</td>
        <td>XboxRight</td>
    </tr>
    <tr>
        <td>Switch0Down</td>
        <td>XboxDown</td>
    </tr>
    <tr>
        <td>Switch0Left</td>
        <td>XboxLeft</td>
    </tr>
</table>

<!--### Label strings

* XboxBack
* XboxStart
* XboxMenu
* XboxView
* XboxUp
* XboxDown
* XboxLeft
* XboxRight
* XboxA
* XboxB
* XboxX
* XboxY
* XboxLeftBumper
* XboxLeftTrigger
* XboxLeftStickButton
* XboxRightBumper
* XboxRightTrigger
* XboxRightStickButton
* XboxPaddle1
* XboxPaddle2
* XboxPaddle3
* XboxPaddle4
* Mode
* Select
* Menu
* View
* Back
* Start
* Options
* Share
* Up
* Down
* Left
* Right
* LetterA
* LetterB
* LetterC
* LetterL
* LetterR
* LetterX
* LetterY
* LetterZ
* Cross
* Circle
* Square
* Triangle
* LeftBumper
* LeftTrigger
* LeftStickButton
* Left1
* Left2
* Left3
* RightBumper
* RightTrigger
* RightStickButton
* Right1
* Right2
* Right3
* Paddle1
* Paddle2
* Paddle3
* Paddle4
* Plus
* Minus
* DownLeftArrow
* DialLeft
* DialRight
* Suspension-->

## <a name="example-registry-file"></a>示例注册表文件

下面是通用 **RacingWheel** 的示例注册表文件，用于显示所有这些映射和值汇集在一起的方式：

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004]
"Description" = "Example Wheel Device"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\Labels\Buttons]
"Button0" = "LetterA"
"Button1" = "LetterB"
"Button2" = "LetterX"
"Button3" = "LetterY"
"Button6" = "Menu"
"Button7" = "View"
"Button8" = "RightStickButton"
"Button9" = "LeftStickButton"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\Labels\Switches]
"Switch0Down" = "Down"
"Switch0Left" = "Left"
"Switch0Right" = "Right"
"Switch0Up" = "Up"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel]
"MaxWheelAngle" = dword:000001c2

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Brake]
"AxisIndex" = dword:00000002
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button1]
"ButtonIndex" = dword:00000000

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button2]
"ButtonIndex" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button3]
"ButtonIndex" = dword:00000002

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button4]
"ButtonIndex" = dword:00000003

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button5]
"ButtonIndex" = dword:00000009

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button6]
"ButtonIndex" = dword:00000008

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button7]
"ButtonIndex" = dword:00000007

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button8]
"ButtonIndex" = dword:00000006

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Clutch]
"AxisIndex" = dword:00000003
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadDown]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Down"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadLeft]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Left"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadRight]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Right"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadUp]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Up"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FifthGear]
"ButtonIndex" = dword:00000010

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FirstGear]
"ButtonIndex" = dword:0000000c

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FourthGear]
"ButtonIndex" = dword:0000000f

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\NextGear]
"ButtonIndex" = dword:00000004

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\PreviousGear]
"ButtonIndex" = dword:00000005

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\ReverseGear]
"ButtonIndex" = dword:0000000b

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\SecondGear]
"ButtonIndex" = dword:0000000d

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\SixthGear]
"ButtonIndex" = dword:00000011

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\ThirdGear]
"ButtonIndex" = dword:0000000e

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Throttle]
"AxisIndex" = dword:00000001
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Wheel]
"AxisIndex" = dword:00000000
"Invert" = dword:00000000
```

## <a name="see-also"></a>另请参阅

* [Windows.Gaming.Input 命名空间](https://docs.microsoft.com/uwp/api/windows.gaming.input)
* [Windows.Gaming.Input.Custom 命名空间](https://docs.microsoft.com/uwp/api/windows.gaming.input.custom)
* [INF 文件](https://docs.microsoft.com/windows-hardware/drivers/install/inf-files)
---
ms.assetid: B4A550E7-1639-4C9A-A229-31E22B1415E7
title: 传感器方向
description: 来自 Accelerometer、Gyrometer、Compass、Inclinometer 和 OrientationSensor 类的传感器数据由其参考轴定义。 这些轴由设备的横向方向定义，并在用户转动设备时与其一起旋转。
ms.date: 05/24/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bfd84cd2f2255138b738ecb6dd7f6dab824d7ec4
ms.sourcegitcommit: d1ef530ef4dfa34db7bc429ab5a0c19fc405885f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2019
ms.locfileid: "71247452"
---
# <a name="sensor-orientation"></a>传感器方向

来自 [**Accelerometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer)、[**Gyrometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Gyrometer)、[**Compass**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Compass)、[**Inclinometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer) 和 [**OrientationSensor**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.OrientationSensor) 类的传感器数据由其参考轴定义。 这些轴由设备的参考帧定义，并在用户转动设备时与其一起旋转。 如果你的应用支持自动旋转，会在用户旋转设备时自行重定向以适应设备，则必须先调整关于旋转的传感器数据才能使用它。

### <a name="important-apis"></a>重要的 API

- [**Windows. 传感器**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors)
- [**Windows. 传感器自定义**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Custom)

## <a name="display-orientation-vs-device-orientation"></a>显示方向和设备方向对比

为了了解传感器的参考轴，你需要区分显示方向和设备方向。 显示方向是指方向文本且图像均显示在屏幕上，设备方向是指设备的物理定位。

在下图中，设备和显示方向都处于[横向](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations)（显示的传感器轴特定于横向方向），正 z 轴从设备向外扩展。

此图显示了[横向](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations)显示和设备方向。

![采用 Landscape 的显示和设备方向](images/sensor-orientation-a.PNG)

下图显示了[LandscapeFlipped](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations)中的显示和设备方向。

![显示和设备方向采用 LandscapeFlipped 的显示和设备方向](images/sensor-orientation-b.PNG)

此最终关系图显示了在设备方向为 " [LandscapeFlipped](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations)" 时横向显示方向。

![采用 Landscape 的显示方向和采用 LandscapeFlipped 的设备方向](images/sensor-orientation-c.PNG)

你可以使用具有 [**CurrentOrientation**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.currentorientation) 属性的 [**GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.getforcurrentview) 方法以通过 [**DisplayInformation**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayInformation) 类查询方向值。 然后，你可以通过与 [**DisplayOrientations**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations) 枚举进行比较来创建逻辑。 请记住，对于你支持的每一个方向，必须支持该方向的参考轴的转换。

## <a name="landscape-first-vs-portrait-first-devices"></a>横向优先设备和纵向优先设备对比

制造商既生产横向优先设备，也生产纵向优先设备。 参考帧在横向优先设备（如台式机和笔记本电脑）和纵向优先设备（如手机和某些平板电脑）之间各不相同。 下表显示了适用于横向优先设备和纵向优先设备的传感器轴。

| Orientation | 优先横向 | 优先纵向 |
|-------------|-----------------|----------------|
| **纵向** | ![采用 Landscape 方向的优先横向设备](images/sensor-orientation-0.PNG) | ![采用 Landscape 方向的优先纵向设备](images/sensor-orientation-1.PNG) |
| **沿** | ![采用 Portrait 方向的优先横向设备](images/sensor-orientation-2.PNG) | ![采用 Portrait 方向的优先纵向设备](images/sensor-orientation-3.PNG) |
| **LandscapeFlipped** | ![采用 LandscapeFlipped 方向的优先横向设备](images/sensor-orientation-4.PNG) | ![采用 LandscapeFlipped 方向的优先纵向设备](images/sensor-orientation-5.PNG) | 
| **PortraitFlipped** | ![采用 PortraitFlipped 方向的优先横向设备](images/sensor-orientation-6.PNG)| ![采用 PortraitFlipped 方向的优先纵向设备](images/sensor-orientation-7.PNG) |

## <a name="devices-broadcasting-display-and-headless-devices"></a>广播屏幕和无外设设备的设备

某些设备具有将屏幕广播到另一台设备的能力。 例如，可拿出平板电脑，将其屏幕广播到横向显示的投影仪。 在此方案中请务必记住，设备方向根据原始设备而定，而非基于显示屏幕的设备。 因此，加速计将报告平板电脑数据。

此外，某些设备没有屏幕。 对于这些设备，它们的默认方向为纵向。

## <a name="display-orientation-and-compass-heading"></a>显示方向和指南针方位

指南针方位具体取决于参考轴，因此它随设备方向发生变化。 可以基于此表进行修正（假设用户面朝北方）。

| 显示方向 | 用于指南针方位的参考轴 | 面朝北方时的 API 指南针方位（横向优先） | 面朝北方时的 API 指南针方位（纵向优先） |指南针方位修正（横向优先） | 指南针方位修正（纵向优先） |
|---------------------|------------------------------------|---------------------------------------------------------|--------------------------------------------------------|------------------------------------------------|-----------------------------------------------|
| Landscape           | -Z | 0   | 270 | 标题               | （方位 + 90）% 360  |
| Portrait            |  Y | 90  | 0   | （方位 + 270）% 360 |  标题              |
| LandscapeFlipped    |  Z | 180 | 90  | （方位 + 180）% 360 | （方位 + 270）% 360 |
| PortraitFlipped     |  Y | 270 | 180 | （方位 + 90）% 360  | （方位 + 180）% 360 |

修改指南针方位（如该表中所示），以便正确显示方位。 下面的代码段将演示如何执行此操作。

```csharp
private void ReadingChanged(object sender, CompassReadingChangedEventArgs e)
{
    double heading = e.Reading.HeadingMagneticNorth;
    double displayOffset;

    // Calculate the compass heading offset based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();

    switch (displayInfo.CurrentOrientation)
    {
        case DisplayOrientations.Landscape:
            displayOffset = 0;
            break;
        case DisplayOrientations.Portrait:
            displayOffset = 270;
            break;
        case DisplayOrientations.LandscapeFlipped:
            displayOffset = 180;
            break;
        case DisplayOrientations.PortraitFlipped:
            displayOffset = 90;
            break;
     }

    double displayCompensatedHeading = (heading + displayOffset) % 360;

    // Update the UI...
}
```

## <a name="display-orientation-with-the-accelerometer-and-gyrometer"></a>使用加速度计和陀螺测试仪显示方向

此表可转换用于显示方向的加速度计和陀螺测试仪数据。

| 参考轴        |  X |  Y | Z |
|-----------------------|----|----|---|
| **纵向**         |  X |  Y | Z |
| **沿**          |  Y | -X | Z |
| **LandscapeFlipped**  | -X | -Y | Z |
| **PortraitFlipped**   | -Y |  X | Z |

以下代码示例可将这些转换应用到陀螺测试仪。

```csharp
private void ReadingChanged(object sender, GyrometerReadingChangedEventArgs e)
{
    double x_Axis;
    double y_Axis;
    double z_Axis;

    GyrometerReading reading = e.Reading;  

    // Calculate the gyrometer axes based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();
    switch (displayInfo.CurrentOrientation)
    {
        case DisplayOrientations.Landscape:
            x_Axis = reading.AngularVelocityX;
            y_Axis = reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.Portrait:
            x_Axis = reading.AngularVelocityY;
            y_Axis = -1 * reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.LandscapeFlipped:
            x_Axis = -1 * reading.AngularVelocityX;
            y_Axis = -1 * reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.PortraitFlipped:
            x_Axis = -1 * reading.AngularVelocityY;
            y_Axis = reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break;
     }

    // Update the UI...
}
```

## <a name="display-orientation-and-device-orientation"></a>显示方向和设备方向

必须采用不同的方式更改 [**OrientationSensor**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.OrientationSensor) 数据。 将这些不同的方向视为逆时针旋转到 Z 轴的方向，因此我们需要反向旋转才能返回用户的方向。 对于四元数数据，我们可以使用欧拉公式定义参考四元数的旋转，也可以使用参考旋转矩阵。

![欧拉公式](images/eulers-formula.png)

若要获取所需的相对方向，请用参考对象乘以绝对对象。 请注意，此数学算法不可应用交换律。

![用参考对象乘以绝对对象](images/orientation-formula.png)

在之前的表达式中，绝对对象由传感器数据返回。

| 显示方向  | 围绕 Z 进行逆时针旋转 | 参考四元数（反向旋转） | 参考旋转矩阵（反向旋转） |
|----------------------|------------------------------------|-----------------------------------------|----------------------------------------------|
| **纵向**        | 0                                  | 1 + 0i + 0j + 0k                        | \[1 0 0<br/> 0 1 0<br/> 0 0 1\]               |
| **沿**         | 90                                 | cos(-45⁰) + (i + j + k)*sin(-45⁰)       | \[0 1 0<br/>-1 0 0<br/>0 0 1]              |
| **LandscapeFlipped** | 180                                | 0 - i - j - k                           | \[1 0 0<br/> 0 1 0<br/> 0 0 1]               |
| **PortraitFlipped**  | 270                                | cos(-135⁰) + (i + j + k)*sin(-135⁰)     | \[0 -1 0<br/> 1  0 0<br/> 0  0 1]             |

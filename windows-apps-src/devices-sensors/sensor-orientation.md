---
ms.assetid: B4A550E7-1639-4C9A-A229-31E22B1415E7
title: 传感器方向
description: 来自 Accelerometer、Gyrometer、Compass、Inclinometer 和 OrientationSensor 类的传感器数据由其参考轴定义。 这些轴由设备的横向方向定义，并在用户转动设备时与其一起旋转。
ms.date: 05/24/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a4c7f1ad75e1e0544486049f9bd721d8a82edf03
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8786340"
---
# <a name="sensor-orientation"></a>传感器方向


**重要的 API**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Windows.Devices.Sensors.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn895032)

来自 [**Accelerometer**](https://msdn.microsoft.com/library/windows/apps/BR225687)、[**Gyrometer**](https://msdn.microsoft.com/library/windows/apps/BR225718)、[**Compass**](https://msdn.microsoft.com/library/windows/apps/BR225705)、[**Inclinometer**](https://msdn.microsoft.com/library/windows/apps/BR225766) 和 [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) 类的传感器数据由其参考轴定义。 这些轴由设备的参考帧定义，并在用户转动设备时与其一起旋转。 如果你的应用支持自动旋转，会在用户旋转设备时自行重定向以适应设备，则必须先调整关于旋转的传感器数据才能使用它。

## <a name="display-orientation-vs-device-orientation"></a>显示方向和设备方向对比

为了了解传感器的参考轴，你需要区分显示方向和设备方向。 显示方向是指方向文本且图像均显示在屏幕上，设备方向是指设备的物理定位。 在下图中，设备和显示方向都采用**横向**（请注意，所示的传感器轴仅适用于横向优先设备）。

![采用横向的显示和设备方向](images/sensor-orientation-a.PNG)

下图显示了采用 **LandscapeFlipped** 的显示和设备方向。

![显示和设备方向采用 LandscapeFlipped 的显示和设备方向](images/sensor-orientation-b.PNG)

下一张图片显示了采用横向的显示方向和采用横向翻转的设备方向。

![采用横向的显示方向和采用横向翻转的设备方向](images/sensor-orientation-c.PNG)

你可以使用具有 [**CurrentOrientation**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.currentorientation.aspx) 属性的 [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.getforcurrentview.aspx) 方法以通过 [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/Dn264258) 类查询方向值。 然后，你可以通过与 [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/BR226142) 枚举进行比较来创建逻辑。 请记住，对于你支持的每一个方向，必须支持该方向的参考轴的转换。

## <a name="landscape-first-vs-portrait-first-devices"></a>横向优先设备和纵向优先设备对比

制造商既生产横向优先设备，也生产纵向优先设备。 参考帧在横向优先设备（如台式机和笔记本电脑）和纵向优先设备（如手机和某些平板电脑）之间各不相同。 下表显示了适用于横向优先设备和纵向优先设备的传感器轴。

| Orientation | 优先横向 | 优先纵向 |
|-------------|-----------------|----------------|
| **横向** | ![采用横向方向的优先横向设备](images/sensor-orientation-0.PNG) | ![采用横向方向的优先纵向设备](images/sensor-orientation-1.PNG) |
| **纵向** | ![采用纵向方向的优先横向设备](images/sensor-orientation-2.PNG) | ![采用纵向方向的优先纵向设备](images/sensor-orientation-3.PNG) |
| **横向翻转** | ![采用横向翻转方向的优先横向设备](images/sensor-orientation-4.PNG) | ![采用横向翻转方向的优先纵向设备](images/sensor-orientation-5.PNG) | 
| **纵向翻转** | ![采用纵向翻转方向的优先横向设备](images/sensor-orientation-6.PNG)| ![采用纵向翻转方向的优先纵向设备](images/sensor-orientation-7.PNG) |

## <a name="devices-broadcasting-display-and-headless-devices"></a>广播屏幕和无外设设备的设备

某些设备具有将屏幕广播到另一台设备的能力。 例如，可拿出平板电脑，将其屏幕广播到横向显示的投影仪。 在此方案中请务必记住，设备方向根据原始设备而定，而非基于显示屏幕的设备。 因此，加速计将报告平板电脑数据。

此外，某些设备没有屏幕。 对于这些设备，它们的默认方向为纵向。

## <a name="display-orientation-and-compass-heading"></a>显示方向和指南针方位


指南针方位具体取决于参考轴，因此它随设备方向发生变化。 可以基于此表进行修正（假设用户面朝北方）。

| 显示方向 | 用于指南针方位的参考轴 | 面朝北方时的 API 指南针方位（横向优先） | 面朝北方时的 API 指南针方位（纵向优先） |指南针方位修正（横向优先） | 指南针方位修正（纵向优先） |
|---------------------|------------------------------------|---------------------------------------------------------|--------------------------------------------------------|------------------------------------------------|-----------------------------------------------|
| 横向           | -Z | 0   | 270 | 方位               | （方位 + 90）% 360  |
| 纵向            |  Y | 90  | 0   | （方位 + 270）% 360 |  方位              |
| 横向翻转    |  Z | 180 | 90  | （方位 + 180）% 360 | （方位 + 270）% 360 |
| 纵向翻转     |  Y | 270 | 180 | （方位 + 90）% 360  | （方位 + 180）% 360 |

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
| **横向**         |  X |  Y | Z |
| **纵向**          |  Y | -X | Z |
| **横向翻转**  | -X | -Y | Z |
| **纵向翻转**   | -Y |  X | Z |

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

必须采用不同的方式更改 [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) 数据。 请考虑逆时针旋转到 Z 轴时的不同方向，因此我们需要反向旋转以重新获取用户的方向。 对于四元数数据，我们可以使用欧拉公式定义参考四元数的旋转，也可以使用参考旋转矩阵。

![欧拉公式](images/eulers-formula.png)

若要获取所需的相对方向，请用参考对象乘以绝对对象。 请注意，此数学算法不可应用交换律。

![用参考对象乘以绝对对象](images/orientation-formula.png)

在之前的表达式中，绝对对象由传感器数据返回。


| 显示方向  | 围绕 Z 进行逆时针旋转 | 参考四元数（反向旋转） | 参考旋转矩阵（反向旋转） | 
|----------------------|------------------------------------|-----------------------------------------|----------------------------------------------|
| **横向**        | 0                                  | 1 + 0i + 0j + 0k                        | \[1 0 0<br/> 0 1 0<br/> 0 0 1\]               |
| **纵向**         | 90                                 | cos(-45⁰) + (i + j + k)*sin(-45⁰)       | \[0 1 0<br/>-1 0 0<br/>0 0 1]              |
| **横向翻转** | 180                                | 0 - i - j - k                           | \[1 0 0<br/> 0 1 0<br/> 0 0 1]               |
| **纵向翻转**  | 270                                | cos(-135⁰) + (i + j + k)*sin(-135⁰)     | \[0 -1 0<br/> 1  0 0<br/> 0  0 1]             |


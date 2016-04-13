---
ms.assetid: B4A550E7-1639-4C9A-A229-31E22B1415E7
传感器方向
来自 Accelerometer、Gyrometer、Compass、Inclinometer 和 OrientationSensor 类的传感器数据由其参考轴定义。 这些轴由设备的横向方向定义，并在用户转动设备时与其一起旋转。
---
# 传感器方向

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** 重要的 API **

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Windows.Devices.Sensors.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn895032)

来自 [**Accelerometer**](https://msdn.microsoft.com/library/windows/apps/BR225687)、[**Gyrometer**](https://msdn.microsoft.com/library/windows/apps/BR225718)、[**Compass**](https://msdn.microsoft.com/library/windows/apps/BR225705)、[**Inclinometer**](https://msdn.microsoft.com/library/windows/apps/BR225766) 和 [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) 类的传感器数据由其参考轴定义。 这些轴由设备的横向方向定义，并在用户转动设备时与其一起旋转。 如果你的应用支持自动旋转，会在用户旋转设备时自行重定向以适应设备，则必须先调整关于旋转的传感器数据才能使用它。

## 显示方向和设备方向对比

为了了解传感器的参考轴，你需要区分显示方向和设备方向。 显示方向是方向文本且图像均显示在屏幕上，而设备方向则是设备的物理定位。 在下图中，设备和显示方向都采用 **Landscape**。

![采用 Landscape 的显示和设备方向](images/accelerometer-axis-orientation-landscape-with-text.png)

下图显示了采用 **LandscapeFlipped** 的显示和设备方向。

![显示和设备方向采用 LandscapeFlipped 的显示和设备方向](images/accelerometer-axis-orientation-landscape-180-with-text.png)

下一张图片显示了采用 Landscape 的显示方向和采用 LandscapeFlipped 的设备方向。

![采用 Landscape 的显示方向和采用 LandscapeFlipped 的设备方向](images/accelerometer-axis-orientation-landscape-180-with-text-inverted.png)

你可以使用具有 [**CurrentOrientation**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.graphics.display.displayinformation.currentorientation.aspx) 属性的 [**GetForCurrentView**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.graphics.display.displayinformation.getforcurrentview.aspx) 方法以通过 [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/Dn264258) 类查询方向值。 然后，你可以通过与 [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/BR226142) 枚举进行比较来创建逻辑。 请记住，对于你支持的每一个方向，必须支持该方向的参考轴的转换。

## 横向优先设备和纵向优先设备对比

制造商既生产横向优先设备，也生产纵向优先设备。 当制造商将组件集成到设备中时，他们采用统一且一致的方式来完成，以便所有设备均可在相同的参考帧中运行。 下表显示了适用于优先横向设备和优先纵向设备的传感器轴。

| 方向 | 优先横向 | 优先纵向 |
|-------------|-----------------|----------------|
| **Landscape** | ![采用 Landscape 方向的优先横向设备](images/accelerometer-axis-orientation-landscape.png) | ![采用 Landscape 方向的优先纵向设备](images/accelerometer-axis-orientation-portrait-270.png) |
| **Portrait** | ![采用 Portrait 方向的优先横向设备](images/accelerometer-axis-orientation-landscape-90.png) | ![采用 Portrait 方向的优先纵向设备](images/accelerometer-axis-orientation-portrait.png) |
| **LandscapeFlipped ** | ![采用 LandscapeFlipped 方向的优先横向设备](images/accelerometer-axis-orientation-landscape-180.png) | ![采用 LandscapeFlipped 方向的优先纵向设备](images/accelerometer-axis-orientation-portrait-90.png) | 
| **PortraitFlipped** | ![采用 PortraitFlipped 方向的优先横向设备](images/accelerometer-axis-orientation-landscape-270.png)| ![采用 PortraitFlipped 方向的优先纵向设备](images/accelerometer-axis-orientation-portrait-180.png) |

## 广播屏幕和无外设设备的设备

某些设备具有将屏幕广播到另一台设备的能力。 例如，可拿出平板电脑，将其屏幕广播到横向显示的投影仪。 在此方案中请务必记住，设备方向根据原始设备而定，而非基于显示屏幕的设备。 因此，加速计将报告平板电脑数据。

此外，某些设备没有屏幕。 对于这些设备，它们的默认方向为纵向。

## 显示方向和指南针方位


指南针方位具体取决于参考轴，因此它随设备方向发生变化。 可以基于此表进行修正（假设用户面朝北方）。

| 显示方向 | 用于指南针方位的参考轴 | 面朝北方时的 API 指南针方位 | 指南针方位修正 | 
|---------------------|------------------------------------|---------------------------------------|------------------------------|
| Landscape           | -Z | 0   | 方位               |
| Portrait            |  Y | 90  | （方位 + 270）% 360 | 
| LandscapeFlipped    |  Z | 180 | （方位 + 180）% 360 |
| PortraitFlipped     |  Y | 270 | （方位 + 90）% 360  |

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

## 使用加速度计和陀螺测试仪显示方向

此表可转换用于显示方向的加速度计和陀螺测试仪数据。

| 参考轴        |  X |  Y | Z |
|-----------------------|----|----|---|
| **Landscape**         |  X |  Y | Z |
| **Portrait**          |  Y | -X | Z |
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

## 显示方向和设备方向

必须采用不同的方式更改 [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) 数据。 请考虑逆时针旋转到 Z 轴时的不同方向，因此我们需要反向旋转以重新获取用户的方向。 对于四元数数据，我们可以使用欧拉公式定义参考四元数的旋转，也可以使用参考旋转矩阵。

![欧拉公式](images/eulers-formula.png)
若要获取所需的相对方向，请用参考对象乘以绝对对象。 请注意，此数学算法不可应用交换律。

![用参考对象乘以绝对对象](images/orientation-formula.png)
在之前的表达式中，绝对对象由传感器数据返回。

| 显示方向  | 围绕 Z 进行逆时针旋转 | 参考四元数（反向旋转） | 参考旋转矩阵（反向旋转） | 
|----------------------|------------------------------------|-----------------------------------------|----------------------------------------------|
| **Landscape**        | 0                                  | 1 + 0i + 0j + 0k                        | \[1 0 0<br/> 0 1 0<br/> 0 0 1\]               |
| **Portrait**         | 90                                 | cos(-45⁰) + (i + j + k)*sin(-45⁰)       | \[0 1 0<br/>-1 0 0<br/>0 0 1]              |
| **LandscapeFlipped** | 180                                | 0 - i - j - k                           | \[1 0 0<br/> 0 1 0<br/> 0 0 1]               |
| **PortraitFlipped**  | 270                                | cos(-135⁰) + (i + j + k)*sin(-135⁰)     | \[0 -1 0<br/> 1 0 0<br/> 0 0 1]             |



<!--HONumber=Mar16_HO1-->



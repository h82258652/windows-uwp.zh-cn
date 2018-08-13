---
author: muhsinking
ms.assetid: 415F4107-0612-4235-9722-0F5E4E26F957
title: 传感器
description: 传感器使你的应用了解它周围的设备和外界之间的关系。 传感器可以告知你的应用设备的方向、定位和移动。
ms.author: mukin
ms.date: 06/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3bf1d905874fc420dc0a39c1082e213e4ea4c327
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "959172"
---
# <a name="sensors"></a>传感器



传感器使你的应用了解它周围的设备和外界之间的关系。 传感器可以告知你的应用设备的方向、定位和移动。 这些传感器通过提供独特的输入形式（例如使用设备的运动在屏幕上排列角色或模拟位于驾驶舱中的情况并将该设备用作方向盘）有助于使你的游戏、增强的现实应用或实用程序应用更有用且更具交互性。

按照一般规则，从开始确定你的应用是专门依赖于传感器，还是传感器提供其他控件机制。 例如，使用某个设备作为虚拟方向盘的赛车游戏可以替换为通过屏幕上的 GUI 控制。通过这种方式，不管传感器在系统上是否可用，该应用都有效。 另一方面，可以编码弹珠迷阵冲锋游戏，以使其仅在具有合适的传感器的系统上有效。 你必须对是否完全依赖于传感器进行策略性选择。 请注意，通过鼠标/触摸控制方案可享受到更大的控制权。

| 主题                                                       | 描述  |
|-------------------------------------------------------------|--------------|
| [校准传感器](calibrate-sensors.md)                   | 基于磁力计的设备中的传感器（指南针、测斜仪和方向传感器）因环境因素需要校准。 [<strong>MagnetometerAccuracy</strong>](https://msdn.microsoft.com/library/windows/apps/Dn297552) 枚举可帮助确定设备需要校准时的做法。 |
| [传感器方向](sensor-orientation.md)                 | 来自 [<strong>OrientationSensor</strong>](https://msdn.microsoft.com/library/windows/apps/BR206371) 类的传感器数据由其参考轴定义。 这些轴由设备的横向方向定义，并在用户转动设备时与其一起旋转。 |
| [使用加速计](use-the-accelerometer.md)           | 了解如何使用加速计响应用户移动。 |
| [使用指南针](use-the-compass.md)                       | 了解如何使用指南针确定当前方位。 |
| [使用陀螺测试仪](use-the-gyrometer.md)                   | 了解如何使用陀螺测试仪检测用户移动变化。 | 
| [使用测斜仪](use-the-inclinometer.md)             | 了解如何使用测斜仪确定俯仰、滚转和偏航。 |
| [使用光传感器](use-the-light-sensor.md)             | 了解如何使用氛围光传感器检测照明变化。 |
| [使用方向传感器](use-the-orientation-sensor.md) | 了解如何使用方向传感器确定设备方向。|

## <a name="sensor-batching"></a>传感器批处理

一些传感器支持批处理的概念。 这会因可用的各个传感器而不同。 当传感器实现批处理时，它将收集指定时间间隔内的几个数据点，然后一次传输所有这些数据。 正常情况下，只要执行读取，传感器就会报告其查找结果，而这不同于正常情况。 请参考下图，该图显示了如何收集然后传递数据，首先使用普通传递，然后使用批处理传递。

![传感器批处理集合](images/batchsample.png)

传感器批处理的主要优点是延长电池使用时间。 如果数据没有立即发送，则会节省处理器耗电量，同时防止数据需要被立即处理。 系统的部分组件可以进入睡眠状态，直到需要它们，这会节省大量电量。

可以通过调整延迟影响传感器发送批量数据的频率。 例如，[**Accelerometer**](https://msdn.microsoft.com/library/windows/apps/BR225687) 传感器具有 [**ReportLatency**](https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.accelerometer.reportlatency) 属性。 当为应用程序设置此属性时，传感器将在指定时间间隔后发送数据。 你可以通过设置 [**ReportInterval**](https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.accelerometer.reportinterval) 属性，控制在给定延迟时间内积累的数据量。

在设置延迟时，需要记住几点。 第一点是每个传感器都具有一个可以基于该传感器本身而支持的 [**MaxBatchSize**](https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.accelerometer.maxbatchsize.aspx)。 这是在强制发送事件之前，传感器可以缓存的事件数。 如果将 **MaxBatchSize** 乘以 [**ReportInterval**](https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.accelerometer.reportinterval)，则可以确定最大的 [**ReportLatency**](https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.accelerometer.reportlatency) 值。 如果指定一个比此值更大的值，则将使用最大延迟，以免丢失数据。 此外，多个应用程序都可以分别设置所需的延迟时间。 为满足所有应用程序的需求，将使用最短的延迟时间。 因此，你在你的应用程序中所设置的延迟时间可能不会与观察到的延迟时间相匹配。

如果传感器使用的是批处理报告，调用 [**GetCurrentReading**](https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.accelerometer.getcurrentreading) 将会清除当前这批数据并启动新的延迟时间。

## <a name="accelerometer"></a>加速计

[**Accelerometer**](https://msdn.microsoft.com/library/windows/apps/BR225687) 传感器可以测量沿设备的 X、Y 和 Z 轴的重力值，且对于基于运动的简单应用程序很有用。 请注意，重力值包括重力加速度。 如果设备在表格上具有 **FaceUp** 的 [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/BR206399)，则加速计将在 Z 轴上读取 -1 G。 因此，加速计不必仅测量坐标加速度 – 速度的变化率。 使用加速计时，请确保区分重力的重力矢量和运动的线性加速度矢量。 请注意，针对固定设备，重力矢量应规范化为 1。

下图演示：

-   V1 = 矢量 1 = 重力
-   V2 = 矢量 2 = 设备底盘的-Z 轴（指出屏幕后面）
-   Θi = 斜角（倾斜） = 设备底盘的 –Z 轴和重力矢量之间的角度

![加速计](images/accelerometer1.png)![加速计测量](images/accelerometer2.png)

可能使用加速计传感器的应用包含一款游戏，在该游戏中，屏幕上的弹珠会随着你倾斜设备的方向（重力矢量）进行滚动。 此类功能密切反映了 [**Inclinometer**](https://msdn.microsoft.com/library/windows/apps/BR225766) 功能，并且也可通过使用俯仰和滚动这一组合操作借助该传感器实现该功能。 使用加速计的重力矢量通过针对设备倾斜提供轻松进行数学运算的矢量而稍微简化了这一点。 另一个示例是一个应用，当用户在空中轻弹该设备（线性加速度矢量）时，该应用会发出抽打鞭子的声音。

有关实现的示例，请参阅[加速计示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Accelerometer)。

## <a name="activity-sensor"></a>活动传感器

[**Activity**](https://msdn.microsoft.com/library/windows/apps/Dn785096) 传感器将确定连接到传感器的设备的当前状态。 此传感器经常在健身应用程序中用于跟踪携带设备的用户是在跑步还是步行。 有关可通过此传感器 API 检测到的可能活动列表，请参阅 [**ActivityType**](https://msdn.microsoft.com/library/windows/apps/Dn785128)。

有关实现的示例，请参阅[活动传感器示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ActivitySensor)。

## <a name="altimeter"></a>高度计

[**Altimeter**](https://msdn.microsoft.com/library/windows/apps/Dn858893) 传感器将返回一个指示该传感器高度的值。 这使你能够按照米数跟踪相对于海平面的高度变化。 可能使用此传感器的一个示例是，跟踪跑步期间的海拔高度变化以计算消耗的热量的跑步应用。 在此情况下，此传感器数据可能会与[**活动**](https://msdn.microsoft.com/library/windows/apps/Dn785096) 传感器组合起来，提供更精确的跟踪信息。

有关实现的示例，请参阅[高度计示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Altimeter)。

## <a name="barometer"></a>气压计

[**Barometer**](https://msdn.microsoft.com/library/windows/apps/Dn872405) 传感器使应用程序能够获取气压读数。 天气应用程序可以使用此信息来提供当前大气压力。 这可用于提供更详细的信息和预测潜在天气变化。

有关实现的示例，请参阅[气压计示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Barometer)。

## <a name="compass"></a>指南针

[**Compass**](https://msdn.microsoft.com/library/windows/apps/BR225705) 传感器基于地球的水平面返回与磁北相关的二维方向。 不应该将指南针传感器用于确定特定设备方向或用于表示三维空间中的任何内容。 地理功能可导致方向出现自然偏差，因此某些系统同时支持 [**HeadingMagneticNorth**](https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.compassreading.headingmagneticnorth.aspx) 和 [**HeadingTrueNorth**](https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.compassreading.headingtruenorth.aspx)。 考虑你的应用将首选哪一个值，但要记住并非所有系统都会报告真北值。 陀螺测试仪和磁力计（测量磁场强度大小的设备）传感器组合其数据以生成指南针方向，该方向具有稳定数据的有效效应（由于电力系统组件，磁场强度非常不稳定）。

![有关磁北极的指南针读数](images/compass.png)

想要显示指南针刻度盘或导航某个地图的应用通常会使用指南针传感器。

有关实现的示例，请参阅[指南针示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Compass)。

## <a name="gyrometer"></a>陀螺测试仪

[**Gyrometer**](https://msdn.microsoft.com/library/windows/apps/BR225718) 传感器将测量沿 X、Y 和 Z 轴的角速度。 这些角速度在基于运动的简单应用中非常有用，这些应用不关心设备方向，但却关注以不同速度旋转的设备。 陀螺测试仪可能受到沿一个或多个轴的数据杂音或恒定偏置的影响。 你应该查询加速计以验证是否移动设备，以便确定陀螺测试仪是否受偏置的影响，然后在你的应用中进行相应地修正。

![具有俯仰、倾斜和偏航的陀螺测试仪](images/gyrometer.png)

可使用陀螺测试仪传感器的应用的一个示例是一款游戏，该游戏基于设备的快速旋转操作旋转轮盘。

有关实现的示例，请参阅[陀螺测试仪示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Gyrometer)。

## <a name="inclinometer"></a>测斜仪

[**Inclinometer**](https://msdn.microsoft.com/library/windows/apps/BR225766) 传感器指定设备的偏航、俯仰和倾斜值，并且最适合关注设备如何位于空间中的应用。 俯仰和倾斜通过使用加速计的重力矢量和集成陀螺测试仪的数据得出。 从磁力计和陀螺测试仪（类似于指南针方向）数据确定偏航。 测斜仪采用可轻松领会并理解的方式提供高级方向数据。 当你需要设备方向，但不需要使用传感器数据时，请使用测斜仪。

![具有俯仰、倾斜和偏航的测斜仪](images/inclinometer.png)

更改其视野以匹配设备方向的应用可以使用测斜仪传感器。 同样，可显示飞机（匹配设备的偏航、俯仰和倾斜）的应用还将使用测斜仪读数。

实施示例，请参阅 inclinometer 示例[https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Inclinometer](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Inclinometer)。

## <a name="light-sensor"></a>光传感器

[**Light**](https://msdn.microsoft.com/library/windows/apps/BR225790) 传感器能够确定传感器周围的环境光。 这使应用能够确定设备周围的光环境发生更改的时间。 例如，配备平板电脑设备的用户可能会在晴天从室内走到户外。 智能应用程序可以使用此值来增加背景和要呈现字体之间的对比度。 这样，内容在更明亮的户外环境中仍可读取。

有关实现的示例，请参阅[光传感器示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LightSensor)。

## <a name="orientation-sensor"></a>方向传感器

通过四元数和旋转矩阵表示设备方向。 [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) 提供了一个高精度，以用于确定设备如何根据相关绝对方向位于空间中。 **OrientationSensor** 数据从加速计、陀螺测试仪和磁力计中得出。 因此，测斜仪和指南针传感器均可以得出四元数值。 四元数和旋转矩阵能很好地实现高级数学运算，并且通常用于图形编程中。 由于许多转换都基于四元数和旋转矩阵，因此使用复杂运算的应用应该支持方向传感器。

![方向传感器数据](images/orientation-sensor.png)

方向传感器通常用于先进的增强现实应用，这些应用可基于指向的设备的后方在你的周围绘制一个覆盖物。

有关实现的示例，请参阅[方向传感器示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/OrientationSensor)。

## <a name="pedometer"></a>步程计

[**Pedometer**](https://msdn.microsoft.com/library/windows/apps/Dn878203) 传感器跟踪携带已连接设备的用户所走的步数。 该传感器将配置为跟踪给定时段内的步数。 多个健身应用程序都会跟踪用户所走步数，以帮助用户设置并达到各种目标。 然后可以收集并存储此信息，以显示时间进度。

有关实现的示例，请参阅[步程计示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Pedometer)。

## <a name="proximity-sensor"></a>邻近感应传感器

[**Proximity**](https://msdn.microsoft.com/library/windows/apps/Dn872427) 传感器可用于指示该传感器是否检测到对象。 除了确定对象是否位于设备范围内以外，邻近感应传感器还可以确定到受检测对象的距离。 可以使用此传感器的一个示例是，当用户出现在指定的范围内时，要脱离睡眠状态的应用程序。 在邻近感应传感器检测到对象之前，设备可能处于低功耗睡眠状态，接着可以进入更活动的状态。

有关实现的示例，请参阅[邻近感应传感器示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ProximitySensor)。

## <a name="simple-orientation"></a>简单方向

[**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.simpleorientationsensor.aspx) 将检测指定设备的当前象限方向或其朝上方向或朝下方向。 它具有六个可能的 [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/BR206399) 状态（**NotRotated**、**Rotated90**、**Rotated180**、**Rotated270**、**FaceUp**、**FaceDown**）。

可根据平行或垂直于地面的设备更改其显示的阅读器应用将使用 SimpleOrientationSensor 中的值，以确定如何定位该设备。

有关实现的示例，请参阅[简单方向传感器示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleOrientationSensor)。

---
author: muhsinking
ms.assetid: 1889AC3A-A472-4294-89B8-A642668A8A6E
title: 使用方向传感器
description: 了解如何使用方向传感器确定设备方向。
ms.author: mukin
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 49199a91f6713b3f18928eaafb6875a49deaf451
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5686949"
---
# <a name="use-the-orientation-sensor"></a>使用方向传感器


**重要的 API**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371)
-   [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/BR206399)

**示例**

-   [方向传感器示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/OrientationSensor)
-   [简单方向传感器示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleOrientationSensor)

了解如何使用方向传感器确定设备方向。

有两种不同类型的方向传感器 API 包含在 [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408) 命名空间中：[**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) 和 [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/BR206399)。 虽然这两个传感器都是方向传感器，但该术语具有不同的意义，并且这两者的用途具有显著区别。 但是，由于这两者都是方向传感器，因此均涵盖在本文中。

[**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) API 用于三维应用以获取四元数和旋转矩阵。 四元数可以最简单地理解为一个点 \[x,y,z\] 围绕任意一个轴旋转（与旋转矩阵不同，它表示围绕三个轴旋转）。 四元数后的数学非常奇特，因为它涉及到复数的几何属性以及虚数的数学属性，但是使用它们却非常简单，而且 DirectX 等框架也支持它们。 复杂的三维应用可使用方向传感器调整用户的视角。 此传感器结合了来自加速计、陀螺测试仪以及指南针的输入。

[**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/BR206399) API 用于根据 portrait up、portrait down、landscape left 和 landscape right 等定义确定当前设备方向。 它还可以检测设备是正面朝上还是正面朝下。 此传感器不会返回如“portrait up”或“landscape left”等属性，而是返回一个旋转值，如“Not rotated”、“Rotated90DegreesCounterclockwise”等。 下表将常见的方向属性映射到相应的传感器读取方向。

| 方向     | 相应的传感器读取方向      |
|-----------------|-----------------------------------|
| Portrait Up     | NotRotated                        |
| Landscape Left  | Rotated90DegreesCounterclockwise  |
| Portrait Down   | Rotated180DegreesCounterclockwise |
| Landscape Right | Rotated270DegreesCounterclockwise |

## <a name="prerequisites"></a>先决条件

你应熟悉 Extensible Application Markup Language (XAML)、 Microsoft VisualC # 和事件。

你使用的设备或仿真器必须支持方向传感器。

## <a name="create-an-orientationsensor-app"></a>创建一个 OrientationSensor 应用

此部分划分为两个子部分。 第一个子部分将指导你完成从头开始创建方向应用程序所需的步骤。 以下子部分介绍你刚创建的应用。

###  <a name="instructions"></a>说明

-   创建新项目，从“Visual C#”**** 项目模板中选择“空白应用(通用 Windows)”****。

-   打开项目的 MainPage.xaml.cs 文件，使用下列内容替换现有代码。

```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Windows.Foundation;
    using Windows.Foundation.Collections;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Controls.Primitives;
    using Windows.UI.Xaml.Data;
    using Windows.UI.Xaml.Input;
    using Windows.UI.Xaml.Media;
    using Windows.UI.Xaml.Navigation;

    using Windows.UI.Core;
    using Windows.Devices.Sensors;

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/p/?linkid=234238

    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private OrientationSensor _sensor;

            private async void ReadingChanged(object sender, OrientationSensorReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    OrientationSensorReading reading = e.Reading;

                    // Quaternion values
                    txtQuaternionX.Text = String.Format("{0,8:0.00000}", reading.Quaternion.X);
                    txtQuaternionY.Text = String.Format("{0,8:0.00000}", reading.Quaternion.Y);
                    txtQuaternionZ.Text = String.Format("{0,8:0.00000}", reading.Quaternion.Z);
                    txtQuaternionW.Text = String.Format("{0,8:0.00000}", reading.Quaternion.W);

                    // Rotation Matrix values
                    txtM11.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M11);
                    txtM12.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M12);
                    txtM13.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M13);
                    txtM21.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M21);
                    txtM22.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M22);
                    txtM23.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M23);
                    txtM31.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M31);
                    txtM32.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M32);
                    txtM33.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M33);
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _sensor = OrientationSensor.GetDefault();

                // Establish the report interval for all scenarios
                uint minReportInterval = _sensor.MinimumReportInterval;
                uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                _sensor.ReportInterval = reportInterval;

                // Establish event handler
                _sensor.ReadingChanged += new TypedEventHandler<OrientationSensor, OrientationSensorReadingChangedEventArgs>(ReadingChanged);
            }
        }
    }
```

你需要使用你给予项目的名称重命名以上代码片段中的命名空间。 例如，如果你创建了一个名为**OrientationSensorCS**的项目，则使用 `namespace OrientationSensorCS` 替换 `namespace App1`。

-   打开文件 MainPage.xaml 并使用以下 XML 替换原始内容。

```xml
        <Page
        x:Class="App1.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:App1"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid x:Name="LayoutRoot" Background="Black">
            <TextBlock HorizontalAlignment="Left" Height="28" Margin="4,4,0,0" TextWrapping="Wrap" Text="M11:" VerticalAlignment="Top" Width="46"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="4,36,0,0" TextWrapping="Wrap" Text="M12:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="4,72,0,0" TextWrapping="Wrap" Text="M13:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="31" Margin="4,118,0,0" TextWrapping="Wrap" Text="M21:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="4,160,0,0" TextWrapping="Wrap" Text="M22:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="8,201,0,0" TextWrapping="Wrap" Text="M23:" VerticalAlignment="Top" Width="35"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="4,234,0,0" TextWrapping="Wrap" Text="M31:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="28" Margin="4,274,0,0" TextWrapping="Wrap" Text="M32:" VerticalAlignment="Top" Width="46"/>
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="4,322,0,0" TextWrapping="Wrap" Text="M33:" VerticalAlignment="Top" Width="39"/>
            <TextBlock x:Name="txtM11" HorizontalAlignment="Left" Height="19" Margin="43,4,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM12" HorizontalAlignment="Left" Height="23" Margin="43,36,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM13" HorizontalAlignment="Left" Height="15" Margin="43,72,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM21" HorizontalAlignment="Left" Height="20" Margin="43,114,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM22" HorizontalAlignment="Left" Height="19" Margin="43,156,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM23" HorizontalAlignment="Left" Height="16" Margin="43,197,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM31" HorizontalAlignment="Left" Height="17" Margin="43,230,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM32" HorizontalAlignment="Left" Height="19" Margin="43,270,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM33" HorizontalAlignment="Left" Height="21" Margin="43,322,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock HorizontalAlignment="Left" Height="15" Margin="194,8,0,0" TextWrapping="Wrap" Text="Quaternion X:" VerticalAlignment="Top" Width="81"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="194,36,0,0" TextWrapping="Wrap" Text="Quaternion Y:" VerticalAlignment="Top" Width="81"/>
            <TextBlock HorizontalAlignment="Left" Height="15" Margin="194,72,0,0" TextWrapping="Wrap" Text="Quaternion Z:" VerticalAlignment="Top" Width="81"/>
            <TextBlock x:Name="txtQuaternionX" HorizontalAlignment="Left" Height="15" Margin="279,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="104"/>
            <TextBlock x:Name="txtQuaternionY" HorizontalAlignment="Left" Height="12" Margin="275,36,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="108"/>
            <TextBlock x:Name="txtQuaternionZ" HorizontalAlignment="Left" Height="19" Margin="275,68,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="89"/>
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="194,96,0,0" TextWrapping="Wrap" Text="Quaternion W:" VerticalAlignment="Top" Width="81"/>
            <TextBlock x:Name="txtQuaternionW" HorizontalAlignment="Left" Height="12" Margin="279,96,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="72"/>

        </Grid>
    </Page>
```

你将需要用你的应用的命名空间替换上面的代码片段中类名称的第一部分。 例如，如果你创建了一个名为**OrientationSensorCS**的项目，则使用 `x:Class="OrientationSensorCS.MainPage"` 替换 `x:Class="App1.MainPage"`。 你还应当使用 `xmlns:local="using:OrientationSensorCS"` 替换 `xmlns:local="using:App1"`。

-   按 F5 或依次选择“调试”**** > “开始调试”**** 来生成、部署并运行应用。

应用运行后，你可以通过移动设备或使用仿真器工具更改方向。

-   通过返回到 Visual Studio 并按 Shift+F5 或依次选择“调试”**** > “停止调试”**** 来停止应用。

###  <a name="explanation"></a>描述

前面的示例演示了，只需要写入极少的代码即可将方向传感器输入集成到你的应用。

该应用在 **MainPage** 方法中建立了与默认方向传感器的连接。

```csharp
_sensor = OrientationSensor.GetDefault();
```

该应用在 **MainPage** 方法中建立了报告间隔。 此代码检索设备支持的最短间隔，并将它与所请求的间隔 16 毫秒（大约 60-Hz 刷新率）进行比较。 如果支持的最短间隔大于所请求的间隔，则此代码会将报告间隔设置为所支持的最短间隔。 否则，它会将报告间隔设置为所请求的间隔。

```csharp
uint minReportInterval = _sensor.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_sensor.ReportInterval = reportInterval;
```

在 **ReadingChanged** 方法中捕获新的传感器数据。 每当传感器驱动程序从传感器接收到新数据时，它都将使用此事件处理程序将该值传递到你的应用中。 应用在下行中注册此事件处理程序。

```csharp
_sensor.ReadingChanged += new TypedEventHandler<OrientationSensor,
OrientationSensorReadingChangedEventArgs>(ReadingChanged);
```

这些新值将写入位于项目 XAML 中的 TextBlock。

## <a name="create-a-simpleorientation-app"></a>创建一个 SimpleOrientation 应用

此部分划分为两个子部分。 第一个子部分将指导你完成从头开始创建简单的方向应用程序所需的步骤。 以下子部分介绍你刚创建的应用。

### <a name="instructions"></a>说明

-   创建新项目，从“Visual C#”**** 项目模板中选择“空白应用(通用 Windows)”****。

-   打开项目的 MainPage.xaml.cs 文件，使用下列内容替换现有代码。

```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Windows.Foundation;
    using Windows.Foundation.Collections;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Controls.Primitives;
    using Windows.UI.Xaml.Data;
    using Windows.UI.Xaml.Input;
    using Windows.UI.Xaml.Media;
    using Windows.UI.Xaml.Navigation;

    using Windows.UI.Core;
    using Windows.Devices.Sensors;
    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/p/?linkid=234238

    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            // Sensor and dispatcher variables
            private SimpleOrientationSensor _simpleorientation;

            // This event handler writes the current sensor reading to
            // a text block on the app' s main page.

            private async void OrientationChanged(object sender, SimpleOrientationSensorOrientationChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    SimpleOrientation orientation = e.Orientation;
                    switch (orientation)
                    {
                        case SimpleOrientation.NotRotated:
                            txtOrientation.Text = "Not Rotated";
                            break;
                        case SimpleOrientation.Rotated90DegreesCounterclockwise:
                            txtOrientation.Text = "Rotated 90 Degrees Counterclockwise";
                            break;
                        case SimpleOrientation.Rotated180DegreesCounterclockwise:
                            txtOrientation.Text = "Rotated 180 Degrees Counterclockwise";
                            break;
                        case SimpleOrientation.Rotated270DegreesCounterclockwise:
                            txtOrientation.Text = "Rotated 270 Degrees Counterclockwise";
                            break;
                        case SimpleOrientation.Faceup:
                            txtOrientation.Text = "Faceup";
                            break;
                        case SimpleOrientation.Facedown:
                            txtOrientation.Text = "Facedown";
                            break;
                        default:
                            txtOrientation.Text = "Unknown orientation";
                            break;
                    }
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _simpleorientation = SimpleOrientationSensor.GetDefault();

                // Assign an event handler for the sensor orientation-changed event
                if (_simpleorientation != null)
                {
                    _simpleorientation.OrientationChanged += new TypedEventHandler<SimpleOrientationSensor, SimpleOrientationSensorOrientationChangedEventArgs>(OrientationChanged);
                }
            }
        }
    }
```

你需要使用你给予项目的名称重命名以上代码片段中的命名空间。 例如，如果你创建了一个名为**SimpleOrientationCS**的项目，则使用 `namespace SimpleOrientationCS` 替换 `namespace App1`。

-   打开文件 MainPage.xaml 并使用以下 XML 替换原始内容。

```xml
    <Page
        x:Class="App1.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:App1"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid x:Name="LayoutRoot" Background="#FF0C0C0C">
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="8,8,0,0" TextWrapping="Wrap" Text="Current Orientation:" VerticalAlignment="Top" Width="101" Foreground="#FFF8F7F7"/>
            <TextBlock x:Name="txtOrientation" HorizontalAlignment="Left" Height="24" Margin="118,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="175" Foreground="#FFFEFAFA"/>

        </Grid>
    </Page>
```

你将需要用你的应用的命名空间替换上面的代码片段中类名称的第一部分。 例如，如果你创建了一个名为**SimpleOrientationCS**的项目，则使用 `x:Class="SimpleOrientationCS.MainPage"` 替换 `x:Class="App1.MainPage"`。 你还应当使用 `xmlns:local="using:SimpleOrientationCS"` 替换 `xmlns:local="using:App1"`。

-   按 F5 或依次选择“调试”**** > “开始调试”**** 来生成、部署并运行应用。

应用运行后，你可以通过移动设备或使用仿真器工具更改方向。

-   通过返回到 Visual Studio 并按 Shift+F5 或依次选择“调试”**** > “停止调试”**** 来停止应用。

### <a name="explanation"></a>描述

前面的示例演示了，只需要写入极少的代码即可在应用中集成简单方向传感器输入。

该应用在 **MainPage** 方法中建立了与默认传感器的连接。

```csharp
_simpleorientation = SimpleOrientationSensor.GetDefault();
```

在 **OrientationChanged** 方法中捕获新的传感器数据。 每当传感器驱动程序从传感器接收到新数据时，它都将使用此事件处理程序将该值传递到你的应用中。 应用在下行中注册此事件处理程序。

```csharp
_simpleorientation.OrientationChanged += new TypedEventHandler<SimpleOrientationSensor,
SimpleOrientationSensorOrientationChangedEventArgs>(OrientationChanged);
```

这些新值将写入位于项目 XAML 的 TextBlock 中。

```csharp
<TextBlock HorizontalAlignment="Left" Height="24" Margin="8,8,0,0" TextWrapping="Wrap" Text="Current Orientation:" VerticalAlignment="Top" Width="101" Foreground="#FFF8F7F7"/>
 <TextBlock x:Name="txtOrientation" HorizontalAlignment="Left" Height="24" Margin="118,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="175" Foreground="#FFFEFAFA"/>
```
---
author: mukin
ms.assetid: 454953E1-DD8F-44B7-A614-7BAD8C683536
title: "使用陀螺测试仪"
description: "了解如何使用陀螺测试仪检测用户移动变化。"
ms.author: mukin
ms.date: 06/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 7b4dc5be2c6e9cd2dfb0b1f8d6f83250430d2c8b
ms.sourcegitcommit: ca060f051e696da2c1e26e9dd4d2da3fa030103d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="use-the-gyrometer"></a>使用陀螺测试仪

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**重要的 API**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**陀螺测试仪**](https://msdn.microsoft.com/library/windows/apps/BR225718)

**示例**

-   有关更完整的实现，请参阅[陀螺测试仪示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/gyrometer)。

\[在商业发行之前可能会实质性修改与预发布产品相关的一些信息。 Microsoft 不对此处提供的信息作任何明示或默示的担保。\]

了解如何使用陀螺测试仪检测用户移动变化。

陀螺测试仪补充了加速计来一起作为游戏控制器。 加速计可以测量线性运动，而陀螺测试仪测量角度矢量或旋转运动。

## <a name="prerequisites"></a>先决条件

你应熟悉 Extensible Application Markup Language (XAML)、Microsoft Visual C# 和事件。

你使用的设备或仿真器必须支持陀螺测试仪。

## <a name="create-a-simple-gyrometer-app"></a>创建简单的陀螺测试仪应用

此部分划分为两个子部分。 第一个子部分将指导你完成从头开始创建简单的陀螺测试仪应用程序所需的步骤。 以下子部分介绍你刚创建的应用。

###  <a name="instructions"></a>说明

-   创建新项目，从“Visual C#”****项目模板中选择“空白应用(通用 Windows)”****。

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

    using Windows.UI.Core; // Required to access the core dispatcher object
    using Windows.Devices.Sensors; // Required to access the sensor platform and the gyrometer


    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private Gyrometer _gyrometer; // Our app' s gyrometer object

            // This event handler writes the current gyrometer reading to
            // the three textblocks on the app' s main page.

            private async void ReadingChanged(object sender, GyrometerReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    GyrometerReading reading = e.Reading;
                    txtXAxis.Text = String.Format("{0,5:0.00}", reading.AngularVelocityX);
                    txtYAxis.Text = String.Format("{0,5:0.00}", reading.AngularVelocityY);
                    txtZAxis.Text = String.Format("{0,5:0.00}", reading.AngularVelocityZ);
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _gyrometer = Gyrometer.GetDefault(); // Get the default gyrometer sensor object

                if (_gyrometer != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _gyrometer.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _gyrometer.ReportInterval = reportInterval;

                    // Assign an event handler for the gyrometer reading-changed event
                    _gyrometer.ReadingChanged += new TypedEventHandler<Gyrometer, GyrometerReadingChangedEventArgs>(ReadingChanged);
                }

            }
        }
    }
```

你需要使用你给予项目的名称重命名以上代码片段中的命名空间。 例如，如果你创建了一个名为**GyrometerCS**的项目，则使用 `namespace GyrometerCS` 替换 `namespace App1`。

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
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="8,8,0,0" TextWrapping="Wrap" Text="X-Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFDFDFD"/>
            <TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="23" Margin="67,8,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="88" Foreground="#FFFDFAFA"/>
            <TextBlock HorizontalAlignment="Left" Height="20" Margin="8,52,0,0" TextWrapping="Wrap" Text="Y Axis:" VerticalAlignment="Top" Width="46" Foreground="White"/>
            <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="24" Margin="54,48,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="80" Foreground="#FFFBFBFB"/>
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="8,93,0,0" TextWrapping="Wrap" Text="Z Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFEFBFB"/>
            <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="21" Margin="54,93,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="63" Foreground="#FFF8F3F3"/>

        </Grid>
    </Page>
```

你将需要用你的应用的命名空间替换上面的代码片段中类名称的第一部分。 例如，如果你创建了一个名为**GyrometerCS**的项目，则使用 `x:Class="GyrometerCS.MainPage"` 替换 `x:Class="App1.MainPage"`。 你还应当使用 `xmlns:local="using:GyrometerCS"` 替换 `xmlns:local="using:App1"`。

-   按 F5 或依次选择“调试”**** > “开始调试”****来生成、部署并运行应用。

应用运行后，你可以通过移动设备或使用仿真器工具更改陀螺测试仪的值。

-   通过返回到 Visual Studio 并按 Shift+F5 或依次选择“调试”**** > “停止调试”****来停止应用。

###  <a name="explanation"></a>描述

前面的示例演示了，只需要写入极少的代码即可将陀螺测试仪输入集成到你的应用。

该应用在 **MainPage** 方法中建立了与默认陀螺测试仪的连接。

```csharp
_gyrometer = Gyrometer.GetDefault(); // Get the default gyrometer sensor object
```

该应用在 **MainPage** 方法中建立了报告间隔。 此代码检索设备支持的最短间隔，并将它与所请求的间隔 16 毫秒（大约 60-Hz 刷新率）进行比较。 如果支持的最短间隔大于所请求的间隔，则此代码会将报告间隔设置为所支持的最短间隔。 否则，它会将报告间隔设置为所请求的间隔。

```csharp
uint minReportInterval = _gyrometer.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_gyrometer.ReportInterval = reportInterval;
```

在 **ReadingChanged** 方法中捕获新的陀螺测试仪数据。 每当传感器驱动程序从传感器接收到新数据时，它都将使用此事件处理程序将该值传递到你的应用中。 应用在下行中注册此事件处理程序。

```csharp
_gyrometer.ReadingChanged += new TypedEventHandler<Gyrometer,
GyrometerReadingChangedEventArgs>(ReadingChanged);
```

这些新值写入项目 XAML 中的 TextBlock 中。

```xml
        <TextBlock HorizontalAlignment="Left" Height="23" Margin="8,8,0,0" TextWrapping="Wrap" Text="X-Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFDFDFD"/>
        <TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="23" Margin="67,8,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="88" Foreground="#FFFDFAFA"/>
        <TextBlock HorizontalAlignment="Left" Height="20" Margin="8,52,0,0" TextWrapping="Wrap" Text="Y Axis:" VerticalAlignment="Top" Width="46" Foreground="White"/>
        <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="24" Margin="54,48,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="80" Foreground="#FFFBFBFB"/>
        <TextBlock HorizontalAlignment="Left" Height="21" Margin="8,93,0,0" TextWrapping="Wrap" Text="Z Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFEFBFB"/>
        <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="21" Margin="54,93,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="63" Foreground="#FFF8F3F3"/>
```

 ## <a name="related-topics"></a>相关主题

* [陀螺测试仪示例](http://go.microsoft.com/fwlink/p/?linkid=241379)

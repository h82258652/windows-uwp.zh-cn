---
author: muhsinking
ms.assetid: 16AD53CA-1252-456C-8567-2263D3EC95F3
title: 使用测斜仪
description: 了解如何使用测斜仪确定俯仰、滚转和偏航。
ms.author: mukin
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dd335d56fb2a01ed1b9255f974bcaacd47f623f5
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5621986"
---
# <a name="use-the-inclinometer"></a>使用测斜仪


**重要的 API**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**测斜仪**](https://msdn.microsoft.com/library/windows/apps/BR225766)

**示例**

-   有关更完整的实现，请参阅[测斜仪示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Inclinometer)。

了解如何使用测斜仪确定俯仰、滚转和偏航。

一些三维游戏应用需要将测斜仪作为输入设备。 一个常见的示例是飞行模拟器，它将测斜仪的三条轴（X、Y 和 Z）映射到飞行器的升降舵、副翼和方向舵输入。

 ## <a name="prerequisites"></a>先决条件

你应熟悉 Extensible Application Markup Language (XAML)、 Microsoft VisualC # 和事件。

你使用的设备或仿真器必须支持测斜仪。

 ## <a name="create-a-simple-inclinometer-app"></a>创建简单的测斜仪应用

此部分划分为两个子部分。 第一个子部分将指导你完成从头开始创建简单的测斜仪应用程序所需的步骤。 以下子部分介绍你刚创建的应用。

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


    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private Inclinometer _inclinometer;

            // This event handler writes the current inclinometer reading to
            // the three text blocks on the app' s main page.

            private async void ReadingChanged(object sender, InclinometerReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    InclinometerReading reading = e.Reading;
                    txtPitch.Text = String.Format("{0,5:0.00}", reading.PitchDegrees);
                    txtRoll.Text = String.Format("{0,5:0.00}", reading.RollDegrees);
                    txtYaw.Text = String.Format("{0,5:0.00}", reading.YawDegrees);
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _inclinometer = Inclinometer.GetDefault();


                if (_inclinometer != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _inclinometer.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _inclinometer.ReportInterval = reportInterval;

                    // Establish the event handler
                    _inclinometer.ReadingChanged += new TypedEventHandler<Inclinometer, InclinometerReadingChangedEventArgs>(ReadingChanged);
                }
            }
        }
    }
```

你需要使用你给予项目的名称重命名以上代码片段中的命名空间。 例如，如果你创建了一个名为**InclinometerCS**的项目，则使用 `namespace InclinometerCS` 替换 `namespace App1`。

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
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="0,8,0,0" TextWrapping="Wrap" Text="Pitch: " VerticalAlignment="Top" Width="45" Foreground="#FFF9F4F4"/>
            <TextBlock x:Name="txtPitch" HorizontalAlignment="Left" Height="21" Margin="59,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="71" Foreground="#FFFDF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="0,29,0,0" TextWrapping="Wrap" Text="Roll:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F1F1"/>
            <TextBlock x:Name="txtRoll" HorizontalAlignment="Left" Height="23" Margin="59,29,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="50" Foreground="#FFFCF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="19" Margin="0,56,0,0" TextWrapping="Wrap" Text="Yaw:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F3F3"/>
            <TextBlock x:Name="txtYaw" HorizontalAlignment="Left" Height="19" Margin="55,56,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="54" Foreground="#FFF6F2F2"/>

        </Grid>
    </Page>
```

你将需要用你的应用的命名空间替换上面的代码片段中类名称的第一部分。 例如，如果你创建了一个名为**InclinometerCS**的项目，则使用 `x:Class="InclinometerCS.MainPage"` 替换 `x:Class="App1.MainPage"`。 你还应当使用 `xmlns:local="using:InclinometerCS"` 替换 `xmlns:local="using:App1"`。

-   按 F5 或依次选择“调试”**** > “开始调试”**** 来生成、部署并运行应用。

应用运行后，你可以通过移动设备或使用仿真器工具更改测斜仪的值。

-   通过返回到 Visual Studio 并按 Shift+F5 或依次选择“调试”**** > “停止调试”**** 来停止应用。

###  <a name="explanation"></a>描述

前面的示例演示了，只需要写入极少的代码即可将测斜仪输入集成到你的应用。

该应用在 **MainPage** 方法中建立了与默认测斜仪的连接。

```csharp
_inclinometer = Inclinometer.GetDefault();
```

该应用在 **MainPage** 方法中建立了报告间隔。 此代码检索设备支持的最短间隔，并将它与所请求的间隔 16 毫秒（大约 60-Hz 刷新率）进行比较。 如果支持的最短间隔大于所请求的间隔，则此代码会将报告间隔设置为所支持的最短间隔。 否则，它会将报告间隔设置为所请求的间隔。

```csharp
uint minReportInterval = _inclinometer.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_inclinometer.ReportInterval = reportInterval;
```

在 **ReadingChanged** 方法中捕获新的测斜仪数据。 每当传感器驱动程序从传感器接收到新数据时，它都将使用此事件处理程序将该值传递到你的应用中。 应用在下行中注册此事件处理程序。

```csharp
_inclinometer.ReadingChanged += new TypedEventHandler<Inclinometer,
InclinometerReadingChangedEventArgs>(ReadingChanged);
```

这些新值将写入位于项目 XAML 中的 TextBlock。

```xml
<TextBlock HorizontalAlignment="Left" Height="21" Margin="0,8,0,0" TextWrapping="Wrap" Text="Pitch: " VerticalAlignment="Top" Width="45" Foreground="#FFF9F4F4"/>
 <TextBlock x:Name="txtPitch" HorizontalAlignment="Left" Height="21" Margin="59,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="71" Foreground="#FFFDF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="23" Margin="0,29,0,0" TextWrapping="Wrap" Text="Roll:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F1F1"/>
 <TextBlock x:Name="txtRoll" HorizontalAlignment="Left" Height="23" Margin="59,29,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="50" Foreground="#FFFCF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="19" Margin="0,56,0,0" TextWrapping="Wrap" Text="Yaw:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F3F3"/>
 <TextBlock x:Name="txtYaw" HorizontalAlignment="Left" Height="19" Margin="55,56,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="54" Foreground="#FFF6F2F2"/>
```


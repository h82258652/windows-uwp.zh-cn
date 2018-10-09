---
author: muhsinking
ms.assetid: 5B30E32F-27E0-4656-A834-391A559AC8BC
title: 使用指南针
description: 了解如何使用指南针确定当前方位。
ms.author: mukin
ms.date: 06/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f2bddb9ae3adf8ef6cfdf1b6c078db5eb026c93d
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "4467360"
---
# <a name="use-the-compass"></a>使用指南针


**重要的 API**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Compass**](https://msdn.microsoft.com/library/windows/apps/BR225705)

**示例**

-   有关更完整的实现，请参阅[指南针示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Compass)。

了解如何使用指南针确定当前方位。

应用可以相对于磁性来检索当前的方位，如果为 True，则是北面。 导航应用使用指南针来确定设备面向的方向，然后相应地在地图上定位。

## <a name="prerequisites"></a>先决条件

你应熟悉 Extensible Application Markup Language (XAML)、Microsoft Visual C# 和事件。

你使用的设备或仿真器必须支持指南针。

## <a name="create-a-simple-compass-app"></a>创建一个简单的指南针应用

此部分划分为两个子部分。 第一个子部分将指导你完成从头开始创建简单的指南针应用程序所需的步骤。 以下子部分介绍你刚创建的应用。

### <a name="instructions"></a>说明

-   创建新项目，从“Visual C#”**** 项目模板中选择“空白应用(通用 Windows)”****。

-   打开你项目的 MainPage.xaml.cs 文件，用下列内容替换现有的代码。

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
    using Windows.Devices.Sensors; // Required to access the sensor platform and the compass


    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private Compass _compass; // Our app' s compass object

            // This event handler writes the current compass reading to
            // the textblocks on the app' s main page.

            private async void ReadingChanged(object sender, CompassReadingChangedEventArgs e)
            {
               await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    CompassReading reading = e.Reading;
                    txtMagnetic.Text = String.Format("{0,5:0.00}", reading.HeadingMagneticNorth);
                    if (reading.HeadingTrueNorth.HasValue)
                        txtNorth.Text = String.Format("{0,5:0.00}", reading.HeadingTrueNorth);
                    else
                        txtNorth.Text = "No reading.";
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
               _compass = Compass.GetDefault(); // Get the default compass object

                // Assign an event handler for the compass reading-changed event
                if (_compass != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _compass.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _compass.ReportInterval = reportInterval;
                    _compass.ReadingChanged += new TypedEventHandler<Compass, CompassReadingChangedEventArgs>(ReadingChanged);
                }
            }
        }
    }
    ```

You'll need to rename the namespace in the previous snippet with the name you gave your project. For example, if you created a project named **CompassCS**, you'd replace `namespace App1` with `namespace CompassCS`.

-   Open the file MainPage.xaml and replace the original contents with the following XML.

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
            <TextBlock HorizontalAlignment="Left" Height="22" Margin="8,18,0,0" TextWrapping="Wrap" Text="Magnetic Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFFBF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="18" Margin="8,58,0,0" TextWrapping="Wrap" Text="True North Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFF3F3F3"/>
            <TextBlock x:Name="txtMagnetic" HorizontalAlignment="Left" Height="22" Margin="130,18,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFFBF6F6"/>
            <TextBlock x:Name="txtNorth" HorizontalAlignment="Left" Height="18" Margin="130,58,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFF5F1F1"/>

         </Grid>
    </Page>
```

你将需要用你的应用的命名空间替换上面的代码片段中类名称的第一部分。 例如，如果你创建了一个名为**CompassCS**的项目，则使用 `x:Class="CompassCS.MainPage"` 替换 `x:Class="App1.MainPage"`。 你还应当使用 `xmlns:local="using:CompassCS"` 替换 `xmlns:local="using:App1"`。

-   按 F5 或依次选择“调试”**** > “开始调试”**** 来生成、部署并运行应用。

应用运行后，你可以通过移动设备或使用仿真器工具更改指南针的值。

-   通过返回到 Visual Studio 并按 Shift+F5 或依次选择“调试”**** > “停止调试”**** 来停止应用。

### <a name="explanation"></a>描述

前面的示例演示了，只需要写入极少的代码即可将指南针输入集成到你的应用。

该应用在 **MainPage** 方法中建立了与默认指南针的连接。

```csharp
_compass = Compass.GetDefault(); // Get the default compass object
```

该应用在 **MainPage** 方法中建立了报告间隔。 此代码检索设备支持的最短间隔，并将它与所请求的间隔 16 毫秒（大约 60-Hz 刷新率）进行比较。 如果支持的最短间隔大于所请求的间隔，则此代码会将报告间隔设置为所支持的最短间隔。 否则，它会将报告间隔设置为所请求的间隔。

```csharp
uint minReportInterval = _compass.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_compass.ReportInterval = reportInterval;
```

在 **ReadingChanged** 方法中捕获新的指南针数据。 每当传感器驱动程序从传感器接收到新数据时，它都将使用此事件处理程序将该值传递到你的应用中。 应用在下行中注册此事件处理程序。

```csharp
_compass.ReadingChanged += new TypedEventHandler<Compass,
CompassReadingChangedEventArgs>(ReadingChanged);
```

这些新值将写入位于项目 XAML 中的 TextBlock。

```xml
 <TextBlock HorizontalAlignment="Left" Height="22" Margin="8,18,0,0" TextWrapping="Wrap" Text="Magnetic Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFFBF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="18" Margin="8,58,0,0" TextWrapping="Wrap" Text="True North Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFF3F3F3"/>
 <TextBlock x:Name="txtMagnetic" HorizontalAlignment="Left" Height="22" Margin="130,18,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFFBF6F6"/>
 <TextBlock x:Name="txtNorth" HorizontalAlignment="Left" Height="18" Margin="130,58,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFF5F1F1"/>
```
 

 

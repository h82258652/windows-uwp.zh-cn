---
ms.assetid: 15BAB25C-DA8C-4F13-9B8F-EA9E4270BCE9
title: 使用光传感器
description: 了解如何使用氛围光传感器检测照明变化。
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 47092c128fe3a3855d7e32706451545b357c39c4
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "8207372"
---
# <a name="use-the-light-sensor"></a>使用光传感器


**重要的 API**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**LightSensor**](https://msdn.microsoft.com/library/windows/apps/BR225790)

**示例**

-   有关更完整的实现，请参阅[光传感器示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LightSensor)。

了解如何使用氛围光传感器检测照明变化。

氛围光传感器是允许应用程序响应用户环境变化的多种类型的环境传感器之一。

## <a name="prerequisites"></a>先决条件

你应熟悉 Extensible Application Markup Language (XAML)、 Microsoft VisualC # 和事件。

你使用的设备或仿真器必须支持氛围光传感器。

## <a name="create-a-simple-light-sensor-app"></a>创建一个简单的光传感器应用

此部分划分为两个子部分。 第一个子部分将指导你完成从头开始创建简单的光传感器应用程序所需的步骤。 以下子部分介绍你刚创建的应用。

###  <a name="instructions"></a>说明

-   创建新项目，从“Visual C#”**** 项目模板中选择“空白应用(通用 Windows)”****。

-   打开项目的 BlankPage.xaml.cs 文件，然后使用下列内容替换现有的代码。

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
    using Windows.Devices.Sensors; // Required to access the sensor platform and the ALS

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/p/?linkid=234238

    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class BlankPage : Page
        {
            private LightSensor _lightsensor; // Our app' s lightsensor object

            // This event handler writes the current light-sensor reading to
            // the textbox named "txtLUX" on the app' s main page.

            private void ReadingChanged(object sender, LightSensorReadingChangedEventArgs e)
            {
                Dispatcher.RunAsync(CoreDispatcherPriority.Normal, (s, a) =>
                {
                    LightSensorReading reading = (a.Context as LightSensorReadingChangedEventArgs).Reading;
                    txtLuxValue.Text = String.Format("{0,5:0.00}", reading.IlluminanceInLux);
                });
            }

            public BlankPage()
            {
                InitializeComponent();
                _lightsensor = LightSensor.GetDefault(); // Get the default light sensor object

                // Assign an event handler for the ALS reading-changed event
                if (_lightsensor != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _lightsensor.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _lightsensor.ReportInterval = reportInterval;

                    // Establish the even thandler
                    _lightsensor.ReadingChanged += new TypedEventHandler<LightSensor, LightSensorReadingChangedEventArgs>(ReadingChanged);
                }

            }

        }
    }
```

你需要使用你给予项目的名称重命名以上代码片段中的命名空间。 例如，如果你创建了一个名为**LightingCS**的项目，则使用 `namespace LightingCS` 替换 `namespace App1`。

-   打开文件 MainPage.xaml 并使用以下 XML 替换原始内容。

```xml
    <Page
        x:Class="App1.BlankPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:App1"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid x:Name="LayoutRoot" Background="Black">
            <TextBlock HorizontalAlignment="Left" Height="44" Margin="52,38,0,0" TextWrapping="Wrap" Text="LUX Reading" VerticalAlignment="Top" Width="150"/>
            <TextBlock x:Name="txtLuxValue" HorizontalAlignment="Left" Height="44" Margin="224,38,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="217"/>


        </Grid>

    </Page>
```

你将需要用你的应用的命名空间替换上面的代码片段中类名称的第一部分。 例如，如果你创建了一个名为**LightingCS**的项目，则使用 `x:Class="LightingCS.MainPage"` 替换 `x:Class="App1.MainPage"`。 你还应当使用 `xmlns:local="using:LightingCS"` 替换 `xmlns:local="using:App1"`。

-   按 F5 或依次选择“调试”**** > “开始调试”**** 来生成、部署并运行应用。

应用运行后，你可以通过更改可射入传感器的光线或使用仿真器工具更改光线传感器的值。

-   通过返回到 Visual Studio 并按 Shift+F5 或依次选择“调试”**** > “停止调试”**** 来停止应用。

###  <a name="explanation"></a>描述

前面的示例演示了，只需要写入极少的代码即可将光传感器输入集成到你的应用。

该应用在 **BlankPage** 方法中建立了与默认传感器的连接。

```csharp
_lightsensor = LightSensor.GetDefault(); // Get the default light sensor object
```

该应用在 **BlankPage** 方法中建立了报告间隔。 此代码检索设备支持的最短间隔，并将它与所请求的间隔 16 毫秒（大约 60-Hz 刷新率）进行比较。 如果支持的最短间隔大于所请求的间隔，则此代码会将报告间隔设置为所支持的最短间隔。 否则，它会将报告间隔设置为所请求的间隔。

```csharp
uint minReportInterval = _lightsensor.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_lightsensor.ReportInterval = reportInterval;
```
在 **ReadingChanged** 方法中捕获新的光传感器数据。 每当传感器驱动程序从传感器接收到新数据时，它都将使用此事件处理程序将该值传递到你的应用中。 应用在下行中注册此事件处理程序。

```csharp
_lightsensor.ReadingChanged += new TypedEventHandler<LightSensor,
LightSensorReadingChangedEventArgs>(ReadingChanged);
```

这些新值将写入位于项目 XAML 的 TextBlock 中。

```xml
<TextBlock HorizontalAlignment="Left" Height="44" Margin="52,38,0,0" TextWrapping="Wrap" Text="LUX Reading" VerticalAlignment="Top" Width="150"/>
 <TextBlock x:Name="txtLuxValue" HorizontalAlignment="Left" Height="44" Margin="224,38,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="217"/>
```
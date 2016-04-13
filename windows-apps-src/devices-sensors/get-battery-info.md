---
ms.assetid: 90BB59FC-90FE-453E-A8DE-9315E29EB98C
title: 获取电池信息
description: 了解如何使用 Windows.Devices.Power 命名空间中的 API 获取电池的详细信息。
---
# 获取电池信息

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** 重要的 API **

-   [**Windows.Devices.Power**](https://msdn.microsoft.com/library/windows/apps/Dn895017)
-   [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/BR225432)

了解如何使用 [**Windows.Devices.Power**](https://msdn.microsoft.com/library/windows/apps/Dn895017) 命名空间中的 API 获取电池的详细信息。 *电池报告* ([**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005)) 描述了某一电池或聚合电池的充电、容量和状态。 本主题演示了应用如何获取电池报告和更改通知。 代码示例可从列在本主题末尾处的基本电池应用中获取。

## 获取聚合电池报告


某些设备拥有多个电池，而对于每个电池在此类设备的总能量容量中所发挥的功能，并非总是显而易见。 这时就要用到 [**AggregateBattery**](https://msdn.microsoft.com/library/windows/apps/Dn895011) 类了。 *聚合电池*表示所有连接到设备的电池控制器并可提供一个整体 [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) 对象。

**注意** 实际上，[**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004) 类对应于电池控制器。 控制器有时连接至物理电池，而有时又连接至设备机箱，具体视设备而定。 因此，即使没有电池也可以创建电池对象。 其他时候，电池对象可能为 **null**。

一旦有了聚合电池对象，你便可以调用 [**GetReport**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.getreport) 来获取对应的 [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005)。

```csharp
private void RequestAggregateBatteryReport()
{
    // Create aggregate battery object
    var aggBattery = Battery.AggregateBattery;

    // Get report
    var report = aggBattery.GetReport();

    // Update UI
    AddReportUI(BatteryReportPanel, report, aggBattery.DeviceId);
}
```

## 获取单个电池报告

还可以为各个电池创建 [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) 对象。 将 [**GetDeviceSelector**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.power.battery.getdeviceselector.aspx) 方法与 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/BR225432) 方法结合使用，以获取表示已连接至设备的任何电池控制器的 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 对象集合。 然后，通过所需 **DeviceInformation** 对象的 **Id** 属性，使用 [**FromIdAsync**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.power.battery.fromidasync.aspx) 方法来创建相应的 [**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004)。 最后，调用 [**GetReport**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.getreport) 以获取单个电池报告。

此示例显示了如何为所有连接到设备的电池创建电池报告。

```csharp
async private void RequestIndividualBatteryReports()
{
    // Find batteries 
    var deviceInfo = await DeviceInformation.FindAllAsync(Battery.GetDeviceSelector());
    foreach(DeviceInformation device in deviceInfo)
    {
        try
        {
        // Create battery object
        var battery = await Battery.FromIdAsync(device.Id);

        // Get report
        var report = battery.GetReport();

        // Update UI
        AddReportUI(BatteryReportPanel, report, battery.DeviceId);
        }
        catch { /* Add error handling, as applicable */ }
    }
}
```

## 访问报告详细信息

[
            **BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) 对象提供了大量电池信息。 有关详细信息，请参阅其属性的 API 参考：**Status**（[**BatteryStatus**](https://msdn.microsoft.com/library/windows/apps/Dn818458) 枚举）、[**ChargeRateInMilliwatts**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.power.batteryreport.chargerateinmilliwatts.aspx)、[**DesignCapacityInMilliwattHours**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.power.batteryreport.designcapacityinmilliwatthours.aspx)、[**FullChargeCapacityInMilliwattHours**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.power.batteryreport.fullchargecapacityinmilliwatthours.aspx) 和 [**RemainingCapacityInMilliwattHours**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.batteryreport.remainingcapacityinmilliwatthours)。 此示例显示了一些基本电池应用所使用的电池报告属性，这将在本主题后面介绍。

```csharp
...
TextBlock txt3 = new TextBlock { Text = "Charge rate (mW): " + report.ChargeRateInMilliwatts.ToString() };
TextBlock txt4 = new TextBlock { Text = "Design energy capacity (mWh): " + report.DesignCapacityInMilliwattHours.ToString() };
TextBlock txt5 = new TextBlock { Text = "Fully-charged energy capacity (mWh): " + report.FullChargeCapacityInMilliwattHours.ToString() };
TextBlock txt6 = new TextBlock { Text = "Remaining energy capacity (mWh): " + report.RemainingCapacityInMilliwattHours.ToString() };
...
...
```

## 请求报告更新

当电池的充电、容量或状态发生变化时，[**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004) 对象会触发 [**ReportUpdated**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.reportupdated) 事件。 这通常会在发生状态变化时立即发生，在发生所有其他变化时定期发生。 此示例显示了如何注册电池报告更新。

```csharp
...
Battery.AggregateBattery.ReportUpdated += AggregateBattery_ReportUpdated;
...
```

## 处理报告更新

发生电池更新时，[**ReportUpdated**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.reportupdated) 事件会将相应的 [**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004) 对象传递到事件处理程序方法。 但是，不会从 UI 线程调用此事件处理程序。 你将需要使用 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211) 对象来调用任意 UI 更改，如此示例中所示。

```csharp
async private void AggregateBattery_ReportUpdated(Battery sender, object args)
{
    if (reportRequested)
    {

        await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            // Clear UI
            BatteryReportPanel.Children.Clear();


            if (AggregateButton.IsChecked == true)
            {
                // Request aggregate battery report
                RequestAggregateBatteryReport();
            }
            else
            {
                // Request individual battery report
                RequestIndividualBatteryReports();
            }
        });
    }
}
```

## 示例：基本电池应用

通过在 Microsoft Visual Studio 中生成下列基本电池应用来测试这些 API。 在 Visual Studio 开始页上，单击**“新建项目”**，然后在**“Visual C#”>“Windows”>“通用”**模板下，使用**“空白应用”**模板创建一个新应用。

接下来，打开文件 **MainPage.xaml**，并将下列 XML 复制到此文件中（替换其原始内容）。

```xml
<Page
    x:Class="App1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" >
        <StackPanel VerticalAlignment="Center" Margin="15,30,0,0" >
            <RadioButton x:Name="AggregateButton" Content="Aggregate results" GroupName="Type" IsChecked="True" />
            <RadioButton x:Name="IndividualButton" Content="Individual results" GroupName="Type" IsChecked="False" />
        </StackPanel>
        <StackPanel Orientation="Horizontal">
        <Button x:Name="GetBatteryReportButton" 
                Content="Get battery report" 
                Margin="15,15,0,0" 
                Click="GetBatteryReport"/>
        </StackPanel>
        <StackPanel x:Name="BatteryReportPanel" Margin="15,15,0,0"/>
    </StackPanel>
</Page>
```

如果你的应用未命名为**“App1”**，将需要使用应用的命名空间替换上面的代码片段中类名称的第一部分。 例如，如果创建了一个名为**“BasicBatteryApp”**的项目，请将 `x:Class="App1.MainPage"` 替换为 `x:Class="BasicBatteryApp.MainPage"`。 还应当将 `xmlns:local="using:App1"` 替换为 `xmlns:local="using:BasicBatteryApp"`。

下一步，打开你项目的 **MainPage.xaml.cs** 文件，用下列内容替换现有的代码。

```csharp
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
using Windows.Devices.Enumeration;
using Windows.Devices.Power;
using Windows.UI.Core;

namespace App1
{
    public sealed partial class MainPage : Page
    {
        bool reportRequested = false;
        public MainPage()
        {
            this.InitializeComponent();
            Battery.AggregateBattery.ReportUpdated += AggregateBattery_ReportUpdated;
        }


        private void GetBatteryReport(object sender, RoutedEventArgs e)
        {
            // Clear UI
            BatteryReportPanel.Children.Clear();


            if (AggregateButton.IsChecked == true)
            {
                // Request aggregate battery report
                RequestAggregateBatteryReport();
            }
            else
            {
                // Request individual battery report
                RequestIndividualBatteryReports();
            }

            // Note request
            reportRequested = true;
        }

        private void RequestAggregateBatteryReport()
        {
            // Create aggregate battery object
            var aggBattery = Battery.AggregateBattery;

            // Get report
            var report = aggBattery.GetReport();

            // Update UI
            AddReportUI(BatteryReportPanel, report, aggBattery.DeviceId);
        }

        async private void RequestIndividualBatteryReports()
        {
            // Find batteries 
            var deviceInfo = await DeviceInformation.FindAllAsync(Battery.GetDeviceSelector());
            foreach(DeviceInformation device in deviceInfo)
            {
                try
                {
                // Create battery object
                var battery = await Battery.FromIdAsync(device.Id);

                // Get report
                var report = battery.GetReport();

                // Update UI
                AddReportUI(BatteryReportPanel, report, battery.DeviceId);
                }
                catch { /* Add error handling, as applicable */ }
            }
        }


        private void AddReportUI(StackPanel sp, BatteryReport report, string DeviceID)
        {
            // Create battery report UI
            TextBlock txt1 = new TextBlock { Text = "Device ID: " + DeviceID };
            txt1.FontSize = 15;
            txt1.Margin = new Thickness(0, 15, 0, 0);
            txt1.TextWrapping = TextWrapping.WrapWholeWords;

            TextBlock txt2 = new TextBlock { Text = "Battery status: " + report.Status.ToString() };
            txt2.FontStyle = Windows.UI.Text.FontStyle.Italic;
            txt2.Margin = new Thickness(0, 0, 0, 15);

            TextBlock txt3 = new TextBlock { Text = "Charge rate (mW): " + report.ChargeRateInMilliwatts.ToString() };
            TextBlock txt4 = new TextBlock { Text = "Design energy capacity (mWh): " + report.DesignCapacityInMilliwattHours.ToString() };
            TextBlock txt5 = new TextBlock { Text = "Fully-charged energy capacity (mWh): " + report.FullChargeCapacityInMilliwattHours.ToString() };
            TextBlock txt6 = new TextBlock { Text = "Remaining energy capacity (mWh): " + report.RemainingCapacityInMilliwattHours.ToString() };

            // Create energy capacity progress bar &amp; labels
            TextBlock pbLabel = new TextBlock { Text = "Percent remaining energy capacity" };
            pbLabel.Margin = new Thickness(0,10, 0, 5);
            pbLabel.FontFamily = new FontFamily("Segoe UI");
            pbLabel.FontSize = 11;

            ProgressBar pb = new ProgressBar();
            pb.Margin = new Thickness(0, 5, 0, 0);
            pb.Width = 200;
            pb.Height = 10;
            pb.IsIndeterminate = false;
            pb.HorizontalAlignment = HorizontalAlignment.Left;

            TextBlock pbPercent = new TextBlock();
            pbPercent.Margin = new Thickness(0, 5, 0, 10);
            pbPercent.FontFamily = new FontFamily("Segoe UI");
            pbLabel.FontSize = 11;

            // Disable progress bar if values are null
            if ((report.FullChargeCapacityInMilliwattHours == null)||
                (report.RemainingCapacityInMilliwattHours == null))
            {
                pb.IsEnabled = false;
                pbPercent.Text = "N/A";
            }
            else
            {
                pb.IsEnabled = true;
                pb.Maximum = Convert.ToDouble(report.FullChargeCapacityInMilliwattHours);
                pb.Value = Convert.ToDouble(report.RemainingCapacityInMilliwattHours);
                pbPercent.Text = ((pb.Value / pb.Maximum) * 100).ToString("F2") + "%";
            }

            // Add controls to stackpanel
            sp.Children.Add(txt1);
            sp.Children.Add(txt2);
            sp.Children.Add(txt3);
            sp.Children.Add(txt4);
            sp.Children.Add(txt5);
            sp.Children.Add(txt6);
            sp.Children.Add(pbLabel);
            sp.Children.Add(pb);
            sp.Children.Add(pbPercent);
        }

        async private void AggregateBattery_ReportUpdated(Battery sender, object args)
        {
            if (reportRequested)
            {

                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    // Clear UI
                    BatteryReportPanel.Children.Clear();


                    if (AggregateButton.IsChecked == true)
                    {
                        // Request aggregate battery report
                        RequestAggregateBatteryReport();
                    }
                    else
                    {
                        // Request individual battery report
                        RequestIndividualBatteryReports();
                    }
                });
            }
        }
    }
}
```

如果你的应用未命名为**“App1”**，将需要用你命名项目的名称为以上示例中的命名空间重命名。 例如，如果你创建了一个名为**“BasicBatteryApp”**的项目，请将命名空间 `App1` 替换为命名空间 `BasicBatteryApp`。

最后，若要运行此基本电池应用：在**“调试”**菜单上，单击**“启动调试”**以测试该解决方案。

**提示** 若要从 [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) 对象接收数值，请在**“本地计算机”**或外部**“设备”**（如 Windows Phone）上调试你的应用。 在设备仿真器上调试时，**BatteryReport** 对象会将 **null** 返回到容量和比率属性。

 



<!--HONumber=Mar16_HO1-->



---
author: PatrickFarley
title: "获取用户位置"
description: "查找用户的位置并响应位置更改。 对用户位置的访问由“设置”应用中的隐私设置来管理。 本主题还介绍了如何查看你的应用是否具有访问用户位置的权限。"
ms.assetid: 24DC9A41-8CC1-48B0-BC6D-24BF571AFCC8
translationtype: Human Translation
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: d35bf3ef13e2b36dfed6613a00f65d19b9013464

---

# 获取用户位置


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


查找用户的位置并响应位置更改。 对用户位置的访问由“设置”应用中的隐私设置来管理。 本主题还介绍了如何查看你的应用是否具有访问用户位置的权限。

**提示**若要了解有关在你的应用中访问用户位置的详细信息，请从 GitHub 上的 [Windows-universal-samples 存储库](http://go.microsoft.com/fwlink/p/?LinkId=619979)下载以下示例。

-   [通用 Windows 平台 (UWP) 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## 启用位置功能


1.  在“解决方案资源管理器”****中，双击“package.appxmanifest”****并选择“功能”****选项卡。
2.  在**“功能”**列表中，选择**“功能”**选项卡。 这将向程序包清单文件中添加 `Location` 设备功能。

```XML
  <Capabilities>
    <!-- DeviceCapability elements must follow Capability elements (if present) -->
    <DeviceCapability Name="location"/>
  </Capabilities>
```

## 获取当前位置


此部分介绍如何使用 [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603) 命名空间中的 API 检测用户的地理位置。

### 步骤 1：请求访问用户的位置

**重要提示** 在尝试访问用户的位置之前，必须通过使用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) 方法请求对该位置的访问权限。 必须从 UI 线程调用 **RequestAccessAsync** 方法，并且你的应用必须在前台。 只有在用户授予相应的应用权限后，你的应用才可以访问用户的位置信息。

```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();
```

[
            **RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) 方法提示用户提供访问其位置的权限。 仅提示用户一次（每个应用）。 在他们第一次授予或拒绝授予权限之后，此方法不会再提示用户提供权限。 若要在已进行提示之后帮助用户更改位置权限，我们建议提供一个指向位置设置的链接，如本主题中后面部分所示。

### 步骤 2：获取用户的位置并注册位置权限的更改

[
            **GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) 方法执行当前位置的一次读取。 在此处，**switch** 语句将与 **accessStatus**（来自上一示例）结合使用以便仅在允许访问用户位置时进行操作。 如果允许访问用户位置，该代码将创建 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 对象、注册位置权限的更改，并请求用户位置。

```csharp
switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        _rootPage.NotifyUser("Waiting for update...", NotifyType.StatusMessage);

        // If DesiredAccuracy or DesiredAccuracyInMeters are not set (or value is 0), DesiredAccuracy.Default is used.
        Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = _desireAccuracyInMetersValue };

        // Subscribe to the StatusChanged event to get updates of location status changes.
        _geolocator.StatusChanged += OnStatusChanged;
                        
        // Carry out the operation.
        Geoposition pos = await geolocator.GetGeopositionAsync();

        UpdateLocationData(pos);
        _rootPage.NotifyUser("Location updated.", NotifyType.StatusMessage);
        break;

    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Visible;
        UpdateLocationData(null);
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecified error.", NotifyType.ErrorMessage);
        UpdateLocationData(null);
        break;
}
```

### 步骤 3：处理位置权限的更改

[
            **Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 对象触发 [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 事件以指示用户位置设置已更改。 该事件通过参数的 **Status** 属性（类型为 [**PositionStatus**](https://msdn.microsoft.com/library/windows/apps/br225599)）传递相应的状态。 请注意，此方法不是从 UI 线程中调用的，并且 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 对象调用了 UI 更改。

```csharp
using Windows.UI.Core;
...
async private void OnStatusChanged(Geolocator sender, StatusChangedEventArgs e)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        // Show the location setting message only if status is disabled.
        LocationDisabledMessage.Visibility = Visibility.Collapsed;

        switch (e.Status)
        {
            case PositionStatus.Ready:
                // Location platform is providing valid data.
                ScenarioOutput_Status.Text = "Ready";
                _rootPage.NotifyUser("Location platform is ready.", NotifyType.StatusMessage);
                break;

            case PositionStatus.Initializing:
                // Location platform is attempting to acquire a fix. 
                ScenarioOutput_Status.Text = "Initializing";
                _rootPage.NotifyUser("Location platform is attempting to obtain a position.", NotifyType.StatusMessage);
                break;

            case PositionStatus.NoData:
                // Location platform could not obtain location data.
                ScenarioOutput_Status.Text = "No data";
                _rootPage.NotifyUser("Not able to determine the location.", NotifyType.ErrorMessage);
                break;

            case PositionStatus.Disabled:
                // The permission to access location data is denied by the user or other policies.
                ScenarioOutput_Status.Text = "Disabled";
                _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);

                // Show message to the user to go to location settings.
                LocationDisabledMessage.Visibility = Visibility.Visible;

                // Clear any cached location data.
                UpdateLocationData(null);
                break;

            case PositionStatus.NotInitialized:
                // The location platform is not initialized. This indicates that the application 
                // has not made a request for location data.
                ScenarioOutput_Status.Text = "Not initialized";
                _rootPage.NotifyUser("No request for location is made yet.", NotifyType.StatusMessage);
                break;

            case PositionStatus.NotAvailable:
                // The location platform is not available on this version of the OS.
                ScenarioOutput_Status.Text = "Not available";
                _rootPage.NotifyUser("Location is not available on this version of the OS.", NotifyType.ErrorMessage);
                break;

            default:
                ScenarioOutput_Status.Text = "Unknown";
                _rootPage.NotifyUser(string.Empty, NotifyType.StatusMessage);
                break;
        }
    });
}
```

## 响应位置更新


本部分介绍如何使用 [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 事件来接收一段时间内的用户的位置更新。 由于用户随时可能会取消对位置的访问，请务必调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) 和使用 [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 事件，如上一部分中所示。

本部分假定你已启用位置功能并且已从前台应用的 UI 线程调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152)。

### 步骤 1：定义报告间隔和注册位置更新

在本示例中，将一个 **switch** 语句与 **accessStatus**（来自上一示例）一起使用，以便仅在允许访问用户位置时进行操作。 如果允许访问用户位置，该代码将创建 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 对象、指定跟踪类型并注册位置更新。

[
            **Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 对象可以基于位置更改（基于距离的跟踪）或时间更改（基于定期的跟踪）触发 [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 事件。

-   对于基于距离的跟踪，设置 [**MovementThreshold**](https://msdn.microsoft.com/library/windows/apps/br225539) 属性。
-   对于基于周期的跟踪，设置 [**ReportInterval**](https://msdn.microsoft.com/library/windows/apps/br225541) 属性。

如果两个属性都未设置，则每隔 1 秒（相当于 `ReportInterval = 1000`）返回一个位置。 此处，使用 2 秒 (`ReportInterval = 2000`) 报告间隔。
```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();

switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        // Create Geolocator and define perodic-based tracking (2 second interval).
        _geolocator = new Geolocator { ReportInterval = 2000 };

        // Subscribe to the PositionChanged event to get location updates.
        _geolocator.PositionChanged += OnPositionChanged;

        // Subscribe to StatusChanged event to get updates of location status changes.
        _geolocator.StatusChanged += OnStatusChanged;
                    
        _rootPage.NotifyUser("Waiting for update...", NotifyType.StatusMessage);
        LocationDisabledMessage.Visibility = Visibility.Collapsed;
        StartTrackingButton.IsEnabled = false;
        StopTrackingButton.IsEnabled = true;
        break;

    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Visible;
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecificed error!", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Collapsed;
        break;
}
```

### 步骤 2：处理位置更新

[
            **Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 对象通过触发 [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 事件以指示用户的位置更改或时间已过，具体取决于配置它的方式。 该事件通过参数的 **Position** 属性（属于类型 [**Geoposition**](https://msdn.microsoft.com/library/windows/apps/br225543)）来传递相应的位置信息。 在此示例中，该方法不是从 UI 线程中调用的，并且 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 对象调用了 UI 更改。

```csharp
using Windows.UI.Core;
...
async private void OnPositionChanged(Geolocator sender, PositionChangedEventArgs e)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        _rootPage.NotifyUser("Location updated.", NotifyType.StatusMessage);
        UpdateLocationData(e.Position);
    });
}
```

## 更改位置隐私设置


如果位置隐私设置不允许你的应用访问用户位置，我们建议提供一个指向**“设置”**应用中的**“位置隐私设置”**的便捷链接。 在此示例中，“超链接”控件用于导航到 `ms-settings:privacy-location` URI。

```xml
<!--Set Visibility to Visible when access to location is denied -->  
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic" 
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access Location. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-location">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the location privacy settings."/>
</TextBlock>
```

此外，你的应用可以调用 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 方法，以从代码启动**“设置”**应用。 有关详细信息，请参阅[启动 Windows 设置应用](https://msdn.microsoft.com/library/windows/apps/mt228342)。

```csharp
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-location"));
```

## 对应用进行故障排除


在你的应用可以访问用户位置之前，必须在设备上启用**“位置”**。 在“设置”****应用中，检查以下“位置隐私设置”****是否已打开：

-   **“此设备的位置...”**已**“打开”**（在 Windows 10 移动版中不适用）
-   位置服务设置（“位置”****）已“打开”****
-   在**“选择可以使用你的位置的应用”**下，你的应用已设置为**“打开”**

## 相关主题

* [UWP 地理位置示例](http://go.microsoft.com/fwlink/p/?linkid=533278)
* [地理围栏设计指南](https://msdn.microsoft.com/library/windows/apps/dn631756)
* [位置感知应用设计指南](https://msdn.microsoft.com/library/windows/apps/hh465148)





<!--HONumber=Jun16_HO4-->



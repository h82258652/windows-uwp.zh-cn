---
author: PatrickFarley
title: "设置地理围栏"
description: "在你的应用中设置地理围栏并了解如何处理前台和后台中的通知。"
ms.assetid: A3A46E03-0751-4DBD-A2A1-2323DB09BDBA
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 地图, 位置, 地理围栏, 通知"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 8a143359948e536d30efb425055969ae8ac0987f
ms.lasthandoff: 02/07/2017

---

# <a name="set-up-a-geofence"></a>设置地理围栏


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


在你的应用中设置 [**Geofence**](https://msdn.microsoft.com/library/windows/apps/dn263587) 并了解如何处理前台和后台中的通知。

**提示** 若要了解有关在你的应用中访问位置的详细信息，请从 GitHub 上的 [Windows-universal-samples 存储库](http://go.microsoft.com/fwlink/p/?LinkId=619979)下载以下示例。

-   [通用 Windows 平台 (UWP) 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## <a name="enable-the-location-capability"></a>启用位置功能


1.  在“解决方案资源管理器”****中，双击“package.appxmanifest”****并选择“功能”****选项卡。
2.  在**“功能”**列表中，选中**“位置”**。 这将向程序包清单文件中添加 `Location` 设备功能。

```xml
  <Capabilities>
    <!-- DeviceCapability elements must follow Capability elements (if present) -->
    <DeviceCapability Name="location"/>
  </Capabilities>
```

## <a name="set-up-a-geofence"></a>设置地理围栏


### <a name="step-1-request-access-to-the-users-location"></a>步骤 1：请求访问用户的位置

**重要提示** 在尝试访问用户的位置之前，必须通过使用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) 方法请求对该位置的访问权限。 必须从 UI 线程调用 **RequestAccessAsync** 方法，并且你的应用必须在前台。 只有在用户授予相应的应用权限后，你的应用才可以访问用户的位置信息。

```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();
```

[**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) 方法提示用户提供访问其位置的权限。 仅提示用户一次（每个应用）。 在他们第一次授予或拒绝授予权限之后，此方法不会再提示用户提供权限。 若要在提示之后帮助用户更改位置权限，我们建议提供位置设置的链接，如本主题中后面部分所示。

### <a name="step-2-register-for-changes-in-geofence-state-and-location-permissions"></a>步骤 2：注册地理围栏状态和位置权限的更改

在本示例中，将一个 **switch** 语句与 **accessStatus**（来自上一示例）一起使用，以便仅在允许访问用户位置时进行操作。 如果允许访问用户的位置，代码将访问当前地理围栏、注册地理围栏状态更改并注册位置权限的更改。

**提示** 在使用地理围栏时，使用 GeofenceMonitor 类中的 [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/dn263646) 事件监视位置权限的更改，而不使用 Geolocator 类中的 StatusChanged 事件。 **Disabled** 的 [**GeofenceMonitorStatus**](https://msdn.microsoft.com/library/windows/apps/dn263599) 等效于已禁用的 [**PositionStatus**](https://msdn.microsoft.com/library/windows/apps/br225599)，两者都指示该应用没有访问用户位置的权限。

```csharp
switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        geofences = GeofenceMonitor.Current.Geofences;

        FillRegisteredGeofenceListBoxWithExistingGeofences();
        FillEventListBoxWithExistingEvents();

        // Register for state change events.
        GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged += OnGeofenceStatusChanged;
        break;

    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access denied.", NotifyType.ErrorMessage);
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecified error.", NotifyType.ErrorMessage);
        break;
}
```

然后，当导航离开前台应用时，取消注册事件侦听器。

```csharp
protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    GeofenceMonitor.Current.GeofenceStateChanged -= OnGeofenceStateChanged;
    GeofenceMonitor.Current.StatusChanged -= OnGeofenceStatusChanged;

    base.OnNavigatingFrom(e);
}
```

### <a name="step-3-create-the-geofence"></a>步骤 3：创建地理围栏

现在，你可以随时定义和设置 [**Geofence**](https://msdn.microsoft.com/library/windows/apps/dn263587) 对象。 有多个不同的构造函数重载可供选择，具体取决于你的需求。 在最基本的地理围栏构造函数中，仅指定 [**Id**](https://msdn.microsoft.com/library/windows/apps/dn263724) 和 [**Geoshape**](https://msdn.microsoft.com/library/windows/apps/dn263718)，如下所示。

```csharp
// Set the fence ID.
string fenceId = "fence1";

// Define the fence location and radius.
BasicGeoposition position;
position.Latitude = 47.6510;
position.Longitude = -122.3473;
position.Altitude = 0.0;
double radius = 10; // in meters

// Set a circular region for the geofence.
Geocircle geocircle = new Geocircle(position, radius);

// Create the geofence.
Geofence geofence = new Geofence(fenceId, geocircle);
```

你可以使用其他构造函数之一来进一步微调地理围栏。 在下一个示例中，地理围栏构造函数将指定这些其他参数：

-   [**MonitoredStates**](https://msdn.microsoft.com/library/windows/apps/dn263728) - 指示你想要哪些地理围栏事件来接收有关输入定义的区域、保留定义的区域或者删除地理围栏的通知。
-   [**SingleUse**](https://msdn.microsoft.com/library/windows/apps/dn263732) - 在满足监视地理围栏的所有状态后，将删除地理围栏。
-   [**DwellTime**](https://msdn.microsoft.com/library/windows/apps/dn263703) - 指示在触发进入/退出事件之前，用户必须位于所定义区域之内/之外的时间。
-   [**StartTime**](https://msdn.microsoft.com/library/windows/apps/dn263735) - 指示何时开始监视地理围栏。
-   [**Duration**](https://msdn.microsoft.com/library/windows/apps/dn263697) - 指示监视地理围栏的时段。

```csharp
// Set the fence ID.
string fenceId = "fence2";

// Define the fence location and radius.
BasicGeoposition position;
position.Latitude = 47.6510;
position.Longitude = -122.3473;
position.Altitude = 0.0;
double radius = 10; // in meters

// Set the circular region for geofence.
Geocircle geocircle = new Geocircle(position, radius);

// Remove the geofence after the first trigger.
bool singleUse = true;

// Set the monitored states.
MonitoredGeofenceStates monitoredStates =
                MonitoredGeofenceStates.Entered |
                MonitoredGeofenceStates.Exited |
                MonitoredGeofenceStates.Removed;

// Set how long you need to be in geofence for the enter event to fire.
TimeSpan dwellTime = TimeSpan.FromMinutes(5);

// Set how long the geofence should be active.
TimeSpan duration = TimeSpan.FromDays(1);

// Set up the start time of the geofence.
DateTimeOffset startTime = DateTime.Now;

// Create the geofence.
Geofence geofence = new Geofence(fenceId, geocircle, monitoredStates, singleUse, dwellTime, startTime, duration);
```

### <a name="step-4-handle-changes-in-location-permissions"></a>步骤 4：处理位置权限的更改

[**GeofenceMonitor**](https://msdn.microsoft.com/library/windows/apps/dn263595) 对象触发 [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/dn263646) 事件以指示用户位置设置已更改。 该事件通过参数的 **sender.Status** 属性（类型为 [**GeofenceMonitorStatus**](https://msdn.microsoft.com/library/windows/apps/dn263599)）传递相应的状态。 请注意，此方法不是从 UI 线程中调用的，并且 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 对象调用了 UI 更改。

```csharp
using Windows.UI.Core;
...
public async void OnGeofenceStatusChanged(GeofenceMonitor sender, object e)
{
   await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
   {
    // Show the location setting message only if the status is disabled.
    LocationDisabledMessage.Visibility = Visibility.Collapsed;

    switch (sender.Status)
    {
     case GeofenceMonitorStatus.Ready:
      _rootPage.NotifyUser("The monitor is ready and active.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.Initializing:
      _rootPage.NotifyUser("The monitor is in the process of initializing.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.NoData:
      _rootPage.NotifyUser("There is no data on the status of the monitor.", NotifyType.ErrorMessage);
      break;

     case GeofenceMonitorStatus.Disabled:
      _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);

      // Show the message to the user to go to the location settings.
      LocationDisabledMessage.Visibility = Visibility.Visible;
      break;

     case GeofenceMonitorStatus.NotInitialized:
      _rootPage.NotifyUser("The geofence monitor has not been initialized.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.NotAvailable:
      _rootPage.NotifyUser("The geofence monitor is not available.", NotifyType.ErrorMessage);
      break;

     default:
      ScenarioOutput_Status.Text = "Unknown";
      _rootPage.NotifyUser(string.Empty, NotifyType.StatusMessage);
      break;
    }
   });
}
```

## <a name="set-up-foreground-notifications"></a>设置前台通知


在创建地理围栏后，必须添加逻辑，以便处理发生地理围栏事件时所发生的情况。 按照你设置的 [**MonitoredStates**](https://msdn.microsoft.com/library/windows/apps/dn263728)，你可能会在以下情况下收到事件：

-   用户进入关注的区域。
-   用户离开关注的区域。
-   地理围栏已过期或者被删除。 请注意，删除事件并不能激活后台应用。

当应用正在运行时，你可以直接从应用中侦听事件，或者注册一个后台任务，以便在发生事件时接收后台通知。

### <a name="step-1-register-for-geofence-state-change-events"></a>步骤 1：注册地理围栏状态更改事件

为了让你的应用能够收到地理围栏状态更改的前台通知，必须注册一个事件处理程序。 这通常需要在创建地理围栏时进行设置。

```csharp
private void Initialize()
{
    // Other initialization logic

    GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
}

```

### <a name="step-2-implement-the-geofence-event-handler"></a>步骤 2：实现地理围栏事件处理程序

下一步是实现事件处理程序。 此处采取的措施取决于你的应用使用地理围栏的意图。

```csharp
public async void OnGeofenceStateChanged(GeofenceMonitor sender, object e)
{
    var reports = sender.ReadReports();

    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        foreach (GeofenceStateChangeReport report in reports)
        {
            GeofenceState state = report.NewState;

            Geofence geofence = report.Geofence;

            if (state == GeofenceState.Removed)
            {
                // Remove the geofence from the geofences collection.
                GeofenceMonitor.Current.Geofences.Remove(geofence);
            }
            else if (state == GeofenceState.Entered)
            {
                // Your app takes action based on the entered event.

                // NOTE: You might want to write your app to take a particular
                // action based on whether the app has internet connectivity.

            }
            else if (state == GeofenceState.Exited)
            {
                // Your app takes action based on the exited event.

                // NOTE: You might want to write your app to take a particular
                // action based on whether the app has internet connectivity.

            }
        }
    });
}



```

## <a name="set-up-background-notifications"></a>设置后台通知


在创建地理围栏后，必须添加逻辑，以便处理发生地理围栏事件时所发生的情况。 按照你设置的 [**MonitoredStates**](https://msdn.microsoft.com/library/windows/apps/dn263728)，你可能会在以下情况下收到事件：

-   用户进入关注的区域。
-   用户离开关注的区域。
-   地理围栏已过期或者被删除。 请注意，删除事件并不能激活后台应用。

侦听后台的地理围栏事件

-   在应用清单中声明后台任务。
-   在应用中注册后台任务。 如果你的应用需要 Internet 访问权限（例如要访问云服务），你可以在触发事件时为其设置标记。 你还可以设置一个标记以确保当触发事件时用户在场，这样就可以保证用户收到通知。
-   如果应用在前台运行，则应提示用户授予应用位置权限。

### <a name="step-1-register-for-geofence-state-change-events"></a>步骤 1：注册地理围栏状态更改事件

在应用清单的“声明”****选项卡下，为位置后台任务添加一个声明。 执行此操作的步骤：

-   添加“后台任务”类型****的声明。
-   设置“位置”类型****的属性任务。
-   在应用中设置一个入口点，以便在事件被触发时调用。

### <a name="step-2-register-the-background-task"></a>步骤 2：注册后台任务

本步骤中的代码将注册地理围栏后台任务。 回想一下，在创建地理围栏时，我们已经检查了位置权限。

```csharp
async private void RegisterBackgroundTask(object sender, RoutedEventArgs e)
{
    // Get permission for a background task from the user. If the user has already answered once,
    // this does nothing and the user must manually update their preference via PC Settings.
    BackgroundAccessStatus backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();

    // Regardless of the answer, register the background task. Note that the user can use
    // the Settings app to prevent your app from running background tasks.
    // Create a new background task builder.
    BackgroundTaskBuilder geofenceTaskBuilder = new BackgroundTaskBuilder();

    geofenceTaskBuilder.Name = SampleBackgroundTaskName;
    geofenceTaskBuilder.TaskEntryPoint = SampleBackgroundTaskEntryPoint;

    // Create a new location trigger.
    var trigger = new LocationTrigger(LocationTriggerType.Geofence);

    // Associate the location trigger with the background task builder.
    geofenceTaskBuilder.SetTrigger(trigger);

    // If it is important that there is user presence and/or
    // internet connection when OnCompleted is called
    // the following could be called before calling Register().
    // SystemCondition condition = new SystemCondition(SystemConditionType.UserPresent | SystemConditionType.InternetAvailable);
    // geofenceTaskBuilder.AddCondition(condition);

    // Register the background task.
    geofenceTask = geofenceTaskBuilder.Register();

    // Associate an event handler with the new background task.
    geofenceTask.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);

    BackgroundTaskState.RegisterBackgroundTask(BackgroundTaskState.LocationTriggerBackgroundTaskName);

    switch (backgroundAccessStatus)
    {
    case BackgroundAccessStatus.Unspecified:
    case BackgroundAccessStatus.Denied:
        rootPage.NotifyUser("This app is not allowed to run in the background.", NotifyType.ErrorMessage);
        break;

    }
}


```

### <a name="step-3-handling-the-background-notification"></a>步骤 3：处理后台通知

应用的状态决定了通知用户时所要采取的具体措施，不过，你可以显示一个 Toast 通知、播放一段声音或者更新一个动态磁贴。 本步骤中的代码处理通知。

```csharp
async private void OnCompleted(IBackgroundTaskRegistration sender, BackgroundTaskCompletedEventArgs e)
{
    if (sender != null)
    {
        // Update the UI with progress reported by the background task.
        await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            try
            {
                // If the background task threw an exception, display the exception in
                // the error text box.
                e.CheckResult();

                // Update the UI with the completion status of the background task.
                // The Run method of the background task sets the LocalSettings.
                var settings = ApplicationData.Current.LocalSettings;

                // Get the status.
                if (settings.Values.ContainsKey("Status"))
                {
                    rootPage.NotifyUser(settings.Values["Status"].ToString(), NotifyType.StatusMessage);
                }

                // Do your app work here.

            }
            catch (Exception ex)
            {
                // The background task had an error.
                rootPage.NotifyUser(ex.ToString(), NotifyType.ErrorMessage);
            }
        });
    }
}


```

## <a name="change-the-privacy-settings"></a>更改隐私设置


如果位置隐私设置不允许你的应用访问用户位置，我们建议提供一个指向**“设置”**应用中的**“位置隐私设置”**的便捷链接。 在此示例中，“超链接”控件用于导航到 `ms-settings:privacy-location` URI。

```xml
<!--Set Visibility to Visible when access to the user's location is denied. -->  
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

## <a name="test-and-debug-your-app"></a>测试和调试应用


测试和调试地理围栏应用非常具有挑战性，因为它们取决于设备的位置。 我们在此处概述了用于测试前台和后台地理围栏的几种方法。

**调试地理围栏应用**

1.  以物理方式将设备移动到新位置。
2.  通过创建一个包含当前物理位置的地理围栏区域来测试是否进入地理围栏，因此你已进入地理围栏中并将立即触发“已进入地理围栏”事件。
3.  使用 Microsoft Visual Studio 仿真器模拟设备的位置。

### <a name="test-and-debug-a-geofencing-app-that-is-running-in-the-foreground"></a>测试和调试在前台运行的地理围栏应用

**测试在前台运行的地理围栏应用**

1.  在 Visual Studio 中生成应用。
2.  在 Visual Studio 模拟器中启动你的应用。
3.  使用这些工具模拟地理围栏区域内外的各种位置。 确保在 [**DwellTime**](https://msdn.microsoft.com/library/windows/apps/dn263703) 属性指定的时间过后等待足够长的时间以触发事件。 请注意，你必须接受启用应用的位置权限提示。 有关模拟位置的详细信息，请参阅[设置设备的模拟地理位置](http://go.microsoft.com/fwlink/p/?LinkID=325245)。
4.  你还可以使用仿真器来估计围栏的大小以及以不同的速度进行检测所需的大致停留时间。

### <a name="test-and-debug-a-geofencing-app-that-is-running-in-the-background"></a>测试和调试在后台运行的地理围栏应用

**测试在后台运行的地理围栏应用**

1.  在 Visual Studio 中生成应用。 请注意，你的应用应该设置“位置”****后台任务类型。
2.  先在本地部署应用。
3.  关闭在本地运行的应用。
4.  在 Visual Studio 模拟器中启动你的应用。 请注意，模拟器一次仅支持在一个应用上模拟后台地理围栏。 不要在模拟器中启动多个地理围栏应用。
5.  在模拟器中，模拟地理围栏区域内外的各种位置。 确保在 [**DwellTime**](https://msdn.microsoft.com/library/windows/apps/dn263703) 后等待足够长的时间以触发事件。 请注意，你必须接受提示才能启用应用的位置权限。
6.  使用 Visual Studio 触发位置后台任务。 有关在 Visual Studio 中触发后台任务的详细信息，请参阅[如何触发后台任务](http://go.microsoft.com/fwlink/p/?LinkID=325378)。

## <a name="troubleshoot-your-app"></a>应用疑难解答


在你的应用可以访问位置之前，必须在设备上启用**“位置”**。 在“设置”****应用中，检查以下“位置隐私设置”****是否已打开：

-   **“此设备的位置...”**已**“打开”**（在 Windows 10 移动版中不适用）
-   位置服务设置（“位置”****）已“打开”****
-   在**“选择可以使用你的位置的应用”**下，你的应用已设置为**“打开”**

## <a name="related-topics"></a>相关主题

* [UWP 地理位置示例](http://go.microsoft.com/fwlink/p/?linkid=533278)
* [地理围栏设计指南](https://msdn.microsoft.com/library/windows/apps/dn631756)
* [位置感知应用设计指南](https://msdn.microsoft.com/library/windows/apps/hh465148)


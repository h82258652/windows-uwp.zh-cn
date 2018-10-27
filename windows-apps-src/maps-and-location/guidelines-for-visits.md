---
author: PatrickFarley
Description: Learn how to use the powerful Visits Tracking feature for more practical location tracking.
title: 有关使用访问跟踪的指南
ms.assetid: 0c101684-48a9-4592-9ed5-6c20f3b830f2
ms.author: pafarley
ms.date: 05/18/2017
ms.topic: article
keywords: windows 10, uwp, 地图, 位置, geovisit, geovisits
ms.localizationpriority: medium
ms.openlocfilehash: 3a78b2689a10dff50696e5e65cc44f79a6f1ea7d
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5691429"
---
# <a name="guidelines-for-using-visits-tracking"></a>有关使用访问跟踪的指南

访问功能简化了位置跟踪过程，使其能够更有效地实现许多应用的实际用途。 访问的定义为用户进出的重要地理区域。 访问类似于[地理围栏](guidelines-for-geofencing.md)，因为只有在用户进入或退出某些感兴趣的区域时，才允许通知应用，并且不需要进行持续位置跟踪，持续跟踪可能会耗用电池使用时间。 但是，与地理围栏不同，访问区域是在平台级别动态标识的，并且不需要由单独的应用显式定义。 此外，选择应用将跟踪哪些访问由单个粒度设置来处理，而不是通过订阅单独的位置来处理。

## <a name="preliminary-setup"></a>初步设置

在继续之前，请确保你的应用能够访问设备的位置。 将需要在清单中声明 `Location`功能并调用 **[Geolocator.RequestAccessAsync](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator.RequestAccessAsync)** 方法，以确保用户为应用提供位置权限。 请参阅[获取用户的位置](get-location.md)，了解有关如何执行该操作的详细信息。 

请记住将 `Geolocation` 命名空间添加到类中。 若要使本指南中的所有代码段都能起作用，将需要执行此操作。

```csharp
using Windows.Devices.Geolocation;
```

## <a name="check-the-latest-visit"></a>检查最新访问
使用访问跟踪功能的最简单方法是检索与访问相关的上一个已知状态更改。 状态更改是一个平台记录事件，在此事件中，用户进入/退出了重要位置、自上次报告以来有重大变化，或者用户位置丢失（请参阅 **[VisitStateChange](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.visitstatechange)** 枚举）。 状态更改由 **[Geovisit](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geovisit)** 实例来表示。 若要检索上次记录的状态更改的 **Geovisit** 实例，只需使用 **[GeovisitMonitor](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geovisitmonitor)** 类中的指定方法。

> [!NOTE]
> 检查上次记录的访问并不能保证系统当前正在跟踪访问。 为了在访问发生时跟踪访问，你必须在前台监视它们或注册后台跟踪（请参阅下面的部分）。

```csharp
private async void GetLatestStateChange() {
    // retrieve the Geovisit instance
    Geovisit latestVisit = await GeovisitMonitor.GetLastReportAsync();

    // Using the properties of "latestVisit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```

### <a name="parse-a-geovisit-instance-optional"></a>分析 Geovisit 实例（可选）
以下方法将存储在 **Geovisit** 实例中的所有信息转换为易读的字符串。 它可用于本指南中的任何方案，有帮于针对正在报告的访问提供反馈。

```csharp
private string ParseGeovisit(Geovisit visit){
    string visitString = null;

    // Use a DateTimeFormatter object to process the timestamp. The following
    // format configuration will give an intuitive representation of the date/time
    Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatterLongTime;
    
    formatterLongTime = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(
        "{hour.integer}:{minute.integer(2)}:{second.integer(2)}", new[] { "en-US" }, "US", 
        Windows.Globalization.CalendarIdentifiers.Gregorian, 
        Windows.Globalization.ClockIdentifiers.TwentyFourHour);
    
    // use this formatter to convert the timestamp to a string, and add to "visitString"
    visitString = formatterLongTime.Format(visit.Timestamp);

    // Next, add on the state change type value
    visitString += " " + visit.StateChange.ToString();

    // Next, add the position information (if any is provided; this will be null if 
    // the reported event was "TrackingLost")
    if (visit.Position != null) {
        visitString += " (" +
        visit.Position.Coordinate.Point.Position.Latitude.ToString() + "," +
        visit.Position.Coordinate.Point.Position.Longitude.ToString() + 
        ")";
    }

    return visitString;
}
```

## <a name="monitor-visits-in-the-foreground"></a>在前台监视访问

上一个部分中使用的 **GeovisitMonitor** 类也会处理随时间发生的状态变化的侦听方案。 你可以通过实例化此类、为其事件注册处理程序方法并调用 `Start` 方法来执行此操作。

```csharp
// this GeovisitMonitor instance will belong to the class scope
GeovisitMonitor monitor;

public void RegisterForVisits() {

    // Create and initialize a new monitor instance.
    monitor = new GeovisitMonitor();
    
    // Attach a handler to receive state change notifications.
    monitor.VisitStateChanged += OnVisitStateChanged;
    
    // Calling the start method will start Visits tracking for a specified scope:
    // For higher granularity such as venue/building level changes, choose Venue.
    // For lower granularity in the range of zipcode level changes, choose City.
    monitor.Start(VisitMonitoringScope.Venue);
}
```

在此示例中，`OnVisitStateChanged` 方法将处理传入访问报告。 相应的 **Geovisit** 实例通过事件参数被传入。

```csharp
private void OnVisitStateChanged(GeoVisitWatcher sender, GeoVisitStateChangedEventArgs args) {
    Geovisit visit = args.Visit;
    
    // Using the properties of "visit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```
针对应用监视完与访问相关的状态变化后，应该停止监视并取消注册事件处理程序。 每当应用暂停或关闭时，也应该完成此操作。

```csharp
public void UnregisterFromVisits() {
    
    // Stop the monitor to stop tracking Visits. Otherwise, tracking will
    // continue until the monitor instance is destroyed.
    monitor.Stop();
    
    // Remove the handler to stop receiving state change events.
    monitor.VisitStateChanged -= OnVisitStateChanged;
}
```

## <a name="monitor-visits-in-the-background"></a>在后台监视访问

你还可以在后台任务中执行访问监视，以便即使在你的应用未打开时，也可以在设备上处理与访问相关的活动。 这是推荐的方法，因为它更通用、更节能。 

本指南将使用[创建和注册进程外后台任务](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)中的模型，其中主应用程序文件位于一个项目中，而后台任务文件位于相同解决方案内的单独项目中。 如果你对执行后台任务不熟悉，建议你首先遵循该指导原则，并在下面进行必要的替换以创建访问处理后台任务。

> [!NOTE]
> 为了简单起见，下面的代码段中省略了一些重要的功能，如错误处理和本地存储。 为了可靠地执行后台访问处理，请参阅[示例应用](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)。


首先，确保你的应用声明了后台任务权限。 在 *Package.appxmanifest* 文件的 `Application/Extensions` 元素中，添加以下扩展（如果还没有 `Extensions` 元素，请添加）。

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.VisitBackgroundTask">
    <BackgroundTasks>
        <Task Type="location" />
    </BackgroundTasks>
</Extension>
```

接下来，将以下代码粘贴到后台任务类的定义中。 此后台任务的 `Run` 方法只将触发器详细信息（包含访问信息）传递到单独的方法中。

```csharp
using Windows.ApplicationModel.Background;

namespace Tasks {
    
    public sealed class VisitBackgroundTask : IBackgroundTask {
        
        public void Run(IBackgroundTaskInstance taskInstance) {
            
            // get a deferral
            BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
            
            // this task's trigger will be a Geovisit trigger
            GeovisitTriggerDetails triggerDetails = taskInstance.TriggerDetails as GeovisitTriggerDetails;

            // Handle Visit reports
            GetVisitReports(triggerDetails);         

            finally {
                deferral.Complete();
            }
        }        
    }
}
```

在此同一类中的某个位置定义 `GetVisitReports` 方法。

```csharp
private void GetVisitReports(GeovisitTriggerDetails triggerDetails) {

    // Read reports from the triggerDetails. This populates the "reports" variable 
    // with all of the Geovisit instances that have been logged since the previous
    // report reading.
    IReadOnlyList<Geovisit> reports = triggerDetails.ReadReports();

    foreach (Geovisit report in reports) {
        // Using the properties of "visit", parse out the time that the state 
        // change was recorded, the device's location when the change was recorded,
        // and the type of state change.
    }

    // Note: depending on the intent of the app, you many wish to store the
    // reports in the app's local storage so they can be retrieved the next time 
    // the app is opened in the foreground.
}
```

接下来，在你的应用的主项目中，你将需要注册此后台任务。 创建一个可以通过某种用户操作来调用或者每次激活类时都会调用的注册方法。

```csharp
// a reference to this registration should be declared at the class level
private IBackgroundTaskRegistration visitTask = null;

// The app must call this method at some point to allow future use of 
// the background task. 
private async void RegisterBackgroundTask(object sender, RoutedEventArgs e) {
    
    string taskName = "MyVisitTask";
    string taskEntryPoint = "Tasks.VisitBackgroundTask";

    // First check whether the task in question is already registered
    foreach (var task in BackgroundTaskRegistration.AllTasks) {
        if (task.Value.Name == taskName) {
            // if a task is found with the name of this app's background task, then
            // return and do not attempt to register this task
            return;
        }
    }
    
    // Attempt to register the background task.
    try {
        // Get permission for a background task from the user. If the user has 
        // already responded once, this does nothing and the user must manually 
        // update their preference via Settings.
        BackgroundAccessStatus backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();

        switch (backgroundAccessStatus) {
            case BackgroundAccessStatus.AlwaysAllowed:
            case BackgroundAccessStatus.AllowedSubjectToSystemPolicy:
                // BackgroundTask is allowed
                break;

            default:
                // notify user that background tasks are disabled for this app
                //...
                break;
        }

        // Create a new background task builder
        BackgroundTaskBuilder visitTaskBuilder = new BackgroundTaskBuilder();

        visitTaskBuilder.Name = exampleTaskName;
        visitTaskBuilder.TaskEntryPoint = taskEntryPoint;

        // Create a new Visit trigger
        var trigger = new GeovisitTrigger();

        // Set the desired monitoring scope.
        // For higher granularity such as venue/building level changes, choose Venue.
        // For lower granularity in the range of zipcode level changes, choose City. 
        trigger.MonitoringScope = VisitMonitoringScope.Venue; 

        // Associate the trigger with the background task builder
        visitTaskBuilder.SetTrigger(trigger);

        // Register the background task
        visitTask = visitTaskBuilder.Register();      
    }
    catch (Exception ex) {
        // notify user that the task failed to register, using ex.ToString()
    }
}
```

这可以确定命名空间 `Tasks` 中名为 `VisitBackgroundTask` 的后台任务类将使用 `location` 触发器类型来进行一些操作。 

你的应用现在应该能够注册访问处理后台任务，并且每当设备记录与访问相关的状态变化时，应该会激活此任务。 你将需要在后台任务类中填写逻辑，以确定如何处理此状态变化信息。

## <a name="related-topics"></a>相关主题
* [创建和注册进程外后台任务](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
* [获取用户位置](get-location.md)
* [Windows.Devices.Geolocation 命名空间](https://docs.microsoft.com/uwp/api/windows.devices.geolocation)
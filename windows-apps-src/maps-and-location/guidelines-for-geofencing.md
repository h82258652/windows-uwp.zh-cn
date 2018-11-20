---
author: PatrickFarley
Description: Follow these best practices for geofencing in your app.
title: 地理围栏应用指南
ms.assetid: F817FA55-325F-4302-81BE-37E6C7ADC281
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 地图, 位置, 地理围栏
ms.localizationpriority: medium
ms.openlocfilehash: 86104f00ed0189290fd0cd718042573d9d592cc3
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7285442"
---
# <a name="guidelines-for-geofencing-apps"></a>地理围栏应用指南




**重要的 API**

-   [**Geofence 类 (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn263587)
-   [**Geolocator 类 (XAML)**](https://msdn.microsoft.com/library/windows/apps/br225534)

在应用中遵循这些适用于[**地理围栏**](https://msdn.microsoft.com/library/windows/apps/dn263744)的最佳做法。

## <a name="recommendations"></a>建议


-   如果在发生 [**Geofence**](https://msdn.microsoft.com/library/windows/apps/dn263587) 事件时应用需要具备 Internet 访问权限，请在创建地理围栏之前先检查有无 Internet 访问权限。
    -   如果应用当前没有 Internet 访问权限，你可以提示用户在你设置地理围栏之前连接到 Internet。
    -   如果不能访问 Internet，请避免消耗进行地理围栏位置检查所需的电源。
-   在地理围栏事件指示 [**Entered**](https://msdn.microsoft.com/library/windows/apps/dn263660) 或 **Exited** 状态发生更改时，通过检查时间戳和当前位置来确保地理围栏通知的相关性。 有关详细信息，请参阅下面的**检查时间戳和当前位置**。
下面的 (#timestamp)，以了解详细信息。
-   当设备无法访问位置信息时，创建异常来管理这些情况，并在需要时通知用户。 由于以下原因，可能不提供位置信息：已禁用权限、设备未包含 GPS 无线电、GPS 信号受阻或者 Wi-Fi 信号不够强。
-   总体而言，没有必要同时在前台和后台侦听地理围栏事件。 然而，如果你的应用需要同时在前台和后台侦听地理围栏事件：

    -   请调用 [**ReadReports**](https://msdn.microsoft.com/library/windows/apps/dn263633) 方法以查明是否发生了某个事件。
    -   在用户看不到你的应用时，将前台事件侦听器取消注册，并在它再次可见时为其重新注册。

    有关代码示例和详细信息，请参阅[后台和前台侦听器](#background-and-foreground-listeners)。

-   对每个应用不要使用多于 1000 个地理围栏。 系统实际上支持每个应用数千个地理围栏，你可以通过使用多于 1000 个来维持良好的应用性能以帮助减少应用的内存使用量。
-   不要创建带有小于 50 米的 radius 的地理围栏。 如果应用必须带有小型 radius 的地理围栏，则最好建议用户在具备 GPS 无线电的设备上使用该应用，确保获得最佳性能。

## <a name="additional-usage-guidance"></a>其他使用指南

### <a name="checking-the-time-stamp-and-current-location"></a>检查时间戳和当前位置

当某个事件表明 [**Entered**](https://msdn.microsoft.com/library/windows/apps/dn263660) 或 **Exited** 状态存在变化时，应同时检查事件的时间戳和你的当前位置。 各种各样的因素都可能会影响用户具体何时处理事件，例如系统没有足够资源来启动后台任务、用户没注意到通知，或者设备处在待机状态（在 Windows 上）。 例如，可能会出现下面的顺序：

-   你的应用创建了一个地理围栏并监视地理围栏有无进入和退出事件。
-   当用户在地理围栏内移动设备时，则会触发一个进入事件。
-   应用会向用户发送一个通知，指明用户当前正处于地理围栏内。
-   用户一直忙于其他的事情，没有注意到这则通知，直至 10 分钟以后才察觉。
-   在这 10 分钟的延迟期内，用户将设备移出了地理围栏。

通过时间戳，你可以判断出过去发生的操作。 通过当前位置，你可以了解到用户现已离开地理围栏。 根据应用的功能，你可能希望过滤掉这个事件。

### <a name="background-and-foreground-listeners"></a>后台侦听器和前台侦听器

通常，你的应用不需要同时在前台和后台任务中侦听 [**Geofence**](https://msdn.microsoft.com/library/windows/apps/dn263587) 事件。 如果遇到可能同时需要在前台和后台任务中侦听的情况，最简洁的方法是由后台任务来处理通知。 如果你同时设置了前台和后台地理围栏侦听器，则无法保证哪个侦听器先被触发，为此你必须一直调用 [**ReadReports**](https://msdn.microsoft.com/library/windows/apps/dn263633) 方法以判断是否发生了事件。

此外，如果你已经同时设置了前台和后台地理围栏侦听器，请在应用对用户不可见时取消注册前台事件侦听器；当应用再次对用户可见时，应重新注册应用。 下面是一些注册可见性事件的示例代码。

```csharp
    Windows.UI.Core.CoreWindow coreWindow;    

    // This needs to be set before InitializeComponent sets up event registration for app visibility
    coreWindow = CoreWindow.GetForCurrentThread();
    coreWindow.VisibilityChanged += OnVisibilityChanged;
```

```javascript
 document.addEventListener("visibilitychange", onVisibilityChanged, false);
```

当可见性出现变化时，你可以按照此处显示的情况，启用或禁用前台事件处理程序。

```csharp
private void OnVisibilityChanged(CoreWindow sender, VisibilityChangedEventArgs args)
{
    // NOTE: After the app is no longer visible on the screen and before the app is suspended
    // you might want your app to use toast notification for any geofence activity.
    // By registering for VisibiltyChanged the app is notified when the app is no longer visible in the foreground.

    if (args.Visible)
    {
        // register for foreground events
        GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged += OnGeofenceStatusChanged;
    }
    else
    {
        // unregister foreground events (let background capture events)
        GeofenceMonitor.Current.GeofenceStateChanged -= OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged -= OnGeofenceStatusChanged;
    }
}
```

```javascript
function onVisibilityChanged() {
    // NOTE: After the app is no longer visible on the screen and before the app is suspended
    // you might want your app to use toast notification for any geofence activity.
    // By registering for VisibiltyChanged the app is notified when the app is no longer visible in the foreground.

    if (document.msVisibilityState === "visible") {
        // register for foreground events
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.addEventListener("geofencestatechanged", onGeofenceStateChanged);
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.addEventListener("statuschanged", onGeofenceStatusChanged);
    } else {
        // unregister foreground events (let background capture events)
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.removeEventListener("geofencestatechanged", onGeofenceStateChanged);
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.removeEventListener("statuschanged", onGeofenceStatusChanged);
    }
}
```

### <a name="sizing-your-geofences"></a>确定地理围栏的大小

虽说 GPS 可以提供最精确的位置信息，但是地理围栏也可以使用 Wi-Fi 或其他位置传感器来确定用户的当前位置。 不过，使用这些方法会影响你创建地理围栏的大小范围。 如果精确度较低，创建较小的地理围栏不会有任何帮助。 通常，建议不要创建半径小于 50 米的地理围栏。 同样，地理围栏后台任务仅在 Windows 上定期运行；如果你使用较小的地理围栏，你可能会完全错过 [**Enter**](https://msdn.microsoft.com/library/windows/apps/dn263660) 或 **Exit** 事件。

如果应用需要使用半径较小的的地理围栏，最好建议用户在具备 GPS 无线电的设备上使用该应用，确保获得最佳性能。

## <a name="related-topics"></a>相关主题


* [设置地理围栏](https://msdn.microsoft.com/library/windows/apps/mt219702)
* [获取当前位置](https://msdn.microsoft.com/library/windows/apps/mt219698)
* [UWP 位置示例（地理位置）](http://go.microsoft.com/fwlink/p/?linkid=533278)
 

 

---
author: msatranjr
title: "使用 2D、3D 和街景视图方式显示地图"
description: "通过使用 MapControl 类，在应用中显示可自定义的地图。 本主题还介绍了鸟瞰图 3D 视图和街景视图。"
ms.assetid: 3839E00B-2C1E-4627-A45F-6DDA98D7077F
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 地图, 位置, 地图控件, 地图视图"
translationtype: Human Translation
ms.sourcegitcommit: 32b5230d62f23430393fc51c73f80fa46bd525fa
ms.openlocfilehash: 7a1687ceb188fdd28943f807b877b28e93ae6937
ms.lasthandoff: 02/07/2017

---

# <a name="display-maps-with-2d-3d-and-streetside-views"></a>使用 2D、3D 和街景视图显示地图


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


通过使用 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 类，在应用中显示可自定义的地图。 本主题还介绍了鸟瞰图 3D 视图和街景视图。

**提示** 若要了解有关在你的应用中使用地图的详细信息，请从 GitHub 上的 [Windows-universal-samples 存储库](http://go.microsoft.com/fwlink/p/?LinkId=619979)下载以下示例。

-   [通用 Windows 平台 (UWP) 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## <a name="add-the-map-control-to-your-app"></a>将地图控件添加到你的应用


通过添加 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 在 XAML 页面上显示地图。 若要使用 **MapControl**，你必须在 XAML 页面或代码中声明 [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751) 命名空间。 如果你从工具箱中拖动控件，此命名空间声明将自动添加。 如果你手动将 **MapControl** 添加到 XAML 页面，则必须在该页面顶部手动添加命名空间声明。

以下示例显示基本地图控件，以及配置地图以便除了接受触摸输入之外，还显示缩放和倾斜控件。 有关自定义地图外观的详细信息，请参阅[配置地图](#configure-the-map)。

```xml
<Page
    x:Class="MapsAndLocation1.DisplayMaps"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MapsAndLocation1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:Maps="using:Windows.UI.Xaml.Controls.Maps"
    mc:Ignorable="d">

 <Grid x:Name="pageGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Maps:MapControl
       x:Name="MapControl1"            
       ZoomInteractionMode="GestureAndControl"
       TiltInteractionMode="GestureAndControl"   
       MapServiceToken="EnterYourAuthenticationKeyHere"/>

 </Grid>
</Page>
```

如果在你的代码中添加地图控件，则必须在该代码文件顶部手动声明命名空间。

```csharp
using Windows.UI.Xaml.Controls.Maps;
...

// Add the MapControl and the specify maps authentication key.
MapControl MapControl2 = new MapControl();
MapControl2.ZoomInteractionMode = MapInteractionMode.GestureAndControl;
MapControl2.TiltInteractionMode = MapInteractionMode.GestureAndControl;
MapControl2.MapServiceToken = "EnterYourAuthenticationKeyHere";
pageGrid.Children.Add(MapControl2);
```

## <a name="get-and-set-a-maps-authentication-key"></a>获取和设置地图验证密钥


在可以使用 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 和地图服务之前，你必须将地图身份验证密钥指定为 [**MapServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn637036) 属性的值。 在上一个示例中，将 `EnterYourAuthenticationKeyHere` 替换为你从[必应地图开发人员中心](https://www.bingmapsportal.com/)获取的密钥。 在你指定地图身份验证密钥之前，文本**“警告：未指定 MapServiceToken”**会继续显示在控件下方。 有关获取和设置地图身份验证密钥的详细信息，请参阅[请求地图身份验证密钥](authentication-key.md)。

## <a name="set-a-starting-location-for-the-map"></a>设置地图的起始位置


在地图上设置要显示的位置，方法为在代码中指定 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637005) 的 [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637004) 属性或在 XAML 标记中绑定属性。 以下示例以西雅图市为中心显示地图。

**提示**  由于字符串不能转换为 [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675)，因此无法在 XAML 标记中为 [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005) 属性指定值，除非使用数据绑定。 （此限制同样适用于 [**MapControl.Location**](https://msdn.microsoft.com/library/windows/apps/dn653264) 附加属性。）

 

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
   // Specify a known location.
   BasicGeoposition cityPosition = new BasicGeoposition() { Latitude = 47.604, Longitude = -122.329 };
   Geopoint cityCenter = new Geopoint(cityPosition);

   // Set the map location.
   MapControl1.Center = cityCenter;
   MapControl1.ZoomLevel = 12;
   MapControl1.LandmarksVisible = true;
}
```

![地图控件的示例。](images/displaymapsexample1.png)

## <a name="set-the-current-location-of-the-map"></a>设置地图的当前位置


在应用可以访问用户位置之前，你的应用必须调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) 方法。 此时，你的应用必须位于前台，并且 **RequestAccessAsync** 必须从 UI 线程中进行调用。 除非用户向你的应用授予访问其位置的权限，否则你的应用将无法访问位置数据。

使用 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/hh973536) 类的 [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/br225534) 方法获取设备的当前位置（如果位置可用）。 若要获取相应的 [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675)，请使用地理位置的地理坐标的 [**Point**](https://msdn.microsoft.com/library/windows/apps/dn263665) 属性。 有关详细信息，请参阅[获取当前位置](get-location.md)。

```csharp
// Set your current location.
var accessStatus = await Geolocator.RequestAccessAsync();
switch (accessStatus)
{
   case GeolocationAccessStatus.Allowed:

      // Get the current location.
      Geolocator geolocator = new Geolocator();
      Geoposition pos = await geolocator.GetGeopositionAsync();
      Geopoint myLocation = pos.Coordinate.Point;

      // Set the map location.
      MapControl1.Center = myLocation;
      MapControl1.ZoomLevel = 12;
      MapControl1.LandmarksVisible = true;
      break;

   case GeolocationAccessStatus.Denied:
      // Handle the case  if access to location is denied.
      break;

   case GeolocationAccessStatus.Unspecified:
      // Handle the case if  an unspecified error occurs.
      break;
}
```

当在地图上显示设备位置时，请考虑显示图形，并根据该位置数据的精确度设置缩放级别。 有关详细信息，请参阅[位置感知应用指南](https://msdn.microsoft.com/library/windows/apps/hh465148)。

## <a name="change-the-location-of-the-map"></a>更改地图的位置


若要更改在 2D 地图中显示的位置，请调用其中一个 [**TrySetViewAsync**](https://msdn.microsoft.com/library/windows/apps/dn637060) 方法的重载。 使用该方法以指定 [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005)、[**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068)、[**Heading**](https://msdn.microsoft.com/library/windows/apps/dn637019) 和 [**Pitch**](https://msdn.microsoft.com/library/windows/apps/dn637044) 的新值。 你也可以在查看更改时，通过提供一个来自 [**MapAnimationKind**](https://msdn.microsoft.com/library/windows/apps/dn637002) 枚举的常数，指定要使用的可选动画。

若要更改 3D 地图的位置，请改为使用 [**TrySetSceneAsync**](https://msdn.microsoft.com/library/windows/apps/dn974296) 方法。 有关详细信息，请参阅[显示 3D 视图](#display-aerial-3d-views)。

调用 [**TrySetViewBoundsAsync**](https://msdn.microsoft.com/library/windows/apps/dn637065) 方法以在地图上显示 [**GeoboundingBox**](https://msdn.microsoft.com/library/windows/apps/dn607949) 的内容。 例如，使用此方法可在地图上显示路线或部分路线。 有关详细信息，请参阅[在地图上显示路线和方向](routes-and-directions.md)。

## <a name="configure-the-map"></a>配置地图


通过设置以下 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 属性的值配置地图及其外观。

**地图设置**

-   通过设置 [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005) 属性将地图的**中心**设置为一个地理点。
-   通过将 [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) 属性设置为介于 1 和 20 之间的某个值，设置地图 **缩放级别**。
-   通过设置 [**Heading**](https://msdn.microsoft.com/library/windows/apps/dn637019) 属性（其中 0 或 360 度 = 北，90 度 = 东，180 度 = 南，270 度 = 西），设置地图的**旋转角度**。
-   通过将 [**DesiredPitch**](https://msdn.microsoft.com/library/windows/apps/dn637012) 属性设置为介于 0 和 65 度之间的某个值，设置地图的**倾斜程度**。

**地图外观**

-   通过使用一个 [**MapStyle**](https://msdn.microsoft.com/library/windows/apps/dn637051) 常数设置 [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637127) 属性来指定地图（例如，道路地图和鸟瞰地图）的**类型**。
-   通过使用一个 [**MapColorScheme**](https://msdn.microsoft.com/library/windows/apps/dn637010) 常数设置 [**ColorScheme**](https://msdn.microsoft.com/library/windows/apps/dn637003) 属性将地图的**配色方案**设置为浅色或深色。

通过设置以下 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 属性的值在地图上显示信息。

-   通过启用或禁用 [**LandmarksVisible**](https://msdn.microsoft.com/library/windows/apps/dn637023) 属性在地图上显示**建筑物和地标**。
-   通过启用或禁用 [**PedestrianFeaturesVisible**](https://msdn.microsoft.com/library/windows/apps/dn637042) 属性在地图上显示**步行功能**，例如公共楼梯。
-   通过启用或禁用 [**TrafficFlowVisible**](https://msdn.microsoft.com/library/windows/apps/dn637055) 属性在地图上显示**路况**。
-   通过将 [**WatermarkMode**](https://msdn.microsoft.com/library/windows/apps/dn637066) 属性设置为一个 [**MapWatermarkMode**](https://msdn.microsoft.com/library/windows/apps/dn610749) 常数，指定是否在地图上显示**水位线**。
-   通过将 [**MapRouteView**](https://msdn.microsoft.com/library/windows/apps/dn637122) 添加到“地图”控件的 [**Routes**](https://msdn.microsoft.com/library/windows/apps/dn637047) 集合，在地图上显示**驾车或步行路线**。 有关详细信息和示例，请参阅[在地图上显示路线和方向](routes-and-directions.md)。

有关如何显示图钉、图形和 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 中的 XAML 控件的详细信息，请参阅[在地图上显示目标点 (POI)](display-poi.md)。

## <a name="display-streetside-views"></a>显示街景视图


街景视图是显示在地图控件顶部的位置的街景级视角。

![地图控件的街景视图的示例。](images/onlystreetside-730width.png)

考虑“内部”街景视图与地图控件中最初显示的地图有所不同的体验。 例如，更改街景视图中的位置不会改变街景视图“之下”的地图的位置或外观。 在你关闭街景视图（通过单击控件右上角的“X”****）之后，原始地图将保持不变。

显示街景视图

1.  通过检查 [**IsStreetsideSupported**](https://msdn.microsoft.com/library/windows/apps/dn974271) 来确定街景视图在设备上是否受支持。
2.  如果支持街景视图，请通过调用 [**FindNearbyAsync**](https://msdn.microsoft.com/library/windows/apps/dn974360) 在指定位置附近创建 [**StreetsidePanorama**](https://msdn.microsoft.com/library/windows/apps/dn974361)。
3.  通过检查 [**StreetsidePanorama**](https://msdn.microsoft.com/library/windows/apps/dn974360) 是否不为空来确定是否可以找到附近全景。
4.  如果找到附近全景，则创建地图控件的 [**CustomExperience**](https://msdn.microsoft.com/library/windows/apps/dn974356) 属性的 [**StreetsideExperience**](https://msdn.microsoft.com/library/windows/apps/dn974263)。

此示例介绍如何显示类似于上一个图像的街景视图。

**注意**  如果地图控件的大小太小，将不会显示总览图。

 

```csharp
private async void showStreetsideView()
{
   // Check if Streetside is supported.
   if (MapControl1.IsStreetsideSupported)
   {
      // Find a panorama near Avenue Gustave Eiffel.
      BasicGeoposition cityPosition = new BasicGeoposition() { Latitude = 48.858, Longitude = 2.295};
      Geopoint cityCenter = new Geopoint(cityPosition);
      StreetsidePanorama panoramaNearCity = await StreetsidePanorama.FindNearbyAsync(cityCenter);

      // Set the Streetside view if a panorama exists.
      if (panoramaNearCity != null)
      {
         // Create the Streetside view.
         StreetsideExperience ssView = new StreetsideExperience(panoramaNearCity);
         ssView.OverviewMapVisible = true;
         MapControl1.CustomExperience = ssView;
      }
   }
   else
   {
      // If Streetside is not supported
      ContentDialog viewNotSupportedDialog = new ContentDialog()
      {
         Title = "Streetside is not supported",
         Content ="\nStreetside views are not supported on this device.",
         PrimaryButtonText = "OK"
      };
      await viewNotSupportedDialog.ShowAsync();            
   }
}
```

## <a name="display-aerial-3d-views"></a>显示鸟瞰图 3D 视图


使用 [**MapScene**](https://msdn.microsoft.com/library/windows/apps/dn974329) 类指定地图的 3D 视角。 地图场景表示显示在地图上的 3D 视图。 [**MapCamera**](https://msdn.microsoft.com/library/windows/apps/dn974244) 类表示可能显示如此一个视图的相机的位置。

![MapCamera 位置与地图场景位置图示](images/mapcontrol-techdiagram.png)

若要使地图表面上的建筑物和其他功能以 3D 方式显示，请将地图控件的 [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) 属性设置为 [**MapStyle.Aerial3DWithRoads**](https://msdn.microsoft.com/library/windows/apps/dn637127)。 这是一个以 **Aerial3DWithRoads** 样式显示 3D 视图的示例。

![3D 地图视图的示例。](images/only3d-730width.png)

显示 3D 视图

1.  通过检查 [**Is3DSupported**](https://msdn.microsoft.com/library/windows/apps/dn974265) 来确定 3D 视图在设备上是否受支持。
2.  如果支持 3D 视图，请将地图控件的 [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) 属性设置为 [**MapStyle.Aerial3DWithRoads**](https://msdn.microsoft.com/library/windows/apps/dn637127)。
3.  使用许多 **CreateFrom** 方法之一（例如 [**CreateFromLocationAndRadius**](https://msdn.microsoft.com/library/windows/apps/dn974329) 和 [**CreateFromCamera**](https://msdn.microsoft.com/library/windows/apps/dn974336)）创建 [**MapScene**](https://msdn.microsoft.com/library/windows/apps/dn974334) 对象。
4.  调用 [**TrySetSceneAsync**](https://msdn.microsoft.com/library/windows/apps/dn974296) 以显示 3D 视图。 你也可以在查看更改时，通过提供一个来自 [**MapAnimationKind**](https://msdn.microsoft.com/library/windows/apps/dn637002) 枚举的常数，指定要使用的可选动画。

此示例介绍如何显示 3D 视图。

```csharp
private async void display3DLocation()
{
   if (MapControl1.Is3DSupported)
   {
      // Set the aerial 3D view.
      MapControl1.Style = MapStyle.Aerial3DWithRoads;

      // Specify the location.
      BasicGeoposition hwGeoposition = new BasicGeoposition() { Latitude = 34.134, Longitude = -118.3216};
      Geopoint hwPoint = new Geopoint(hwGeoposition);

      // Create the map scene.
      MapScene hwScene = MapScene.CreateFromLocationAndRadius(hwPoint,
                                                                           80, /* show this many meters around */
                                                                           0, /* looking at it to the North*/
                                                                           60 /* degrees pitch */);
      // Set the 3D view with animation.
      await MapControl1.TrySetSceneAsync(hwScene,MapAnimationKind.Bow);
   }
   else
   {
      // If 3D views are not supported, display dialog.
      ContentDialog viewNotSupportedDialog = new ContentDialog()
      {
         Title = "3D is not supported",
         Content = "\n3D views are not supported on this device.",
         PrimaryButtonText = "OK"
      };
      await viewNotSupportedDialog.ShowAsync();
   }
}
```

## <a name="get-info-about-locations-and-elements"></a>获取有关位置和元素的信息


通过调用 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 的以下方法，在地图上获取有关位置的信息。

-   [**GetLocationFromOffset**](https://msdn.microsoft.com/library/windows/apps/dn637016) 方法 - 获取与地图控件视口中指定点相对应的地理位置。
-   [**GetOffsetFromLocation**](https://msdn.microsoft.com/library/windows/apps/dn637018) 方法 - 在地图控件的视口中获取与指定地理位置相对应的点。
-   [**IsLocationInView**](https://msdn.microsoft.com/library/windows/apps/dn637022) 方法 - 确定指定的地理位置当前在地图控件的视口中是否可见。
-   [**FindMapElementsAtOffset**](https://msdn.microsoft.com/library/windows/apps/dn637014) 方法 - 在位于地图控件视口中指定点的地图上获取元素。

## <a name="handle-user-interaction-and-changes"></a>处理用户交互和更改


通过处理 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 的以下事件，处理地图上的用户输入手势。 通过检查 [**MapInputEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637091) 的 [**Location**](https://msdn.microsoft.com/library/windows/apps/dn637093) 和 [**Position**](https://msdn.microsoft.com/library/windows/apps/dn637090) 属性的值，获取有关地图上的地理位置和出现手势的视口所在的物理位置的信息。

-   [**MapTapped**](https://msdn.microsoft.com/library/windows/apps/dn637038)
-   [**MapDoubleTapped**](https://msdn.microsoft.com/library/windows/apps/dn637032)
-   [**MapHolding**](https://msdn.microsoft.com/library/windows/apps/dn637035)

通过处理控件的 [**LoadingStatusChanged**](https://msdn.microsoft.com/library/windows/apps/dn637028) 事件，确定地图是正在加载还是已完全加载。

通过处理 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 的以下事件，处理用户或应用更改地图的设置时所发生的更改。 [地图指南](https://msdn.microsoft.com/library/windows/apps/dn596102)

-   [**CenterChanged**](https://msdn.microsoft.com/library/windows/apps/dn637006)
-   [**HeadingChanged**](https://msdn.microsoft.com/library/windows/apps/dn637020)
-   [**PitchChanged**](https://msdn.microsoft.com/library/windows/apps/dn637045)
-   [**ZoomLevelChanged**](https://msdn.microsoft.com/library/windows/apps/dn637069)

## <a name="related-topics"></a>相关主题

* [必应地图开发人员中心](https://www.bingmapsportal.com/)
* [UWP 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [获取当前位置](get-location.md)
* [位置感知应用设计指南](https://msdn.microsoft.com/library/windows/apps/hh465148)
* [地图设计指南](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [版本 2015 视频：在 Windows 应用中跨手机、平板电脑和 PC 利用地图和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 路况应用示例](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)


---
author: normesta
title: 使用 2D、3D 和街景视图方式显示地图
description: 你可以在名为地图*地点卡* 的可轻型消除窗口中或在功能齐全的地图控件中显示地图。
ms.assetid: 3839E00B-2C1E-4627-A45F-6DDA98D7077F
ms.author: normesta
ms.date: 03/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 地图, 位置, 地图控件, 地图视图
ms.localizationpriority: medium
ms.openlocfilehash: ba03d430031ad2bdad6959e2c59500dc6f2d2666
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4352848"
---
# <a name="display-maps-with-2d-3d-and-streetside-views"></a>使用 2D、3D 和 Streetside 视图显示地图

你可以在名为地图*地点卡* 的可轻型消除窗口中或在功能齐全的地图控件中显示地图。

下载[地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)来尝试本指南中所述的一些功能。

<a id="placecard" />

## <a name="display-map-in-a-placecard"></a>在地点卡中显示地图
在上面的轻型弹出窗口中，你可以将地图展示在 UI 元素的上方、下方或侧面，或者展示在应用中用户触摸的区域。 地图可以显示与你的应用中的信息相关的城市或地址。  

此地点卡显示西雅图市。

![显示西雅图市的地点卡](images/placecard-city.png)

以下代码可在一个按钮下方的地点卡中显示西雅图。

```csharp
private void Seattle_Click(object sender, RoutedEventArgs e)
{
    Geopoint seattlePoint = new Geopoint
        (new BasicGeoposition { Latitude = 47.6062, Longitude = -122.3321 });

    PlaceInfo spaceNeedlePlace = PlaceInfo.Create(seattlePoint);

    FrameworkElement targetElement = (FrameworkElement)sender;

    GeneralTransform generalTransform =
        targetElement.TransformToVisual((FrameworkElement)targetElement.Parent);

    Rect rectangle = generalTransform.TransformBounds(new Rect(new Point
        (targetElement.Margin.Left, targetElement.Margin.Top), targetElement.RenderSize));

    spaceNeedlePlace.Show(rectangle, Windows.UI.Popups.Placement.Below);
}
```

此地点卡显示西雅图太空针塔的位置。

![显示太空针塔位置的地点卡](images/placecard-needle.png)

以下代码可在一个按钮下方的地点卡中显示太空针塔。

```csharp
private void SpaceNeedle_Click(object sender, RoutedEventArgs e)
{
    Geopoint spaceNeedlePoint = new Geopoint
        (new BasicGeoposition { Latitude = 47.6205, Longitude = -122.3493 });

    PlaceInfoCreateOptions options = new PlaceInfoCreateOptions();

    options.DisplayAddress = "400 Broad St, Seattle, WA 98109";
    options.DisplayName = "Seattle Space Needle";

    PlaceInfo spaceNeedlePlace =  PlaceInfo.Create(spaceNeedlePoint, options);

    FrameworkElement targetElement = (FrameworkElement)sender;

    GeneralTransform generalTransform =
        targetElement.TransformToVisual((FrameworkElement)targetElement.Parent);

    Rect rectangle = generalTransform.TransformBounds(new Rect(new Point
        (targetElement.Margin.Left, targetElement.Margin.Top), targetElement.RenderSize));

    spaceNeedlePlace.Show(rectangle, Windows.UI.Popups.Placement.Below);
}
```

<a id="map-control" />

## <a name="display-map-in-a-control"></a>在控件中显示地图

在你的应用中使用地图控件来显示丰富且可自定义的地图数据。 地图控件可以显示道路地图、鸟瞰图、3D、视图、路线、搜索结果和交通。 在地图上，你可以显示用户的位置、路线和目标点。 地图还可以显示 3D 鸟瞰图、Streetside 视图、交通、公交和本地企业。

当你希望应用内具有一个允许用户查看特定于应用或通用地理信息的地图时，请使用地图控件。 在应用中有一个地图控件意味着用户无需为了获取该信息而离开应用。

> [!NOTE]
>如果你不介意用户离开应用，请考虑使用 Windows 地图应用提供该信息。 你的应用可以启动 Windows 地图应用来显示特定的地图、路线和搜索结果。 有关详细信息，请参阅[启动 Windows 地图应用](https://msdn.microsoft.com/library/windows/apps/mt228341)。

### <a name="add-a-map-control-to-your-app"></a>向应用中添加地图控件

通过添加 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 在 XAML 页面上显示地图。 若要使用 **MapControl**，你必须在 XAML 页面或代码中声明 [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751) 命名空间。 如果你从工具箱中拖动控件，此命名空间声明将自动添加。 如果你手动将 **MapControl** 添加到 XAML 页面，则必须在该页面顶部手动添加命名空间声明。

以下示例显示基本地图控件，以及配置地图以便除了接受触摸输入之外，还显示缩放和倾斜控件。

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

### <a name="get-and-set-a-maps-authentication-key"></a>获取和设置地图验证密钥

在可以使用 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 和地图服务之前，你必须将地图身份验证密钥指定为 [**MapServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn637036) 属性的值。 在上一个示例中，将 `EnterYourAuthenticationKeyHere` 替换为你从[必应地图开发人员中心](https://www.bingmapsportal.com/)获取的密钥。 在你指定地图身份验证密钥之前，文本 **“警告：未指定 MapServiceToken”** 会继续显示在控件下方。 有关获取和设置地图身份验证密钥的详细信息，请参阅[请求地图身份验证密钥](authentication-key.md)。

## <a name="set-the-location-of-a-map"></a>设置地图的位置
将地图指向你所需的任何位置，或使用用户的当前位置。  

### <a name="set-a-starting-location-for-the-map"></a>设置地图的起始位置

在地图上设置要显示的位置，方法为在代码中指定 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637005) 的 [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637004) 属性或在 XAML 标记中绑定属性。 以下示例以西雅图市为中心显示地图。

> [!NOTE]
> 由于字符串不能转换为 [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675)，因此无法在 XAML 标记中为 [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005) 属性指定值，除非使用数据绑定。 （此限制同样适用于 [**MapControl.Location**](https://msdn.microsoft.com/library/windows/apps/dn653264) 附加属性。）

 
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

### <a name="set-the-current-location-of-the-map"></a>设置地图的当前位置

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

### <a name="change-the-location-of-the-map"></a>更改地图的位置

若要更改在 2D 地图中显示的位置，请调用其中一个 [**TrySetViewAsync**](https://msdn.microsoft.com/library/windows/apps/dn637060) 方法的重载。 使用该方法以指定 [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005)、[**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068)、[**Heading**](https://msdn.microsoft.com/library/windows/apps/dn637019) 和 [**Pitch**](https://msdn.microsoft.com/library/windows/apps/dn637044) 的新值。 你也可以在查看更改时，通过提供一个来自 [**MapAnimationKind**](https://msdn.microsoft.com/library/windows/apps/dn637002) 枚举的常数，指定要使用的可选动画。

若要更改 3D 地图的位置，请改为使用 [**TrySetSceneAsync**](https://msdn.microsoft.com/library/windows/apps/dn974296) 方法。 有关详细信息，请参阅[显示鸟瞰图 3D 视图](#3Dviews)。

调用 [**TrySetViewBoundsAsync**](https://msdn.microsoft.com/library/windows/apps/dn637065) 方法以在地图上显示 [**GeoboundingBox**](https://msdn.microsoft.com/library/windows/apps/dn607949) 的内容。 例如，使用此方法可在地图上显示路线或部分路线。 有关详细信息，请参阅[在地图上显示路线和方向](routes-and-directions.md)。

## <a name="change-the-appearance-of-a-map"></a>更改地图的外观

要自定义地图的外观，请将地图控件的 [**StyleSheet**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.StyleSheet) 属性设置为任何现有 [**MapStyleSheet**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet) 对象。

```csharp
myMap.StyleSheet = MapStyleSheet.RoadDark();
```

![深色样式地图](images/style-dark.png)

你还可以使用 JSON 来定义自定义样式，然后使用该 JSON 创建 [**MapStyleSheet**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet)对象。

可以以交互方式使用[地图样式表编辑器](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft)应用程序创建样式表 JSON。

```csharp
myMap.StyleSheet = MapStyleSheet.ParseFromJson(@"
    {
        ""version"": ""1.0"",
        ""settings"": {
            ""landColor"": ""#FFFFFF"",
            ""spaceColor"": ""#000000""
        },
        ""elements"": {
            ""mapElement"": {
                ""labelColor"": ""#000000"",
                ""labelOutlineColor"": ""#FFFFFF""
            },
            ""water"": {
                ""fillColor"": ""#DDDDDD""
            },
            ""area"": {
                ""fillColor"": ""#EEEEEE""
            },
            ""political"": {
                ""borderStrokeColor"": ""#CCCCCC"",
                ""borderOutlineColor"": ""#00000000""
            }
        }
    }
");
```

![自定义样式地图](images/style-custom.png)

有关完整 JSON 条目参考，请参阅[地图样式表参考](elements-of-map-style-sheet.md)。

你可以基于现有样式表开始自定义，然后根据具体需要使用 JSON 覆盖任何元素。 此示例就使用了一个现有的样式开始自定义，并使用 JSON 仅更改水域的颜色。

```csharp
 MapStyleSheet \customSheet = MapStyleSheet.ParseFromJson(@"
    {
        ""version"": ""1.0"",
        ""elements"": {
            ""water"": {
                ""fillColor"": ""#DDDDDD""
            }
        }
    }
");

MapStyleSheet builtInSheet = MapStyleSheet.RoadDark();

myMap.StyleSheet = MapStyleSheet.Combine(new List<MapStyleSheet> { builtInSheet, customSheet });
```

![结合样式地图](images/style-combined.png)

>[!NOTE]
>你在第二个样式表中定义的样式会覆盖第一个样式表中的样式。

## <a name="set-orientation-and-perspective"></a>设置方向和角度

放大、缩小、旋转和倾斜地图的照相机，调整为你所需效果的相应角度。 尝试这些属性。

-   通过设置 [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005) 属性将地图的**中心**设置为一个地理点。
-   通过将 [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) 属性设置为介于 1 和 20 之间的某个值，设置地图 **缩放级别**。
-   通过设置 [**Heading**](https://msdn.microsoft.com/library/windows/apps/dn637019) 属性（其中 0 或 360 度 = 北，90 度 = 东，180 度 = 南，270 度 = 西），设置地图的**旋转角度**。
-   通过将 [**DesiredPitch**](https://msdn.microsoft.com/library/windows/apps/dn637012) 属性设置为介于 0 和 65 度之间的某个值，设置地图的**倾斜程度**。

## <a name="show-and-hide-map-features"></a>显示和隐藏地图功能

通过设置以下 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 属性的值显示或隐藏地图功能，如道路和地标。

* 通过启用或禁用 [**LandmarksVisible**](https://msdn.microsoft.com/library/windows/apps/dn637023) 属性在地图上显示**建筑物和地标**。

  > [!NOTE]
  > 你可以显示或隐藏建筑物，但无法禁止它们以 3D 形式显示。  

* 通过启用或禁用 [**PedestrianFeaturesVisible**](https://msdn.microsoft.com/library/windows/apps/dn637042) 属性在地图上显示**步行功能**，例如公共楼梯。
* 通过启用或禁用 [**TrafficFlowVisible**](https://msdn.microsoft.com/library/windows/apps/dn637055) 属性在地图上显示**路况**。
* 通过将 [**WatermarkMode**](https://msdn.microsoft.com/library/windows/apps/dn637066) 属性设置为一个 [**MapWatermarkMode**](https://msdn.microsoft.com/library/windows/apps/dn610749) 常数，指定是否在地图上显示**水位线**。
* 通过将 [**MapRouteView**](https://msdn.microsoft.com/library/windows/apps/dn637122) 添加到“地图”控件的 [**Routes**](https://msdn.microsoft.com/library/windows/apps/dn637047) 集合，在地图上显示**驾车或步行路线**。 有关详细信息和示例，请参阅[在地图上显示路线和方向](routes-and-directions.md)。

有关如何显示图钉、图形和 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 中的 XAML 控件的详细信息，请参阅[在地图上显示目标点 (POI)](display-poi.md)。

## <a name="display-streetside-views"></a>显示街景视图


街景视图是显示在地图控件顶部的位置的街景级视角。

![地图控件的街景视图的示例。](images/onlystreetside-730width.png)

考虑“内部”街景视图与地图控件中最初显示的地图有所不同的体验。 例如，更改街景视图中的位置不会改变街景视图“之下”的地图的位置或外观。 在你关闭街景视图（通过单击控件右上角的 **X**）之后，原始地图将保持不变。

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

<a id="3Dviews" />
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
      BasicGeoposition hwGeoposition = new BasicGeoposition() { Latitude = 43.773251, Longitude = 11.255474};
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

## <a name="get-info-about-locations"></a>获取有关位置的信息


通过调用 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 的以下方法，在地图上获取有关位置的信息。

-   [**GetLocationFromOffset**](https://msdn.microsoft.com/library/windows/apps/dn637016) 方法 - 获取与地图控件视口中指定点相对应的地理位置。
-   [**GetOffsetFromLocation**](https://msdn.microsoft.com/library/windows/apps/dn637018) 方法 - 在地图控件的视口中获取与指定地理位置相对应的点。
-   [**IsLocationInView**](https://msdn.microsoft.com/library/windows/apps/dn637022) 方法 - 确定指定的地理位置当前在地图控件的视口中是否可见。
-   [**FindMapElementsAtOffset**](https://msdn.microsoft.com/library/windows/apps/dn637014) 方法 - 在位于地图控件视口中指定点的地图上获取元素。

## <a name="handle-interaction-and-changes"></a>处理交互和更改


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

## <a name="best-practice-recommendations"></a>最佳做法建议

-   使用充足的屏幕空间（或整个屏幕）显示地图，以便用户无需过度平移和缩放来查看地理信息。

-   如果地图仅用于呈现静态的、信息性的视图，则使用较小的地图可能更合适。 如果你选择较小的静态地图，请根据可用性设置其维度：小到足够节省屏幕空间，大到足够保持清晰。

-   使用 [**map elements**](https://msdn.microsoft.com/library/windows/apps/dn637034) 在地图场景中嵌入目标点；任何其他信息都可作为覆盖地图场景的瞬态 UI 显示。

## <a name="related-topics"></a>相关主题

* [必应地图开发人员中心](https://www.bingmapsportal.com/)
* [UWP 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [获取当前位置](get-location.md)
* [位置感知应用设计指南](https://msdn.microsoft.com/library/windows/apps/hh465148)
* [地图设计指南](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [版本 2015 视频：在 Windows 应用中跨手机、平板电脑和 PC 利用地图和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 路况应用示例](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)

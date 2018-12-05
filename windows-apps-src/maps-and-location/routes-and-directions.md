---
title: 在地图上显示路线和方向
description: 请求路线和方向并在应用中显示它们。
ms.assetid: BBB4C23A-8F10-41D1-81EA-271BE01AED81
ms.date: 09/20/2017
ms.topic: article
keywords: Windows 10, uwp, 路线, 地图, 位置, 方向
ms.localizationpriority: medium
ms.openlocfilehash: dd93a092ee0db0821e9326d0f9ffa86890850b87
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8686073"
---
# <a name="display-routes-and-directions-on-a-map"></a>在地图上显示路线和方向



请求路线和方向并在应用中显示它们。

>[!Note]
>若要了解有关在应用中使用地图的详细信息，请下载[通用 Windows 平台 (UWP) 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)。
>如果映射不是你的应用的核心功能，请考虑改为启动 Windows 地图应用。 你可以使用 `bingmaps:`、`ms-drive-to:` 和 `ms-walk-to:` URI 方案来将 Windows 地图应用启动为特定的地图和路线规划。 有关详细信息，请参阅[启动 Windows 地图应用](https://msdn.microsoft.com/library/windows/apps/mt228341)。

 
## <a name="an-intro-to-maproutefinder-results"></a>MapRouteFinder 结果简介


下面介绍了路线的类如何与方向的类相关联：

* [**MapRouteFinder**](https://msdn.microsoft.com/library/windows/apps/dn636938) 类提供了获取路线和方向的方法。 这些方法将返回一个 [**MapRouteFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn636939)。

* [**MapRouteFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn636939) 包含一个 [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) 对象。 通过 **MapRouteFinderResult** 的 [**Route**](https://msdn.microsoft.com/library/windows/apps/dn636940) 属性访问该对象。

* [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) 包含 [**MapRouteLeg**](https://msdn.microsoft.com/library/windows/apps/dn636955) 对象的集合。 通过 **MapRoute** 的 [**Legs**](https://msdn.microsoft.com/library/windows/apps/dn636973) 属性访问该集合。

* 每个 [**MapRouteLeg**](https://msdn.microsoft.com/library/windows/apps/dn636955) 都包含一个 [**MapRouteManeuver**](https://msdn.microsoft.com/library/windows/apps/dn636961) 对象的集合。 通过 **MapRouteLeg** 的 [**Maneuvers**](https://msdn.microsoft.com/library/windows/apps/dn636959) 属性访问该集合。

通过调用 [**MapRouteFinder**](https://msdn.microsoft.com/library/windows/apps/dn636938) 类的方法，获取驾车或步行路线和方向。 例如，[**GetDrivingRouteAsync**](https://msdn.microsoft.com/library/windows/apps/dn636943) 或 [**GetWalkingRouteAsync**](https://msdn.microsoft.com/library/windows/apps/dn636953)。

在请求路线时，你可以指定以下操作：

* 你可以仅提供起点和终点，或者你可以提供一系列的经过点来计算该路线。

    *Stop* 路点添加额外的路线行程，每一个都有自己的路线。 若要指定 *stop* 路点，请使用任一 [**GetDrivingRouteFromWaypointsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getwalkingroutefromwaypointsasync) 重载。

    *Via* 路点定义 *stop* 路点之间的中间位置。 它们不添加路线行程。  它们只是路线必须通过的路点。 若要指定 *via* 路点，请使用任一 [**GetDrivingRouteFromEnhancedWaypointsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getdrivingroutefromenhancedwaypointsasync) 重载。

* 你可以指定优化项（例如：最小化距离）。

* 你可以指定限制项（例如：避开高速公路）。

## <a name="display-directions"></a>显示路线

[**MapRouteFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn636939) 对象包含一个 [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) 对象，你可以通过其 [**Route**](https://msdn.microsoft.com/library/windows/apps/dn636940) 属性访问该对象。

计算的 [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) 具有多个属性，可提供遍历该路线所需的时间、路线的长度以及包含路线段的 [**MapRouteLeg**](https://msdn.microsoft.com/library/windows/apps/dn636955) 对象的集合。 每个 **MapRouteLeg** 对象都包含 [**MapRouteManeuver**](https://msdn.microsoft.com/library/windows/apps/dn636961) 对象的集合。 **MapRouteManeuver** 对象包含可以通过其 [**InstructionText**](https://msdn.microsoft.com/library/windows/apps/dn636964) 属性进行访问的路线。

>[!IMPORTANT]
>必须先指定地图验证密钥，才能使用地图服务。 有关详细信息，请参阅[请求地图身份验证密钥](authentication-key.md)。

 

```csharp
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void button_Click(object sender, RoutedEventArgs e)
{
   // Start at Microsoft in Redmond, Washington.
   BasicGeoposition startLocation = new BasicGeoposition() {Latitude=47.643,Longitude=-122.131};

   // End at the city of Seattle, Washington.
   BasicGeoposition endLocation = new BasicGeoposition() {Latitude = 47.604,Longitude= -122.329};

   // Get the route between the points.
   MapRouteFinderResult routeResult =
         await MapRouteFinder.GetDrivingRouteAsync(
         new Geopoint(startLocation),
         new Geopoint(endLocation),
         MapRouteOptimization.Time,
         MapRouteRestrictions.None);

   if (routeResult.Status == MapRouteFinderStatus.Success)
   {
      System.Text.StringBuilder routeInfo = new System.Text.StringBuilder();

      // Display summary info about the route.
      routeInfo.Append("Total estimated time (minutes) = ");
      routeInfo.Append(routeResult.Route.EstimatedDuration.TotalMinutes.ToString());
      routeInfo.Append("\nTotal length (kilometers) = ");
      routeInfo.Append((routeResult.Route.LengthInMeters / 1000).ToString());

      // Display the directions.
      routeInfo.Append("\n\nDIRECTIONS\n");

      foreach (MapRouteLeg leg in routeResult.Route.Legs)
      {
         foreach (MapRouteManeuver maneuver in leg.Maneuvers)
         {
            routeInfo.AppendLine(maneuver.InstructionText);
         }
      }

      // Load the text box.
      tbOutputText.Text = routeInfo.ToString();
   }
   else
   {
      tbOutputText.Text =
            "A problem occurred: " + routeResult.Status.ToString();
   }
}
```

此示例向 `tbOutputText` 文本框显示以下结果。

``` syntax
Total estimated time (minutes) = 18.4833333333333
Total length (kilometers) = 21.847

DIRECTIONS
Head north on 157th Ave NE.
Turn left onto 159th Ave NE.
Turn left onto NE 40th St.
Turn left onto WA-520 W.
Enter the freeway WA-520 from the right.
Keep left onto I-5 S/Portland.
Keep right and leave the freeway at exit 165A towards James St..
Turn right onto James St.
You have reached your destination.
```

## <a name="display-routes"></a>显示路线


若要在 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 上显示 [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937)，请使用 **MapRoute** 构建一个 [**MapRouteView**](https://msdn.microsoft.com/library/windows/apps/dn637122)。 然后，将 **MapRouteView** 添加到 **MapControl** 的 [**Routes**](https://msdn.microsoft.com/library/windows/apps/dn637047) 集合。

>[!IMPORTANT]
>必须先指定地图身份验证密钥，然后才能使用地图服务或地图控件。 有关详细信息，请参阅[请求地图身份验证密钥](authentication-key.md)。

 

```csharp
using System;
using Windows.Devices.Geolocation;
using Windows.Services.Maps;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Maps;
...
private async void ShowRouteOnMap()
{
   // Start at Microsoft in Redmond, Washington.
   BasicGeoposition startLocation = new BasicGeoposition() { Latitude = 47.643, Longitude = -122.131 };

   // End at the city of Seattle, Washington.
   BasicGeoposition endLocation = new BasicGeoposition() { Latitude = 47.604, Longitude = -122.329 };


   // Get the route between the points.
   MapRouteFinderResult routeResult =
         await MapRouteFinder.GetDrivingRouteAsync(
         new Geopoint(startLocation),
         new Geopoint(endLocation),
         MapRouteOptimization.Time,
         MapRouteRestrictions.None);

   if (routeResult.Status == MapRouteFinderStatus.Success)
   {
      // Use the route to initialize a MapRouteView.
      MapRouteView viewOfRoute = new MapRouteView(routeResult.Route);
      viewOfRoute.RouteColor = Colors.Yellow;
      viewOfRoute.OutlineColor = Colors.Black;

      // Add the new MapRouteView to the Routes collection
      // of the MapControl.
      MapWithRoute.Routes.Add(viewOfRoute);

      // Fit the MapControl to the route.
      await MapWithRoute.TrySetViewBoundsAsync(
            routeResult.Route.BoundingBox,
            null,
            Windows.UI.Xaml.Controls.Maps.MapAnimationKind.None);
   }
}
```

此示例在名为 **MapWithRoute** 的 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 上显示以下内容。

![显示路线的地图控件。](images/routeonmap.png)

下面是使用两个 *stop* 路点之间的 *via* 路点的示例版本：

```csharp
using System;
using Windows.Devices.Geolocation;
using Windows.Services.Maps;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Maps;
...
private async void ShowRouteOnMap()
{
  Geolocator locator = new Geolocator();
  locator.DesiredAccuracyInMeters = 1;
  locator.PositionChanged += Locator_PositionChanged;

  BasicGeoposition point1 = new BasicGeoposition() { Latitude = 47.649693, Longitude = -122.144908 };
  BasicGeoposition point2 = new BasicGeoposition() { Latitude = 47.6205, Longitude = -122.3493 };
  BasicGeoposition point3 = new BasicGeoposition() { Latitude = 48.649693, Longitude = -122.144908 };

  // Get Driving Route from point A  to point B thru point C
  var path = new List<EnhancedWaypoint>();

  path.Add(new EnhancedWaypoint(new Geopoint(point1), WaypointKind.Stop));
  path.Add(new EnhancedWaypoint(new Geopoint(point2), WaypointKind.Via));
  path.Add(new EnhancedWaypoint(new Geopoint(point3), WaypointKind.Stop));

  MapRouteFinderResult routeResult =  await MapRouteFinder.GetDrivingRouteFromEnhancedWaypointsAsync(path);

  if (routeResult.Status == MapRouteFinderStatus.Success)
  {
      MapRouteView viewOfRoute = new MapRouteView(routeResult.Route);
      viewOfRoute.RouteColor = Colors.Yellow;
      viewOfRoute.OutlineColor = Colors.Black;

      myMap.Routes.Add(viewOfRoute);

      await myMap.TrySetViewBoundsAsync(
            routeResult.Route.BoundingBox,
            null,
            Windows.UI.Xaml.Controls.Maps.MapAnimationKind.None);
  }
}
```

## <a name="related-topics"></a>相关主题

* [必应地图开发人员中心](https://www.bingmapsportal.com/)
* [UWP 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [地图设计指南](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [版本 2015 视频：在 Windows 应用中跨手机、平板电脑和 PC 利用地图和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 路况应用示例](http://go.microsoft.com/fwlink/p/?LinkId=619982)

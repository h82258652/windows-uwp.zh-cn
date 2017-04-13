---
author: PatrickFarley
title: "执行地理编码和反向地理编码"
description: "通过调用 Windows.Services.Maps 命名空间中 MapLocationFinder 类的方法将地址转换为地理位置（地理编码）以及将地理位置转换为地址（反向地理编码）。"
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 地理编码, 地图, 位置"
ms.openlocfilehash: a68898e86ad2e901499077e8f856c5318a7feae7
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>执行地理编码和反向地理编码


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


通过调用 [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空间中 [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 类的方法将地址转换为地理位置（地理编码）以及将地理位置转换为地址（反向地理编码）。

**提示**若要了解有关在你的应用中使用地图的详细信息，请从 GitHub 上的 [Windows-universal-samples 存储库](http://go.microsoft.com/fwlink/p/?LinkId=619979)下载以下示例。

-   [通用 Windows 平台 (UWP) 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)

下面介绍了地理编码的类如何与反向地理编码的类相关联：

-   [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 类提供执行地理编码 ([**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)) 和反向地理编码 ([**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)) 的方法。
-   这些方法将返回一个 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551)。
-   [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 包含 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 对象的集合。 通过 **MapLocationFinderResult** 的 [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552) 属性访问该集合。
-   每个 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 对象都包含一个 [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533) 对象。 通过每个 **MapLocation** 的 [**Address**](https://msdn.microsoft.com/library/windows/apps/dn636929) 属性来访问该对象。

**重要提示**  必须先指定地图验证密钥，才能使用地图服务。 有关详细信息，请参阅[请求地图验证密钥](authentication-key.md)。

 

## <a name="get-a-location-geocode"></a>获取位置（地理编码）


通过执行以下步骤，将地址或地点名称转换为地理位置（地理编码）。

1.  调用 [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 类的 [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) 方法的其中一项重载。
2.  [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) 方法将返回一个 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 对象，该对象包含匹配的 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 对象的集合。
3.  通过 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 的 [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552) 属性访问该集合。

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void geocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The address or business to geocode.
   string addressToGeocode = "Microsoft";

   // The nearby location to use as a query hint.
   BasicGeoposition queryHint = new BasicGeoposition();
   queryHint.Latitude = 47.643;
   queryHint.Longitude = -122.131;
   Geopoint hintPoint = new Geopoint(queryHint);

   // Geocode the specified address, using the specified reference point
   // as a query hint. Return no more than 3 results.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAsync(
                           addressToGeocode,
                           hintPoint,
                           3);

   // If the query returns results, display the coordinates
   // of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "result = (" +
            result.Locations[0].Point.Position.Latitude.ToString() + "," +
            result.Locations[0].Point.Position.Longitude.ToString() + ")";
   }
}
```

此代码向 `tbOutputText` 文本框显示以下结果。

``` syntax
result = (47.6406099647284,-122.129339994863)
```

## <a name="get-an-address-reverse-geocode"></a>获取地址（反向地理编码）


通过执行以下步骤，将地理位置转换为地址（反向地理编码）。

1.  调用 [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 类的 [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) 方法。
2.  [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) 方法将返回一个 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 对象，该对象包含匹配的 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 对象的集合。
3.  通过 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 的 [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552) 属性访问该集合。
4.  通过每个 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 的 [**Address**](https://msdn.microsoft.com/library/windows/apps/dn636929) 属性来访问 [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533) 对象。

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void reverseGeocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The location to reverse geocode.
   BasicGeoposition location = new BasicGeoposition();
   location.Latitude = 47.643;
   location.Longitude = -122.131;
   Geopoint pointToReverseGeocode = new Geopoint(location);

   // Reverse geocode the specified geographic location.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAtAsync(pointToReverseGeocode);

   // If the query returns results, display the name of the town
   // contained in the address of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "town = " +
            result.Locations[0].Address.Town;
   }
}
```

此代码向 `tbOutputText` 文本框显示以下结果。

``` syntax
town = Redmond
```

## <a name="related-topics"></a>相关主题

* [必应地图开发人员中心](https://www.bingmapsportal.com/)
* [UWP 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [地图设计指南](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [版本 2015 视频：在 Windows 应用中跨手机、平板电脑和 PC 利用地图和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 路况应用示例](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)
* [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)
* [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)

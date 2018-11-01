---
author: PatrickFarley
title: 执行地理编码和反向地理编码
description: 本指南介绍了如何将街道地址转换为地理位置 （地理编码），并通过调用 Windows.Services.Maps 命名空间中 MapLocationFinder 类的方法，将转换为街道地址 （反向地理编码） 的地理位置。
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.author: pafarley
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp, 地理编码, 地图, 位置
ms.localizationpriority: medium
ms.openlocfilehash: bdd956dece4435ceb8e14121ec2b545095af3a11
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5871934"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>执行地理编码和反向地理编码

本指南介绍了如何将街道地址转换为地理位置 （地理编码） 以及将为街道地址 （反向地理编码） 的地理位置转换通过调用 Windows.Services.Maps[**中[**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)类的方法**](https://msdn.microsoft.com/library/windows/apps/dn636979)命名空间。

> [!TIP]
> 若要了解有关在你的应用中使用地图的详细信息，请从 GitHub 上的[Windows 通用示例存储库](hhttps://github.com/Microsoft/Windows-universal-samples)下载[MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)示例。

涉及地理编码和反向地理编码的类如下所示。

-   [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)类包含处理地理编码 ([**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)) 和反向地理编码 ([**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)) 的方法。
-   这些方法都返回[**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551)实例。
-   [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) [**位置**](https://msdn.microsoft.com/library/windows/apps/dn627552)属性会公开[**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)对象的集合。 
-   [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)对象具有[**地址**](https://msdn.microsoft.com/library/windows/apps/dn636929)属性，它将公开表示街道地址[**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533)对象，并公开表示地理位置的[**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint)对象[**点**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point)属性。

> [!IMPORTANT]
> 必须先指定地图验证密钥，才能使用地图服务。 有关详细信息，请参阅[请求地图验证密钥](authentication-key.md)。

## <a name="get-a-location-geocode"></a>获取位置（地理编码）

本部分显示了如何将街道地址或地点名称转换为地理位置 （地理编码）。

1.  调用一个[**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)类与地点名称或街道地址[**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)方法的重载。
2.  [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)方法返回一个[**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551)对象。
3.  使用[**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) [**位置**](https://msdn.microsoft.com/library/windows/apps/dn627552)属性公开集合[**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)对象。 可能是多个[**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)对象，因为系统可能会发现对应于给定的输入的多个位置。

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

本部分显示了如何将地理位置转换为地址 （反向地理编码）。

1.  调用 [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 类的 [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) 方法。
2.  [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) 方法将返回一个 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 对象，该对象包含匹配的 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 对象的集合。
3.  使用[**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) [**位置**](https://msdn.microsoft.com/library/windows/apps/dn627552)属性公开集合[**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)对象。 可能是多个[**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)对象，因为系统可能会发现对应于给定的输入的多个位置。
4.  通过每个[**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)[**地址**](https://msdn.microsoft.com/library/windows/apps/dn636929)属性访问[**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533)对象。

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

* [UWP 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [UWP 路况应用示例](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [地图设计指南](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [视频： 跨手机、 平板电脑和 Windows 应用中的 PC 利用地图和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [必应地图开发人员中心](https://www.bingmapsportal.com/)
* [**MapLocationFinder** 类](https://msdn.microsoft.com/library/windows/apps/dn627550)
* [**FindLocationsAsync**方法](https://msdn.microsoft.com/library/windows/apps/dn636925)
* [**FindLocationsAtAsync**方法](https://msdn.microsoft.com/library/windows/apps/dn636928)

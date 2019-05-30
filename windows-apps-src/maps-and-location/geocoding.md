---
title: 执行地理编码和反向地理编码
description: 本指南演示如何将街道地址转换为地理位置 （地理编码） 和通过 Windows.Services.Maps 命名空间中调用 MapLocationFinder 类的方法转换为街道地址 （反向地理编码） 的地理位置。
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp, 地理编码, 地图, 位置
ms.localizationpriority: medium
ms.openlocfilehash: 88687f6e7074ff7c914927a81a08720336b3c60c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371875"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>执行地理编码和反向地理编码

本指南演示如何将街道地址转换为地理位置 （地理编码），并将地理位置转换为街道地址 （反向地理编码），通过调用的方法[ **MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)类中[ **Windows.Services.Maps** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps)命名空间。

> [!TIP]
> 若要了解有关应用程序中使用映射的详细信息，请下载[MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)从示例[Windows 通用示例存储库](h https://github.com/Microsoft/Windows-universal-samples)GitHub 上。

所涉及的地理编码和反向地理编码类进行组织，如下所示。

-   [ **MapLocationFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)类包含处理地理编码的方法 ([**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)) 和反向地理编码 ([**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync))。
-   这些方法都返回[ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)实例。
-   [**位置**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations)属性[ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)公开一系列[ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)对象。 
-   [**MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)对象同时具有[**地址**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address)属性，它公开[ **MapAddress** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress)表示的街道地址、 对象和一个[**点**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point)属性，它公开[ **Geopoint** ](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint)对象表示地理位置。

> [!IMPORTANT]
> 然后可以使用地图服务，必须指定一个地图身份验证密钥。 有关详细信息，请参阅[请求地图验证密钥](authentication-key.md)。

## <a name="get-a-location-geocode"></a>获取位置（地理编码）

本部分介绍如何将街道地址或位置名称转换为地理位置 （地理编码）。

1.  调用的重载之一[ **FindLocationsAsync** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)方法[ **MapLocationFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)位置名称或街道类地址。
2.  [ **FindLocationsAsync** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)方法将返回[ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)对象。
3.  使用[**位置**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations)属性[ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)公开集合[ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)对象。 可能有多个[ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)对象，因为系统可能会发现多个对应于给定的输入的位置。

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

本部分介绍如何将地理位置转换为一个地址 （反向地理编码）。

1.  调用 [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) 类的 [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) 方法。
2.  [  **FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) 方法将返回一个 [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 对象，该对象包含匹配的 [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) 对象的集合。
3.  使用[**位置**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations)属性[ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)公开集合[ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)对象。 可能有多个[ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)对象，因为系统可能会发现多个对应于给定的输入的位置。
4.  访问[ **MapAddress** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress)对象通过[**地址**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address)每个属性[ **MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation).

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

* [UWP 地图示例](https://go.microsoft.com/fwlink/p/?LinkId=619977)
* [UWP 路况应用示例](https://go.microsoft.com/fwlink/p/?LinkId=619982)
* [地图设计指南](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [视频：在 Windows 应用中跨手机、平板电脑和 PC 利用地图和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [必应地图开发人员中心](https://www.bingmapsportal.com/)
* [**MapLocationFinder** class](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [**FindLocationsAsync**方法](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [**FindLocationsAtAsync**方法](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)

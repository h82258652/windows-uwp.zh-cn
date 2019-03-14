---
title: 执行地理编码和反向地理编码
description: 本指南演示如何将街道地址转换为地理位置 （地理编码） 和通过 Windows.Services.Maps 命名空间中调用 MapLocationFinder 类的方法转换为街道地址 （反向地理编码） 的地理位置。
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp, 地理编码, 地图, 位置
ms.localizationpriority: medium
ms.openlocfilehash: a30ca89242b15866019fffc6972bdae7086f3f7e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637622"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>执行地理编码和反向地理编码

本指南演示如何将街道地址转换为地理位置 （地理编码），并将地理位置转换为街道地址 （反向地理编码），通过调用的方法[ **MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)类中[ **Windows.Services.Maps** ](https://msdn.microsoft.com/library/windows/apps/dn636979)命名空间。

> [!TIP]
> 若要了解有关应用程序中使用映射的详细信息，请下载[MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)从示例[Windows 通用示例存储库](h https://github.com/Microsoft/Windows-universal-samples)GitHub 上。

所涉及的地理编码和反向地理编码类进行组织，如下所示。

-   [ **MapLocationFinder** ](https://msdn.microsoft.com/library/windows/apps/dn627550)类包含处理地理编码的方法 ([**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)) 和反向地理编码 ([**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928))。
-   这些方法都返回[ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551)实例。
-   [**位置**](https://msdn.microsoft.com/library/windows/apps/dn627552)属性[ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551)公开一系列[ **MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549)对象。 
-   [**MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549)对象同时具有[**地址**](https://msdn.microsoft.com/library/windows/apps/dn636929)属性，它公开[ **MapAddress** ](https://msdn.microsoft.com/library/windows/apps/dn627533)表示的街道地址、 对象和一个[**点**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point)属性，它公开[ **Geopoint** ](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint)对象表示地理位置。

> [!IMPORTANT]
> 然后可以使用地图服务，必须指定一个地图身份验证密钥。 有关详细信息，请参阅[请求地图验证密钥](authentication-key.md)。

## <a name="get-a-location-geocode"></a>获取位置（地理编码）

本部分介绍如何将街道地址或位置名称转换为地理位置 （地理编码）。

1.  调用的重载之一[ **FindLocationsAsync** ](https://msdn.microsoft.com/library/windows/apps/dn636925)方法[ **MapLocationFinder** ](https://msdn.microsoft.com/library/windows/apps/dn627550)位置名称或街道类地址。
2.  [ **FindLocationsAsync** ](https://msdn.microsoft.com/library/windows/apps/dn636925)方法将返回[ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551)对象。
3.  使用[**位置**](https://msdn.microsoft.com/library/windows/apps/dn627552)属性[ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551)公开集合[ **MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549)对象。 可能有多个[ **MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549)对象，因为系统可能会发现多个对应于给定的输入的位置。

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

1.  调用 [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 类的 [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) 方法。
2.  [  **FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) 方法将返回一个 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 对象，该对象包含匹配的 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 对象的集合。
3.  使用[**位置**](https://msdn.microsoft.com/library/windows/apps/dn627552)属性[ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551)公开集合[ **MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549)对象。 可能有多个[ **MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549)对象，因为系统可能会发现多个对应于给定的输入的位置。
4.  访问[ **MapAddress** ](https://msdn.microsoft.com/library/windows/apps/dn627533)对象通过[**地址**](https://msdn.microsoft.com/library/windows/apps/dn636929)每个属性[ **MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549).

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

* [UWP 映射示例](https://go.microsoft.com/fwlink/p/?LinkId=619977)
* [UWP 流量应用示例](https://go.microsoft.com/fwlink/p/?LinkId=619982)
* [映射的设计准则](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [视频：利用跨手机、 平板电脑和 Windows 应用程序中的 PC 的地图和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [必应地图开发人员中心](https://www.bingmapsportal.com/)
* [**MapLocationFinder** class](https://msdn.microsoft.com/library/windows/apps/dn627550)
* [**FindLocationsAsync**方法](https://msdn.microsoft.com/library/windows/apps/dn636925)
* [**FindLocationsAtAsync**方法](https://msdn.microsoft.com/library/windows/apps/dn636928)

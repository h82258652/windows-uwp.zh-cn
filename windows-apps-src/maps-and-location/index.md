---
title: 地图和位置概述
description: 本部分介绍如何在应用中显示地图、使用地图服务、查找位置和设置地理围栏。 本节还向你显示如何将 Windows 地图应用启动到特定地图、路线或一组逐向路线。
ms.assetid: F4C1F094-CF46-4B15-9D80-C1A26A314521
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 地图, 位置, 地图服务
ms.localizationpriority: medium
ms.openlocfilehash: aea553a46357a26028848db5ff0e9b5debbeae56
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8694178"
---
# <a name="maps-and-location-overview"></a>地图和位置概述




本部分介绍如何在应用中显示地图、使用地图服务、查找位置和设置地理围栏。 本部分还向你显示如何将 Windows 地图应用启动到特定地图、路线或一组逐向路线。

> [!TIP]
> 若要了解有关你的应用中使用地图和位置的详细信息，请从 GitHub 上的[Windows 通用示例存储库](http://go.microsoft.com/fwlink/p/?LinkId=619979)中下载以下示例：
-   [通用 Windows 平台 (UWP) 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
-   [UWP 地理位置示例](http://go.microsoft.com/fwlink/p/?linkid=533278)

 

## <a name="display-maps"></a>显示地图


使用 [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751) 命名空间中的 API 在应用中以 2D、3D 或街景视图方式显示地图。 你可以使用图钉、图像、图形或 XAML UI 元素在地图上标记兴趣点 (POI)。 你还可以覆盖平铺图像或完全替换地图图像。

| 主题 | 说明 |
|-------|-------------|
| [请求地图身份验证密钥](authentication-key.md) | 应用必须先进行身份验证，然后才能在 [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空间中使用 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 和地图服务。 若要对你的应用进行身份验证，你必须指定地图身份验证密钥。 本文介绍如何从[必应地图开发人员中心](https://www.bingmapsportal.com/)请求地图身份验证密钥并将其添加到应用。 |
| [使用 2D、3D 和 Streetside 方式显示地图](display-maps.md) | 通过使用 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 类，在应用中显示可自定义的地图。 本主题还介绍了鸟瞰图 3D 视图和街景视图。 |
| [在地图上显示兴趣点 (POI)](display-poi.md) | 使用图钉、图像、图形和 XAML UI 元素向地图添加兴趣点 (POI)。 |
| [覆盖地图上的平铺图像](overlay-tiled-images.md) | 使用磁贴源覆盖地图上的第三方或自定义平铺图像。 使用磁贴源可覆盖专业信息（例如，天气数据、人口数据或地震数据）；或者使用磁贴源替换所有默认地图。 |



## <a name="access-map-services"></a>访问地图服务

通过使用 [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空间中的 API 将路线、方向和地理编码功能添加到你的应用。

| 主题 | 说明 |
|-----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [请求地图身份验证密钥](authentication-key.md) | 应用必须先进行身份验证，然后才能在 [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空间中使用 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 和地图服务。 若要对你的应用进行身份验证，你必须指定地图身份验证密钥。 本文介绍如何从[必应地图开发人员中心](https://www.bingmapsportal.com/)请求地图身份验证密钥并将其添加到应用。 |
| [在地图上显示目标点 (POI)](display-poi.md) | 使用图钉、图像、图形和 XAML UI 元素向地图添加目标点 (POI)。 |
| [显示路线和方向](routes-and-directions.md) | 请求路线和方向并在应用中显示它们。 |
| [执行地理编码和反向地理编码](geocoding.md) | 通过调用 [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空间中 [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 类的方法将地址转换为地理位置（地理编码）以及将地理位置转换为地址（反向地理编码）。 |
| [查找并下载供离线使用的地图包](https://docs.microsoft.com/uwp/api/windows.services.maps.offlinemaps)| 在过去，你的应用必须将用户定向到设置应用下载离线地图。 现在，可以使用类[Windows.Services.Maps.OfflineMaps](https://docs.microsoft.com/en-us/uwp/api/windows.services.maps.offlinemaps)命名空间中以查找中 （基于[Geopoint](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint)、 [GeoboundingBox](https://docs.microsoft.com/en-us/uwp/api/windows.devices.geolocation.geoboundingbox)等。） 在给定区域的已下载的程序包。 <br> 你可以也检查和侦听的地图包的已下载状态以及启动下载，而无需用户离开你的应用。 <br> 你可以找到有关如何执行此操作的参考内容和[通用 Windows 平台 (UWP) 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)中的示例。

## <a name="get-the-users-location"></a>获取用户的位置

获取用户的当前位置，并使用 [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603) 命名空间中的 API 在发生位置更改时在应用中收到通知。 这些 API 成员还经常在地图 API 的参数中使用。 [**Windows.Devices.Geolocation.Geofencing**](https://msdn.microsoft.com/library/windows/apps/dn263744) 命名空间中的 API 会在用户进入或退出地理围栏（预定义的地理区域）时通知你的应用。

| 主题 | 说明 |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [请求地图身份验证密钥](authentication-key.md) | 应用必须先进行身份验证，然后才能在 [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空间中使用 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 和地图服务。 若要对你的应用进行身份验证，你必须指定地图身份验证密钥。 本文介绍如何从[必应地图开发人员中心](https://www.bingmapsportal.com/)请求地图身份验证密钥并将其添加到应用。 |
| [位置感知应用设计指南](guidelines-and-checklist-for-detecting-location.md) | 需要访问用户位置的应用的性能指南。 |
| [获取用户的位置](get-location.md) | 获取对用户位置的访问权限，然后检索该位置。 | 
| [有关使用访问跟踪的指南](guidelines-for-visits.md) | 了解如何使用功能强大的访问跟踪功能进行更实用的位置跟踪。 |
| [地理围栏设计指南](guidelines-for-geofencing.md) | 地理围栏功能的应用的性能指南。 |
| [设置地理围栏](set-up-a-geofence.md) | 在你的应用中设置地理围栏并了解如何处理前台和后台中的通知。 |

## <a name="launch-the-windows-maps-app"></a>启动 Windows 地图应用

你的应用可以启动 Windows 地图应用（如此处所示）来显示特定地图和逐向路线。 考虑使用 Windows 地图应用提供地图功能，而不是在你自己的应用中直接提供该功能。 有关详细信息，请参阅[启动 Windows 地图应用](https://msdn.microsoft.com/library/windows/apps/mt228341)。

![Windows 地图应用的示例。](images/mapnyc.png)

## <a name="related-topics"></a>相关主题

* [UWP 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [UWP 地理位置示例](http://go.microsoft.com/fwlink/p/?linkid=533278)
* [必应地图开发人员中心](https://www.bingmapsportal.com/)
* [获取当前位置](get-location.md)
* [位置感知应用设计指南](guidelines-and-checklist-for-detecting-location.md)
* [地图设计指南](controls-map.md)
* [隐私感知应用设计指南](https://msdn.microsoft.com/library/windows/apps/hh768223)
* [Build 2015 视频：在 Windows 应用中跨手机、平板电脑和 PC 利用地图和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 路况应用示例](http://go.microsoft.com/fwlink/p/?LinkId=619982)

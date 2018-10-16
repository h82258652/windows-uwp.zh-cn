---
author: normesta
title: 覆盖地图上的平铺图像
description: 使用磁贴源覆盖地图上的第三方或自定义平铺图像。 使用磁贴源可覆盖专业信息（例如，天气数据、人口数据或地震数据），或者使用磁贴源替换所有默认地图。
ms.assetid: 066BD6E2-C22B-4F5B-AA94-5D6C86A09BDF
ms.author: normesta
ms.date: 07/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 地图, 位置, 图像, 覆盖
ms.localizationpriority: medium
ms.openlocfilehash: ba1f7d52a1b16fbb421202229ce724dab384ffa0
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2018
ms.locfileid: "4617849"
---
# <a name="overlay-tiled-images-on-a-map"></a>覆盖地图上的平铺图像

使用磁贴源覆盖地图上的第三方或自定义平铺图像。 使用磁贴源可覆盖专业信息（例如，天气数据、人口数据或地震数据）；或者使用磁贴源替换所有默认地图。

**提示** 若要了解有关在应用中使用地图的详细信息，请在 Github 上下载[通用 Windows 平台 (UWP) 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)。

<a id="tileintro" />

## <a name="tiled-image-overview"></a>平铺图像概述

地图服务（例如 Nokia 地图和必应地图）将地图剪切成多个方形磁贴，以供快速检索和显示。 这些磁贴的大小为 256 像素 X 256 像素，并以多个级别的详细信息的形式进行预呈现。 许多第三方服务还提供剪切成磁贴的基于地图的数据。 使用磁贴源可检索第三方磁贴，还可以创建你自己的自定义磁贴，并且可覆盖显示在 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 中的地图上的这些磁贴。

**重要提示**  
在使用磁贴源时，无需编写代码即可请求或放置个别磁贴。 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 会按需请求磁贴。 每个请求均会为单个磁贴指定 X 和 Y 坐标以及缩放级别。 仅需指定要使用的 URI 或文件名的格式，即可检索采用 **UriFormatString** 属性的磁贴。 换言之，在基本 URI 或文件名中插入可替换的参数，以指示每个磁贴的 X 和 Y 坐标及缩放级别的传递位置。

下面是 [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986) 的 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) 属性的示例，并显示了 X 和 Y 坐标及缩放级别的可替换参数。

```syntax
http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
```

（X 和 Y 坐标以指定级别的详细信息形式表示单个磁贴在世界地图中的位置。 磁贴编号系统从位于地图左上角的 {0, 0} 开始。 例如，坐标为 {1, 2} 的磁贴位于磁贴网格中的第 2 列，第 3 行。）

有关地图服务使用的磁贴系统的详细信息，请参阅[必应地图磁贴系统](http://go.microsoft.com/fwlink/p/?LinkId=626692)。

### <a name="overlay-tiles-from-a-tile-source"></a>覆盖来自磁贴源的磁贴

使用 [**MapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn637141) 覆盖地图上来自磁贴源的平铺图像。

1.  实例化继承自 [**MapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn637141) 的三个磁贴数据源类之一。

    -   [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986)
    -   [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994)
    -   [**CustomMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636983)

    通过在基本 Uri 或文件名中插入可替换参数，将 **UriFormatString** 配置为用于请求磁贴。

    以下示例将实例化 [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986)。 该示例在 **HttpMapTileDataSource** 的构造函数中指定 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) 的值。

    ```csharp
        HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
          "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");
    ```

2.  实例化并配置 [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144)。 指定 [**MapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn637141)，它在之前的步骤中已配置为 **MapTileSource** 的 [**DataSource**](https://msdn.microsoft.com/library/windows/apps/dn637149)。

    以下示例在 [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144) 的构造函数中指定 [**DataSource**](https://msdn.microsoft.com/library/windows/apps/dn637149)。

    ```csharp
        MapTileSource tileSource = new MapTileSource(dataSource);
    ```

    使用 [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144) 的属性可限制磁贴的显示条件。

    -   若为 [**Bounds**](https://msdn.microsoft.com/library/windows/apps/dn637147) 属性提供值，将仅在特定的地理区域内显示磁贴。
    -   若为 [**ZoomLevelRange**](https://msdn.microsoft.com/library/windows/apps/dn637171) 属性提供值，将仅以特定级别的详细信息的形式显示磁贴。

    也可以配置影响磁贴加载或显示的 [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144) 的其他属性，例如 [**Layer**](https://msdn.microsoft.com/library/windows/apps/dn637157)、[**AllowOverstretch**](https://msdn.microsoft.com/library/windows/apps/dn637145)、[**IsRetryEnabled**](https://msdn.microsoft.com/library/windows/apps/dn637153) 和 [**IsTransparencyEnabled**](https://msdn.microsoft.com/library/windows/apps/dn637155)。

3.  将 [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144) 添加到 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 的 [**TileSources**](https://msdn.microsoft.com/library/windows/apps/dn637053) 集合。

    ```csharp
         MapControl1.TileSources.Add(tileSource);
    ```

## <a name="overlay-tiles-from-a-web-service"></a>覆盖来自 Web 服务的磁贴


使用 [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986) 覆盖从 Web 服务检索的平铺图像。

1.  实例化 [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986)。
2.  根据 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) 属性的值，指定 Web 服务预期的 URI 的格式。 若要创建此值，请在基本 Uri 中插入可替换参数。 例如，在以下代码示例中，**UriFormatString** 的值是：

    ``` syntax
    http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
    ```

    Web 服务必须支持包含可替换参数 {x}、{y} 和 {zoomlevel} 的 Uri。 大多数 Web 服务（例如，Nokia、必应和 Google）支持这种格式的 URI。 如果 Web 服务需要不能应用于 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) 属性的其他参数，则必须创建自定义 Uri。 通过处理 [**UriRequested**](https://msdn.microsoft.com/library/windows/apps/dn636993) 事件创建并返回自定义 Uri。 有关详细信息，请参阅本主题后面的[提供自定义 URI](#customuri) 部分。

3.  然后，按照之前在[平铺图像概述](#tileintro)中介绍的剩余步骤操作。

以下示例将覆盖北美地区地图上来自虚拟 Web 服务的磁贴。 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) 的值在 [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986) 的构造函数中指定。 在此示例中，磁贴仅显示在由可选的 [**Bounds**](https://msdn.microsoft.com/library/windows/apps/dn637147) 属性指定的地理边界内。

```csharp
private void AddHttpMapTileSource()
{
    // Create the bounding box in which the tiles are displayed.
    // This example represents North America.
    BasicGeoposition northWestCorner =
        new BasicGeoposition() { Latitude = 48.38544, Longitude = -124.667360 };
    BasicGeoposition southEastCorner =
        new BasicGeoposition() { Latitude = 25.26954, Longitude = -80.30182 };
    GeoboundingBox boundingBox = new GeoboundingBox(northWestCorner, southEastCorner);

    // Create an HTTP data source.
    // This example retrieves tiles from a fictitious web service.
    HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
        "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");

    // Optionally, add custom HTTP headers if the web service requires them.
    dataSource.AdditionalRequestHeaders.Add("header name", "header value");

    // Create a tile source and add it to the Map control.
    MapTileSource tileSource = new MapTileSource(dataSource);
    tileSource.Bounds = boundingBox;
    MapControl1.TileSources.Add(tileSource);
}
```

```cppwinrt
...
#include <winrt/Windows.Devices.Geolocation.h>
#include <winrt/Windows.UI.Xaml.Controls.Maps.h>
...
void MainPage::AddHttpMapTileSource()
{
    Windows::Devices::Geolocation::BasicGeoposition northWest{ 48.38544, -124.667360 };
    Windows::Devices::Geolocation::BasicGeoposition southEast{ 25.26954, -80.30182 };
    Windows::Devices::Geolocation::GeoboundingBox boundingBox{ northWest, southEast };

    Windows::UI::Xaml::Controls::Maps::HttpMapTileDataSource dataSource{
        L"http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}" };

    dataSource.AdditionalRequestHeaders().Insert(L"header name", L"header value");

    Windows::UI::Xaml::Controls::Maps::MapTileSource tileSource{ dataSource };
    tileSource.Bounds(boundingBox);

    MapControl1().TileSources().Append(tileSource);
}
...
```

```cpp
void MainPage::AddHttpMapTileSource()
{
    BasicGeoposition northWest = { 48.38544, -124.667360 };
    BasicGeoposition southEast = { 25.26954, -80.30182 };
    GeoboundingBox^ boundingBox = ref new GeoboundingBox(northWest, southEast);

    auto dataSource = ref new Windows::UI::Xaml::Controls::Maps::HttpMapTileDataSource(
        "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");

    dataSource->AdditionalRequestHeaders->Insert("header name", "header value");

    auto tileSource = ref new Windows::UI::Xaml::Controls::Maps::MapTileSource(dataSource);
    tileSource->Bounds = boundingBox;

    this->MapControl1->TileSources->Append(tileSource);
}
```

## <a name="overlay-tiles-from-local-storage"></a>覆盖来自本地存储的磁贴


使用 [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994) 覆盖以文件形式存储在本地存储中的平铺图像。 通常，你会与自己的应用一起打包并分配这些文件。

1.  实例化 [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994)。
2.  根据 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998) 属性的值，指定文件名的格式。 若要创建此值，请在基本文件名中插入可替换参数。 例如，在以下代码示例中，[**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) 的值是：

    ``` syntax
        Tile_{zoomlevel}_{x}_{y}.png
    ```

    如果文件名的格式需要不能应用于 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998) 属性的其他参数，则必须创建自定义 Uri。 通过处理 [**UriRequested**](https://msdn.microsoft.com/library/windows/apps/dn637001) 事件创建并返回自定义 Uri。 有关详细信息，请参阅本主题后面的[提供自定义 URI](#customuri) 部分。

3.  然后，按照之前在[平铺图像概述](#tileintro)中介绍的剩余步骤操作。

可以使用以下协议和位置从本地存储加载磁贴：

| URI | 详细信息 |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| ms-appx:/// | 指向应用的安装文件夹所在的根目录。 |
|  | 这是 [Package.InstalledLocation](https://msdn.microsoft.com/library/windows/apps/br224681) 属性所引用的位置。 |
| ms-appdata:///local | 指向应用的本地存储的根文件夹。 |
|  | 这是 [ApplicationData.LocalFolder](https://msdn.microsoft.com/library/windows/apps/br241621) 属性所引用的位置。 |
| ms-appdata:///temp | 指向应用的临时文件夹。 |
|  | 这是 [ApplicationData.TemporaryFolder](https://msdn.microsoft.com/library/windows/apps/br241629) 属性所引用的位置。 |

 

以下示例通过使用 `ms-appx:///` 协议来加载以文件形式存储在应用的安装文件夹中的磁贴。 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998) 的值在 [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994) 的构造函数中指定。 在此示例中，磁贴仅当地图的缩放级别在可选的 [**ZoomLevelRange**](https://msdn.microsoft.com/library/windows/apps/dn637171) 属性所指定的范围时才显示。

```csharp
        void AddLocalMapTileSource()
        {
            // Specify the range of zoom levels
            // at which the overlaid tiles are displayed.
            MapZoomLevelRange range;
            range.Min = 11;
            range.Max = 20;

            // Create a local data source.
            LocalMapTileDataSource dataSource = new LocalMapTileDataSource(
                "ms-appx:///TileSourceAssets/Tile_{zoomlevel}_{x}_{y}.png");

            // Create a tile source and add it to the Map control.
            MapTileSource tileSource = new MapTileSource(dataSource);
            tileSource.ZoomLevelRange = range;
            MapControl1.TileSources.Add(tileSource);
        }
```

<a id="customuri" />

## <a name="provide-a-custom-uri"></a>提供自定义 URI

如果没有足够的由 [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986) 的 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) 属性或 [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994) 的 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998) 属性提供的可替换参数来检索磁贴，则必须创建自定义 Uri。 通过为 **UriRequested** 事件提供自定义处理程序创建和返回自定义 Uri。 每个单独的磁贴都会引发 **UriRequested** 事件。

1.  在 **UriRequested** 事件的自定义处理程序中，将所需的自定义参数与 [**MapTileUriRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637177) 的 [**X**](https://msdn.microsoft.com/library/windows/apps/dn610743)、[**Y**](https://msdn.microsoft.com/library/windows/apps/dn610744) 及 [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn610745) 属性进行合并，以创建自定义 Uri。
2.  在 [**MapTileUriRequest**](https://msdn.microsoft.com/library/windows/apps/dn637173)（它包含在 [**MapTileUriRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637177) 的 [**Request**](https://msdn.microsoft.com/library/windows/apps/dn637179) 属性中）的 [**Uri**](https://msdn.microsoft.com/library/windows/apps/dn610748) 属性中返回自定义 Uri。

以下示例显示了如何通过为 **UriRequested** 事件创建自定义处理程序来提供自定义 Uri。 该示例还显示了如何在必须异步执行某些操作的情况下，实现延迟模式来创建自定义 Uri。

```csharp
using Windows.UI.Xaml.Controls.Maps;
using System.Threading.Tasks;
...
            var httpTileDataSource = new HttpMapTileDataSource();
            // Attach a handler for the UriRequested event.
            httpTileDataSource.UriRequested += HandleUriRequestAsync;
            MapTileSource httpTileSource = new MapTileSource(httpTileDataSource);
            MapControl1.TileSources.Add(httpTileSource);
...
        // Handle the UriRequested event.
        private async void HandleUriRequestAsync(HttpMapTileDataSource sender,
            MapTileUriRequestedEventArgs args)
        {
            // Get a deferral to do something asynchronously.
            // Omit this line if you don't have to do something asynchronously.
            var deferral = args.Request.GetDeferral();

            // Get the custom Uri.
            var uri = await GetCustomUriAsync(args.X, args.Y, args.ZoomLevel);

            // Specify the Uri in the Uri property of the MapTileUriRequest.
            args.Request.Uri = uri;

            // Notify the app that the custom Uri is ready.
            // Omit this line also if you don't have to do something asynchronously.
            deferral.Complete();
        }

        // Create the custom Uri.
        private async Task<Uri> GetCustomUriAsync(int x, int y, int zoomLevel)
        {
            // Do something asynchronously to create and return the custom Uri.        }
        }
```

## <a name="overlay-tiles-from-a-custom-source"></a>覆盖来自自定义源的磁贴

使用 [**CustomMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636983) 覆盖自定义磁贴。 在内存中以编程方式快速创建磁贴，或者编写自己的代码以从其他源加载现有磁贴。

若要创建或加载自定义磁贴，请为 [**BitmapRequested**](https://msdn.microsoft.com/library/windows/apps/dn636984) 事件提供自定义处理程序。 每个单独的磁贴都会引发 **BitmapRequested** 事件。

1.  在 [**BitmapRequested**](https://msdn.microsoft.com/library/windows/apps/dn636984) 事件的自定义处理程序中，将所需的自定义参数与 [**MapTileBitmapRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637132) 的 [**X**](https://msdn.microsoft.com/library/windows/apps/dn637135)、[**Y**](https://msdn.microsoft.com/library/windows/apps/dn637136) 及 [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637137) 属性进行合并，以创建或检索自定义磁贴。
2.  在 [**MapTileBitmapRequest**](https://msdn.microsoft.com/library/windows/apps/dn637128)（它包含在 [**MapTileBitmapRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637132) 的 [**Request**](https://msdn.microsoft.com/library/windows/apps/dn637134) 属性中）的 [**PixelData**](https://msdn.microsoft.com/library/windows/apps/dn637140) 属性中返回自定义磁贴。 **PixelData** 属性属于类型 [**IRandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701664)。

以下示例显示了如何通过为 **BitmapRequested** 事件创建自定义处理程序来提供自定义磁贴。 此示例创建相同的红色磁贴，这些磁贴局部是透明的。 该示例忽略了 [**MapTileBitmapRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637132) 的 [**X**](https://msdn.microsoft.com/library/windows/apps/dn637135)、[**Y**](https://msdn.microsoft.com/library/windows/apps/dn637136) 和 [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637137) 属性。 尽管这不是真实的示例，但该示例演示如何在内存中快速创建自定义磁贴。 该示例还显示了如何在必须异步执行某些操作的情况下，实现延迟模式来创建自定义磁贴。

```csharp
using Windows.UI.Xaml.Controls.Maps;
using Windows.Storage.Streams;
using System.Threading.Tasks;
...
        CustomMapTileDataSource customDataSource = new CustomMapTileDataSource();
        // Attach a handler for the BitmapRequested event.
        customDataSource.BitmapRequested += customDataSource_BitmapRequestedAsync;
        customTileSource = new MapTileSource(customDataSource);
        MapControl1.TileSources.Add(customTileSource);
...
        // Handle the BitmapRequested event.
        private async void customDataSource_BitmapRequestedAsync(
            CustomMapTileDataSource sender,
            MapTileBitmapRequestedEventArgs args)
        {
            var deferral = args.Request.GetDeferral();
            args.Request.PixelData = await CreateBitmapAsStreamAsync();
            deferral.Complete();
        }

        // Create the custom tiles.
        // This example creates red tiles that are partially opaque.
        private async Task<RandomAccessStreamReference> CreateBitmapAsStreamAsync()
        {
            int pixelHeight = 256;
            int pixelWidth = 256;
            int bpp = 4;

            byte[] bytes = new byte[pixelHeight * pixelWidth * bpp];

            for (int y = 0; y < pixelHeight; y++)
            {
                for (int x = 0; x < pixelWidth; x++)
                {
                    int pixelIndex = y * pixelWidth + x;
                    int byteIndex = pixelIndex * bpp;

                    // Set the current pixel bytes.
                    bytes[byteIndex] = 0xff;        // Red
                    bytes[byteIndex + 1] = 0x00;    // Green
                    bytes[byteIndex + 2] = 0x00;    // Blue
                    bytes[byteIndex + 3] = 0x80;    // Alpha (0xff = fully opaque)
                }
            }

            // Create RandomAccessStream from byte array.
            InMemoryRandomAccessStream randomAccessStream =
                new InMemoryRandomAccessStream();
            IOutputStream outputStream = randomAccessStream.GetOutputStreamAt(0);
            DataWriter writer = new DataWriter(outputStream);
            writer.WriteBytes(bytes);
            await writer.StoreAsync();
            await writer.FlushAsync();
            return RandomAccessStreamReference.CreateFromStream(randomAccessStream);
        }
```

```cppwinrt
...
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Foundation::IAsyncOperation<Windows::Storage::Streams::InMemoryRandomAccessStream> MainPage::CustomRandomAccessStream()
{
    constexpr int pixelHeight{ 256 };
    constexpr int pixelWidth{ 256 };
    constexpr int bpp{ 4 };

    std::array<uint8_t, pixelHeight * pixelWidth * bpp> bytes;

    for (int y = 0; y < pixelHeight; y++)
    {
        for (int x = 0; x < pixelWidth; x++)
        {
            int pixelIndex{ y * pixelWidth + x };
            int byteIndex{ pixelIndex * bpp };

            // Set the current pixel bytes.
            bytes[byteIndex] = (byte)(std::rand() % 256);        // Red
            bytes[byteIndex + 1] = (byte)(std::rand() % 256);    // Green
            bytes[byteIndex + 2] = (byte)(std::rand() % 256);    // Blue
            bytes[byteIndex + 3] = (byte)((std::rand() % 56) + 200);    // Alpha (0xff = fully opaque)
        }
    }

    // Create RandomAccessStream from byte array.
    Windows::Storage::Streams::InMemoryRandomAccessStream randomAccessStream;
    Windows::Storage::Streams::IOutputStream outputStream{ randomAccessStream.GetOutputStreamAt(0) };
    Windows::Storage::Streams::DataWriter writer{ outputStream };
    writer.WriteBytes(bytes);

    co_await writer.StoreAsync();
    co_await writer.FlushAsync();

    co_return randomAccessStream;
}
...
```

```cpp
InMemoryRandomAccessStream^ TileSources::CustomRandomAccessStream::get()
{
    int pixelHeight = 256;
    int pixelWidth = 256;
    int bpp = 4;

    Array<byte>^ bytes = ref new Array<byte>(pixelHeight * pixelWidth * bpp);

    for (int y = 0; y < pixelHeight; y++)
    {
        for (int x = 0; x < pixelWidth; x++)
        {
            int pixelIndex = y * pixelWidth + x;
            int byteIndex = pixelIndex * bpp;

            // Set the current pixel bytes.
            bytes[byteIndex] = (byte)(std::rand() % 256);        // Red
            bytes[byteIndex + 1] = (byte)(std::rand() % 256);    // Green
            bytes[byteIndex + 2] = (byte)(std::rand() % 256);    // Blue
            bytes[byteIndex + 3] = (byte)((std::rand() % 56) + 200);    // Alpha (0xff = fully opaque)
        }
    }

    // Create RandomAccessStream from byte array.
    InMemoryRandomAccessStream^ randomAccessStream = ref new InMemoryRandomAccessStream();
    IOutputStream^ outputStream = randomAccessStream->GetOutputStreamAt(0);
    DataWriter^ writer = ref new DataWriter(outputStream);
    writer->WriteBytes(bytes);

    create_task(writer->StoreAsync()).then([writer](unsigned int)
    {
        create_task(writer->FlushAsync());
    });

    return randomAccessStream;
}
```

## <a name="replace-the-default-map"></a>替换默认地图

使用第三方或自定义磁贴来替换所有默认地图：

-   将 [**MapTileLayer**](https://msdn.microsoft.com/library/windows/apps/dn637143).**BackgroundReplacement** 指定为 [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144) 的 [**Layer**](https://msdn.microsoft.com/library/windows/apps/dn637157) 属性的值。
-   将 [**MapStyle**](https://msdn.microsoft.com/library/windows/apps/dn637127).**None** 指定为 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 的 [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) 属性的值。

## <a name="related-topics"></a>相关主题

* [必应地图开发人员中心](https://www.bingmapsportal.com/)
* [UWP 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [地图设计指南](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [版本 2015 视频：在 Windows 应用中跨手机、平板电脑和 PC 利用地图和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 路况应用示例](http://go.microsoft.com/fwlink/p/?LinkId=619982)

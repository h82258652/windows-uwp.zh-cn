---
title: 在地图上显示兴趣点 (POI)
description: 使用图钉、图像、图形和 XAML UI 元素向地图添加目标点 (POI)。
ms.assetid: CA00D8EB-6C1B-4536-8921-5EAEB9B04FCA
ms.date: 08/11/2017
ms.topic: article
keywords: windows 10, uwp, 地图, 位置, 图钉
ms.localizationpriority: medium
ms.openlocfilehash: f67c93a6f56fd466d981bce10eb41c16ff8da1f3
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8746843"
---
# <a name="display-points-of-interest-on-a-map"></a>在地图上显示目标点

使用图钉、图像、图形和 XAML UI 元素向地图添加目标点 (POI)。 POI 是地图上表示对某事物感兴趣的特殊的点。 例如，企业、城市或好友的位置。

若要了解有关在你的应用上显示 POI 的详细信息，请从 GitHub 上的 [Windows-universal-samples 存储库](http://go.microsoft.com/fwlink/p/?LinkId=619979)下载以下示例：[通用 Windows 平台 (UWP) 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)。

通过将  [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)、[**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)、[**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103) 和 [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114) 对象添加到 [**MapElementsLayer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer) 对象的 **MapElements** 集合，在地图上显示图钉、图像和图形。 然后，将该层对象添加到地图控件的 **Layers** 集合。

>[!NOTE]
> 在以前的版本中，此指南向您展示如何将地图元素添加到 [**MapElements**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements)集合。 虽然你仍然可以使用此方法，但将无法利用新地图层模型的一些优势。 若要更多信息，请参阅本指南的[使用层](#layers)部分。

你还可以通过将 XAML 用户界面元素（例如，[**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)、 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 或 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)）添加到 [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094) 或添加为 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 的 [**Children**](https://msdn.microsoft.com/library/windows/apps/dn637008)，在地图上显示它们。

如果你想要将大量元素放置在地图上，请考虑[在地图上覆盖平铺图像](overlay-tiled-images.md)。 若要在地图上显示道路，请参阅[显示路线和方向](routes-and-directions.md)

## <a name="add-a-pushpin"></a>添加图钉

通过使用 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 类在地图上显示图像（例如图钉）和可选文本。 可以接受默认图像或通过使用 [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) 属性提供自定义图像。 以下图像显示了未为 [**Title**](https://msdn.microsoft.com/library/windows/apps/dn637088) 属性指定任何值、具有短标题、具有长标题和具有非常长的标题的 **MapIcon** 的默认图像。

![具有不同长度的磁贴的 MapIcon 示例。](images/mapctrl-mapicons.png)

下面的示例显示了西雅图市的地图并添加了一个带默认图像和可选标题的 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)，可指示 Space Needle 的位置。 地图还以该图标为中心并放大。 有关使用地图控件的常规信息，请参阅[以 2D、3D 和街景视图方式显示地图](display-maps.md)。

```csharp
public void AddSpaceNeedleIcon()
{
    var MyLandmarks = new List<MapElement>();

    BasicGeoposition snPosition = new BasicGeoposition { Latitude = 47.620, Longitude = -122.349 };
    Geopoint snPoint = new Geopoint(snPosition);

    var spaceNeedleIcon = new MapIcon
    {
        Location = snPoint,
        NormalizedAnchorPoint = new Point(0.5, 1.0),
        ZIndex = 0,
        Title = "Space Needle"
    };

    MyLandmarks.Add(spaceNeedleIcon);

    var LandmarksLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyLandmarks
    };

    myMap.Layers.Add(LandmarksLayer);

    myMap.Center = snPoint;
    myMap.ZoomLevel = 14;

}
```

本示例在地图上显示以下 POI（以默认图像为中心）。

![带 MapIcon 的地图](images/displaypoidefault.png)

下面的代码行将显示 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 和保存在项目“资源”文件夹中的自定义图像。 **MapIcon** 的 [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) 属性需要类型 [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813) 的值。 此类型需要 [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791) 命名空间的 **using** 声明。

>[!NOTE]
>如果将同一图像用于多个地图图标，请声明页面级别或应用级别的 [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813) 以获得最佳性能。

```csharp
    MapIcon1.Image =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/customicon.png"));
```

使用 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 类时，请记住以下注意事项：

-   [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) 属性支持的最大图像大小为 2048×2048 像素。
-   默认情况下，不一定会显示地图图标的图像。 当它挡住地图上其他元素或标签时，它可能会隐藏。 若要保持可见，将地图图标的 [**CollisionBehaviorDesired**](https://msdn.microsoft.com/library/windows/apps/dn974327) 属性设置为 [**MapElementCollisionBehavior.RemainVisible**](https://msdn.microsoft.com/library/windows/apps/dn974314)。
-   [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637088) 的可选 [**Title**](https://msdn.microsoft.com/library/windows/apps/dn637077) 并不一定会显示。 如果你没有看到此文本，通过减少 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637068) 的 [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637004) 属性的值来缩小显示。
-   当你显示指向地图上特定位置的 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 图像时（例如，图钉或箭头），考虑将 [**NormalizedAnchorPoint**](https://msdn.microsoft.com/library/windows/apps/dn637082) 属性的值设置为图像上指针的大致位置。 如果你将 **NormalizedAnchorPoint** 的值保留为默认值 (0, 0)（表示图像的左上角），地图的 [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) 中的更改可能会导致图像指向其他位置。
-   如果你不显式设置 [Altitude](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.basicgeoposition) 和 [AltitudeReferenceSystem](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint.AltitudeReferenceSystem)，[**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 将放置在图面。

## <a name="add-a-3d-pushpin"></a>添加 3D 图钉

你可以在地图中添加三维对象。 使用 [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) 类从 [3D 制造格式 (3MF)](http://3mf.io/specification/) 文件中导入 3D 对象。

这张图使用 3D 咖啡杯标记邻近咖啡店的位置。

![地图上的杯子](images/mugs.png)

以下代码通过导入 3MF 文件在地图中添加咖啡杯。 为简单起见，这段代码将该图像添加到地图中心，但你的代码可以将图像添加到特定位置。

```csharp
public async void Add3DMapModel()
{
    var mugStreamReference = RandomAccessStreamReference.CreateFromUri
        (new Uri("ms-appx:///Assets/mug.3mf"));

    var myModel = await MapModel3D.CreateFrom3MFAsync(mugStreamReference,
        MapModel3DShadingOption.Smooth);

    myMap.Layers.Add(new MapElementsLayer
    {
       ZIndex = 1,
       MapElements = new List<MapElement>
       {
          new MapElement3D
          {
              Location = myMap.Center,
              Model = myModel,
          },
       },
    });
}
```

## <a name="add-an-image"></a>添加图像

显示与地图位置相关的大图像（如餐馆或地标图片）。 当用户缩小时，此图像将按比例收缩，使用户可以从地图上看到更大的地理范围。 这与 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 略有不同，后者标记特定的位置，大小通常很小，并且用户放大和缩小地图时，其大小也保持不变。

![MapBillboard 图像](images/map-billboard.png)

下面的代码显示上图中所示的 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)。

```csharp
public void AddLandmarkPhoto()
{
    // Create MapBillboard.

    RandomAccessStreamReference mapBillboardStreamReference =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/billboard.jpg"));

    var mapBillboard = new MapBillboard(myMap.ActualCamera)
    {
        Location = myMap.Center,
        NormalizedAnchorPoint = new Point(0.5, 1.0),
        Image = mapBillboardStreamReference
    };

    // Add MapBillboard to a layer on the map control.

    var MyLandmarkPhotos = new List<MapElement>();

    MyLandmarkPhotos.Add(mapBillboard);

    var LandmarksPhotoLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyLandmarkPhotos
    };

    myMap.Layers.Add(LandmarksPhotoLayer);
}
```

这段代码中有三个部分值得更仔细地进行检查：图像、参考照相机，以及 [**NormalizedAnchorPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) 属性。

### <a name="image"></a>图像

此示例显示在项目的**资源**文件夹中保存的自定义图像。 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 的 [**Image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Image) 属性需要类型 [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813) 的值。 此类型需要 [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791) 命名空间的 **using** 声明。

>[!NOTE]
>如果将同一图像用于多个地图图标，请声明页面级别或应用级别的 [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813) 以获得最佳性能。

### <a name="reference-camera"></a>参考照相机

 因为 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 图像随 [**ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ZoomLevel) 的变化而缩放，请务必定义正常的一倍尺寸图像以哪个 [**ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ZoomLevel) 显示。 该位置在 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 的参考照相机中定义，为了设置它，你必须将 [**MapCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcamera) 对象传递到 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 的构造函数中。

 你可以在 [**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) 中定义任意位置，然后使用该 [**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) 来创建一个 [**MapCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcamera) 对象。  但是，在此示例中，我们只需使用地图控件的 [**ActualCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ActualCamera) 属性返回的 [**MapCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcamera) 对象。 这是地图内部照相机。 该照相机的当前位置成为参考照相机位置；[**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 图像以 1 倍尺寸显示的位置。

 如果你的应用允许用户缩小地图，图像将缩小，因为地图内部照相机的高度将上升，而 1 倍大小的图像将仍然保持在参考照相机的位置上不动。

### <a name="normalizedanchorpoint"></a>NormalizedAnchorPoint

[**NormalizedAnchorPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) 是图像上锚定到 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 的 [**Location**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Location) 属性的点。 点 0.5,1 是图像的底部中心。 由于我们已经将 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 的 [**Location**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Location) 属性设置为图像控件的中心，该图像的底部中心将锚定到图像控件的中心。 如果希望图像以某个点为中心显示出来，请将 [**NormalizedAnchorPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) 设置为 0.5,0.5。  

## <a name="add-a-shape"></a>添加形状

通过使用 [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103) 类在地图上显示多点形状。 以下 [UWP 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)中的示例将在地图上显示一个具有蓝色边框的红色框。

```csharp
public void HighlightArea()
{
    // Create MapPolygon.

    double centerLatitude = myMap.Center.Position.Latitude;
    double centerLongitude = myMap.Center.Position.Longitude;

    var mapPolygon = new MapPolygon
    {
        Path = new Geopath(new List<BasicGeoposition> {
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude+0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
                }),
        ZIndex = 1,
        FillColor = Colors.Red,
        StrokeColor = Colors.Blue,
        StrokeThickness = 3,
        StrokeDashed = false,
    };

    // Add MapPolygon to a layer on the map control.
    var MyHighlights = new List<MapElement>();

    MyHighlights.Add(mapPolygon);

    var HighlightsLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyHighlights
    };

    myMap.Layers.Add(HighlightsLayer);
}
```

## <a name="add-a-line"></a>添加线条


通过使用 [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114) 类在地图上显示线条。 以下 [UWP 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)中的示例将在地图上显示一条虚线。

```csharp
public void DrawLineOnMap()
{
    // Create Polyline.

    double centerLatitude = myMap.Center.Position.Latitude;
    double centerLongitude = myMap.Center.Position.Longitude;
    var mapPolyline = new MapPolyline
    {
        Path = new Geopath(new List<BasicGeoposition> {
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
                }),
        StrokeColor = Colors.Black,
        StrokeThickness = 3,
        StrokeDashed = true,
    };

   // Add Polyline to a layer on the map control.

   var MyLines = new List<MapElement>();

   MyLines.Add(mapPolyline);

   var LinesLayer = new MapElementsLayer
   {
       ZIndex = 1,
       MapElements = MyLines
   };

   myMap.Layers.Add(LinesLayer);

}
```

## <a name="add-xaml"></a>添加 XAML

通过使用 XAML 在地图上显示自定义 UI 元素。 通过指定 XAML 的位置和标准化的定位点在地图上放置 XAML。

-   通过调用 [**SetLocation**](https://msdn.microsoft.com/library/windows/desktop/ms704369) 在地图上设置要放置 XAML 的位置。
-   通过调用 [**SetNormalizedAnchorPoint**](https://msdn.microsoft.com/library/windows/apps/dn637050) 在 XAML 上设置对应于指定位置的相对位置。

下面的示例显示了西雅图市的地图并添加了一个 XAML [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) 控件，以指示 Space Needle 的位置。 地图还以该区域为中心并放大。 有关使用地图控件的常规信息，请参阅 [以 2D、3D 和街景视图方式显示地图](display-maps.md)。

```csharp
private void displayXAMLButton_Click(object sender, RoutedEventArgs e)
{
   // Specify a known location.
   BasicGeoposition snPosition = new BasicGeoposition { Latitude = 47.620, Longitude = -122.349 };
   Geopoint snPoint = new Geopoint(snPosition);

   // Create a XAML border.
   Border border = new Border
   {
      Height = 100,
      Width = 100,
      BorderBrush = new SolidColorBrush(Windows.UI.Colors.Blue),
      BorderThickness = new Thickness(5),
   };

   // Center the map over the POI.
   MapControl1.Center = snPoint;
   MapControl1.ZoomLevel = 14;

   // Add XAML to the map.
   MapControl1.Children.Add(border);
   MapControl.SetLocation(border, snPoint);
   MapControl.SetNormalizedAnchorPoint(border, new Point(0.5, 0.5));
}
```

本示例在地图上显示蓝色边框。

![xaml 的快照显示在地图上的兴趣点中](images/displaypoixaml.png)

接下来的示例介绍如何使用数据绑定在页面的 XAML 标记中直接添加 XAML UI 元素。 与显示内容的其他 XAML 元素一样，[**Children**](https://msdn.microsoft.com/library/windows/apps/dn637008) 是 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 的默认内容属性并且无需在 XAML 标记中进行显式指定。

此示例介绍如何将两个 XAML 控件显示为 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 的隐式子对象。 这些控件在图上绑定数据的位置显示。

```xml
<maps:MapControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{x:Bind SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{x:Bind BellevueLocation}"/>
</maps:MapControl>
```

在代码隐藏文件中使用属性来设置这些位置。

```csharp
public Geopoint SeattleLocation { get; set; }
public Geopoint BellevueLocation { get; set; }
```

此示例显示如何显示包含在 [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094) 中的两个 XAML 控件。这些控件显示在地图上的数据绑定位置。

```xml
<maps:MapControl>
  <maps:MapItemsControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{x:Bind SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{x:Bind BellevueLocation}"/>
  </maps:MapItemsControl>
</maps:MapControl>
```

此示例显示绑定到 [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094) 的 XAML 元素的集合。

```xml
<maps:MapControl x:Name="MapControl" MapTapped="MapTapped" MapDoubleTapped="MapTapped" MapHolding="MapTapped">
  <maps:MapItemsControl ItemsSource="{x:Bind LandmarkOverlays}">
      <maps:MapItemsControl.ItemTemplate>
          <DataTemplate>
              <StackPanel Background="Black" Tapped ="Overlay_Tapped">
                  <TextBlock maps:MapControl.Location="{Binding Location}" Text="{Binding Title}"
                    maps:MapControl.NormalizedAnchorPoint="0.5,0.5" FontSize="20" Margin="5"/>
              </StackPanel>
          </DataTemplate>
      </maps:MapItemsControl.ItemTemplate>
  </maps:MapItemsControl>
</maps:MapControl>
```

以上示例中的 ``ItemsSource`` 属性绑定到代码隐藏文件中一个类型为 [IList](https://docs.microsoft.com/dotnet/api/system.collections.ilist?view=netframework-4.70) 的属性。

```csharp
public sealed partial class Scenario1 : Page
{
    public IList LandmarkOverlays { get; set; }

    public MyClassConstructor()
    {
         SetLandMarkLocations();
         this.InitializeComponent();   
    }

    private void SetLandMarkLocations()
    {
        LandmarkOverlays = new List<MapElement>();

        var pikePlaceIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.610, Longitude = -122.342 }),
            Title = "Pike Place Market"
        };

        LandmarkOverlays.Add(pikePlaceIcon);

        var SeattleSpaceNeedleIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.6205, Longitude = -122.3493 }),
            Title = "Seattle Space Needle"
        };

        LandmarkOverlays.Add(SeattleSpaceNeedleIcon);
    }
}
```

<a id="layers" />

## <a name="working-with-layers"></a>使用层

本指南中的示例向 [MapElementLayers](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer) 集合中添加元素。 然后它们显示如何将该集合添加到地图控件的 **Layers** 属性。 在以前的版本中，此指南如下向您展示如何将地图元素添加到 [**MapElements**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements)：。

```csharp
var pikePlaceIcon = new MapIcon
{
    Location = new Geopoint(new BasicGeoposition
    { Latitude = 47.610, Longitude = -122.342 }),
    NormalizedAnchorPoint = new Point(0.5, 1.0),
    ZIndex = 0,
    Title = "Pike Place Market"
};

myMap.MapElements.Add(pikePlaceIcon);
```

虽然你仍然可以使用此方法，但将无法利用新地图层模型的一些优势。 通过将元素分组为层，可以相互独立地操作每一个层。 例如，每个层都有自己的事件组，你可以响应特定层的事件，并执行特定于该事件的操作。

此外，你可以将 XAML 直接绑定到 [MapLayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maplayer)。 这是使用 [MapElements](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements) 集合无法实现的。

实现此目的的一种方法是使用视图模型类、代码隐藏 XAML 页和 XAML 页面。

### <a name="view-model-class"></a>视图模型类

```csharp
public class LandmarksViewModel
{
    public ObservableCollection<MapLayer> LandmarkLayer
        { get; } = new ObservableCollection<MapLayer>();

    public LandmarksViewModel()
    {
        var MyElements = new List<MapElement>();

        var pikePlaceIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.610, Longitude = -122.342 }),
            Title = "Pike Place Market"
        };

        MyElements.Add(pikePlaceIcon);

        var LandmarksLayer = new MapElementsLayer
        {
            ZIndex = 1,
            MapElements = MyElements
        };

        LandmarkLayer.Add(LandmarksLayer);         
    }

```

### <a name="code-behind-a-xaml-page"></a>代码隐藏 XAML 页

将视图模型类连接到代码隐藏页。

```csharp
public LandmarksViewModel ViewModel { get; set; }

public myMapPage()
{
    this.InitializeComponent();
    this.ViewModel = new LandmarksViewModel();
}
```

### <a name="xaml-page"></a>XAML 页

在 XAML 页中，绑定到模型类中返回层的属性。

```XML
<maps:MapControl
    x:Name="myMap" TransitFeaturesVisible="False" Loaded="MyMap_Loaded" Grid.Row="2"
    MapServiceToken="Your token" Layers="{x:Bind ViewModel.LandmarkLayer}"/>
```

## <a name="related-topics"></a>相关主题

* [必应地图开发人员中心](https://www.bingmapsportal.com/)
* [UWP 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [地图设计指南](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [版本 2015 视频：在 Windows 应用中跨手机、平板电脑和 PC 利用地图和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 路况应用示例](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)
* [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103)
* [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114)

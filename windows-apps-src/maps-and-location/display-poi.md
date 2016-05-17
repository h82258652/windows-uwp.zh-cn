---
author: PatrickFarley
title: '在地图上显示兴趣点 (POI)'
description: 使用图钉、图像、图形和 XAML UI 元素向地图添加目标点 (POI)。
ms.assetid: CA00D8EB-6C1B-4536-8921-5EAEB9B04FCA
---

# 在地图上显示目标点 (POI)


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


使用图钉、图像、图形和 XAML UI 元素向地图添加目标点 (POI)。 POI 是地图上表示对某事物感兴趣的特殊的点。 例如，企业、城市或好友的位置。

**提示** 若要了解有关在你的应用上显示 POI 的详细信息，请从 GitHub 上的 [Windows-universal-samples 存储库](http://go.microsoft.com/fwlink/p/?LinkId=619979)下载以下示例。

-   [通用 Windows 平台 (UWP) 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)

通过将 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)、[**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103) 和 [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114) 对象添加到地图控件的 [**MapElements**](https://msdn.microsoft.com/library/windows/apps/dn637033) 集合，在地图上显示图钉、图像和图形。 以编程方式使用数据绑定或添加项目；你无法以声明方式在 XAML 标记中绑定到你的 **MapElements** 集合。

通过将 XAML 用户界面元素（例如，[**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)、[**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 或 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)）添加为 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 的 [**Children**](https://msdn.microsoft.com/library/windows/apps/dn637008)，在地图上显示它们。 你也可以将它们添加到 [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094)，或将 **MapItemsControl** 绑定到项目集合。

总结：

-   [向地图添加 MapIcon](#mapicon) 以显示图像（例如，图钉）和可选文本。
-   [向地图添加 MapPolygon](#mappolygon) 以显示多点图形。
-   [向地图添加 MapPolyline](#mappolyline) 以在地图上显示线条。
-   [向地图添加 XAML](#mapxaml) 以显示自定义 UI 元素。

如果你想要将大量元素放置在地图上，请考虑[在地图上覆盖平铺图像](overlay-tiled-images.md)。 若要在地图上显示道路，请参阅[显示路线和方向](routes-and-directions.md)

## 添加 MapIcon


通过使用 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 类在地图上显示图像（例如图钉）和可选文本。 可以接受默认图像或通过使用 [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) 属性提供自定义图像。 以下图像显示了未为 [**Title**](https://msdn.microsoft.com/library/windows/apps/dn637088) 属性指定任何值、具有短标题、具有长标题和具有非常长的标题的 **MapIcon** 的默认图像。

![具有不同长度的磁贴的 MapIcon 示例。](images/mapctrl-mapicons.png)

下面的示例显示了西雅图市的地图并添加了一个带默认图像和可选标题的 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)，可指示 Space Needle 的位置。 地图还以该图标为中心并放大。 有关使用地图控件的常规信息，请参阅[以 2D、3D 和街景视图方式显示地图](display-maps.md)

```csharp
      private void displayPOIButton_Click(object sender, RoutedEventArgs e)
      {
         // Specify a known location.
         BasicGeoposition snPosition = new BasicGeoposition() { Latitude = 47.620, Longitude = -122.349 };
         Geopoint snPoint = new Geopoint(snPosition);

         // Create a MapIcon.
         MapIcon mapIcon1 = new MapIcon();
         mapIcon1.Location = snPoint;
         mapIcon1.NormalizedAnchorPoint = new Point(0.5, 1.0);
         mapIcon1.Title = "Space Needle";
         mapIcon1.ZIndex = 0;

         // Add the MapIcon to the map.
         MapControl1.MapElements.Add(mapIcon1);

         // Center the map over the POI.
         MapControl1.Center = snPoint;
         MapControl1.ZoomLevel = 14;
      }
```

本示例在地图上显示以下 POI（以默认图像为中心）。

![带 MapIcon 的地图](images/displaypoidefault.png)

下面的代码行将显示 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 和保存在项目“资源”文件夹中的自定义图像。 **MapIcon** 的 [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) 属性需要类型 [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813) 的值。 此类型需要 [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791) 命名空间的 **using** 声明。

**提示** 如果将同一图像用于多个地图图标，请声明页面级别或应用级别的 [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813) 以获得最佳性能。

```csharp
    MapIcon1.Image =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/customicon.png"));
```

使用 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 类时，请记住以下注意事项：

-   [
            **Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) 属性支持的最大图像大小为 2048×2048 像素。
-   默认情况下，不一定会显示地图图标的图像。 当它挡住地图上其他元素或标签时，它可能会隐藏。 若要保持可见，将地图图标的 [**CollisionBehaviorDesired**](https://msdn.microsoft.com/library/windows/apps/dn974327) 属性设置为 [**MapElementCollisionBehavior.RemainVisible**](https://msdn.microsoft.com/library/windows/apps/dn974314)
-   [
            **MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 的可选 [**Title**](https://msdn.microsoft.com/library/windows/apps/dn637088) 并不一定会显示。 如果你没有看到此文本，通过减少 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 的 [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) 属性的值来缩小显示
-   当你显示指向地图上特定位置的 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 图像时（例如，图钉或箭头），考虑将 [**NormalizedAnchorPoint**](https://msdn.microsoft.com/library/windows/apps/dn637082) 属性的值设置为图像上指针的大致位置。 如果你将 **NormalizedAnchorPoint** 的值保留为默认值 (0, 0)（表示图像的左上角），地图的 [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) 中的更改可能会导致图像指向其他位置。

## 添加 MapPolygon


通过使用 [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103) 类在地图上显示多点形状。 以下 [UWP 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)中的示例将在地图上显示一个具有蓝色边框的红色框。

```csharp
private void mapPolygonAddButton_Click(object sender, Windows.UI.Xaml.RoutedEventArgs e)
{
   double centerLatitude = myMap.Center.Position.Latitude;
   double centerLongitude = myMap.Center.Position.Longitude;
   MapPolygon mapPolygon = new MapPolygon();
   mapPolygon.Path = new Geopath(new List<BasicGeoposition>() {
         new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude-0.001 },                
         new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },                
         new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude+0.001 },
         new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },

   });
           
   mapPolygon.ZIndex = 1;
   mapPolygon.FillColor = Colors.Red;
   mapPolygon.StrokeColor = Colors.Blue;
   mapPolygon.StrokeThickness = 3;
   mapPolygon.StrokeDashed = false;
   myMap.MapElements.Add(mapPolygon);
}
```

## 添加 MapPolyline


通过使用 [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114) 类在地图上显示线条。 以下 [UWP 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)中的示例将在地图上显示一条虚线。

```csharp
private void mapPolylineAddButton_Click(object sender, Windows.UI.Xaml.RoutedEventArgs e)
{
   double centerLatitude = myMap.Center.Position.Latitude;
   double centerLongitude = myMap.Center.Position.Longitude;
   MapPolyline mapPolyline = new MapPolyline();
   mapPolyline.Path = new Geopath(new List<BasicGeoposition>() {                
         new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },                
         new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
   });
              
   mapPolyline.StrokeColor = Colors.Black;
   mapPolyline.StrokeThickness = 3;
   mapPolyline.StrokeDashed = true;
   myMap.MapElements.Add(mapPolyline);
}
```

## 添加 XAML


通过使用 XAML 在地图上显示自定义 UI 元素。 通过指定 XAML 的位置和标准化的定位点在地图上放置 XAML。

-   通过调用 [**SetLocation**](https://msdn.microsoft.com/library/windows/desktop/ms704369) 在地图上设置要放置 XAML 的位置
-   通过调用 [**SetNormalizedAnchorPoint**](https://msdn.microsoft.com/library/windows/apps/dn637050) 在 XAML 上设置对应于指定位置的相对位置

下面的示例显示了西雅图市的地图并添加了一个 XAML [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) 控件，以指示 Space Needle 的位置。 地图还以该区域为中心并放大。 有关使用地图控件的常规信息，请参阅[以 2D、3D 和街景视图方式显示地图](display-maps.md)

```csharp
private void displayXAMLButton_Click(object sender, RoutedEventArgs e)
{
   // Specify a known location.
   BasicGeoposition snPosition = new BasicGeoposition() { Latitude = 47.620, Longitude = -122.349 };
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

![](images/displaypoixaml.png)

接下来的示例介绍如何使用数据绑定在页面的 XAML 标记中直接添加 XAML UI 元素。 与显示内容的其他 XAML 元素一样，[**Children**](https://msdn.microsoft.com/library/windows/apps/dn637008) 是 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 的默认内容属性并且无需在 XAML 标记中进行显式指定。

此示例介绍如何将两个 XAML 控件显示为 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 的隐式子对象

```xml
<maps:MapControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{Binding SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{Binding BellevueLocation}"/>
</maps:MapControl>
```

此示例介绍如何显示 [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094) 中包含的两个 XAML 控件

```xml
<maps:MapControl>
  <maps:MapItemsControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{Binding SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{Binding BellevueLocation}"/>
  </maps:MapItemsControl>
</maps:MapControl>
```

此示例介绍绑定到 [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094) 的 XAML 元素的集合

```xml
<maps:MapControl x:Name="MapControl" MapTapped="MapTapped" MapDoubleTapped="MapTapped" MapHolding="MapTapped">
  <maps:MapItemsControl ItemsSource="{Binding StateOverlays}">
    <maps:MapItemsControl.ItemTemplate>
      <DataTemplate>
        <StackPanel Background="Black" Tapped="Overlay_Tapped">
          <TextBlock maps:MapControl.Location="{Binding Location}" Text="{Binding Name}" maps:MapControl.NormalizedAnchorPoint="0.5,0.5" FontSize="20" Margin="5"/>
        </StackPanel>
      </DataTemplate>
    </maps:MapItemsControl.ItemTemplate>
  </maps:MapItemsControl>
</maps:MapControl>
```

## 相关主题

* [必应地图开发人员中心](https://www.bingmapsportal.com/)
* [UWP 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [地图设计指南](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [版本 2015 视频：在 Windows 应用中跨手机、平板电脑和 PC 利用地图和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 路况应用示例](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)
* [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103)
* [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114)




<!--HONumber=May16_HO2-->



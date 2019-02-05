---
Description: A semantic zoom control allows the user to zoom between two different semantic views of the same data set.
title: 语义式缩放
ms.assetid: B5C21FE7-BA83-4940-9CC1-96F6A2DC28C7
label: Semantic zoom
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, uwp
pm-contact: predavid
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 5689fd967756025872fd45bf242076e854e700aa
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2019
ms.locfileid: "9048094"
---
# <a name="semantic-zoom"></a>语义式缩放

 

语义式缩放使用户能够在相同内容的两个不同视图之间切换，以便他们可以在大型分组数据集中快速导航。
 
- 放大视图是内容的主视图。 这是显示个别数据项的主视图。 
- 缩小视图是相同内容的更高级别的视图。 通常，你会在此视图中显示分组数据集的组标头。 

例如，当查看地址簿时，用户可以通过缩小快速跳转到字母“W”，然后在该字母上放大并查看与之相关联的名称。 

> **重要 API**：[SemanticZoom 类](https://msdn.microsoft.com/library/windows/apps/hh702601)、[ListView 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx)、[GridView 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx)

**功能**：

-   缩小视图的大小受语义式缩放控件的边界限制。
-   点击组标题可切换视图。 可以启用收缩作为一种在视图之间切换的方式。
-   视图之间的活动标题切换。

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

在你需要显示大到无法在一个或两个页面上全部显示的分组数据集时，请使用 **SemanticZoom** 控件。

不要将语义上的缩放与光学缩放混淆。 尽管它们的交互和基本行为（基于缩放比例显示更多或更少细节）一致，但是光学缩放是指调整内容区域或对象（如照片）的放大倍数。 有关执行光学缩放的控件的信息，请参阅 [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) 控件。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/SemanticZoom">打开此应用，了解 SemanticZoom 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

**“照片”应用**

下面是在“照片”应用中使用的语义式缩放。 照片按月分组。 为了实现更快的导航，在默认网格视图中选择月标头可缩小到月列表视图。

![在“照片”应用中使用的语义式缩放](images/control-examples/semantic-zoom-photos.png)

**通讯簿**

通讯簿是使用语义式缩放可更方便地导航数据集的另一个示例。 你可以使用缩小视图快速跳转到所需的字母（左图），同时视图中的放大视图显示个别数据项（右图）。

![在联系人列表中使用的语义式缩放示例](images/semanticzoom-win10.png)

## <a name="create-a-semantic-zoom"></a>创建语义式缩放

**SemanticZoom** 控件没有任何其自己的可视表示形式。 它是一个主机控件，用于管理提供内容视图的另外 2 个控件，通常为 **ListView** 或 **GridView** 控件。  将视图控件设置为 SemanticZoom 的 [ZoomedInView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.zoomedinview.aspx) 和 [ZoomedOutView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.zoomedoutview.aspx) 属性。

语义式缩放所需的 3 个要素为：
- 分组数据源
- 显示项目级数据的放大视图。
- 显示组级数据的缩小视图。

在使用语义式缩放前，应了解如何将列表视图用于分组数据。 有关详细信息，请参阅[列表视图和网格视图](listview-and-gridview.md)和[在列表中为项目分组]()。 

> **注意**&nbsp;&nbsp;若要定义 SemanticZoom 控件的放大视图和缩小视图，可以使用任意两个可实现 [ISemanticZoomInformation](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.isemanticzoominformation.aspx) 接口的控件。 XAML 框架提供 3 个可实现此接口的控件：ListView、GridView 和 Hub。
 
 此 XAML 显示 SemanticZoom 控件的结构。 将其他控件分配到 ZoomedInView 和 ZoomedOutView 属性。
 
 ```xaml
<SemanticZoom>
    <SemanticZoom.ZoomedInView>
        <!-- Put the GridView for the zoomed in view here. -->   
    </SemanticZoom.ZoomedInView>

    <SemanticZoom.ZoomedOutView>
        <!-- Put the ListView for the zoomed out view here. -->       
    </SemanticZoom.ZoomedOutView>
</SemanticZoom>
 ```
 
下面的示例摘自 [XAML UI 基本示例](https://go.microsoft.com/fwlink/p/?LinkId=619992)的 SemanticZoom 页面。 可以下载该示例来查看完整代码，包括数据源。 此语义式缩放使用 GridView 提供放大视图，使用 ListView 提供缩小视图。
  
**定义放大视图**

下面是用于放大视图的 GridView 控件。 放大视图应分组显示个别数据项。 此示例介绍如何在具有图像和文本的网格中显示项目。 

```xaml
<SemanticZoom.ZoomedInView>
    <GridView ItemsSource="{x:Bind cvsGroups.View}" 
              ScrollViewer.IsHorizontalScrollChainingEnabled="False" 
              SelectionMode="None" 
              ItemTemplate="{StaticResource ZoomedInTemplate}">
        <GridView.GroupStyle>
            <GroupStyle HeaderTemplate="{StaticResource ZoomedInGroupHeaderTemplate}"/>
        </GridView.GroupStyle>
    </GridView>
</SemanticZoom.ZoomedInView>
```
 
`ZoomedInGroupHeaderTemplate` 资源中定义了组标头的外观。 `ZoomedInTemplate` 资源中定义了项目的外观。 

```xaml
<DataTemplate x:Key="ZoomedInGroupHeaderTemplate" x:DataType="data:ControlInfoDataGroup">
    <TextBlock Text="{x:Bind Title}" 
               Foreground="{ThemeResource ApplicationForegroundThemeBrush}" 
               Style="{StaticResource SubtitleTextBlockStyle}"/>
</DataTemplate>

<DataTemplate x:Key="ZoomedInTemplate" x:DataType="data:ControlInfoDataItem">
    <StackPanel Orientation="Horizontal" MinWidth="200" Margin="12,6,0,6">
        <Image Source="{x:Bind ImagePath}" Height="80" Width="80"/>
        <StackPanel Margin="20,0,0,0">
            <TextBlock Text="{x:Bind Title}" 
                       Style="{StaticResource BaseTextBlockStyle}"/>
            <TextBlock Text="{x:Bind Subtitle}" 
                       TextWrapping="Wrap" HorizontalAlignment="Left" 
                       Width="300" Style="{StaticResource BodyTextBlockStyle}"/>
        </StackPanel>
    </StackPanel>
</DataTemplate>
```

**定义放大视图**

此 XAML 为缩小视图定义 ListView 控件。 此示例介绍如何在列表中将组标头显示为文本。

```xaml
<SemanticZoom.ZoomedOutView>
    <ListView ItemsSource="{x:Bind cvsGroups.View.CollectionGroups}" 
              SelectionMode="None" 
              ItemTemplate="{StaticResource ZoomedOutTemplate}" />
</SemanticZoom.ZoomedOutView>
```

 `ZoomedOutTemplate` 资源中定义了外观。
 
```xaml    
<DataTemplate x:Key="ZoomedOutTemplate" x:DataType="wuxdata:ICollectionViewGroup">
    <TextBlock Text="{x:Bind Group.(data:ControlInfoDataGroup.Title)}" 
               Style="{StaticResource SubtitleTextBlockStyle}" TextWrapping="Wrap"/>
</DataTemplate>
```

**同步视图**

放大视图和缩小视图应该同步，因此如果用户在缩小视图中选择某个组，则此相同组的详细信息应该显示在放大视图中。 你可以使用 [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx) 或添加代码以同步视图。

绑定到同一 CollectionViewSource 的所有控件始终具有相同的当前项。 如果这两个视图使用同一 CollectionViewSource 作为它们的数据源，CollectionViewSource 将自动同步视图。 有关详细信息，请参阅 [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx)。

如果你不使用 CollectionViewSource 同步视图，则应该处理 [ViewChangeStarted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.viewchangestarted.aspx) 事件并在事件处理程序中同步项目，如下所示。

```xaml
<SemanticZoom x:Name="semanticZoom" ViewChangeStarted="SemanticZoom_ViewChangeStarted">
```

```csharp
private void SemanticZoom_ViewChangeStarted(object sender, SemanticZoomViewChangedEventArgs e)
{
    if (e.IsSourceZoomedInView == false)
    {
        e.DestinationItem.Item = e.SourceItem.Item;
    }
}
```

## <a name="recommendations"></a>建议

-   在你的应用中使用语义式缩放时，请确保项目布局和平移方向不会根据缩放级别而更改。 布局和平移交互应该在各个缩放级别上保持一致且可预测。
-   语义式缩放支持用户快速跳转到内容，因此请将缩小模式中的页面/屏幕数限制为三个。 过多的平移会减少语义式缩放的实用性。
-   避免使用语义式缩放来更改内容的范围。 例如，相册不应在文件资源管理器中切换为文件夹视图。
-   使用视图中必不可少的结构和语义。
-   对分组集合中的项使用组名称。
-   对未分组但已排序的集合使用分类排序（例如，按日期时间先后排序或按名称列表字母排序）。


## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。


## <a name="related-articles"></a>相关文章

- [导航设计基础知识](../basics/navigation-basics.md)
- [列表视图和网格视图](listview-and-gridview.md)
- [项目容器和模板](item-containers-templates.md)






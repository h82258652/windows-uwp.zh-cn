---
使用布局面板来为应用中的 UI 元素排列和分组。
通用 Windows 平台 (UWP) 应用的布局面板
ms.assetid: 07A7E022-EEE9-4C81-AF07-F80868665994
布局面板
template: detail.hbs
---
# 布局面板

使用布局面板来为应用中的 UI 元素排列和分组。 内置 XAML 布局面板包括 [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx)、[**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx)、[**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx)、[**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.aspx) 和 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx)。 在此处，我们介绍每个面板并显示如何使用它设置 XAML UI 元素的布局。

选择布局面板时有多个需要考虑的事项：
- 面板如何定位其子元素。
- 面板如何调整其子元素的大小。
- 如何交错放置重叠子元素（z 顺序）。
- 创建所需布局时需要的嵌套面板元素的数量和复杂度。


**面板附加属性**

大多数 XAML 布局面板使用附加属性，使其子元素通知父面板应如何在 UI 中放置它们。 附加属性使用语法 *AttachedPropertyProvider.PropertyName*。 如果你有嵌套在其他面板内的面板，则 UI 元素上用于将布局特征指定到父项的附加属性仅由关系最直接的父面板解释。

以下是一个有关如何才能在 XAML 中的 Button 控件上设置 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.left.aspx) 附加属性的示例。 这会通知父项 Canvas 应将 Button 放置在距 Canvas 的左边缘 50 个有效像素的位置。

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

有关附加属性的详细信息，请参阅[附加属性概述](../xaml-platform/attached-properties-overview.md)。

> **注意** 附加属性是一个 XAML 概念，需要特殊语法才能从代码中获取或设置。 若要在代码中使用附加属性，请参阅*附加属性概述*文章的*代码中的附加属性*部分。

**面板边框**

RelativePanel、StackPanel 和 Grid 面板定义边框属性，可使你在面板周围绘制边框，而无需将它们包装在其他 Border 元素中。 这些边框属性为 **BorderBrush**、**BorderThickness**、**CornerRadius** 和 **Padding**。

下面是如何在 Grid 上设置边框属性的示例。

```xaml
<Grid BorderBrush="Blue" BorderThickness="12" CornerRadius="12" Padding="12">
    <TextBlock Text="Hello World!"/>
</Grid>
```

![带有边框的 Grid](images/layout-panel-grid-border.png)

使用内置边框属性可以减少 XAML 元素计数，从而改进应用的 UI 性能。 有关布局面板和 UI 性能的详细信息，请参阅[优化你的 XAML 布局](https://msdn.microsoft.com/en-us/library/windows/apps/mt404609.aspx)。

## RelativePanel

[
            **RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx) 允许你设置 UI 元素的布局，方法是指定它们相对于其他元素的位置和相对于面板的位置。 默认情况下，将一个元素放置在面板的左上角。 你可以将 RelativePanel 与 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstatemanager.aspx) 和 [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.adaptivetrigger.aspx) 一起使用，以针对不同的窗口大小重新排列 UI。

此表显示了可用于将元素与面板的边缘或中心对齐以及相对于其他元素对齐和放置该元素的附加属性。

面板对齐 | 同级对齐 | 同级位置
----------------|-------------------|-----------------
[**AlignTopWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aligntopwithpanel.aspx) | [**AlignTopWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aligntopwith.aspx) | [**Above**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.above.aspx)  
[**AlignBottomWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignbottomwithpanel.aspx) | [**AlignBottomWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignbottomwith.aspx) | [**Below**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.below.aspx)  
[**AlignLeftWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignleftwithpanel.aspx) | [**AlignLeftWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignleftwith.aspx) | [**LeftOf**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.leftof.aspx)  
[**AlignRightWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignrightwithpanel.aspx) | [**AlignRightWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignrightwith.aspx) | [**RightOf**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.rightof.aspx)  
[**AlignHorizontalCenterWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithpanel.aspx) | [**AlignHorizontalCenterWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwith.aspx) | &nbsp;   
[**AlignVerticalCenterWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignverticalcenterwithpanel.aspx) | [**AlignVerticalCenterWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignverticalcenterwith.aspx) | &nbsp;   

 
此 XAML 演示了如何排列 RelativePanel 中的元素。

```xaml
<RelativePanel BorderBrush="Gray" BorderThickness="1">
    <Rectangle x:Name="RedRect" Fill="Red" Height="44" Width="44"/>
    <Rectangle x:Name="BlueRect" Fill="Blue"
               Height="44" Width="88"
               RelativePanel.RightOf="RedRect" />

    <Rectangle x:Name="GreenRect" Fill="Green" 
               Height="44"
               RelativePanel.Below="RedRect" 
               RelativePanel.AlignLeftWith="RedRect" 
               RelativePanel.AlignRightWith="BlueRect"/>
    <Rectangle Fill="Yellow"
               RelativePanel.Below="GreenRect" 
               RelativePanel.AlignLeftWith="BlueRect" 
               RelativePanel.AlignRightWithPanel="True"
               RelativePanel.AlignBottomWithPanel="True"/>
</RelativePanel>
```

结果如下所示。 

![相对面板](images/layout-panel-relative-panel.png)

以下是一些在调整矩形大小时需要注意的事项。
- 红色矩形给定 44x44 的显式大小。 它放置在面板的左上角，该位置是默认位置。
- 绿色矩形给定 44 的显式高度。 它的左边与红色矩形对齐，它的右边与蓝色矩形对齐，这决定了它的宽度。
- 黄色矩形未给定显式大小。 它的左边与蓝色矩形对齐。 它的右边和底边与面板的边缘对齐。 它的大小由这些对齐来决定，而且它将随着面板调整大小而调整大小。

## StackPanel

[
            **StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx) 是简单的布局面板，它将其子元素排列成一条水平或垂直方向的直线。 如果你需要在页面 UI 中排列一个小的子部分，通常使用 StackPanel 控件。

你可以使用 [**Orientation**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.orientation.aspx) 属性来指定子元素的方向。 默认方向为 [**Vertical**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.orientation.aspx)。

以下 XAML 显示了如何创建项目的垂直 StackPanel。

```xaml
<StackPanel>
    <Rectangle Fill="Red" Height="44"/>
    <Rectangle Fill="Blue" Height="44"/>
    <Rectangle Fill="Green" Height="44"/>
    <Rectangle Fill="Yellow" Height="44"/>
</StackPanel>
```


结果如下所示。

![堆栈面板](images/layout-panel-stack-panel.png)

在 StackPanel 中，如果未明确设置子元素的大小，则该元素会拉伸以填满可用宽度（或如果 Orientation 为 **Horizontal**，则为高度）。 在本示例中，未设置矩形的宽度。 扩展矩形以填充 StackPanel 的整个宽度。

## Grid

[
            **Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx) 面板支持以多行或多列布局排列控件。 你可以通过使用 [**RowDefinitions**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.rowdefinitions.aspx) 和 [**ColumnDefinitions**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.columndefinitions.aspx) 属性来指定 Grid 面板的行和列。 在 XAML 中，使用属性元素语法声明 Grid 元素内的行和列。 通过使用 **Auto** 缩放或比例缩放，你可以分配列或行中的空间。

通过使用 [**Grid.Column**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.column.aspx) 和 [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.row.aspx) 附加属性，你可以将对象放置到 Grid 的特定单元中。

通过使用 [**Grid.RowSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.rowspan.aspx) 和 [**Grid.ColumnSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.columnspan.aspx) 附加属性，你可使内容分散到多个行和列中。

此 XAML 示例显示了如何创建包含三行和两列的 Grid。 第一行和第三行的高度刚好足够包含文本。 第二行的高度填满可用高度的剩余部分。 所有列的宽度使用平均划分的可用容器宽度。

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition Height="44"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Rectangle Fill="Red" Width="44"/>
    <Rectangle Fill="Blue" Grid.Row="1"/>
    <Rectangle Fill="Green" Grid.Column="1"/>
    <Rectangle Fill="Yellow" Grid.Row="1" Grid.Column="1"/>
</Grid>
```


结果如下所示。

![Grid](images/layout-panel-grid.png)

在此示例中，大小调整的工作方式如下所示： 
- 第二行的显式高度为 44 个有效像素。 默认情况下，第一行的高度将填充所遗留的任何空间。
- 第一列的宽度设置为 **Auto**，因此它是其子项所需的宽度。 在此情况下，它是 44 个有效像素的宽度，以适应红色矩形的宽度。
- 对矩形没有其他大小约束，因此每一个都进行拉伸以填充它所在的网格单元格。

## VariableSizedWrapGrid

[
            **VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.aspx) 提供一个网格样式的布局面板，其中元素以行或列排列，当达到 [**MaximumRowsOrColumns**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.maximumrowsorcolumns.aspx) 值时会自动换行至新行或新列。 

[
            **Orientation**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.orientation.aspx) 属性在换行前指定网格将在行还是列中添加其项目。 默认方向为 **Vertical**，这意味着网格将从上到下添加项目，直到列填满，然后换行到新列。 当该值为 **Horizontal** 时，网格从左到右添加项目，然后换行到新行。

单元格尺寸由 [**ItemHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.itemheight.aspx) 和 [**ItemWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.itemwidth.aspx) 指定。 每个单元格的大小相同。 如果未指定 ItemHeight 或 ItemWidth，则第一个单元格调整大小以适应其内容，并且每个其他单元格都采用第一个单元格的大小。

你可以使用 [**VariableSizedWrapGrid.ColumnSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.columnspan.aspx) 和 [**VariableSizedWrapGrid.RowSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.rowspan.aspx) 附加属性来指定子元素应填充多少个相邻单元格。

以下是在 XAML 中使用 VariableSizedWrapGrid 的方法。

```xaml
<VariableSizedWrapGrid MaximumRowsOrColumns="3" ItemHeight="44" ItemWidth="44">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" 
               VariableSizedWrapGrid.RowSpan="2"/>
    <Rectangle Fill="Green" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
    <Rectangle Fill="Yellow" 
               VariableSizedWrapGrid.RowSpan="2" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
</VariableSizedWrapGrid>
```


结果如下所示。

![大小可变的换行网格](images/layout-panel-variable-size-wrap-grid.png)

在此示例中，每列中的最大行数是 3。 第一列仅包含 2 个项目（红色和蓝色矩形），因为蓝色矩形跨 2 个行。 然后绿色矩形换行到下一列的顶部。

## 画布

[
            **Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx) 面板使用固定的坐标点定位其子元素。 通过在每个元素上设置 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.left.aspx) 和 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.top.aspx) 附加属性，你可以在单独子元素上指定点。 在布局期间，父项 Canvas 对象从其子项中读取这些附加属性值，并在布局的 [Arrange](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.arrange.aspx) 传递期间使用这些值。

Canvas 中的对象可以重叠，即在一个对象顶部绘制另一个对象。 默认情况下，Canvas 以声明子对象的顺序呈现子对象，因此最后一个子对象在顶部呈现（每个元素的默认 z-index 为 0）。 这与其他内置面板相同。 但是，Canvas 还支持 [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.zindex.aspx) 附加属性，可在每个子元素上设置。 你可以在代码中设置此属性，以在运行时期间更改元素的绘制顺序。 带有最高 Canvas.ZIndex 值的元素将最后绘制，因此它将在位于相同位置的任何其他元素上重叠绘制，或者将以任何方式进行重叠。 请注意，因为需要遵循 alpha 值（透明度），所以即使元素重叠，在重叠区域中显示的内容也可能混合（如果顶部元素具有非最大 alpha 值）。

Canvas 不对其子素的大小进行任何调整。 每个元素都必须指定其大小。

以下是 XAML 中 Canvas 的示例。

```xaml
<Canvas Width="120" Height="120">
    <Rectangle Fill="Red" Height="44" Width="44"/>
    <Rectangle Fill="Blue" Height="44" Width="44" Canvas.Left="20" Canvas.Top="20"/>
    <Rectangle Fill="Green" Height="44" Width="44" Canvas.Left="40" Canvas.Top="40"/>
    <Rectangle Fill="Yellow" Height="44" Width="44" Canvas.Left="60" Canvas.Top="60"/>
</Canvas>
```


结果如下所示。

![Canvas](images/layout-panel-canvas.png)

谨慎使用 Canvas 面板。 尽管在某些情况下，在 UI 中精确控制元素的位置很简单，但固定定位布局面板会导致你的 UI 区域不太适应整个应用窗口大小的更改。 应用窗口大小可能会随设备方向更改、拆分应用窗口、更改监视器和许多其他用户方案而进行调整。

## ItemsControl 的面板

存在多种只能用作 [**ItemsPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx) 来显示 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx) 中的项的特殊用途面板。 它们是 [**ItemsStackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemsstackpanel.aspx)、[**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemswrapgrid.aspx)、[**VirtualizingStackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.virtualizingstackpanel.aspx) 和 [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.wrapgrid.aspx)。 你无法将这些面板用于常规 UI 布局。


<!--HONumber=Mar16_HO1-->



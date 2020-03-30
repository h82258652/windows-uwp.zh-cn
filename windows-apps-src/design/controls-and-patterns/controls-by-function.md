---
Description: 提供了按功能列出的可在应用中使用的某些控件列表。
title: 按功能列出控件
ms.assetid: 8DB4347B-91D6-4659-91F2-80ECF7BBB596
label: Controls by function
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8f1717a59399fb95f7b71a38ee8d2d46de4ca765
ms.sourcegitcommit: e11e0f65930665579d1f296861234893e82bf8fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80301428"
---
# <a name="controls-by-function"></a>按功能列出控件

面向 Windows 的 XAML UI 框架提供丰富的控件库，这些控件可支持 UI 开发。 这些控件中的一部分具有直观的表示形式；其他控件发挥作为其他控件或内容（如图像和媒体）的容器的作用。 

通过下载 [XAML UI 基本示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)，可以查看许多实际应用的 Windows UI 控件。

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/NavigationView">打开此应用，了解 NavigationView 的实际应用</a> 。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>


下面是按功能列出的可在应用中使用的常见 XAML 控件列表。

## <a name="appbars-and-commands"></a>应用栏和命令

### <a name="app-bar"></a>应用栏
用于显示特定于应用程序的命令的工具栏。 请参阅“命令栏”。

参考：[AppBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) 

### <a name="app-bar-button"></a>应用栏按钮
使用应用栏样式显示命令的按钮。

![应用栏按钮图标](images/controls/app-bar-buttons.png) 

参考：[AppBarButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)、[SymbolIcon](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SymbolIcon)、[BitmapIcon](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.BitmapIcon)、[FontIcon](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FontIcon)、[PathIcon](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PathIcon) 

设计和操作方法：[应用栏和命令栏控件指南](app-bars.md) 

示例代码：[XAML 命令示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

### <a name="app-bar-separator"></a>应用栏分隔符
在视觉上分隔命令栏中的命令组。

参考：[AppBarSeparator](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator) 

示例代码：[XAML 命令示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

### <a name="app-bar-toggle-button"></a>应用栏切换按钮
用于在命令栏中切换命令的按钮。

参考：[AppBarToggleButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton) 

示例代码：[XAML 命令示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

### <a name="command-bar"></a>命令栏
一个特殊的应用栏，用于调整应用栏按钮元素的大小。

![命令栏控件](images/command-bar-compact.png)

```xaml
<CommandBar>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
</CommandBar>
```
参考：[CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 

设计和操作方法：[应用栏和命令栏控件指南](app-bars.md)

示例代码：[XAML 命令示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

## <a name="buttons"></a>按钮

### <a name="button"></a>Button
响应用户输入和引发 **Click** 事件的控件。

![标准按钮](images/controls/button.png)

```xaml
<Button x:Name="button1" Content="Button" 
        Click="Button_Click" />
```

参考：[Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) 

设计和操作方法：[按钮控件指南](buttons.md) 

### <a name="hyperlink"></a>Hyperlink
请参阅“超链接”按钮。

### <a name="hyperlink-button"></a>“超链接”按钮
显示为标记文本并在浏览器中打开指定 URI 的按钮。

![“超链接”按钮](images/controls/hyperlink-button.png)

```xaml
<HyperlinkButton Content="www.microsoft.com" 
                 NavigateUri="https://www.microsoft.com"/>
```

参考：[HyperlinkButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) 

设计和操作方法：[超链接控件指南](hyperlinks.md)

### <a name="repeat-button"></a>重复按钮
从按下到释放为止重复引发 **Click** 事件的按钮。 

![重复按钮控件](images/controls/repeat-button.png) 

```xaml
<RepeatButton x:Name="repeatButton1" Content="Repeat Button" 
              Click="RepeatButton_Click" />
```

参考：[RepeatButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.RepeatButton) 

设计和操作方法：[按钮控件指南](buttons.md) 

## <a name="collectiondata-controls"></a>集合/数据控件

### <a name="flip-view"></a>翻转视图
显示用户可逐一浏览的项目集合的控件。

```xaml
<FlipView x:Name="flipView1" SelectionChanged="FlipView_SelectionChanged">
    <Image Source="Assets/Logo.png" />
    <Image Source="Assets/SplashScreen.png" />
    <Image Source="Assets/SmallLogo.png" />
</FlipView>
```

参考：[FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView) 

设计和操作方法：[翻转视图控件指南](flipview.md) 

### <a name="grid-view"></a>网格视图
显示可在行和列中垂直滚动的项集合的控件。

```xaml
<GridView x:Name="gridView1" SelectionChanged="GridView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</GridView>
```

参考：[GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) 

设计和操作方法：[列表](lists.md) 

示例代码：[ListView 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)

### <a name="items-control"></a>项目控件
显示数据模板所指定 UI 中项目集合的控件。 

```xaml
<ItemsControl/>
```

参考：[ItemsControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) 

### <a name="list-view"></a>列表视图
显示可在列表中垂直滚动的项目集合的控件。

```xaml
<ListView x:Name="listView1" SelectionChanged="ListView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</ListView>
```

参考：[ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 

设计和操作方法：[列表](lists.md) 

示例代码：[ListView 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)

## <a name="date-and-time-controls"></a>日期和时间控件

### <a name="calendar-date-picker"></a>日历日期选取器
允许用户使用下拉日历显示选择日期的控件。

![日历视图打开的日历日期选取器](images/controls/calendar-date-picker-open.png)

```xaml
<CalendarDatePicker/>
```

参考：[CalendarDatePicker](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker) 

设计和操作方法：[日历、日期和时间控件](date-and-time.md)
 
### <a name="calendar-view"></a>日历视图
允许用户选择单个或多个日期的可配置日历屏幕。

```xaml
<CalendarView/>
```

参考：[CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) 

设计和操作方法：[日历、日期和时间控件](date-and-time.md) 

### <a name="date-picker"></a>日期选取器
使用户可以选择日期的控件。

![日期选取器控件](images/controls/date-picker.png)

```xaml
<DatePicker Header="Arrival Date"/>
```

参考：[DatePicker](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker) 

设计和操作方法：[日历、日期和时间控件](date-and-time.md)
 
### <a name="time-picker"></a>时间选取器
使用户可以设置时间值的控件。

![TimePicker 控件](images/controls/time-picker.png) 

```xaml
<TimePicker Header="Arrival Time"/>
```

参考：[TimePicker](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) 

设计和操作方法：[日历、日期和时间控件](date-and-time.md)

## <a name="flyouts"></a>浮出控件

### <a name="context-menu"></a>上下文菜单
请参阅菜单浮出控件和弹出菜单。

### <a name="flyout"></a>浮出控件
显示要求用户交互的消息。 （与对话框不同，浮出控件不创建一个单独的窗口，而且不阻止其他用户交互。）

![浮出控件控制](images/controls/flyout.png)

```xaml
<Flyout>
    <StackPanel>
        <TextBlock Text="All items will be removed. Do you want to continue?"/>
        <Button Click="DeleteConfirmation_Click" Content="Yes, empty my cart"/>
    </StackPanel>
</Flyout>
```

参考：[浮出控件](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout) 

设计和操作方法：[浮出控件](dialogs-and-flyouts/flyouts.md) 

### <a name="menu-flyout"></a>菜单浮出控件
临时显示与用户当前正在执行的操作相关的命令或选项的列表。

![菜单浮出控件控制](images/controls/menu-flyout.png) 

```xaml
<MenuFlyout>
    <MenuFlyoutItem Text="Reset" Click="Reset_Click"/>
    <MenuFlyoutSeparator/>
    <ToggleMenuFlyoutItem Text="Shuffle" 
                          IsChecked="{Binding IsShuffleEnabled, Mode=TwoWay}"/>
    <ToggleMenuFlyoutItem Text="Repeat" 
                          IsChecked="{Binding IsRepeatEnabled, Mode=TwoWay}"/>
</MenuFlyout>
```

参考：[MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout)、[MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem)、[MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutSeparator)、[ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToggleMenuFlyoutItem) 

设计和操作方法：[菜单和上下文菜单](menus.md) 

示例代码：[XAML 上下文菜单示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

### <a name="popup-menu"></a>弹出菜单
一个自定义菜单，用于显示你所指定的命令。

参考：[PopupMenu](https://docs.microsoft.com/uwp/api/Windows.UI.Popups.PopupMenu) 

设计和操作方法：[对话框](dialogs-and-flyouts/dialogs.md) 

### <a name="tooltip"></a>工具提示
显示元素信息的弹出窗口。 
 
![工具提示控件](images/controls/tool-tip.png)

```xaml
<Button Content="Button" Click="Button_Click" 
        ToolTipService.ToolTip="Click to perform action" />
```

参考：[ToolTip](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTip)、[ToolTipService](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTipService) 

设计和操作方法：工具提示指南 

## <a name="images"></a>映像

### <a name="image"></a>映像
显示图像的控件。

```xaml
<Image Source="Assets/Logo.png" />
```

参考：[映像](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) 

设计和操作方法：[Image 和 ImageBrush](images-imagebrushes.md) 

示例代码：[XAML 图像示例](https://code.msdn.microsoft.com/windowsapps/0f5d56ae-5e57-48e1-9cd9-993115b027b9)

## <a name="graphics-and-ink"></a>图形和墨迹

### <a name="inkcanvas"></a>InkCanvas
接收和显示墨迹笔划的控件。

```xaml
<InkCanvas/>
```

参考：[InkCanvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 

### <a name="shapes"></a>形状
可以显示的各种保留模式图形对象，例如椭圆、矩形、直线、贝塞尔曲线等。

![多边形](images/controls/shapes-polygon.png) 
![路径](images/controls/shapes-path.png) 

```xaml
<Ellipse/>
<Path/>
<Rectangle/>
```

参考：[形状](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Shapes.Shape) 

如何执行此操作：[绘制形状](../../graphics/drawing-shapes.md) 

示例代码：[基于 XAML 矢量的绘图示例](https://code.msdn.microsoft.com/windowsapps/Drawing-bfc39296)

## <a name="layout-controls"></a>布局控件

### <a name="border"></a>边框
绘制另一对象周围的边框、背景或二者的容器控件。

![2 个矩形周围的边框](images/controls/border.png) 

```xaml
<Border BorderBrush="Blue" BorderThickness="4" 
        Height="108" Width="64" 
        Padding="8" CornerRadius="4">
    <Canvas>
        <Rectangle Fill="Orange"/>
        <Rectangle Fill="Green" Margin="0,44"/>
    </Canvas>
</Border>
```

参考：[Border](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border)

### <a name="canvas"></a>画布
一个布局面板，支持相对于画布左上角的子元素的绝对位置。
 
![画布布局面板](images/controls/canvas.png) 

```xaml
<Canvas Width="120" Height="120">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Canvas.Left="20" Canvas.Top="20"/>
    <Rectangle Fill="Green" Canvas.Left="40" Canvas.Top="40"/>
    <Rectangle Fill="Orange" Canvas.Left="60" Canvas.Top="60"/>
</Canvas>
```

参考：[画布](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas)
 
### <a name="grid"></a>网格
一个布局面板，支持以行和列方式排列子元素。

![网格布局面板](images/controls/grid.png) 

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="50"/>
        <RowDefinition Height="50"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="50"/>
        <ColumnDefinition Width="50"/>
    </Grid.ColumnDefinitions>
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Grid.Row="1"/>
    <Rectangle Fill="Green" Grid.Column="1"/>
    <Rectangle Fill="Orange" Grid.Row="1" Grid.Column="1"/>
</Grid>
```

参考：[网格](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)
 
### <a name="panning-scroll-viewer"></a>平移滚动查看器
请参阅滚动查看器。

### <a name="relativepanel"></a>RelativePanel
允许你放置子对象并使其相互对齐或与父面板对齐的面板。

![相对面板布局面板](images/controls/relative-panel.png) 

```xaml
<RelativePanel>
    <TextBox x:Name="textBox1" RelativePanel.AlignLeftWithPanel="True"/>
    <Button Content="Submit" RelativePanel.Below="textBox1"/>
</RelativePanel>
```

参考：[RelativePanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel)

### <a name="scroll-bar"></a>滚动条
请参阅“滚动查看器”。 （ScrollBar 是 ScrollViewer 的元素。 你通常不会将其用作独立控件。）

参考：[ScrollBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ScrollBar)
 
### <a name="scroll-viewer"></a>滚动查看器
可让用户平移和缩放其内容的容器控件。

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10" 
              HorizontalScrollMode="Enabled" 
              HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

参考：[ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)

设计和操作方法：[滚动和平移控件指南](scroll-controls.md) 

示例代码：[XAML 滚动、平移以及缩放示例](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)

### <a name="stack-panel"></a>堆栈面板
可以将子元素按水平或垂直方向排列到单行中的布局面板。

![堆叠面板布局控件](images/controls/stack-panel.png) 

```xaml
<StackPanel>
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue"/>
    <Rectangle Fill="Green"/>
    <Rectangle Fill="Orange"/>
</StackPanel>
```

参考：[StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel)
 
### <a name="variablesizedwrapgrid"></a>VariableSizedWrapGrid
一个布局面板，支持以行和列方式排列子元素。 每个子元素都可以跨越多行和多列。

![大小可变的包围式网格布局面板](images/controls/variable-sized-wrap-grid.png) 

```xaml
<VariableSizedWrapGrid MaximumRowsOrColumns="3" ItemHeight="44" ItemWidth="44">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Height="80" 
               VariableSizedWrapGrid.RowSpan="2"/>
    <Rectangle Fill="Green" Width="80" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
    <Rectangle Fill="Orange" Height="80" Width="80" 
               VariableSizedWrapGrid.RowSpan="2" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
</VariableSizedWrapGrid>
```

参考：[VariableSizedWrapGrid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid)

### <a name="viewbox"></a>Viewbox
可以将其内容缩放至指定大小的容器控件。

![Viewbox 控件](images/controls/view-box.png) 

```xaml
<Viewbox MaxWidth="25" MaxHeight="25">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
<Viewbox MaxWidth="75" MaxHeight="75">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
<Viewbox MaxWidth="150" MaxHeight="150">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
```

参考：[Viewbox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Viewbox)
 
### <a name="zooming-scroll-viewer"></a>缩放滚动查看器
请参阅滚动查看器。

## <a name="media-controls"></a>媒体控件

### <a name="audio"></a>音频
请参阅媒体元素。

### <a name="media-element"></a>媒体元素
可播放音频和视频内容的控件。

```xaml
<MediaElement x:Name="myMediaElement"/>
```

参考：[MediaElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 

设计和操作方法：[媒体元素控件指南](media-playback.md)

### <a name="mediatransportcontrols"></a>MediaTransportControls
可为 MediaElement 提供播放控件的控件。

![具有传输控件的媒体元素](images/controls/media-transport-controls.png) 

```xaml
<MediaTransportControls MediaElement="myMediaElement"/>
```

参考：[MediaTransportControls](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) 

设计和操作方法：[媒体元素控件指南](media-playback.md) 

示例代码：[媒体传输控件示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCustomMediaTransportControls)

### <a name="video"></a>视频
请参阅媒体元素。

## <a name="navigation"></a>导航

### <a name="navigationview"></a>NavigationView

一个自适应的容器和灵活的导航模型，可以实现左侧导航窗格、顶部导航和选项卡模式。

参考：[NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)

设计和操作方法：[NavigationView 控件指南](navigationview.md)

### <a name="splitview"></a>SplitView

具有两个视图的容器控件；一个视图用于主内容，另一个视图通常用于导航菜单。

![拆分视图控件](images/controls/split-view.png) 

```xaml
<SplitView>
    <SplitView.Pane>
        <!-- Menu content -->
    </SplitView.Pane>
    <SplitView.Content>
        <!-- Main content -->
    </SplitView.Content>
</SplitView>
```

参考：[SplitView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SplitView) 

设计和操作方法：[拆分视图控件指南](split-view.md)

### <a name="web-view"></a>Web 视图

可托管 Web 内容的容器控件。

```xaml
<WebView x:Name="webView1" Source="https://developer.microsoft.com" 
         Height="400" Width="800"/>
```

参考：[WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) 

设计和操作方法：Web 视图指南 

示例代码：[XAML WebView 控件示例](https://code.msdn.microsoft.com/windowsapps/XAML-WebView-control-sample-58ad63f7)

### <a name="semantic-zoom"></a>语义式缩放

让用户在项目集合的两个视图之间缩放的容器控件。

```xaml
<SemanticZoom>
    <ZoomedInView>
        <GridView></GridView>
    </ZoomedInView>
    <ZoomedOutView>
        <GridView></GridView>
    </ZoomedOutView>
</SemanticZoom>
```

参考：[SemanticZoom](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 

设计和操作方法：[语义式缩放控件指南](semantic-zoom.md)

示例代码：[XAML GridView 组和 SemanticZoom 示例](https://code.msdn.microsoft.com/windowsapps/groupedgridview-77c59e8e)

## <a name="progress-controls"></a>进度控件

### <a name="progress-bar"></a>进度条
通过显示一个长条来指示进度的控件。

![进度条控件](images/controls/progress-bar-determinate.png)

显示特定值的进度条。

```xaml
<ProgressBar x:Name="progressBar1" Value="50" Width="100"/>
```

![不确定进度条控件](images/controls/progress-bar-indeterminate.png)

显示不确定进度的进度条。

```xaml
<ProgressBar x:Name="indeterminateProgressBar1" IsIndeterminate="True" Width="100"/>
```

参考：[ProgressBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) 

设计和操作方法：[进度控件指南](progress-controls.md) 

### <a name="progress-ring"></a>进度环
通过显示一个 环来指示不确定进度的控件。 

![进度环控件](images/controls/progress-ring.png) 

```xaml
<ProgressRing x:Name="progressRing1" IsActive="True"/>
```

参考：[ProgressRing](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) 

设计和操作方法：[进度控件指南](progress-controls.md) 

## <a name="text-controls"></a>文本控件

### <a name="auto-suggest-box"></a>自动建议框
提供建议文本作为用户键入的文本输入框。

![用于搜索的自动建议框](images/controls/auto-suggest-box.png) 

参考：[AutoSuggestBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox)

设计和操作方法：[文本控件](text-controls.md)、[自动建议框控件指南](auto-suggest-box.md)

示例代码：[AutoSuggestBox 迁移示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

### <a name="multi-line-text-box"></a>多行文本框
请参阅文本框。

### <a name="password-box"></a>密码框
用于输入密码的一种控件。

 ![密码框](images/controls/password-box.png)

```xaml
<PasswordBox x:Name="passwordBox1" 
             PasswordChanged="PasswordBox_PasswordChanged" />
```

参考：[PasswordBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox) 

设计和操作方法：[文本控件](text-controls.md)、[密码框控件指南](password-box.md) 

示例代码：[XAML 文本显示示例](https://code.msdn.microsoft.com/windowsapps/XAML-text-display-sample-2593ba0a)、[XAML 文本编辑示例](https://code.msdn.microsoft.com/windowsapps/XAML-text-editing-sample-fb0493ad)

### <a name="rich-edit-box"></a>富编辑框
使用户可以编辑带有格式化文本、超链接和图像等内容的富文本文档的控件。

```xaml
<RichEditBox />
```

参考：[RichEditBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) 

设计和操作方法：[文本控件](text-controls.md)、[可编辑对话框控件指南](rich-edit-box.md)

示例代码：[XAML 文本示例](https://code.msdn.microsoft.com/windowsapps/XAML-text-display-sample-2593ba0a)

### <a name="search-box"></a>搜索框
请参阅自动建议框

### <a name="single-line-text-box"></a>单行文本框
请参阅文本框。

### <a name="static-textparagraph"></a>静态文本/段落
请参阅文本块。

### <a name="text-block"></a>文本块
显示文本的控件。

![文本块控件](images/controls/text-block.png) 

```xaml
<TextBlock x:Name="textBlock1" Text="I am a TextBlock"/>
```

参考：[TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)、[RichTextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) 

设计和操作方法：[文本控件](text-controls.md)、[文本块控件指南](text-block.md)、[RTF 块控件指南](rich-text-block.md)

示例代码：[XAML 文本示例](https://code.msdn.microsoft.com/windowsapps/XAML-text-display-sample-2593ba0a)

### <a name="text-box"></a>文本框
单行或多行纯文本字段。

![文本框控件](images/controls/text-box.png)

```xaml
<TextBox x:Name="textBox1" Text="I am a Text Box."
         TextChanged="TextBox_TextChanged"/>
```

参考：[TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) 

设计和操作方法：[文本控件](text-controls.md)、[文本框控件指南](text-box.md) 

示例代码：[XAML 文本示例](https://code.msdn.microsoft.com/windowsapps/XAML-text-display-sample-2593ba0a)

## <a name="selection-controls"></a>选择控件

### <a name="check-box"></a>复选框
用户可以选中或清除的一种控件。

![3 种状态的复选框](images/templates-checkbox-states-default.png)

```xaml
<CheckBox x:Name="checkbox1" Content="CheckBox" 
          Checked="CheckBox_Checked"/>
```

参考：[CheckBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox) 

设计和操作方法：[复选框控件指南](checkbox.md) 

### <a name="combo-box"></a>组合框
用户可以从中进行选择的项目下拉列表。

![开放式组合框](images/controls/combo-box-open.png) 

```xaml
<ComboBox x:Name="comboBox1" Width="100"
          SelectionChanged="ComboBox_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
</ComboBox>
```

参考：[ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 

设计和操作方法：[列表](lists.md) 

### <a name="list-box"></a>列表框
显示用户可以从中进行选择的嵌入式项列表的控件。 

![列表框控件](images/controls/list-box.png)

```xaml
<ListBox x:Name="listBox1" Width="100"
         SelectionChanged="ListBox_SelectionChanged">
    <x:String>List item 1</x:String>
    <x:String>List item 2</x:String>
    <x:String>List item 3</x:String>
</ListBox>
```

参考：[ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) 

设计和操作方法：[列表](lists.md) 

### <a name="radio-button"></a>单选按钮
允许用户从一组选项中选择单个选项的控件。 将单选按钮分组到一起时，它们互相排斥。

![单选按钮控件](images/controls/radio-button.png)

```xaml
<RadioButton x:Name="radioButton1" Content="RadioButton 1" GroupName="Group1" 
             Checked="RadioButton_Checked"/>
<RadioButton x:Name="radioButton2" Content="RadioButton 2" GroupName="Group1" 
             Checked="RadioButton_Checked" IsChecked="True"/>
<RadioButton x:Name="radioButton3" Content="RadioButton 3" GroupName="Group1" 
             Checked="RadioButton_Checked"/>
```

参考：[RadioButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton) 

设计和操作方法：[单选按钮控件指南](radio-button.md)
 
### <a name="slider"></a>滑块
一种可让用户通过沿轨迹移动 Thumb 控件从一组值中进行选择的控件。

![滑块控件](images/controls/slider.png)

```xaml
<Slider x:Name="slider1" Width="100" ValueChanged="Slider_ValueChanged" />
```

参考：[滑块](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) 

设计和操作方法：[滑块控件指南](slider.md) 

### <a name="toggle-button"></a>切换按钮
可以在 2 种状态之间进行切换的一种按钮。

```xaml
<ToggleButton x:Name="toggleButton1" Content="Button" 
              Checked="ToggleButton_Checked"/>
```

参考：[ToggleButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton)

设计和操作方法：[切换控制指南](toggles.md) 

### <a name="toggle-switch"></a>切换开关
可以在 2 种状态之间进行切换的一种开关。

![切换开关控件](images/controls/toggle-switch.png) 

```xaml
<ToggleSwitch x:Name="toggleSwitch1" Header="ToggleSwitch" 
              OnContent="On" OffContent="Off" 
              Toggled="ToggleSwitch_Toggled"/>
```

参考：[ToggleSwitch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToggleSwitch) 

设计和操作方法：[切换控制指南](toggles.md) 

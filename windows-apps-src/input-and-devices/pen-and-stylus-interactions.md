---
生成支持使用笔和触笔设备进行自定义交互（包括用于自然书写和绘图体验的数字墨迹）的通用 Windows 平台 (UWP) 应用。
笔和触笔交互
ms.assetid: 3DA4F2D2-5405-42A1-9ED9-3A87BCD84C43
笔和触笔
template: detail.hbs
---

# 笔和触笔交互


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

针对触摸输入优化通用 Windows 平台 (UWP) 应用设计，并在默认情况下获得基本的笔支持。

UWP 应用墨迹平台与笔设备一起提供了一种创建手写便笺、绘图和批注的自然方法。 该平台支持捕获通过数字化器输入的墨迹数据、生成墨迹数据、在输出设备上以墨迹笔划的形式呈现这些数据、管理墨迹数据以及执行手写识别。

![触摸板](images/input-patterns/input-pen.jpg)


**重要的 API**

-   [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
-   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)
-   [**Windows.UI.Input.Inking.Core**](https://msdn.microsoft.com/library/windows/apps/dn958452)


除了用作[**指针设备**](https://msdn.microsoft.com/library/windows/apps/br225633)（类似于鼠标和触摸）之外，笔/触笔通常与使用数字墨迹直接在屏幕上书写和绘图相关联。

通用 Windows 平台 (UWP) 墨迹平台与笔/触笔设备一起提供了一种创建数字手写便笺、绘图和批注的自然方法。 该平台支持将数字化器输入捕获为墨迹数据、生成墨迹数据、管理墨迹数据、以笔划形式呈现墨迹数据以及通过手写识别将墨迹转换为文本。

除了捕获用户在书写或绘图时笔的基本位置和移动外，你的应用还可以跟踪和收集笔划前后使用的不同程度的压力。 此信息连同设置（针对笔尖形状、大小和旋转）、墨迹颜色以及用途（普通墨迹、擦除、突出显示和选择）一起支持你提供非常接近于在纸上使用钢笔、铅笔或画笔书写或绘图的用户体验。

**注意** 你的应用还支持其他基于指针的设备的墨迹输入，包括触摸数字化器和鼠标设备。

 

墨迹平台具有很高的灵活性。 它旨在根据不同要求支持各类功能级别。

墨迹平台具有三个组件：

-   [
            **InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) - XAML UI 平台控件，默认情况下将所有笔输入接收和显示为笔划墨迹或擦除笔划。

-   [
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) - 代码隐藏对象，与 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 控件（通过 [**InkCanvas.InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 属性公开）一起进行实例化。 此对象提供 **InkCanvas** 公开的所有默认墨迹书写功能以及适用于其他自定义和个性化的完整 API 集。

-   [
            **IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263) - 支持将笔划墨迹呈现到通用 Windows 应用的指定 Direct2D 设备上下文，而非默认 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 控件。 这支持完全自定义墨迹书写体验。

## <span id="inkcanvas"> </span> <span id="INKCANVAS"> </span>通过 InkCanvas 实现基本墨迹书写


对于基本的墨迹书写功能，只需将 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 放置在页面上的任意位置即可。

[
            **InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 仅支持来自笔的墨迹输入。 该输入将通过颜色和厚度的默认设置呈现为墨迹笔划，或视为笔划橡皮擦（当输入来源于橡皮擦尖或使用擦除按钮修改的笔尖时）。

在此示例中，[**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 覆盖了背景图。

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10,0,0,0" />            
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

该系列图像显示了 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 控件如何呈现笔输入。

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><img src="images/ink_basic_1_small.png" alt="The blank InkCanvas with a background image" /></td>
<td align="left"><img src="images/ink_basic_2_small.png" alt="The InkCanvas with ink strokes" /></td>
<td align="left"><img src="images/ink_basic_3_small.png" alt="The InkCanvas with one stroke erased" /></td>
</tr>
<tr class="even">
<td align="left">带有背景图像的空白 [<strong>InkCanvas</strong>] (https://msdn.microsoft.com/library/windows/apps/dn858535)。</td>
<td align="left">带有笔划墨迹的 [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535)。</td>
<td align="left">擦除一个笔划的 [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535)。
<p>注意如何在整条笔划而非某个部分上执行擦除。</p></td>
</tr>
</tbody>
</table>

 

[
            **InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 控件支持的墨迹书写功能由名为 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) 的代码隐藏对象提供。

对于基本墨迹书写，你不必考虑 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011)。 但是，若要在 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 上自定义和配置墨迹书写行为，则必须访问其相应的 **InkPresenter** 对象。

## <span id="inkpresenter"> </span> <span id="INKPRESENTER"> </span>通过 InkPresenter 实现基本自定义


[
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) 对象通过每个 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 控件进行实例化。

[
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) 不仅提供了其相应 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 控件的所有默认墨迹书写行为，它还提供了完整 API 集用于进行其他笔划自定义。 这包括笔划属性、支持的输入设备类型以及输入是由对象进行处理还是传递到应用。

**注意**  
[
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) 不能直接进行实例化。 而是通过 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 的 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 属性进行访问。

 

我们在此处将 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 配置为将来自笔和鼠标的输入数据解释为笔划墨迹。 我们还将设置一些用于将笔划呈现到 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 的初始墨迹笔划属性。

```CSharp
public MainPage()
{
    this.InitializeComponent();

    // Set supported inking device types.
    inkCanvas.InkPresenter.InputDeviceTypes = 
        Windows.UI.Core.CoreInputDeviceTypes.Mouse | 
        Windows.UI.Core.CoreInputDeviceTypes.Pen;

    // Set initial ink stroke attributes.
    InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
    drawingAttributes.Color = Windows.UI.Colors.Black;
    drawingAttributes.IgnorePressure = false;
    drawingAttributes.FitToCurve = true;
    inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);
}
```

可以动态设置笔划墨迹属性以适应用户首选项或应用要求。

此处，我们让用户从墨迹颜色列表中进行选择。

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header" 
                   Text="Basic ink customization sample" 
                   VerticalAlignment="Center"
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10,0,0,0" />
        <TextBlock Text="Color:"
                   Style="{StaticResource SubheaderTextBlockStyle}"
                   VerticalAlignment="Center"
                   Margin="50,0,10,0"/>
        <ComboBox x:Name="PenColor"
                  VerticalAlignment="Center"
                  SelectedIndex="0"
                  SelectionChanged="OnPenColorChanged">
            <ComboBoxItem Content="Black"/>
            <ComboBoxItem Content="Red"/>
        </ComboBox>
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

然后处理对所选颜色的更改并相应地更新墨迹笔划属性。

```CSharp
// Update ink stroke color for new strokes.
private void OnPenColorChanged(object sender, SelectionChangedEventArgs e)
{
    if (inkCanvas != null)
    {
        InkDrawingAttributes drawingAttributes = 
            inkCanvas.InkPresenter.CopyDefaultDrawingAttributes();

        string value = ((ComboBoxItem)PenColor.SelectedItem).Content.ToString();

        switch (value)
        {
            case "Black":
                drawingAttributes.Color = Windows.UI.Colors.Black;
                break;
            case "Red":
                drawingAttributes.Color = Windows.UI.Colors.Red;
                break;
            default:
                drawingAttributes.Color = Windows.UI.Colors.Black;
                break;
        };

        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);
    }
}
```

这些图像显示了 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 如何处理和自定义笔输入。

|                                                                                      |                                                                                          |
|--------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| ![具有默认的黑色笔划墨迹的 inkcanvas](images/ink-basic-custom-1-small.png) | ![具有用户选择的红色笔划墨迹的 inkcanvas](images/ink-basic-custom-2-small.png) |
| 具有默认的黑色笔划墨迹的 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)        | 具有用户选择的红色墨迹笔划的 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)。        |

 

若要提供墨迹书写和擦除之外的功能（例如笔划选择），你的应用必须为 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 标识特定输入，以在未处理的情况下进行传递以供应用处理。

## <span id="passthrough"> </span> <span id="PASSTHROUGH"> </span>传递输入以进行高级处理


默认情况下，[**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 将所有输入作为笔划墨迹或擦除笔划进行处理。 这包括由辅助硬件提供（例如笔桶按钮、鼠标右键按钮或类似提供）修改的输入。

在使用这些辅助提供时，用户通常期望提供一些其他功能或修改的行为。

在某些情况下，你可能需要公开未使用辅助提供（通常不会与笔尖关联的功能）时笔的基本墨迹功能、其他输入设备类型、或其他功能或修改的行为，具体取决于用户在应用 UI 中的选择。

若要支持此要求，可将 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 配置为保留特定输入的未处理状态。 这未处理的输入随后将传递到你的应用以进行处理。

以下代码示例分步演示了当输入通过笔桶按钮（或鼠标右键按钮）修改时，如何启用笔划选择。

对于此示例，我们使用 MainPage.xaml 和 MainPage.xaml.cs 文件托管所有代码。

1.  首先，我们在 MainPage.xaml 中设置 UI。

    我们在此处添加一个用于绘制选择笔划的画布（在 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 下面）。 使用单独的图层绘制选择笔划可使 **InkCanvas** 及其内容不会受到影响。

    ![带有基础选择画布的空白 inkcanvas](images/ink-unprocessed-1-small.png)

```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Advanced ink customization sample" 
                       VerticalAlignment="Center"
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
        </StackPanel>
        <Grid Grid.Row="1">
            <!-- Canvas for displaying selection UI. -->
            <Canvas x:Name="selectionCanvas"/>
            <!-- Inking area -->
            <InkCanvas x:Name="inkCanvas"/>
        </Grid>
    </Grid>
```

2.  在 MainPage.xaml.cs 中，我们声明了几个全局变量来保持对选择 UI 各个方面的引用。 具体内容有选择套索笔划和突出显示所选笔划的边界矩形。

```    CSharp
// Stroke selection tool.
    private Polyline lasso;
    // Stroke selection area.
    private Rect boundingRect;
```

3.  接下来，我们配置 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 以将来自笔和鼠标的输入数据解释为墨迹笔划，然后设置一些用于将笔划呈现到 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 的初始墨迹笔划属性。

    最重要的是，我们使用 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 的 [**InputProcessingConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948764) 属性指示任何修改的输入均应由应用进行处理。 通过向 **InputProcessingConfiguration.RightDragAction** 分配值 [**InkInputRightDragAction.LeaveUnprocessed**](https://msdn.microsoft.com/library/windows/apps/dn948760) 指定修改的输入。

    然后，我们为由 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 传递的未经处理的 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn914712)、[**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn914711) 和 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn914713) 事件分配侦听器。 所有选择功能均在这些事件的处理程序中实现。

    最后，我们为 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 的 [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702) 和 [**StrokesErased**](https://msdn.microsoft.com/library/windows/apps/dn948767) 事件分配侦听器。 如果启动了新笔划或擦除了现有笔划，我们将使用这些事件的处理程序清理选择 UI。

    ![具有默认的黑色笔划墨迹的 inkcanvas](images/ink-unprocessed-2-small.png)

```    CSharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // By default, the InkPresenter processes input modified by 
        // a secondary affordance (pen barrel button, right mouse 
        // button, or similar) as ink.
        // To pass through modified input to the app for custom processing 
        // on the app UI thread instead of the background ink thread, set 
        // InputProcessingConfiguration.RightDragAction to LeaveUnprocessed.
        inkCanvas.InkPresenter.InputProcessingConfiguration.RightDragAction = 
            InkInputRightDragAction.LeaveUnprocessed;

        // Listen for unprocessed pointer events from modified input.
        // The input is used to provide selection functionality.
        inkCanvas.InkPresenter.UnprocessedInput.PointerPressed += 
            UnprocessedInput_PointerPressed;
        inkCanvas.InkPresenter.UnprocessedInput.PointerMoved += 
            UnprocessedInput_PointerMoved;
        inkCanvas.InkPresenter.UnprocessedInput.PointerReleased += 
            UnprocessedInput_PointerReleased;

        // Listen for new ink or erase strokes to clean up selection UI.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted += 
            StrokeInput_StrokeStarted;
        inkCanvas.InkPresenter.StrokesErased += 
            InkPresenter_StrokesErased;
    }
```

4.  然后，我们为由 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 传递的未经处理的 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn914712)、[**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn914711) 和 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn914713) 事件定义处理程序。

    所有选择功能（包括套索笔划和边界矩形）均在这些处理程序中实现。

    ![选择套索](images/ink-unprocessed-3-small.png)

```    CSharp
// Handle unprocessed pointer events from modifed input.
    // The input is used to provide selection functionality.
    // Selection UI is drawn on a canvas under the InkCanvas.
    private void UnprocessedInput_PointerPressed(
        InkUnprocessedInput sender, PointerEventArgs args)
    {
        // Initialize a selection lasso.
        lasso = new Polyline()
        {
            Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
            StrokeThickness = 1,
            StrokeDashArray = new DoubleCollection() { 5, 2 },
        };

        lasso.Points.Add(args.CurrentPoint.RawPosition);

        selectionCanvas.Children.Add(lasso);
    }

    private void UnprocessedInput_PointerMoved(
        InkUnprocessedInput sender, PointerEventArgs args)
    {
        // Add a point to the lasso Polyline object.
        lasso.Points.Add(args.CurrentPoint.RawPosition);
    }

    private void UnprocessedInput_PointerReleased(
        InkUnprocessedInput sender, PointerEventArgs args)
    {
        // Add the final point to the Polyline object and 
        // select strokes within the lasso area.
        // Draw a bounding box on the selection canvas 
        // around the selected ink strokes.
        lasso.Points.Add(args.CurrentPoint.RawPosition);

        boundingRect = 
            inkCanvas.InkPresenter.StrokeContainer.SelectWithPolyLine(
                lasso.Points);

        DrawBoundingRect();
    }
```

5.  为结束 PointerReleased 事件处理程序，我们将清除所有内容（套索笔划）的选择图层，然后在套索区域包含的墨迹笔划周围绘制一个边界矩形。

    ![选择边界矩形](images/ink-unprocessed-4-small.png)

```    CSharp
// Draw a bounding rectangle, on the selection canvas, encompassing 
    // all ink strokes within the lasso area.
    private void DrawBoundingRect()
    {
        // Clear all existing content from the selection canvas.
        selectionCanvas.Children.Clear();

        // Draw a bounding rectangle only if there are ink strokes 
        // within the lasso area.
        if (!((boundingRect.Width == 0) || 
            (boundingRect.Height == 0) || 
            boundingRect.IsEmpty))
        {
            var rectangle = new Rectangle()
            {
                Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                StrokeThickness = 1,
                StrokeDashArray = new DoubleCollection() { 5, 2 },
                Width = boundingRect.Width,
                Height = boundingRect.Height
            };

            Canvas.SetLeft(rectangle, boundingRect.X);
            Canvas.SetTop(rectangle, boundingRect.Y);

            selectionCanvas.Children.Add(rectangle);
        }
    }
```

6.  最后，我们将定义 [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702) 和 [**StrokesErased**](https://msdn.microsoft.com/library/windows/apps/dn948767) InkPresenter 事件的处理程序。

    这二者都可以在检测到新笔划时调用相同的清理功能以清除当前选择。

```    CSharp
// Handle new ink or erase strokes to clean up selection UI.
    private void StrokeInput_StrokeStarted(
        InkStrokeInput sender, Windows.UI.Core.PointerEventArgs args)
    {
        ClearSelection();
    }

    private void InkPresenter_StrokesErased(
        InkPresenter sender, InkStrokesErasedEventArgs args)
    {
        ClearSelection();
    }
```

7.  以下是在启动新笔划或擦除现有笔划时，从选择画布中删除所有选择 UI 的功能。

```    CSharp
// Clean up selection UI.
    private void ClearSelection()
    {
        var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        foreach (var stroke in strokes)
        {
            stroke.Selected = false;
        }
        ClearDrawnBoundingRect();
    }

    private void ClearDrawnBoundingRect()
    {
        if (selectionCanvas.Children.Any())
        {
            selectionCanvas.Children.Clear();
            boundingRect = Rect.Empty;
        }
    }
```

## <span id="iinkd2drenderer"> </span> <span id="IINKD2DRENDERER"> </span>自定义墨迹呈现


默认情况下，墨迹输入在低延迟后台线程上进行处理，并在绘制时呈现“墨迹未干”。 笔划完成时（抬起笔或手指，或者释放鼠标按钮），笔划将在 UI 线程上进行处理并向 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 图层呈现“墨迹已干”（在应用程序内容之上，并且替换未干墨迹）。

墨迹平台支持你替代此行为并通过自定义烘干墨迹输入来完全自定义墨迹书写体验。

自定义烘干需要 [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263) 对象管理墨迹输入，并将其呈现到通用 Windows 应用的 Direct2D 设备上下文，而非默认 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 控件。

通过调用 [**ActivateCustomDrying**](https://msdn.microsoft.com/library/windows/apps/dn922012)（在加载 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 之前），应用创建 [**InkSynchronizer**](https://msdn.microsoft.com/library/windows/apps/dn903979) 对象以自定义如何向 [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) 或 [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) 呈现墨迹已干的笔划墨迹。 例如，笔划墨迹可以光栅化并集成到应用程序内容中，而非作为单独的 **InkCanvas** 图层。

有关该功能的完整示例，请参阅[复杂墨迹示例](http://go.microsoft.com/fwlink/p/?LinkID=620314)。


## 本部分中的其他文章 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Recognize ink strokes](convert-ink-to-text.md)</p></td>
<td align="left"><p>使用手写识别将笔划墨迹转换为文本，或使用自定义识别转换为形状。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Store and retrieve ink strokes](save-and-load-ink.md)</p></td>
<td align="left"><p>使用嵌入的墨迹序列化格式 (ISF) 元数据在图形交换格式 (GIF) 文件中存储笔划墨迹数据。</p></td>
</tr>
</tbody>
</table>

 


## <span id="related_topics"> </span>相关文章


* [处理指针输入](handle-pointer-input.md)
* [标识输入设备](identify-input-devices.md)
**示例**
* [墨迹示例](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [简单墨迹示例](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [复杂墨迹示例](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [基本输入示例](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延迟输入示例](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [用户交互模式示例](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [焦点视觉示例](http://go.microsoft.com/fwlink/p/?LinkID=619895)
**存档示例**
* [输入：设备功能示例](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [输入：XAML 用户输入事件示例](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [XAML 滚动、平移以及缩放示例](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [输入：使用 GestureRecognizer 的笔势和操作](http://go.microsoft.com/fwlink/p/?LinkID=231605)
 

 






<!--HONumber=Mar16_HO1-->



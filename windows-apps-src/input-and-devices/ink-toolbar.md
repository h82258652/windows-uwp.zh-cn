---
author: Karl-Bridge-Microsoft
Description: "将默认的 InkToolbar 添加到通用 Windows 平台 (UWP) 墨迹书写应用、将自定义笔按钮添加到 InkToolbar，并将自定义笔按钮绑定到自定义笔定义。"
title: "将 InkToolbar 添加到通用 Windows 平台 (UWP) 墨迹书写应用"
label: Add an InkToolbar to a Universal Windows Platform (UWP) inking app
template: detail.hbs
keyword: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas, InkToolbar, Universal Windows Platform, UWP
translationtype: Human Translation
ms.sourcegitcommit: 71b73605bab71dad36977d0506c090c34359a3e2
ms.openlocfilehash: c4a5b0ae2893fda7697457b9e7449a996707de4b

---

# 将 InkToolbar 添加到通用 Windows 平台 (UWP) 墨迹书写应用

有两种不同的控件可促进通用 Windows 平台 (UWP) 应用中的墨迹书写：[**InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) 和 [**InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)。

[**InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) 控件提供基本 Windows Ink 功能。 使用它将笔输入呈现为笔划墨迹（使用颜色和粗细的默认设置）或擦除笔划。

> 有关 InkCanvas 实现的详细信息，请参阅 [UWP 应用中的笔和触笔交互](pen-and-stylus-interactions.md)。

作为完全透明的覆盖层，InkCanvas 不提供任何用于设置笔划墨迹属性的内置 UI。 如果你希望更改默认的墨迹书写体验，请让用户设置笔划墨迹属性，并支持其他自定义墨迹书写功能，你有两个选项：

- 在代码隐藏中，请使用绑定到 InkCanvas 的 [**InkPresenter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx) 对象。

  InkPresenter API 支持墨迹书写体验的广泛自定义。 有关更多详细信息，请参阅 [UWP 应用中的笔和触笔交互](pen-and-stylus-interactions.md)。

- 将 [**InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) 绑定到 InkCanvas。 默认情况下，InkToolbar 提供用于激活墨迹功能和设置墨迹属性（如笔划大小、墨迹颜色和笔尖形状）的基本 UI。

  我们将在本主题中讨论 InkToolbar。

## 重要的 API

  -   [**InkCanvas 类**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx)
  -   [**InkToolbar 类**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)
  -   [**InkPresenter 类**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx)
  -   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)


## 默认 InkToolbar

默认情况下，InkToolbar 包括用于绘制、擦除、突出显示和显示标尺的按钮。 根据功能，在浮出控件中提供其他设置和命令，如墨迹颜色、笔划粗细、擦除所有墨迹。

![InkToolbar](.\images\ink\ink-tools-invoked-toolbar-small.png)  
*默认的 Windows Ink 工具栏*

若要添加基本的默认 InkToolbar，请执行以下操作：
1. 在 MainPage.xaml 中，为墨迹书写图面声明一个容器对象（对于此示例，我们使用 Grid 控件）。
2. 声明一个 InkCanvas 对象作为容器的子项。 （InkCanvas 大小继承自该容器。）
3. 声明一个 InkToolbar 并使用 TargetInkCanvas 属性将其绑定到 InkCanvas。
  确保在 InkCanvas 之后声明 InkToolbar。 否则，InkCanvas 覆盖层会使 InkToolbar 不可访问。

```xaml
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
        <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
    </Grid>
</Grid>
```

## 基本自定义

在此部分中，我们将介绍一些基本的 Windows Ink 工具栏自定义方案。

### 指定所选的按钮  
![在初始化时选择的铅笔按钮](.\images\ink\ink-tools-default-toolbar.png)  
*带有在初始化时选择的铅笔按钮的 Windows Ink 工具栏*

默认情况下，当你的应用启动并且工具栏初始化时，将选择第一个（或最左边的）按钮。 在默认的 Windows Ink 工具栏中，这是圆珠笔按钮。

由于该框架定义了内置按钮的顺序，因此第一个按钮可能不是你默认要激活的笔或工具。

你可以替代此默认行为，并在工具栏上指定所选的按钮。

对于此示例，我们初始化将选择铅笔按钮并激活铅笔（而不是圆珠笔）的默认工具栏。

1. 为上一个示例中的 InkCanvas 和 InkToolbar 使用 XAML 声明。
2. 在代码隐藏中，为 [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) 对象的 [Loaded](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx) 事件设置处理程序。

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loaded += inkToolbar_Loaded;
  }
  ```

3. 在 [Loaded](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx) 事件的处理程序中：
  1. 获取对内置 [InkToolbarPencilButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) 的引用。

    传递 [GetToolButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.gettoolbutton.aspx) 方法中的 [InkToolbarTool.Pencil](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbartool.aspx) 对象会为 [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) 返回 [InkToolbarToolButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbartoolbutton.aspx) 对象。

  2. 将 [ActiveTool](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.activetool.aspx) 设置为上一步中返回的对象。

```CSharp
/// <summary>
/// Handle the Loaded event of the InkToolbar.
/// By default, the active tool is set to the first tool on the toolbar.
/// Here, we set the active tool to the pencil button.
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void inkToolbar_Loaded(object sender, RoutedEventArgs e)
{
    InkToolbarToolButton pencilButton = inkToolbar.GetToolButton(InkToolbarTool.Pencil);
    inkToolbar.ActiveTool = pencilButton;
}
```

### 指定内置按钮

![指定在初始化时包含的按钮](.\images\ink\ink-tools-specific.png)  
*指定在初始化时包含的按钮*

如上所示，Windows Ink 工具栏包含默认内置按钮的集合。 这些按钮按照以下顺序（从左到右）显示：

- [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx)
- [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx)
- [InkToolbarHighlighterButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarhighlighterbutton.aspx)
- [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx)
- [InkToolbarRulerButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarrulerbutton.aspx)

对于此示例，我们仅使用内置圆珠笔、铅笔和橡皮擦按钮初始化工具栏。

使用 XAML 或代码隐藏执行此操作。

**XAML**

为第一个示例中的 InkCanvas 和 InkToolbar 修改 XAML 声明。
- 添加 [InitialControls](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx) 属性并将其值设置为“[None](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)”。 这将清除内置按钮的默认集合。
- 添加你的应用所需的特定 InkToolbar 按钮。 此处，我们将仅添加 [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx)、[InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) 和 [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx)。
> [!NOTE]
> 按钮将按照框架定义的顺序（而不是此处指定的顺序）添加到工具栏。

```xaml
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
        <!-- Clear the default InkToolbar buttons by setting InitialControls to None. -->
        <!-- Set the active tool to the pencil button. -->
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
                    VerticalAlignment="Top"
                    TargetInkCanvas="{x:Bind inkCanvas}"
                    InitialControls="None">
            <!--
             Add only the ballpoint pen, pencil, and eraser.
             Note that the buttons are added to the toolbar in the order
             defined by the framework, not the order we specify here.
            -->
            <InkToolbarEraserButton />
            <InkToolbarBallpointPenButton />
            <InkToolbarPencilButton/>
        </InkToolbar>
    </Grid>
</Grid>
```

**代码隐藏**
1. 为第一个示例中的 InkCanvas 和 InkToolbar 使用 XAML 声明。

  ```xaml
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
          <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
      </Grid>
  </Grid>
  ```

2. 在代码隐藏中，为 [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) 对象的 [Loading](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loading.aspx) 事件设置处理程序。

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loading += inkToolbar_Loading;
  }
  ```

3. 将 [InitialControls](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx) 设置为“[None](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)”。
4. 为你的应用所需的按钮创建对象引用。 此处，我们将仅添加 [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx)、[InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) 和 [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx)。
  > [!NOTE]
  > 按钮将按照框架定义的顺序（而不是此处指定的顺序）添加到工具栏。

5. 将这些按钮[添加](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.dependencyobjectcollection.add.aspx)到 InkToolbar。

  ```csharp
  /// <summary>
  /// Handles the Loading event of the InkToolbar.
  /// Here, we identify the buttons to include on the InkToolbar.
  /// </summary>
  /// <param name="sender">The InkToolbar</param>
  /// <param name="args">The InkToolbar event data.
  /// If there is no event data, this parameter is null</param>
  private void inkToolbar_Loading(FrameworkElement sender, object args)
  {
      // Clear all built-in buttons from the InkToolbar.
      inkToolbar.InitialControls = InkToolbarInitialControls.None;

      // Add only the ballpoint pen, pencil, and eraser.
      // Note that the buttons are added to the toolbar in the order
      // defined by the framework, not the order we specify here.
      InkToolbarBallpointPenButton ballpoint = new InkToolbarBallpointPenButton();
      InkToolbarPencilButton pencil = new InkToolbarPencilButton();
      InkToolbarEraserButton eraser = new InkToolbarEraserButton();
      inkToolbar.Children.Add(eraser);
      inkToolbar.Children.Add(ballpoint);
      inkToolbar.Children.Add(pencil);
  }
  ```

<!--
### Support touch input
By default, the InkToolbar supports both pen and mouse input, you have to enable support for touch input.
-->

## 自定义按钮和墨迹书写功能

你可以自定义和扩展通过 InkToolbar 提供的按钮（以及关联的墨迹书写功能）集合。

InkToolbar 由两组不同的按钮类型组成：

1. 一组“工具”按钮，包含内置绘制、擦除和突出显示按钮。 在此处添加自定义的笔和工具。
> **注意**&nbsp;&nbsp;功能选择相互排除。

2. 一组“切换”按钮，包含内置标尺按钮。 在此处添加自定义切换。
> **注意**&nbsp;&nbsp;功能相互不排斥，并且可以与其他活动工具同时使用。

根据你的应用程序和所需的墨迹书写功能，你可以将以下任意按钮（绑定到你的自定义墨迹功能）添加到 InkToolbar：

- 自定义笔：由主机应用为其定义墨迹调色板和笔尖属性（如形状、旋转和大小）的笔。
- 自定义工具：非笔工具，由主机应用定义。
- 自定义切换：将应用定义的功能状态设置为开或关。 当打开时，功能将与活动工具结合使用。

> **注意**&nbsp;&nbsp;你无法更改内置按钮的显示顺序。 默认的显示顺序为：圆珠笔、铅笔、荧光笔、橡皮擦和标尺。 自定义笔附加到最后一个默认笔，自定义工具按钮添加到最后一个笔按钮和橡皮擦按钮之间，而自定义切换按钮添加到标尺按钮之后。 （自定义按钮按照指定它们的顺序添加。）

### 自定义笔

![自定义书法笔按钮](.\images\ink\ink-tools-custompen.png)  
*自定义书法笔按钮*

对于此示例，我们定义一个带有宽笔尖的自定义笔，可支持基本书法笔划墨迹。 我们还自定义按钮浮出控件中显示的调色板中的画笔集合。

**代码隐藏**

首先，我们定义自定义笔，并在代码隐藏中指定绘图属性。 稍后，我们从 XAML 引用此自定义笔。

1. 在解决方案资源管理器中右键单击项目，并依次选择“添加”&gt;“新建项目”。
2. 在“Visual C#”-&gt;“代码”下，添加新的类文件，并称它为 CalligraphicPen.cs。
3. 在 Calligraphic.cs 中，将默认 using 块替换为以下内容：
```csharp
using System.Numerics;
using Windows.UI;
using Windows.UI.Input.Inking;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
```

4. 指定 CalligraphicPen 类派生自 [InkToolbarCustomPen](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.aspx)。
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
}
```

5. 替代 [CreateInkDrawingAttributesCore](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.createinkdrawingattributescore.aspx) 以指定你自己的画笔和笔划大小。
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
    }
}
```

6. 创建 [InkDrawingAttributes](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.aspx) 对象，并设置[笔尖形状](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentip.aspx)、[笔尖旋转](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentiptransform.aspx)、[笔划大小](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.size.aspx)和[墨迹颜色](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.color.aspx)。
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
        InkDrawingAttributes inkDrawingAttributes =
          new InkDrawingAttributes();
        inkDrawingAttributes.PenTip = PenTipShape.Circle;
        inkDrawingAttributes.Size =
          new Windows.Foundation.Size(strokeWidth, strokeWidth * 20);
        SolidColorBrush solidColorBrush = brush as SolidColorBrush;
        if (solidColorBrush != null)
        {
            inkDrawingAttributes.Color = solidColorBrush.Color;
        }
        else
        {
            inkDrawingAttributes.Color = Colors.Black;
        }

        Matrix3x2 matrix = Matrix3x2.CreateRotation(45);
        inkDrawingAttributes.PenTipTransform = matrix;

        return inkDrawingAttributes;
    }
}
```

**XAML**

接下来，我们在 MainPage.xaml 中将必要的引用添加到自定义笔。

1. 我们声明一个本地页面资源字典，用于创建对 CalligraphicPen.cs 中定义的自定义笔 (`CalligraphicPen`) 的引用和该自定义笔所支持的[画笔集合](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.UI.Xaml.Media.BrushCollection.aspx) (`CalligraphicPenPalette`)。
```xaml
<Page.Resources>
    <!-- Add the custom CalligraphicPen to the page resources. -->
    <local:CalligraphicPen x:Key="CalligraphicPen" />
    <!-- Specify the colors for the palette of the custom pen. -->
    <BrushCollection x:Key="CalligraphicPenPalette">
        <SolidColorBrush Color="Blue" />
        <SolidColorBrush Color="Red" />
    </BrushCollection>
</Page.Resources>
```

2. 然后，我们添加一个带有子 [InkToolbarCustomPenButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompenbutton.aspx) 元素的 InkToolbar。

  自定义笔按钮包括页面资源中声明的两个静态资源引用：`CalligraphicPen` 和 `CalligraphicPenPalette`。

  我们还指定笔划大小滑块的范围（[MinStrokeWidth](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.minstrokewidth.aspx)、[MaxStrokeWidth](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.maxstrokewidth.aspx) 和 [SelectedStrokeWidth](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedstrokewidthproperty.aspx)）、选定的画笔 ([SelectedBrushIndex](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedbrushindex.aspx)) 和自定义笔按钮的图标 ([SymbolIcon](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.symbolicon.aspx))。
```xaml
<Grid Grid.Row="1">
    <InkCanvas x:Name="inkCanvas" />
    <InkToolbar x:Name="inkToolbar"
                VerticalAlignment="Top"
                TargetInkCanvas="{x:Bind inkCanvas}">
        <InkToolbarCustomPenButton
            CustomPen="{StaticResource CalligraphicPen}"
            Palette="{StaticResource CalligraphicPenPalette}"
            MinStrokeWidth="1" MaxStrokeWidth="3" SelectedStrokeWidth="2"
            SelectedBrushIndex ="1">
            <SymbolIcon Symbol="Favorite" />
            <InkToolbarCustomPenButton.ConfigurationContent>
                <InkToolbarPenConfigurationControl />
            </InkToolbarCustomPenButton.ConfigurationContent>
        </InkToolbarCustomPenButton>
    </InkToolbar>
</Grid>
```

<!--
### Custom toggle

Enable touch inking

>**Note**&nbsp;&nbsp;InkToolbar supports pen and mouse input and can be configured to recognize touch input.
-->


## 相关文章

* [笔和触笔交互](pen-and-stylus-interactions.md)

**示例**
* [墨迹示例](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [简单墨迹示例](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [复杂墨迹示例](http://go.microsoft.com/fwlink/p/?LinkID=620314)



<!--HONumber=Sep16_HO2-->



---
author: muhsinking
Description: Use templates to modify the look of items in ListView or GridView controls.
title: 项目容器和模板
label: Item containers and templates
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: d8eb818d-b62e-4314-a612-f29142dbd93f
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f5a9756a8afc267c9ec8763af49ba02714c2b4f0
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2018
ms.locfileid: "7418145"
---
# <a name="item-containers-and-templates"></a>项目容器和模板

 

**ListView** 和 **GridView** 控件用于管理其项目的排列方式（水平、垂直、环绕等）以及用户与项目的交互方式，而不是各个项目在屏幕上的显示方式。 项目可视化效果由项目容器管理。 向列表视图添加项目时，它们会自动放置在容器中。 用于 ListView 的默认项目容器为 [ListViewItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listviewitem.aspx)；对于 GridView，其项目容器为 [GridViewItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridviewitem.aspx)。

> **重要 API**：[ListView 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)，[GridView 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)，[ItemTemplate 属性](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx)，[ItemContainerStyle 属性](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle.aspx)


> [!NOTE]
> ListView 和 GridView 都从 [ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx) 类派生，因此它们的功能相同，但数据显示方式不同。 在本文中，涉及到列表视图时，信息适用于 ListView 和 GridView 控件，除非另行指定。 我们可能会引用 ListView 或 ListViewItem 等类，但 *List* 前缀可使用相应网格等效项（GridView 或 GridViewItem）的 *Grid* 代替。 

这些容器控件由两个重要的部分组成，将两者组合使用可以创建项目的最佳视觉显示：*数据模板*和*控件模板*。

- **数据模板** - 为列表视图的 [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 属性分配 [DataTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.datatemplate.aspx)，以指定各个数据项的显示方式。
- **控件模板** - 控件模板提供框架负责的部分项目可视化效果，如视觉状态。 你可以使用 [ItemContainerStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle.aspx) 属性修改控件模板。 通常，若要修改列表视图颜色以匹配品牌标记，或者更改所选项目的显示方式，则执行上述操作。

此图显示了如何将控件模板和数据模板组合使用来创建项目的最终可视化效果。

![列表视图控件和数据模板](images/listview-visual-parts.png)

下面是用于创建该项目的 XAML。 我们稍后会介绍这些模板。

```xaml
<ListView Width="220" SelectionMode="Multiple">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="x:String">
            <Grid Background="Yellow">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="54"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Image Source="Assets/placeholder.png" Width="44" Height="44"
                       HorizontalAlignment="Left"/>
                <TextBlock Text="{x:Bind}" Foreground="Black"
                           FontSize="15" Grid.Column="1"
                           VerticalAlignment="Center"
                           Padding="0,0,54,0"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="Background" Value="LightGreen"/>
        </Style>
    </ListView.ItemContainerStyle>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```
 
## <a name="prerequisites"></a>先决条件

- 我们假设你了解如何使用列表视图控件。 有关详细信息，请参阅 [ListView 和 GridView](listview-and-gridview.md) 文章。
- 我们还假设你了解控件样式和模板，包括如何使用样式内联或作为资源。 有关详细信息，请参阅[样式设置控件](xaml-styles.md)和[控件模板](control-templates.md)。

## <a name="the-data"></a>数据

在更深入地了解如何以列表视图形式显示数据项之前，我们需要了解要显示的数据。 在此示例中，将创建名为 `NamedColor` 的数据类型。 该数据类型将合并颜色名称、颜色值和颜色的 **SolidColorBrush**，它们作为以下 3 个属性公开：`Name`、`Color` 和 `Brush`。
 
然后，对于 [Colors](https://msdn.microsoft.com/library/windows/apps/windows.ui.colors.aspx) 类中的每种命名颜色，用 `NamedColor` 对象来填充 **List**。 列表视图的列表将设置为 [ItemsSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx)。

下面是用于定义类并填充 `NamedColors` 列表的代码。

**C#**
```csharp
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

namespace ColorsListApp
{
    public sealed partial class MainPage : Page
    {
        // The list of colors won't change after it's populated, so we use List<T>. 
        // If the data can change, we should use an ObservableCollection<T> intead.
        List<NamedColor> NamedColors = new List<NamedColor>();

        public MainPage()
        {
            this.InitializeComponent();

            // Use reflection to get all the properties of the Colors class.
            IEnumerable<PropertyInfo> propertyInfos = typeof(Colors).GetRuntimeProperties();

            // For each property, create a NamedColor with the property name (color name),
            // and property value (color value). Add it the NamedColors list.
            for (int i = 0; i < propertyInfos.Count(); i++)
            {
                NamedColors.Add(new NamedColor(propertyInfos.ElementAt(i).Name,
                                    (Color)propertyInfos.ElementAt(i).GetValue(null)));
            }

            colorsListView.ItemsSource = NamedColors;
        }
    }

    class NamedColor
    {
        public NamedColor(string colorName, Color colorValue)
        {
            Name = colorName;
            Color = colorValue;
        }

        public string Name { get; set; }

        public Color Color { get; set; }

        public SolidColorBrush Brush
        {
            get { return new SolidColorBrush(Color); }
        }
    }
}
```

## <a name="data-template"></a>数据模板

指定一个数据模板，以指示列表视图应如何显示数据项。 

在默认情况下，数据项以绑定到的数据对象的字符串表示形式显示在列表视图中。 如果要在列表视图中显示“NamedColors”数据，但未指示列表视图应如何显示该数据，它将仅显示 **ToString** 方法返回的内容，如下所示。

**XAML**
```xaml
<ListView x:Name="colorsListView"/>
```

![显示项目的字符串表示形式的列表视图](images/listview-no-template.png)

通过将 [DisplayMemberPath](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.displaymemberpath.aspx) 设置为特定的属性，可以显示数据项的该属性的字符串表示形式。 你在此处将 DisplayMemberPath 设置为 `NamedColor` 项目的 `Name` 属性。

**XAML**
```xaml
<ListView x:Name="colorsListView" DisplayMemberPath="Name" />
```

现在，列表视图将按名称显示项目，如下所示。 该视图较为实用，但不是很有趣，并且许多信息处于隐藏状态。

![显示项属性的字符串表示形式的列表视图](images/listview-display-member-path.png)

你通常希望更丰富地呈现你的数据。 若要具体地指定列表视图中项的显示方式，可以创建 [DataTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.datatemplate.aspx)。 DataTemplate 中的 XAML 定义用于显示各项的控件的布局和外观。 该布局中的控件可绑定到数据对象的属性，或者具有在内联中定义的静态内容。 将 DataTemplate 分配给列表控件的 [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 属性。

> [!IMPORTANT]
> 你不能同时使用 **ItemTemplate** 和 **DisplayMemberPath**。 如果同时设置这两个属性，会发生异常。

你在此处定义一个 DataTemplate，它通过项目的颜色以及颜色名称和 RGB 值显示 [Rectangle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.aspx)。 

> [!NOTE]
> 在 DataTemplate 中使用 [x:Bind 标记扩展](https://msdn.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)时，你必须指定 DataTemplate 中的 DataType (`x:DataType`)。

**XAML**
```XAML
<ListView x:Name="colorsListView">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:NamedColor">
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition MinWidth="54"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Grid.RowDefinitions>
                    <RowDefinition/>
                    <RowDefinition/>
                </Grid.RowDefinitions>
                <Rectangle Width="44" Height="44" Fill="{x:Bind Brush}" Grid.RowSpan="2"/>
                <TextBlock Text="{x:Bind Name}" Grid.Column="1" Grid.ColumnSpan="4"/>
                <TextBlock Text="{x:Bind Color.R}" Grid.Column="1" Grid.Row="1" Foreground="Red"/>
                <TextBlock Text="{x:Bind Color.G}" Grid.Column="2" Grid.Row="1" Foreground="Green"/>
                <TextBlock Text="{x:Bind Color.B}" Grid.Column="3" Grid.Row="1" Foreground="Blue"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

以下是数据项通过此数据模板显示时的外观。

![使用数据模板的列表视图项目](images/listview-data-template-0.png)

你可能想要以 GridView 形式显示数据。 下面是通过更适合网格布局的方式显示数据的另一数据模板。 此时，数据模板将使用 XAML 针对 GridView 定义为资源，而不是内联。


**XAML**
```xaml
<Page.Resources>
    <DataTemplate x:Key="namedColorItemGridTemplate" x:DataType="local:NamedColor">
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="32"/>
                <ColumnDefinition Width="32"/>
                <ColumnDefinition Width="32"/>
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="96"/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
    
            <Rectangle Width="96" Height="96" Fill="{x:Bind Brush}" Grid.ColumnSpan="3" />
            <!-- Name -->
            <Border Background="#AAFFFFFF" Grid.ColumnSpan="3" Height="40" VerticalAlignment="Top">
                <TextBlock Text="{x:Bind Name}" TextWrapping="Wrap" Margin="4,0,0,0"/>
            </Border>
            <!-- RGB -->
            <Border Background="Gainsboro" Grid.Row="1" Grid.ColumnSpan="3"/>
            <TextBlock Text="{x:Bind Color.R}" Foreground="Red"
                   Grid.Column="0" Grid.Row="1" HorizontalAlignment="Center"/>
            <TextBlock Text="{x:Bind Color.G}" Foreground="Green"
                   Grid.Column="1" Grid.Row="1" HorizontalAlignment="Center"/>
            <TextBlock Text="{x:Bind Color.B}" Foreground="Blue" 
                   Grid.Column="2" Grid.Row="1" HorizontalAlignment="Center"/>
            <!-- HEX -->
            <Border Background="Gray" Grid.Row="2" Grid.ColumnSpan="3">
                <TextBlock Text="{x:Bind Color}" Foreground="White" Margin="4,0,0,0"/>
            </Border>
        </Grid>
    </DataTemplate>
</Page.Resources>

...

<GridView x:Name="colorsGridView" 
          ItemTemplate="{StaticResource namedColorItemGridTemplate}"/>
```

当数据使用此数据模板显示在网格中时，其外观如下所示。

![使用数据模板的网格视图项目](images/gridview-data-template.png)

### <a name="performance-considerations"></a>性能注意事项

使用数据模板是定义列表视图外观的主要方法。 如果列表显示大量项目，它们也能对性能产生重大影响。 

为列表视图中的每个项目创建数据模板中每个 XAML 元素的实例。 例如，前面示例中的网格模板具有 10 个 XAML 元素（1 个 Grid，1 个 Rectangle，3 个 Border，5 个 TextBlock）。 使用此数据模板在屏幕上显示 20 个项目的 GridView 至少会创建 200 个元素 (20*10=200)。 减少数据模板中元素的数量可以大大减少针对列表视图创建的元素总数。 有关详细信息，请参阅 [ListView 和 GridView UI 优化：按项目减少元素计数](https://msdn.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview#element-reduction-per-item)。

 考虑这一网格数据模板部分。 让我们看一下减少元素计数的一些事项。

**XAML**
```xaml
<!-- RGB -->
<Border Background="Gainsboro" Grid.Row="1" Grid.ColumnSpan="3"/>
<TextBlock Text="{x:Bind Color.R}" Foreground="Red"
           Grid.Column="0" Grid.Row="1" HorizontalAlignment="Center"/>
<TextBlock Text="{x:Bind Color.G}" Foreground="Green"
           Grid.Column="1" Grid.Row="1" HorizontalAlignment="Center"/>
<TextBlock Text="{x:Bind Color.B}" Foreground="Blue" 
           Grid.Column="2" Grid.Row="1" HorizontalAlignment="Center"/>
```

 - 第一，布局使用单个网格。 你可以具有单列网格，并将这 3 个 Textblock 放在 StackPanel 中，但在多次创建的数据模板中，应当寻找能避免在其他布局面板内嵌入布局面板的方法。
 - 第二，Border 控件可用于呈现背景，实际上无需将项目放置在 Border 元素内。 Border 元素只能有一个子元素，因此将需要添加一个额外的布局面板，将 3 个 TextBlock 元素托管在 XAML 中的 Border 元素内。 通过不使 TextBlock 成为 Border 的子元素，无需面板即可托管 TextBlock。
 - 最后，你可以将 TextBlock 放置在 StackPanel 内，并在 StackPanel 上设置边框属性，而不是使用显式 Border 元素。 但与 StackPanel 相比，Border 元素是更轻量的控件，因此在多次呈现后，后者对性能的影响较小。

## <a name="control-template"></a>控件模板
项目的控件模板包含用于显示状态的视觉对象，例如选择、将指针悬停在上方和对焦。 这些视觉对象呈现在数据模板的顶部或下方。 下面显示了 ListView 控件模板绘制的一些常见默认视觉对象。

- 悬停 – 在数据模板下方绘制的浅灰色矩形。  
- 选择 – 在数据模板下方绘制的浅蓝色矩形。 
- 键盘焦点 – 在项模板顶部绘制的黑白相间的虚线边框。 

![列表视图状态视觉对象](images/listview-state-visuals.png)

列表视图将数据模板和控件模板中的元素结合使用，以便创建屏幕上呈现的最终视觉对象。 状态视觉对象在此处显示在列表视图的上下文中。

![具有不同状态的项目的列表视图](images/listview-states.png)

### <a name="listviewitempresenter"></a>ListViewItemPresenter

正如我们前面关于数据模板所提到的那样，针对每个项目创建的 XAML 元素数可能会对列表视图的性能产生重大影响。 由于合并使用数据模板和控件模板来显示每个项目，因此显示项目所需的实际元素数包括这两个模板中的元素。

对 ListView 和 GridView 控件进行优化，以便减少针对每个项目创建的 XAML 元素数。 **ListViewItem** 视觉对象由 [ListViewItemPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.aspx) 创建，后者是一个特殊的 XAML 元素，用于为对焦、选择和其他视觉状态显示复杂的视觉效果，而无需大量 UIElement 开销。
 
> [!NOTE]
> 在适用于 Windows 10 的 UWP 应用中，**ListViewItem** 和 **GridViewItem** 都使用 **ListViewItemPresenter**；GridViewItemPresenter 已弃用，你不应该使用。 ListViewItem 和 GridViewItem 在 ListViewItemPresenter 上设置不同的属性值来实现不同的默认外观。）

若要修改项目容器的外观，请使用 [ItemContainerStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle.aspx) 属性，并提供 [Style](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.style.aspx)，其中 [TargetType](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.style.targettype.aspx) 已设为 **ListViewItem** 或 **GridViewItem**。

在此示例中，将向 ListViewItem 添加边距，以便在列表中的不同项目之间留出一些间距。

```xaml
<ListView x:Name="colorsListView">
    <ListView.ItemTemplate>
        <!-- DataTemplate XAML shown in previous ListView example -->
    </ListView.ItemTemplate>

    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="Padding" Value="0,4"/>
        </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

现在，列表视图的外观如下所示，并且项目之间留有间距。

![已应用边距的列表视图项目](images/listview-data-template-1.png)

在 ListViewItem 默认样式中，ListViewItemPresenter **ContentMargin** 属性具有 ListViewItem **Padding** 属性的 [TemplateBinding](https://msdn.microsoft.com/windows/uwp/xaml-platform/templatebinding-markup-extension) (`<ListViewItemPresenter ContentMargin="{TemplateBinding Padding}"/>`)。 当我们设置 Padding 属性时，该值实际将传递给 ListViewItemPresenter ContentMargin 属性。

若要修改未绑定到 ListViewItem 属性的模板的其他 ListViewItemPresenter 属性，你需要使用新的 ListViewItemPresenter（可在其上修改属性）为 ListViewItem 重新设置模板。 

> [!NOTE]
> ListViewItem 和 GridViewItem 默认样式在 ListViewItemPresenter 上设置了很多属性。 你始终应该从默认样式的副本开始，同时仅修改所需属性。 否则，视觉对象可能不按预期方式显示，因为某些属性未正确设置。

**在 Visual Studio 中创建默认模板的副本**
 
1. 打开“文档大纲”窗格（**视图 > 其他窗口 > 文档大纲**）。
2. 选择要修改的列表或网格元素。 在此示例中，修改 `colorsGridView` 元素。
3. 右键单击，然后依次选择**编辑其他模板 > 编辑生成的项目容器 (ItemContainerStyle) > 编辑副本**。
    ![Visual Studio 编辑器](images/listview-itemcontainerstyle-vs.png)
4. 在“创建样式资源”对话框中，输入样式的名称。 在此示例中，使用 `colorsGridViewItemStyle`。
    ![Visual Studio Create Style Resource dialog(images/listview-style-resource-vs.png)

默认样式的副本将添加到你的应用作为资源，而 **GridView.ItemContainerStyle** 属性将设置为该资源，如该 XAML 中所示。 

```xaml
<Style x:Key="colorsGridViewItemStyle" TargetType="GridViewItem">
    <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}"/>
    <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}" />
    <Setter Property="Background" Value="Transparent"/>
    <Setter Property="Foreground" Value="{ThemeResource SystemControlForegroundBaseHighBrush}"/>
    <Setter Property="TabNavigation" Value="Local"/>
    <Setter Property="IsHoldingEnabled" Value="True"/>
    <Setter Property="HorizontalContentAlignment" Value="Center"/>
    <Setter Property="VerticalContentAlignment" Value="Center"/>
    <Setter Property="Margin" Value="0,0,4,4"/>
    <Setter Property="MinWidth" Value="{ThemeResource GridViewItemMinWidth}"/>
    <Setter Property="MinHeight" Value="{ThemeResource GridViewItemMinHeight}"/>
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="GridViewItem">
                <ListViewItemPresenter 
                    CheckBrush="{ThemeResource SystemControlForegroundBaseMediumHighBrush}" 
                    ContentMargin="{TemplateBinding Padding}" 
                    CheckMode="Overlay" 
                    ContentTransitions="{TemplateBinding ContentTransitions}" 
                    CheckBoxBrush="{ThemeResource SystemControlBackgroundChromeMediumBrush}" 
                    DragForeground="{ThemeResource ListViewItemDragForegroundThemeBrush}" 
                    DragOpacity="{ThemeResource ListViewItemDragThemeOpacity}" 
                    DragBackground="{ThemeResource ListViewItemDragBackgroundThemeBrush}" 
                    DisabledOpacity="{ThemeResource ListViewItemDisabledThemeOpacity}" 
                    FocusBorderBrush="{ThemeResource SystemControlForegroundAltHighBrush}" 
                    FocusSecondaryBorderBrush="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    HorizontalContentAlignment="{TemplateBinding HorizontalContentAlignment}" 
                    PointerOverForeground="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    PressedBackground="{ThemeResource SystemControlHighlightListMediumBrush}" 
                    PlaceholderBackground="{ThemeResource ListViewItemPlaceholderBackgroundThemeBrush}" 
                    PointerOverBackground="{ThemeResource SystemControlHighlightListLowBrush}" 
                    ReorderHintOffset="{ThemeResource GridViewItemReorderHintThemeOffset}" 
                    SelectedPressedBackground="{ThemeResource SystemControlHighlightListAccentHighBrush}" 
                    SelectionCheckMarkVisualEnabled="True" 
                    SelectedForeground="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    SelectedPointerOverBackground="{ThemeResource SystemControlHighlightListAccentMediumBrush}" 
                    SelectedBackground="{ThemeResource SystemControlHighlightAccentBrush}" 
                    VerticalContentAlignment="{TemplateBinding VerticalContentAlignment}"/>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>

...

<GridView x:Name="colorsGridView" ItemContainerStyle="{StaticResource colorsGridViewItemStyle}"/>
```

现在，可以修改 ListViewItemPresenter 上的属性来对视觉状态的选择复选框、项目定位和画笔颜色进行控制。 

#### <a name="inline-and-overlay-selection-visuals"></a>内联和覆盖选择视觉对象

ListView 和 GridView 以不同方式指示所选项目，具体取决于控件和 [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx)。 有关列表视图选择的详细信息，请参阅 [ListView 和 GridView](listview-and-gridview.md)。 

当 **SelectionMode** 设置为**多选**时，选择复选框将显示为项目控件模板的一部分。 你可以在多选模式下使用 [SelectionCheckMarkVisualEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled.aspx) 属性关闭选择复选框。 但是，此属性在其他选择模式中将忽略，因此你无法在扩展或单选模式下打开复选框。

你可以设置 [CheckMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.checkmode.aspx) 属性，指定是使用内联样式还是覆盖样式显示复选框。

- **内联**：此样式将在内容左侧显示复选框，并设置项目容器背景色以指示选择。 这是 ListView 的默认样式。
- **覆盖**：此样式将在内容顶部显示复选框，并且仅设置项目容器的边框颜色以指示选择。 这是 GridView 的默认样式。

此表显示了用于指示选择的默认视觉对象。

SelectionMode：&nbsp;&nbsp; | 单选/扩展 | 多选
---------------|-----------------|---------
内联 | ![内联单选或扩展选择](images/listview-single-selection.png) | ![内联多选](images/listview-multi-selection.png)
覆盖 | ![覆盖单选或扩展选择](images/gridview-single-selection.png) | ![覆盖多选](images/gridview-multi-selection.png)

> [!NOTE]
> 在此示例和以下示例中，会显示简单的字符串数据项，不过没有数据模板来强调控件模板所提供的视觉对象。

还有一些画笔属性，可用于更改复选框的颜色。 我们接下来就来看一下这些属性以及其他画笔属性。

#### <a name="brushes"></a>画笔 

多个属性指定用于其他视觉状态的画笔。 你可能想要修改这些属性来匹配品牌的颜色。 

此表显示 ListViewItem 的常见和选择视觉状态，以及用于呈现每个状态的视觉对象的画笔。 这些图像显示内联和覆盖选择视觉样式上的画笔效果。

> [!NOTE]
> 在此表中，已修改的画笔颜色值为经过硬编码的命名颜色，并且颜色已选定，以便在它们应用到模板时更明显。 这些颜色并非视觉状态的默认颜色。 如果要在应用中修改默认颜色，应使用画笔资源来修改在默认模板中完成的颜色值。

状态/画笔名称 | 内联样式 | 覆盖样式
------------|--------------|--------------
<b>Normal</b><ul><li><b>CheckBoxBrush="Red"</b></li></ul> | ![内联项目选择常规](images/listview-item-normal.png) | ![覆盖项目选择常规](images/gridview-item-normal.png)
<b>PointerOver</b><ul><li><b>PointerOverForeground="DarkOrange"</b></li><li><b>PointerOverBackground="MistyRose"</b></li><li>CheckBoxBrush="Red"</li></ul> | ![内联项目选择将指针悬停在上方](images/listview-item-pointerover.png) | ![覆盖项目选择将指针悬停在上方](images/gridview-item-pointerover.png)
<b>Pressed</b><ul><li><b>PressedBackground="LightCyan"</b></li><li>PointerOverForeground="DarkOrange"</li><li>CheckBoxBrush="Red"</li></ul> | ![内联项目选择已按下](images/listview-item-pressed.png) | ![覆盖项目选择已按下](images/gridview-item-pressed.png)
<b>Selected</b><ul><li><b>SelectedForeground="Navy"</b></li><li><b>SelectedBackground="Khaki"</b></li><li><b>CheckBrush="Green"</b></li><li>CheckBoxBrush="Red"（仅限内联）</li></ul> | ![内联项目选择已选中](images/listview-item-selected.png) | ![覆盖项目选择已选中](images/gridview-item-selected.png)
<b>PointerOverSelected</b><ul><li><b>SelectedPointerOverBackground="Lavender"</b></li><li>SelectedForeground="Navy"</li><li>SelectedBackground="Khaki"（仅限覆盖）</li><li>CheckBrush="Green"</li><li>CheckBoxBrush="Red"（仅限内联）</li></ul> | ![内联项目选择将指针悬停在上方且已选中](images/listview-item-pointeroverselected.png) | ![覆盖项目选择将指针悬停在上方且已选中](images/gridview-item-pointeroverselected.png)
<b>PressedSelected</b><ul><li><b>SelectedPressedBackground="MediumTurquoise"</b></li></li><li>SelectedForeground="Navy"</li><li>SelectedBackground="Khaki"（仅限覆盖）</li><li>CheckBrush="Green"</li><li>CheckBoxBrush="Red"（仅限内联）</li></ul> | ![内联项目选择已按下且已选中](images/listview-item-pressedselected.png) | ![覆盖项目选择已按下且已选中](images/gridview-item-pressedselected.png)
<b>Focused</b><ul><li><b>FocusBorderBrush="Crimson"</b></li><li><b>FocusSecondaryBorderBrush="Gold"</b></li><li>CheckBoxBrush="Red"</li></ul> | ![内联项目选择已对焦](images/listview-item-focused.png) | ![覆盖项目选择已对焦](images/gridview-item-focused.png)

ListViewItemPresenter 具有数据占位符和拖动状态的其他画笔属性。 如果要在列表视图中使用增量加载或拖放，应考虑是否还需要修改这些附加画笔属性。 有关可以修改的属性完整列表，请参阅 ListViewItemPresenter 类。 

### <a name="expanded-xaml-item-templates"></a>扩展的 XAML 项模板

如果你需要进行的修改比 **ListViewItemPresenter** 属性允许的还要多（例如，当需要更改复选框的位置时），可以使用 *ListViewItemExpanded* 或 *GridViewItemExpanded* 模板。 这些模板包含在 generic.xaml 的默认样式中。 它们遵循从各个 UIElement 生成所有视觉对象的标准 XAML 模式。

如前面所述，项模板中的 UIElement 数量会对列表视图的性能产生重大影响。 将 ListViewItemPresenter 替换为扩展的 XAML 模板会大大增加元素计数，当列表视图将显示大量项目或者性能成为关注的问题时，不建议这样做。

> [!NOTE]
> 仅在列表视图的 [ItemsPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx) 为 [ItemsWrapGrid](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemswrapgrid.aspx) 或 [ItemsStackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx) 时才支持 **ListViewItemPresenter**。 如果你将 ItemsPanel 更改为使用 [VariableSizedWrapGrid](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.variablesizedwrapgrid.aspx)、[WrapGrid](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.wrapgrid.aspx) 或 [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx)，项模板将自动切换为扩展的 XAML 模板。 有关详细信息，请参阅 [ListView 和 GridView UI 优化](https://msdn.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview)。

若要自定义扩展的 XAML 模板，你需要在应用中为其创建副本，并将 **ItemContainerStyle** 属性设置为副本。

**复制扩展的模板**
1. 为 ListView 或 GridView 设置 ItemContainerStyle 属性，如下所示。
    ```xaml
    <ListView ItemContainerStyle="{StaticResource ListViewItemExpanded}"/>
    <GridView ItemContainerStyle="{StaticResource GridViewItemExpanded}"/>
    ```
2. 在“Visual Studio 属性”窗格中，展开“杂项”部分，并查找 ItemContainerStyle 属性。 （确保已选中 ListView 或 GridView。）
3. 单击 ItemContainerStyle 属性的属性标记。 （它是 TextBox 旁边的小框。 该框的颜色为绿色，用于显示其已设置为 StaticResource。）将打开属性菜单。
4. 在属性菜单中，单击**转换为新资源**。 
    
    ![Visual Studio 属性菜单](images/listview-convert-resource-vs.png)
5. 在“创建样式资源”对话框中，输入资源的名称，然后单击“确定”。

generic.xaml 中扩展模板的副本已在你的应用中创建，可根据需要进行修改。


## <a name="related-articles"></a>相关文章

- [列表](lists.md)
- [ListView 和 GridView](listview-and-gridview.md)


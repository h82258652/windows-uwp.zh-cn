---
Description: TwoPaneView 是一个布局控件，用于管理有两个不同内容区域的应用的显示。
title: 双窗格视图
template: detail.hbs
ms.date: 01/22/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 67b97aec970cc655700729743f10c63c666ab0a6
ms.sourcegitcommit: 09571e1c6a01fabed773330aa7ead459a47d94f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2020
ms.locfileid: "76929264"
---
# <a name="two-pane-view"></a>双窗格视图

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) 是一个布局控件，用于管理有两个不同内容区域（例如大纲/细节视图）的应用的显示。

> [!IMPORTANT]
> 本文介绍的功能和指南为公共预览版，在正式发布之前可能会进行重大修改。 Microsoft 不对此处提供的信息作任何明示或默示的担保。

TwoPaneView 控件旨在帮助你自动充分利用双屏设备（尽管它适用于所有 Windows 设备），无需特殊编码。 在双屏设备上，双窗格视图可确保用户界面 (UI) 在跨越屏幕之间的间隙时可以完全拆分，使内容出现在间隙的任意一侧。

> **注意：** 双屏设备  是一种特殊的设备，具有独特的功能。 它不等效于具有多个监视器的桌面设备。 有关双屏设备的详细信息，请参阅 [Introduction to dual-screen devices](/dual-screen/introduction)（双屏设备简介）。
>
>在本文中，我们使用术语“单屏设备”或“单屏显示”   来指任何不是双屏设备的设备，不管该设备是单个监视器还是多监视器设置的一部分。 TwoPaneView 控件在单个屏幕上的行为方式与其他 XAML 控件相同。 请参阅[显示多个视图](/windows/uwp/design/layout/show-multiple-views)，详细了解如何为多个监视器优化应用。

| **获取 Windows UI 库** |
| - |
| 此控件作为 Windows UI 库的一部分提供，该库是一个 Nuget 包，包含新控件和 UWP 应用的 UI 功能。 有关详细信息（包括安装说明），请参阅 [Windows UI 库概述](/uwp/toolkits/winui/)。 |

| **平台 API** | **Windows UI 库** |
| - | - |
| [TwoPaneView 类](/uwp/api/windows.ui.xaml.controls.twopaneview) | [TwoPaneView 类](/uwp/api/microsoft.ui.xaml.controls.twopaneview) |

在本文档中，我们将使用 XAML 中的 **muxc** 别名表示我们已包含在项目中的 Windows UI 库 API。 我们已将此添加到我们的[页](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page)元素：

```xaml
xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
```

在后面的代码中，我们还将使用 C# 中的 **muxc** 别名表示我们已包含在项目中的 Windows UI 库 API。 我们在文件顶部添加了此 **using** 语句：

```csharp
using muxc = Microsoft.UI.Xaml.Controls;
```

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

如果内容有 2 个不同的区域，请使用双窗格视图，然后会出现以下情况：

- 内容会自动重新排列，并根据窗口大小调整大小。
- 内容的辅助区域会根据可用空间进行显示/隐藏。
- 内容会在双屏设备的 2 个屏幕之间完全拆分。

## <a name="examples"></a>示例

以下图像显示了一个在单屏设备和双屏设备上运行的应用。 双窗格视图可根据每台设备上的各种窗格配置调整应用 UI。

![tpv-single.png](images/two-pane-view/tpv-single.png)

单屏设备上的应用。 

![tpv-dual-wide.png](images/two-pane-view/tpv-dual-wide.png)

在横向模式下跨双屏设备的应用。 

![tpv-dual-tall.png](images/two-pane-view/tpv-dual-tall.png)

在纵向模式下跨双屏设备的应用。 

## <a name="how-it-works"></a>工作原理

双窗格视图有两个窗格，可以在其中放置内容。 它根据可供窗口使用的空间调整窗格的大小和排列方式。 可能的窗格布局通过 [TwoPaneViewMode](/uwp/api/microsoft.ui.xaml.controls.twopaneviewmode) 枚举来定义：

- **SinglePane** - 仅显示一个窗格，通过 [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) 属性指定。
- **横向** - 以并排方式显示窗格，或者显示单个窗格，通过 [WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration) 属性指定。
- **纵向** - 以上下方式显示窗格，或者显示单个窗格，通过 [TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration) 属性指定。

可以通过设置 [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) 来配置双窗格视图，指定在空间只允许显示一个窗格的情况下显示哪个窗格。 然后，指定将 Pane1 显示在纵向窗口的顶部还是底部，或者显示在横向窗口的左侧还是右侧。

双窗格视图处理窗格的大小和排列，但你仍需使窗格内的内容适应大小和方向上的变化。 请参阅[采用 XAML 的响应式布局](/windows/uwp/design/layout/layouts-with-xaml)和[布局面板](/windows/uwp/design/layout/layout-panels)，详细了解如何创建自适应 UI。

双窗格视图基于运行应用的设备类型管理窗格的显示：

- 在双屏设备上

    双窗格视图旨在使你能够轻松地优化用于双屏设备的 UI。 窗口会调整自身大小，目的是使用屏幕上的所有可用空间。 当应用只在设备的其中一个屏幕上时，会显示一个窗格，这通过 [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) 属性指定。

    当应用跨双屏设备的两个屏幕时，每个屏幕都会显示其中一个窗格的内容，并会让内容以适当方式跨越间隙。 使用双窗格视图时，会内置跨越感知功能。 只需设置“纵向/横向”配置来指定在哪个屏幕上显示哪个窗格即可。 双窗格视图会处理其余事项。


- 在单屏设备上

    在单屏设备（如笔记本电脑或台式电脑）上运行时，双窗格视图提供了任何 XAML 控件能够提供的行为。 调整窗口大小时，双窗格视图会根据窗口大小调整其窗格的大小和位置。 它包含的你设置的其他属性可用于定义控件在可调整大小的窗口中的单个屏幕上显示时的行为。 我们会在下一部分更详细地介绍这些属性。

## <a name="how-to-use-the-two-pane-view-control"></a>如何使用双窗格视图控件

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) 不需要是页面布局的根元素。 事实上，我们通常会将它用在为应用提供整体导航的 [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview) 控件中。 TwoPaneView 会相应地进行适应，不管它在 XAML 树中处于什么位置；但是，我们建议你不要将一个 TwoPaneView 嵌套在另一个 TwoPaneView 中。

### <a name="add-content-to-the-panes"></a>将内容添加到窗格

双窗格视图的每个窗格都可以保存单个 XAML UIElement。 若要添加内容，通常将 XAML 布局面板置于每个窗格中，然后将其他控件和内容添加到面板。 窗格可以更改大小并在横向模式和纵向模式间切换，因此需确保每个窗格中的内容能够适应这些更改。 请参阅[采用 XAML 的响应式布局](/windows/uwp/design/layout/layouts-with-xaml)和[布局面板](/windows/uwp/design/layout/layout-panels)，详细了解如何创建自适应 UI。

此示例创建如下所示的简单图片/信息应用 UI。 如果有两个窗格的空间，则图片和信息显示在单独的窗格中。 （如果只有一个窗格的空间，则将 Pane2 的内容移到 Pane1 中，让用户滚动查看任何隐藏的内容。 稍后会在“响应模式更改”  部分看到此内容的代码。）

![tpv-add-content.png](images/two-pane-view/tpv-add-content.png)

```xaml
<muxc:TwoPaneView
    Pane1Length="*"
    ModeChanged="TwoPaneView_ModeChanged">

    <muxc:TwoPaneView.Pane1>
        <Grid x:Name="Pane1Root">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" MinHeight="40"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>

            <CommandBar x:Name="MyCommandBar" DefaultLabelPosition="Right">
                <AppBarButton x:Name="Share" Icon="Share" Label="Share"/>
                <AppBarButton x:Name="Print" Icon="Print" Label="Print"/>
            </CommandBar>

            <ScrollViewer Grid.Row="1">
                <StackPanel x:Name="Pane1StackPanel">
                    <Image x:Name="TheImage" Source="Assets\LandscapeImage8.jpg"
                           VerticalAlignment="Top" HorizontalAlignment="Center" 
                           Margin="16,0"/>
                </StackPanel>
            </ScrollViewer>

        </Grid>
    </muxc:TwoPaneView.Pane1>

    <muxc:TwoPaneView.Pane2>
        <Grid x:Name="Pane2Root">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" MinHeight="40"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <StackPanel x:Name="DetailsContent" Grid.Row="1"
                Orientation="Vertical" Padding="16">

                <TextBlock Text="Mountain.jpg" Margin="0,0,0,12"
                   FontWeight="SemiBold" FontSize="18"/>

                <TextBlock Text="Date Taken:" FontWeight="SemiBold"/>
                <TextBlock Text="8/29/2019 9:55am" Margin="0,0,0,12"/>

                <TextBlock Text="Dimensions:" FontWeight="SemiBold"/>
                <TextBlock Text="1000x750" Margin="0,0,0,12"/>

                <TextBlock Text="Resolution:" FontWeight="SemiBold"/>
                <TextBlock Text="96 dpi" Margin="0,0,0,12"/>

                <TextBlock Text="Description:" FontWeight="SemiBold"/>
                <TextBlock TextWrapping="Wrap" 
                           Text="Lorem ipsum dolor sit amet."/>
            </StackPanel>
        </Grid>
    </muxc:TwoPaneView.Pane2>
</muxc:TwoPaneView>
```

### <a name="specify-which-pane-to-display"></a>指定要显示的窗格

双窗格视图在只能显示单个窗格时会使用 [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) 属性来确定要显示的窗格。 默认情况下，PanePriority 设置为“Pane1”  。 下面介绍了如何在 XAML 或代码中设置此属性。

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" PanePriority="Pane2">
```

```csharp
MyTwoPaneView.PanePriority = Microsoft.UI.Xaml.Controls.TwoPaneViewPriority.Pane2;
```

### <a name="pane-sizing"></a>窗格大小设置

单屏设备上的窗格大小取决于 [Pane1Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane1length) 和 [Pane2Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane2length) 属性。 它们使用支持 _auto_ 和 _star_(\*) 大小设置的 [GridLength](/uwp/api/windows.ui.xaml.gridlength) 值。 有关 auto 和 star 大小设置的说明，请参阅[采用 XAML 的响应式布局](/windows/uwp/design/layout/layouts-with-xaml#layout-properties)中的“布局属性”部分。 

默认情况下，Pane1Length 设置为 **Auto**，可根据内容自行调整大小。 Pane2Length 设置为 *，使用所有剩余空间。

![tpv-size-default.png](images/two-pane-view/tpv-size-default.png)

使用默认大小设置的窗格 

默认值适用于典型的大纲/细节布局，其中包含 Pane1 中的项列表以及 Pane2 中的大量详细信息。 但是，你可能更愿意以其他方式划分空间，具体取决于内容。 在这里，Pane1Length 设置为 2*，因此它获取的空间是 Pane2 的两倍。

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" Pane1Length="2*">
```

![tpv-size-2.png](images/two-pane-view/tpv-size-2.png)

大小为 2* 和 * 的窗格 

> **注意：** 如前所述，在双屏设备上，系统会忽略这些属性，并根据设备情况  自动设置窗格的大小并对其进行排列。

如果将窗格设置为使用自动大小设置，则可通过设置用于保存窗格内容的面板的高度和宽度来控制大小。 在这种情况下，可能需要处理 ModeChanged 事件，并根据当前模式设置内容的高度和宽度约束。

### <a name="display-in-wide-or-tall-mode"></a>以横向模式或纵向模式显示

在桌面显示器上，双窗格视图的显示[模式](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode)取决于 [MinWideModeWidth](/uwp/api/microsoft.ui.xaml.controls.twopaneview.minwidemodewidth) 和 [MinTallModeHeight](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mintallmodeheight) 属性。 这两个属性的默认值均为 641px，与 [NavigationView.CompactThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) 的相同。

如果窗口：

- 宽于 MinWideModeWidth，则使用“横向”  模式。
- 窄于 MinWideModeWidth 但高于 MinTallModeHeight，则使用“纵向”  模式。
- 窄于 MinWideModeWidth 但矮于 MinTallModeHeight，则使用“SinglePane”  模式。

> **注意：** 如前所述，在双屏设备上，系统会忽略这些属性，并根据设备情况  自动设置窗格的大小并对其进行排列。

#### <a name="wide-configuration-options"></a>横向模式配置选项

当单个显示器宽于 MinWideModeWidth 属性时，双窗格视图会进入“横向”模式。 MinWideModeWidth 控制双窗格视图进入横向模式的时间。 默认值为 641px，但可以将其更改为所需的值。 通常情况下，应将此属性设置为窗格所需的最小宽度。

当双窗格视图处于横向模式时，WideModeConfiguration 属性决定了要显示的内容：

- **SinglePane** - 单个窗格（由 PanePriority 决定）。 该窗格占用 TwoPaneView 的整个大小（即，在两个方向上将大小设置为 *）。
- **LeftRight** - 左侧为 Pane1/右侧为 Pane2。 两个窗格在垂直方向的大小均设置为 *，Pane1 的宽度设置为 auto，Pane2 的宽度设置为 *。
- **RightLeft** - 右侧为 Pane1/左侧为 Pane2。 两个窗格在垂直方向的大小均设置为 *，Pane2 的宽度设置为 auto，Pane1 的宽度设置为 *。

默认设置为 **LeftRight**。

| LeftRight | RightLeft |
| - | - |
| ![tpv-left-right.png](images/two-pane-view/tpv-left-right.png)  | ![tpv-right-left.png](images/two-pane-view/tpv-right-left.png)  |

> **提示：** 当设备使用从右到左 (RTL) 的语言时，双窗格视图会自动调换顺序：RightLeft 呈现为 LeftRight，而 LeftRight 则呈现为 RightLeft。

#### <a name="tall-configuration-options"></a>纵向模式配置选项

当单个显示器窄于 MinWideModeWidth 但高于 MinTallModeHeight 时，双窗格视图会进入纵向模式。 默认值为 641px，但可以将其更改为所需的值。 通常情况下，应将此属性设置为窗格所需的最小高度。

当双窗格视图处于纵向模式时，TallLayout 属性决定了要显示的内容：

- **SinglePane** - 单个窗格（由 PanePriority 决定）。 该窗格占用 TwoPaneView 的整个大小（即，在两个方向上将大小设置为 *）。
- **TopBottom** - 顶部为 Pane1/底部为 Pane2。 两个窗格在水平方向的大小均设置为 *，Pane1 的高度设置为 auto，Pane2 的高度设置为 *。
- **BottomTop** - 底部为 Pane1/顶部为 Pane2。 两个窗格在水平方向的大小均设置为 *，Pane2 的高度设置为 auto，Pane1 的高度设置为 *。

默认设置为 **TopBottom**。

| TopBottom | BottomTop |
| - | - |
| ![tpv-top-bottom.png](images/two-pane-view/tpv-top-bottom.png)  | ![tpv-bottom-top.png](images/two-pane-view/tpv-bottom-top.png)  |

#### <a name="special-values-for-minwidemodewidth-and-mintallmodeheight"></a>MinWideModeWidth 和 MinTallModeHeight 的特殊值

可以使用 MinWideModeWidth 属性来防止双窗格视图进入“横向”模式 - 只需将 MinWideModeWidth 设置为 [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0) 即可。

如果将 MinTallModeHeight 设置为 [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0)，则会阻止双窗格视图进入“纵向”模式。

如果将 MinTallModeHeight 设置为 0，则会阻止双窗格视图进入“SinglePane”模式。

#### <a name="responding-to-mode-changes"></a>响应模式更改

可以使用只读的 [Mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) 属性获取当前显示模式。 当双窗格视图更改其显示的窗格时，会等到 [ ModeChanged](/uwp/api/microsoft.ui.xaml.controls.twopaneview.modechanged) 事件发生后才呈现更新的内容。 可以处理事件以响应显示模式的更改。

使用此事件的一种方式是更新应用的 UI，使用户能够在 SinglePane 模式下查看所有内容。 例如，示例应用有一个主窗格（图像）和一个信息窗格。

![tpv-add-content.png](images/two-pane-view/tpv-add-content.png)

横向模式 

如果只有显示一个窗格的空间，则将 Pane2 的内容移到 Pane1 中，让用户可以滚动查看所有内容。 它的外观如下所示。

![tpv-mode-change.png](images/two-pane-view/tpv-mode-change.png)

SinglePane 模式 

```csharp
 private void TwoPaneView_ModeChanged(Microsoft.UI.Xaml.Controls.TwoPaneView sender, object args)
 {
     ((Panel)DetailsContent.Parent).Children.Remove(DetailsContent);
     ((Panel)MyCommandBar.Parent).Children.Remove(MyCommandBar);

     // Single pane
     if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.SinglePane)
     {
         // Add the command bar and details content to Pane1.
         Pane1StackPanel.Children.Add(DetailsContent);
         Pane1Root.Children.Add(MyCommandBar);
     }
     // Dual pane.
     else
     {
         // Wide mode.
         if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Wide)
         {
             // Put the command bar in Pane2.
             Pane2Root.Children.Add(MyCommandBar);
         }
         // Tall mode.
         else if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Tall)
         {
             // Put the command bar in Pane1
             Pane1Root.Children.Add(MyCommandBar);
         }

         // Put details content in Pane2.
         Pane2Root.Children.Add(DetailsContent);
     }
 }
```

## <a name="dos-and-donts"></a>应做事项和禁止事项

- 请尽量使用双窗格视图，这样应用就可以利用双显示器和大屏幕。
- 请勿将双窗格视图置于另一个双窗格视图中。

## <a name="related-articles"></a>相关文章

- [布局概述](../layout/index.md)
- [双屏开发](/dual-screen)
- [Introduction to dual-screen devices](/dual-screen/introduction)（双屏设备简介）

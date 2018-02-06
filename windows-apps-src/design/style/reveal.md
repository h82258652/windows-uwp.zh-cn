---
author: mijacobs
description: "“展示”是一种灯光效果，有助于重点深入了解应用的交互式元素。"
title: "突出显示"
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: high
ms.openlocfilehash: 8ba0d9939d7ab1d9826ed2848e476499f09c628f
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2018
---
# <a name="reveal-highlight"></a>突出显示

“展示”是一种灯光效果，有助于重点深入了解应用的交互式元素。

> **重要的 API**：[RevealBrush 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)、[RevealBackgroundBrush 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush)、[RevealBorderBrush 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush)、[RevealBrushHelper 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper) 和 [VisualState 类](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

“展示”行为通过在指针靠近时显示可单击内容的容器来实现此操作。

![显示视觉](images/Nav_Reveal_Animation.gif)

通过显示对象周围的隐藏边框，“展示”可以帮助用户更好地理解他们与之交互的空间以及可用操作。 这一点在列表控件和按钮分组方面尤其重要。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果你已安装了 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，单击此处<a href="xamlcontrolsgallery:/item/Reveal">打开此应用，了解“展示”的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>视频摘要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev013/player]

## <a name="reveal-and-the-fluent-design-system"></a>展示和 Fluent 设计系统

 Fluent 设计系统可帮助你创建包含光线、深度、动画、材料和比例的现代、粗体 UI。 展示是 Fluent 设计系统组件，它将光线添加到你的应用。 要了解详细信息，请参阅 [UWP 的 Fluent 设计概述](../fluent-design-system/index.md)。

## <a name="how-to-use-it"></a>如何使用

“展示”针对某些控件自动运行。 对于其他控件，你可以通过向控件分配特殊样式来启用“展示”。

## <a name="controls-that-automatically-use-reveal"></a>自动使用“展示”的控件

- [**ListView**](../controls-and-patterns/lists.md)
- [**GridView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**AutosuggestBox**](../controls-and-patterns/auto-suggest-box.md)
- [**MediaTransportControl**](../controls-and-patterns/media-playback.md)
- [**CommandBar**](../controls-and-patterns/app-bars.md)
- [**ComboBox**](../controls-and-patterns/lists.md)

这些插图显示几种不同控件的展示效果：

![展示示例](images/RevealExamples_Collage.png)

## <a name="enabling-reveal-on-other-common-controls"></a>在其他常见控件上启用“展示”

如果你在某个场景中需要应用“展示”（此类控件是主要内容并且/或者用于列表或集合中），我们提供了可选资源样式，能帮助你在这类场景中启用“展示”。

默认情况下此类控件没有启用“展示”，因为这些控件属于小型控件，通常是应用程序主要焦点的辅助控件；但是每个应用均各不相同，如果这些控件是应用中使用最多的控件，我们提供了一些样式以在以下方面提供帮助：

| 控件名称   | 资源名称 |
|----------|:-------------:|
| 按钮 |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| SemanticZoom | SemanticZoomRevealStyle |

若要应用这些样式，只需如下更新“样式”属性：

```XAML
<Button Content="Button Content" Style="{StaticResource ButtonRevealStyle}"/>
```

## <a name="enabling-reveal-on-custom-controls"></a>在自定义控件上启用“展示”

在决定自定义控件是否应获得“展示”时的一般思考原则是，你必须有一组交互式元素，且它们全部与你想要在应用中执行的重要功能或操作相关。

例如，NavigationView 的项目与页面导航相关。 CommandBar 的按钮与菜单操作或页面功能操作相关。 下方的 MediaTransportControl 的按钮全部与正在播放的媒体相关。

获得“展示”的控件无需彼此相关，它们只需位于高密度区域，并发挥更大作用。

若要在自定义控件或重新模板化控件上启用“展示”，可以转到该控件模板的“视觉状态”中该控件的“样式”，然后在根网格中指定“展示”：

```xaml
<VisualState x:Name="PointerOver">
    <VisualState.Setters>
        <Setter Target="RootGrid.(RevealBrush.State)" Value="PointerOver" />
        <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}" />
        <Setter Target="ContentPresenter.BorderBrush" Value="Transparent"/>
        <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}" />
    </VisualState.Setters>
</VisualState>
```

请务必注意，“展示”在其视觉状态中既需要画笔也需要资源库，这样才能完全正常工作。 单是将一个控件的画笔设置为“展示”画笔资源中的一个资源，不会为该控件启用“展示”。 相反，只有目标或设置而没有“展示”画笔值，“展示”也不会启用。

我们已创建了一组系统“展示”画笔，你可以用来自定义自己的 UI。 例如，你可以使用 **ButtonRevealBackground** 画笔来创建自定义按钮背景，或使用 **ListViewItemRevealBackground** 画笔创建自定义列表，等等。

（若要了解资源在 XAML 中有何作用，请查阅 [XAML 资源字典](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)一文。）

### <a name="reveal-on-listview-controls-with-nested-buttons"></a>具有嵌套按钮的 ListView 控件的展示

如果你有 [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)，并且还有嵌套在其 [ListViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewitem) 元素内的按钮或可调用内容，你应该为这些嵌套项目启用“展示”。

如果 ListViewItem 中是按钮或类似按钮的控件，只需将控件的[样式](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_Style)属性设置为 **ButtonRevealStyle** 静态资源。 

![嵌套的“展示”](images/NestedListContent.png)

此示例为 ListViewItem 内的多个按钮启用“展示”。 

```XAML
<ListViewItem>
    <StackPanel Orientation="Horizontal">
        <TextBlock Margin="5">Test Text: lorem ipsum.</TextBlock>
        <StackPanel Orientation="Horizontal">
            <Button Content="&#xE71B;" FontFamily="Segoe MDL2 Assets" Width="45" Height="45" Margin="5" Style="{StaticResource ButtonRevealStyle}"/>
            <Button Content="&#xE728;" FontFamily="Segoe MDL2 Assets" Width="45" Height="45" Margin="5" Style="{StaticResource ButtonRevealStyle}"/>
            <Button Content="&#xE74D;" FontFamily="Segoe MDL2 Assets" Width="45" Height="45" Margin="5" Style="{StaticResource ButtonRevealStyle}"/>
         </StackPanel>
    </StackPanel>
</ListViewItem>
```

### <a name="listviewitempresenter-with-reveal"></a>具有“展示”的 ListViewItemPresenter

列表在 XAML 中有点特殊，对于“展示”，我们只需为 ListViewItemPresenter 内的“展示”定义视觉状态管理器：

```XAML
<ListViewItemPresenter>
<!-- ContentTransitions, SelectedForeground, etc. properties -->
RevealBackground="{ThemeResource ListViewItemRevealBackground}"
RevealBorderThickness="{ThemeResource ListViewItemRevealBorderThemeThickness}"
RevealBorderBrush="{ThemeResource ListViewItemRevealBorderBrush}">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
        <VisualState x:Name="Normal" />
        <VisualState x:Name="Selected" />
        <VisualState x:Name="PointerOver">
            <VisualState.Setters>
                <Setter Target="Root.(RevealBrush.State)" Value="PointerOver" />
            </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="PointerOverSelected">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="PointerOver" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="PointerOverPressed">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="Pressed" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="Pressed" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="PressedSelected">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="Pressed" />
                </VisualState.Setters>
            </VisualState>
            </VisualStateGroup>
                <VisualStateGroup x:Name="EnabledGroup">
                    <VisualState x:Name="Enabled" />
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                             <Setter Target="Root.RevealBorderThickness" Value="0"/>
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</ListViewItemPresenter>
```

这意味着要附加到具有“展示”特定视觉状态资源库的 ListViewItemPresenter 内属性集合的末尾。

### <a name="full-template-example"></a>完整模板示例

以下是一个呈现“展示”按钮外观的完整模板：

```XAML
<Style TargetType="Button" x:Key="ButtonStyle1">
    <Setter Property="Background" Value="{ThemeResource ButtonRevealBackground}" />
    <Setter Property="Foreground" Value="{ThemeResource ButtonForeground}" />
    <Setter Property="BorderBrush" Value="{ThemeResource ButtonRevealBorderBrush}" />
    <Setter Property="BorderThickness" Value="2" />
    <Setter Property="Padding" Value="8,4,8,4" />
    <Setter Property="HorizontalAlignment" Value="Left" />
    <Setter Property="VerticalAlignment" Value="Center" />
    <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}" />
    <Setter Property="FontWeight" Value="Normal" />
    <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}" />
    <Setter Property="UseSystemFocusVisuals" Value="True" />
    <Setter Property="FocusVisualMargin" Value="-3" />
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="Button">
                <Grid x:Name="RootGrid" Background="{TemplateBinding Background}">

                    <VisualStateManager.VisualStateGroups>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal">

                                <Storyboard>
                                    <PointerUpThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="PointerOver">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.(RevealBrush.State)" Value="PointerOver" />
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="Transparent"/>
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}" />
                                </VisualState.Setters>

                                <Storyboard>
                                    <PointerUpThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="Pressed">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.(RevealBrush.State)" Value="Pressed" />
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPressed}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBackgroundPressed}" />
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPressed}" />
                                </VisualState.Setters>

                                <Storyboard>
                                    <PointerDownThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="Disabled">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundDisabled}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushDisabled}" />
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundDisabled}" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>

              </VisualStateManager.VisualStateGroups>
                    <ContentPresenter x:Name="ContentPresenter"
                    BorderBrush="{TemplateBinding BorderBrush}"
                    BorderThickness="{TemplateBinding BorderThickness}"
                    Content="{TemplateBinding Content}"
                    ContentTransitions="{TemplateBinding ContentTransitions}"
                    ContentTemplate="{TemplateBinding ContentTemplate}"
                    Padding="{TemplateBinding Padding}"
                    HorizontalContentAlignment="{TemplateBinding HorizontalContentAlignment}"
                    VerticalContentAlignment="{TemplateBinding VerticalContentAlignment}"
                    AutomationProperties.AccessibilityView="Raw" />
                </Grid>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

## <a name="dos-and-donts"></a>应做事项和禁止事项
- 应对用户可以对其执行操作的元素使用“展示”（按钮、选择）
- 应在默认没有视觉分隔符的交互式元素分组中使用“展示”（列表、命令栏）
- 应在具有高密度交互式元素的区域使用“展示”
- 不应对静态内容使用“展示”（背景、文本）
- 不应在一次性隔离情况中使用“展示”
- 不应对非常大的项目（大于 500epx）使用“展示”
- 不应在安全决策中使用“展示”，因为这可能会使你不再关注需要向用户提供的消息

## <a name="how-we-designed-reveal"></a>如何设计“展示”

“展示”有两大主要视觉组件：**悬停展示**行为和**边框展示**行为。

![“展示”层](images/RevealLayers.png)

“悬停展示”与悬停的内容（通过指针或焦点输入）直接相关联，并且在悬停或聚焦项目周围应用一个柔和的光晕形状，从而让你知道可以与该项目交互。

“边框展示”应用于聚焦项目和邻近项目。 这会向你显示，这些邻近项目可以与当前的聚焦项目采用类似操作。

“展示”方式细分：

- “边框展示”将位于所有内容顶部，同时位于指定边缘中
- 文本和内容将直接在“边框展示”下方显示
- “悬停展示”将位于内容和文本下方
- 背板（打开并启用“悬停展示”）
- 背景（控件背景）

<!--
<div class=”microsoft-internal-note”>
To create your own Reveal lighting effect for static comps or prototype purposes, see the full [uni design guidance](http://uni/DesignDepot.FrontEnd/#/ProductNav/3020/1/dv/?t=Resources%7CToolkit%7CReveal&f=Neon) for this effect in illustrator.
</div>
-->  

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

- [RevealBrush 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [亚克力](acrylic.md)
- [合成效果](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [UWP 的 Fluent 设计](../fluent-design-system/index.md)
- [系统内的科学：Fluent 设计和深色](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [系统内的科学：Fluent 设计和浅色](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)

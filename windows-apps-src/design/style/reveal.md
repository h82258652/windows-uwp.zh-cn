---
description: “展示”是一种灯光效果，有助于重点深入了解应用的交互式元素。
title: 显示突出显示
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 0f6a4fef72206058874edf845d5fbbe0768c53ec
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984225"
---
# <a name="reveal-highlight"></a>显示突出显示

![主图](images/header-reveal-highlight.svg)

显示突出显示部分是，当用户将指针放在其附近突出显示交互元素，例如命令栏的光照效果。 

> **重要的 API**：[RevealBrush 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)， [RevealBackgroundBrush 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush)， [RevealBorderBrush 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush)， [RevealBrushHelper 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper)， [VisualState类](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

## <a name="how-it-works"></a>工作原理
显示为交互元素的突出显示调用关注者泄漏元素的容器，当指针位于附近，在此图中所示：

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
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>视频摘要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev013/player]

## <a name="how-to-use-it"></a>如何使用

“展示”针对某些控件自动运行。 对于其他控件，您可以显示启用通过将特殊样式分配给该控件，如中所述[启用显示其他控件](#enabling-reveal-on-other-controls)并[启用自定义控件上显示](#enabling-reveal-on-custom-controls)的这部分一文。

## <a name="controls-that-automatically-use-reveal"></a>自动使用“展示”的控件

- [**ListView**](../controls-and-patterns/lists.md)
- [**GridView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**MediaTransportControl**](../controls-and-patterns/media-playback.md)
- [**CommandBar**](../controls-and-patterns/app-bars.md)

这些图显示多个不同的控件上显示突出显示：

![展示示例](images/RevealExamples_Collage.png)


## <a name="enabling-reveal-on-other-controls"></a>在其他控件上启用“展示”

如果你在某个场景中需要应用“展示”（此类控件是主要内容并且/或者用于列表或集合中），我们提供了可选资源样式，能帮助你在这类场景中启用“展示”。

默认情况下此类控件没有启用“展示”，因为这些控件属于小型控件，通常是应用程序主要焦点的辅助控件；但是每个应用均各不相同，如果这些控件是应用中使用最多的控件，我们提供了一些样式以在以下方面提供帮助：

| 控件名称   | 资源名称 |
|----------|:-------------:|
| Button |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| AppBarToggleButton | AppBarToggleButtonRevealStyle |
| GridViewItem（显示内容的置顶） | GridViewItemRevealBackgroundShowsAboveContentStyle |

若要应用这些样式，只需设置控件的[样式](/uwp/api/Windows.UI.Xaml.Style)属性：

```xaml
<Button Content="Button Content" Style="{StaticResource ButtonRevealStyle}"/>
```

### <a name="reveal-in-themes"></a>在主题中显示

显示会根据请求的控件、应用或用户设置的主题略微更改。 在深色主题显示的边框中，悬停灯光是白色，但在浅色主题中，边框只要变暗为浅灰色。

![深色和浅色显示](images/Dark_vs_LightReveal.png)

若要在浅色主题时启用白色边框，只需将请求的控件主题设置为“深色”。

```xaml
<Grid RequestedTheme="Dark">
    <Button Content="Button" Click="Button_Click" Style="{ThemeResource ButtonRevealStyle}"/>
</Grid>
```

或将 RevealBorderBrush 上的 TargetTheme 更改为“深色”。 记住！ 如果将 TargetTheme 设置为“深色”，那么“展示”将为白色，但如果将其设置为“浅色”，“展示”的边框将为灰色。

```xaml
 <RevealBorderBrush x:Key="MyLightBorderBrush" TargetTheme="Dark" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}" />
```

## <a name="enabling-reveal-on-custom-controls"></a>在自定义控件上启用“展示”

你可以向自定义控件添加“展示”。 在执行之前，最好先稍微了解显示效果的工作原理。 显示由两个单独的效果组成：**显示边框**并**揭示悬停**。

- **边框**会在指针靠近时显示交互式元素的边框。 此效果会向你显示，这些邻近项目可以与当前的聚焦项目采用类似操作。
- **悬停**会在悬停或聚焦的项目周围应用一个柔和的光晕形状，并在单击时播放按下动画。 

![“展示”层](images/RevealLayers.png)

<!-- The Reveal recipe breakdown is:

- Border reveal will be on top of all content but on the designated edges
- Text and content will be displayed directly under Border Reveal
- Hover reveal will be beneath content and text
- The backplate (that turns on and enables Hover Reveal)
- The background (background of control) -->


这些效果由两个画笔定义： 
* 边框显示由定义**RevealBorderBrush**
* 悬停时显示定义的**RevealBackgroundBrush**

```xaml
<RevealBorderBrush x:Key="MyRevealBorderBrush" TargetTheme="Light" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}"/>
<RevealBackgroundBrush x:Key="MyRevealBackgroundBrush" TargetTheme="Light" Color="{StaticResource SystemAccentColor}" FallbackColor="{StaticResource SystemAccentColor}" />
```
在大多数情况下，我们会通过为某些控件自动打开“展示”来处理这两种效果的使用。 但是，其他控件将需要通过应用样式或直接更改其模板来启用。

### <a name="when-to-add-reveal"></a>何时添加“展示”
你可以将“展示”添加到你的自定义控件 - 但在此之前，请考虑控件的类型及其行为方式。 
* 如果你的自定义控件是单个交互式元素，并且没有类似控件与它共用空间（如菜单中的菜单项），那么你的自定义控件可能不需要“展示”。  
* 如果你有一组相关的交互式内容或元素，则有可能应用的这一区域不需要“展示”- 这通常称为[命令处理](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/collection-commanding)表面。

例如，按钮本身不应使用“展示”，但命令栏中的一组按钮则应使用“展示”。

<!-- For example, NavigationView's items are related to page navigation. CommandBar's buttons relate to menu actions or page feature actions. MediaTransportControl's buttons beneath all relate to the media being played. -->

### <a name="using-the-control-template-to-add-reveal"></a>使用控件模板添加“展示” 
若要对自定义控件或重新模板化的控件启用“展示”，请修改控件的控件模板。 大多数控件模板在根部有一个网格；请更新该根网格的 [VisualState](/uwp/api/windows.ui.xaml.visualstate) 以使用“展示”：

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

请务必注意，“展示”在其视觉状态中既需要画笔也需要资源库，这样才能正确工作。 单是将一个控件的画笔设置为“展示”画笔资源中的一个资源，不会为该控件启用“展示”。 相反，只有目标或设置而没有“展示”画笔值，“展示”也不会启用。

若要了解有关修改控件模板的详细信息，请参阅 [XAML 控件模板](../controls-and-patterns/control-templates.md)一文。

我们已创建了一组系统“展示”画笔，你可以用来自定义自己的模板。 例如，你可以使用 **ButtonRevealBackground** 画笔来创建自定义按钮背景，或使用 **ListViewItemRevealBackground** 画笔创建自定义列表，等等。 （若要了解资源在 XAML 中有何作用，请查阅 [Xaml 资源字典](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)一文。）

### <a name="full-template-example"></a>完整模板示例

以下是一个呈现“展示”按钮外观的完整模板：

```xaml
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

### <a name="fine-tuning-the-reveal-effect-on-a-custom-control"></a>微调自定义控件的“展示”效果 

启用时显示的自定义或重新模板化控件或自定义命令的图面上，这些提示可以帮助您优化效果：
 
* 对大小与相邻项的不一致的高度或宽度 （尤其是在列表中）：删除边框方法行为和保留上悬停时仅显示边框。
* 命令项，经常转入和签出的禁用状态：位置上元素的 backplates，以及其边框的边框方法画笔来强调其状态。
* 相邻命令元素的因此关闭它们接触：添加两个元素之间 1px 边距。 

## <a name="dos-and-donts"></a>应做事项和禁止事项
### <a name="do"></a>执行操作：
- 应对用户可以对其执行很多操作的元素使用“展示”（命令栏、导航菜单）
- 应在默认没有视觉分隔符的交互式元素分组中使用“展示”（列表、功能区）
- 应在具有高密度交互式元素的区域使用“展示”（命令处理情况）
- 在“展示”项目之间加入 1px 边距空间

### <a name="dont"></a>禁止事项
- 不应对静态内容使用“展示”（背景、文本）
- 不应对弹出窗口、浮出控件或下拉列表使用“展示”
- 不应在一次性隔离情况中使用“展示”
- 不应对非常大的项目（大于 500epx）使用“展示”
- 不应在安全决策中使用“展示”，因为这可能会使你不再关注需要向用户提供的消息


## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。

## <a name="reveal-and-the-fluent-design-system"></a>展示和 Fluent Design 系统

 Fluent 设计系统可帮助你创建包含光线、深度、动画、材料和比例的现代、粗体 UI。 展示是 Fluent 设计系统组件，它将光线添加到你的应用。 要了解详细信息，请参阅 [UWP 的 Fluent Design 概述](/windows/apps/fluent-design-system)。

## <a name="related-articles"></a>相关文章

- [RevealBrush 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [Acrylic](acrylic.md)
- [组合效果](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [适用于 UWP 的 Fluent 设计](/windows/apps/fluent-design-system)
- [在系统中的科学：Fluent 设计和深度](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [在系统中的科学：Fluent 设计和轻型这类](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)

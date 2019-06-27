---
description: 了解如何在 UWP 应用中使用主题色和主题。
title: UWP 应用中的颜色
ms.date: 04/7/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: karenmui
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3177deb74085737531366f63e9f2e8bbecac06e6
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "67028420"
---
# <a name="color"></a>颜色

![主图](images/header-color.svg)

使用颜色可在应用中向用户直观传达信息：颜色可用于指示交互性、针对用户操作提供反馈并保证应用界面的视觉连续性。

在 UWP 应用中，颜色主要由主题色和主题决定。 本文介绍如何在应用中使用颜色，同时介绍如何使用主题色和主题资源来让 UWP 应用可用于任何主题上下文。

## <a name="color-principles"></a>颜色使用原则

:::row:::
    :::column:::
        **Use color meaningfully.**
        When color is used sparingly to highlight important elements, it can help create a user interface that is fluid and intuitive.
    :::column-end:::
    :::column:::
        **Use color to indicate interactivity.**
        It's a good idea to choose one color to indicate elements of your application that are interactive. For example, many web pages use blue text to denote a hyperlink.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Color is personal.**
        In Windows, users can choose an accent color and a light or dark theme, which are reflected throughout their experience. You can choose how to incorporate the user's accent color and theme into your application, personalizing their experience.
    :::column-end:::
    :::column:::
        **Color is cultural.**
        Consider how the colors you use will be interpreted by people from different cultures. For example, in some cultures the color blue is associated with virtue and protection, while in others it represents mourning.
    :::column-end:::
:::row-end:::

## <a name="themes"></a>主题

可对 UWP 应用使用浅色或深色应用程序主题。 主题将对应用背景、文本、图标和[常见控件](../controls-and-patterns/index.md)的颜色造成影响。

### <a name="light-theme"></a>浅色主题

![浅色主题](images/color/light-theme.svg)

### <a name="dark-theme"></a>深色主题

![深色主题](images/color/dark-theme.svg)

默认情况下，UWP 应用的主题是 Windows 设置中的用户首选主题或设备的默认主题（即 XBox 上的深色主题）。 但是，可设置 UWP 应用的主题。

### <a name="changing-the-theme"></a>更改主题

可通过更改 `App.xaml` 文件中的 **RequestedTheme** 属性更改主题。

```XAML
<Application
    x:Class="App9.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App9"
    RequestedTheme="Dark">
</Application>
```

删除 **RequestedTheme** 属性意味着应用程序将使用该用户的系统设置。

用户还可以选择高对比度主题，该主题使用一块小型的对比色调色板，使界面的对比效果更明显。 在此情况下，系统将替代 RequestedTheme。

### <a name="testing-themes"></a>测试主题

如果不为应用请求主题，请确保用浅色和深色主题测试应用，保证在应用在任何情况下均可清晰显示。

**注意**：Visual Studio 中的 RequestedTheme 默认为浅色，因此需要更改 RequestedTheme 来测试两种主题。

## <a name="theme-brushes"></a>主题画笔

常见控件自动使用[主题画笔](../controls-and-patterns/xaml-theme-resources.md#the-xaml-color-ramp-and-theme-dependent-brushes)调整浅色和深色主题的对比度。

例如，下图演示了 [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 使用主题画笔的方式：

![主题画笔控件示例](images/color/theme-brushes.svg)

主题画笔的用途如下：

- **Base** 用于文本。
- **Alt** 与 Base 的用途相反。
- **Chrome** 用于顶级元素，如导航窗格或命令栏。
- **List** 用于列表控件。

**低**/**中等**/**高**代表颜色强度。

### <a name="using-theme-brushes"></a>使用主题画笔

:::row:::
    :::column:::
        When creating templates for custom controls, use theme brushes rather than hard code color values. This way, your app can easily adapt to any theme.

        For example, these [item templates for ListView](../controls-and-patterns/item-templates-listview.md) demonstrate how to use theme brushes in a custom template.
    :::column-end:::
    :::column:::
         ![double line list item with icon example](images/color/list-view.svg)
    :::column-end:::
:::row-end:::

```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="DoubleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Height="64" AutomationProperties.Name="{x:Bind CompositionName}">
                <Ellipse Height="48" Width="48" VerticalAlignment="Center">
                    <Ellipse.Fill>
                        <ImageBrush ImageSource="Placeholder.png"/>
                    </Ellipse.Fill>
                </Ellipse>
                <StackPanel Orientation="Vertical" VerticalAlignment="Center" Margin="12,0,0,0">
                    <TextBlock Text="{x:Bind CompositionName}"  Style="{ThemeResource BaseTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseHighBrush}" />
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource BodyTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

若要详细了解如何在应用中使用主题画笔，请参阅[主题资源](../controls-and-patterns/xaml-theme-resources.md)。

## <a name="accent-color"></a>主题色

常见控件使用主题色表示状态信息。 默认情况下，主题色是用户在设置中选择的 `SystemAccentColor`。 但是，还可自定义应用的主题色来彰显你的品牌。

![Windows 控件](images/color/windows-controls.svg)

:::row:::
    :::column:::
        ![user-selected accent header](images/color/user-accent.svg)
        ![user-selected accent color](images/color/user-selected-accent.svg)
    :::column-end:::
    :::column:::
        ![custom accent header](images/color/custom-accent.svg)
        ![custom brand accent color](images/color/brand-color.svg)
    :::column-end:::
:::row-end:::

### <a name="overriding-the-accent-color"></a>替代主题色

若要更改应用的主题色，请将以下代码放入 `app.xaml`。

```xaml
<Application.Resources>
    <ResourceDictionary>
        <Color x:Key="SystemAccentColor">#107C10</Color>
    </ResourceDictionary>
</Application.Resources>
```

### <a name="choosing-an-accent-color"></a>选择主题色

为应用选择自定义主题色时，请确保使用主题色的文本和背景拥有合适的对比度，以获得最佳的可读性。 若要测试对比度，可使用 Windows 设置中的颜色选取器工具，也可使用[在线对比度工具](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources)。

![Windows 设置中的自定义主题色选取器](images/color/color-picker.svg)

## <a name="accent-color-palette"></a>主题色调色板

Windows Shell 中的主题色算法可生成主题色的浅色和深色底纹。

![主题色调色板](images/color/accent-color-palette.svg)

可访问用作[主题资源](../controls-and-patterns/xaml-theme-resources.md)的以下底纹：

- `SystemAccentColorLight3`
- `SystemAccentColorLight2`
- `SystemAccentColorLight1`
- `SystemAccentColorDark1`
- `SystemAccentColorDark2`
- `SystemAccentColorDark3`

<!-- check this is true -->
还可通过 [**UISettings.GetColorValue**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UISettings#Windows_UI_ViewManagement_UISettings_GetColorValue_Windows_UI_ViewManagement_UIColorType_) 方法和 [**UIColorType**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType) 枚举以编程方式访问主题色调色板。

可将主题色调色板用于应用中的颜色主题。 以下示例介绍如何针对按钮使用主题色调色板。

![按钮所应用的主题色调色板](images/color/color-theme-button.svg)

```xaml
<Page.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="ButtonBackground" Color="{ThemeResource SystemAccentColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver" Color="{ThemeResource SystemAccentColorLight1}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed" Color="{ThemeResource SystemAccentColorDark1}"/>
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Page.Resources>

<Button Content="Button"></Button>
```

在彩色背景中使用彩色文本时，请确保文本与背景之间具有足够的对比度。 默认情况下，超链接或超文本将使用主题色。 如果对背景应用主题色的不同变体，则应使用原始主题色的变体，从而优化彩色背景与彩色文本的对比度。

下图中的示例介绍了主题色的各种浅色/深色底纹，以及彩色类型应用到彩色表面的方式。

![颜色编号](images/color/color-on-color.png)

若要详细了解如何设置控件样式，请参阅 [XAML 样式](../controls-and-patterns/xaml-styles.md)。

## <a name="color-api"></a>颜色 API

多个 API 可用于向应用程序添加颜色。 首先，[**Colors**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colors) 类可实现广泛的预定义颜色。 可通过 XAML 属性自动访问这些预定义颜色。 在以下示例中，我们创建了一个按钮并将背景色属性和前景色属性设置为 **Colors** 类的成员。

```xaml
<Button Background="MediumSlateBlue" Foreground="White">Button text</Button>
```

可使用 XAML 中的 [**Color**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.color) 结构通过 RGB 或十六进制值自行创建颜色。

```xaml
<Color x:Key="LightBlue">#FF36C0FF</Color>
```

还可使用 **FromArgb** 方法通过代码创建相同的颜色。

```csharp
Color LightBlue = Color.FromArgb(255,54,192,255);
```

“Argb”中的各字母分别代表 Alpha（不透明度）、红、绿和蓝，这四个部分共同组成一种颜色。 每个参数的值可介于 0 至 255 之间。 可选择忽略第一个值，此操作将默认选定 255 或 100% 的透明度。

> [!Note]
> 如果使用 C++，则必须使用 [**ColorHelper**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colorhelper) 类创建颜色。

**Color** 最常用作 [**SolidColorBrush**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.solidcolorbrush) 的参数，该参数可用于以单一纯色绘制 UI 元素。 通常在 [**ResourceDictionary**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.ResourceDictionary) 中定义这些画笔，以便画笔可重复用于多个元素。

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="ButtonBackgroundBrush" Color="#FFFF4F67"/>
    <SolidColorBrush x:Key="ButtonForegroundBrush" Color="White"/>
</ResourceDictionary>
```

若要详细了解如何使用画笔，请参阅 [XAML 画笔](brushes.md)。

## <a name="scoping-system-colors"></a>限定系统颜色的范围

除了在应用中定义自己的颜色以外，还可以使用 **ColorPaletteResources** 标记将系统化的颜色范围限定为整个应用中的所需区域。 使用此 API 不仅可以通过设置少量的属性一次性色彩化和主题化大型控件组，而且还能获得一般情况下手动定义自己的自定义颜色所不能获得的其他系统优势：

- 使用 **ColorPaletteResources** 设置的任何颜色不会影响高对比度
  * 这意味着，更多的人可以访问你的应用，且不会产生额外的设计或开发成本
- 在 API 中设置一个属性即可将两种主题中的颜色设置为浅色、深色或渗透色
- 在 **ColorPaletteResources** 中设置的颜色将传播到也使用该系统颜色的所有类似控件
  * 这可以确保整个应用保持一致的色调，同样可以维护品牌的形象
- 在无需重新创建模板的情况下影响所有可视状态、动画和不透明度变体

### <a name="how-to-use-colorpaletteresources"></a>如何使用 ColorPaletteResources

ColorPaletteResources API 告知系统在何处限定了资源的范围。 ColorPaletteResources 必须采用提供以下三个选项之一的 [x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute)：
- 默认
  * 显示[浅色](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme)和[深色](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)主题中的颜色变化
- 浅色
  * 显示[浅色](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme)主题中的颜色变化
- 深色
  * 显示[深色](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)主题中的颜色变化

如果你想要在任一主题中使用不同的自定义外观，设置该 x:Key 可确保颜色根据系统或应用主题相应地变化。

### <a name="how-to-apply-scoped-colors"></a>如何应用限定了范围的颜色

通过 XAML 中的 **ColorPaletteResources** API 限定资源范围后，可以采用[主题资源](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-theme-resources)库中的任何系统颜色或画笔，并在页面或容器范围内重新对其进行定义。

例如，如果在网格中定义了两种系统颜色 - **BaseLow** 和 **BaseMediumLow**，然后在页面上添加了两个按钮：一个按钮在该网格内部，另一个在外部：

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorPaletteResources x:Key="Default"
        BaseLow="LightGreen"
        BaseMediumLow="DarkCyan"/>
    </Grid.Resources>

    <Buton Content="Button_A"/>
</Grid>
<Buton Content="Button_B"/>
```

**Button_A** 将会应用新颜色，**Button_B** 仍类似于系统默认按钮：

![按钮上限定了范围的系统颜色](images/color/scopedcolors_cyan_button.png)

但是，由于所有系统颜色也会传播到其他控件，设置 **BaseLow** 和 **BaseMediumLow** 不只会影响按钮。 在这种情况下，如果 **ToggleButton**、**RadioButton** 和 **Slider** 等控件放在上述示例网格的范围内，则这些控件也会受到这些系统颜色更改的影响。
若要将系统颜色更改的范围限定为单个控件，可以在该控件的资源中定义 **ColorPaletteResources**： 

```xaml
<Grid x:Name="Grid_A">
    <Button Content="Button_A">
        <Button.Resources>
            <ColorPaletteResources x:Key="Default"
                BaseLow="LightGreen"
                BaseMediumLow="DarkCyan"/>
        </Button.Resources>
    </Button>
</Grid>
<Button Content="Button_B"/>
```
实质上结果与前面相同，只不过现在添加到网格中的任何其他控件不会拾取颜色更改。 这是因为，这些系统颜色的范围已限定为 **Button_A**。

### <a name="nesting-scoped-resources"></a>嵌套限定了范围的资源

还可以嵌套系统颜色，为此，可将 **ColorPaletteResources** 放入应用布局标记中嵌套元素的资源内：

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorPaletteResources x:Key="Default"
            BaseLow="LightGreen"
            BaseMediumLow="DarkCyan"/>
    </Grid.Resources>

    <Button Content="Button_A"/>
    <Grid x:Name="Grid_B">
        <Grid.Resources>
            <ColorPaletteResources x:Key="Default"
                BaseLow="Goldenrod"
                BaseMediumLow="DarkGoldenrod"/>
        </Grid.Resources>

        <Button Content="Nested Button"/>
    </Grid>
</Grid>
```

在此示例中，**Button_A** 继承 **Grid_A** 的资源中定义的颜色，**嵌套按钮**从 **Grid_B** 的资源继承颜色。 延伸开来，这意味着，**Grid_B** 中的任何其他控件会在检查或应用 **Grid_A** 的资源之前，先检查或应用 **Grid_B** 的资源，最后，如果未在页面或应用级别定义任何颜色，则应用默认颜色。

这适用于其资源包含颜色定义的任意数目的嵌套元素。

### <a name="scoping-with-a-resourcedictionary"></a>使用 ResourceDictionary 限定范围

你并不局限于容器或页面的资源，还可以在 ResourceDictionary 中定义这些系统颜色，然后，可以像平时合并字典一样，在任何范围合并这些颜色。

#### <a name="mycustomthemexaml"></a>MyCustomTheme.xaml

首先创建一个 ResourceDictionary。 然后将 **ColorPaletteResources** 放入 ThemeDictionaries，并重写所需的系统颜色：

```xaml
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TestApp">

    <ResourceDictionary.ThemeDictionaries>
        <ResourceDictionary x:Key="Default">
            <ResourceDictionary.MergedDictionaries>

                <ColorPaletteResources x:Key="Default"
                    Accent="#FF0073CF"
                    AltHigh="#FF000000"
                    AltLow="#FF000000"/>

            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>        
    </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

#### <a name="mainpagexaml"></a>MainPage.xaml

在包含布局的页面上，只需在所需的范围合并该字典：

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <ResourceDictionary Source="MyCustomTheme.xaml"/>
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
    </Grid.Resources>

    <Button Content="Button_A"/>
</Grid>
```

现在，可将所有资源、主题和自定义颜色放入单个 **MyCustomTheme** 资源字典，并视需要限定范围，而无需担心布局标记变得更混乱。

### <a name="other-ways-to-define-color-resources"></a>定义颜色资源的其他方式

ColorPaletteResources 还允许放置系统颜色，并直接在其中将颜色定义为包装器，而无需以内联方式进行这种定义：

``` xaml
<ColorPaletteResources x:Key="Dark">
    <Color x:Key="SystemBaseLowColor">Goldenrod</Color>
</ColorPaletteResources>
```

## <a name="usability"></a>可用性

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-contrast.svg)
    :::column-end:::
    :::column span="2":::
        **Contrast**

        Make sure that elements and images have sufficient contrast to differentiate between them, regardless of the accent color or theme.

        When considering what colors to use in your application, accessibility should be a primary concern. Use the guidance below to make sure your application is accessible to as many users as possible.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-lighting.svg)
    :::column-end:::
    :::column span="2":::
        **Lighting**

        Be aware that variation in ambient lighting can affect the usability of your app. For example, a page with a black background might unreadable outside due to screen glare, while a page with a white background might be painful to look at in a dark room.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-colorblindness.svg)
    :::column-end:::
    :::column span="2":::
        **Colorblindness**

        Be aware of how colorblindness could affect the usability of your application. For example, a user with red-green colorblindness will have difficulty distinguishing red and green elements from each other. About **8 percent of men** and **0.5 percent of women** are red-green colorblind, so avoid using these color combinations as the sole differentiator between application elements.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>相关文章

- [XAML 样式](../controls-and-patterns/xaml-styles.md)
- [XAML 主题资源](../controls-and-patterns/xaml-theme-resources.md)

---
description: 了解如何在 Windows 应用中使用主题色和主题。
title: Windows 应用中的颜色
ms.date: 04/07/2019
ms.topic: article
keywords: windows 10, uwp
design-contact: karenmui
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f5e103b7661c53fb70561dd1bd654188be2704ff
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970762"
---
# <a name="color"></a>颜色

![主图](images/header-color.svg)

使用颜色可在应用中向用户直观传达信息：颜色可用于指示交互性、针对用户操作提供反馈并保证应用界面的视觉连续性。

在 Windows 应用中，颜色主要由主题色和主题决定。 本文介绍如何在应用中使用颜色，同时介绍如何使用主题色和主题资源来让 Windows 应用可用于任何主题上下文。

## <a name="color-principles"></a>颜色使用原则

:::row:::
    :::column:::
**有效地使用颜色。**
谨慎地使用颜色突出显示重要元素时，颜色可帮助创建流畅直观的用户界面。
    :::column-end:::
    :::column:::
**使用颜色指示交互性。**
可选择一种颜色来指示应用程序中处于交互状态的元素。 例如，许多网页使用蓝色文本表示超链接。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
**可自定义颜色。**
在 Windows 中，用户可选择要在其整个体验中显示的主题色和浅色/深色主题。 可选择按何种方式将用户的主题色及主题纳入应用程序，为用户提供个性化体验。
    :::column-end:::
    :::column:::
**颜色具有文化性。**
请考虑来自不同文化的人们对所用颜色的解读方式。 例如，蓝色在一些文化中象征着美德和保护，而在另一些文化中代表着哀悼。
    :::column-end:::
:::row-end:::

## <a name="themes"></a>主题

Windows 应用可使用浅色或深色应用程序主题。 主题将对应用背景、文本、图标和[常见控件](../controls-and-patterns/index.md)的颜色造成影响。

### <a name="light-theme"></a>浅色主题

![浅色主题](images/color/light-theme.svg)

### <a name="dark-theme"></a>深色主题

![深色主题](images/color/dark-theme.svg)

默认情况下，Windows 应用的主题是 Windows 设置中的用户首选主题或设备的默认主题（即 Xbox 上的深色主题）。 你也可以自己设置 Windows 应用的主题。

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
创建自定义控件的模板时，请使用主题画笔，而不是硬编码颜色值。 这样，应用可轻松适应任何主题。

例如，这些[用于 ListView 的项模板](../controls-and-patterns/item-templates-listview.md)演示了如何在自定义模板中使用主题画笔。
    :::column-end:::
    :::column:::
 ![带图标的双行列表项示例](images/color/list-view.svg)
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
![用户选择的主题标题](images/color/user-accent.svg)
![用户选择的主题色](images/color/user-selected-accent.svg)
    :::column-end:::
    :::column:::
![自定义主题标题](images/color/custom-accent.svg)
![自定义品牌主题色](images/color/brand-color.svg)
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

多个 API 可用于向应用程序添加颜色。 首先，[**Colors**](https://docs.microsoft.com/uwp/api/windows.ui.colors) 类可实现广泛的预定义颜色。 可通过 XAML 属性自动访问这些预定义颜色。 在以下示例中，我们创建了一个按钮并将背景色属性和前景色属性设置为 **Colors** 类的成员。

```xaml
<Button Background="MediumSlateBlue" Foreground="White">Button text</Button>
```

可使用 XAML 中的 [**Color**](https://docs.microsoft.com/uwp/api/windows.ui.color) 结构通过 RGB 或十六进制值自行创建颜色。

```xaml
<Color x:Key="LightBlue">#FF36C0FF</Color>
```

还可使用 **FromArgb** 方法通过代码创建相同的颜色。

```csharp
Color LightBlue = Color.FromArgb(255,54,192,255);
```

“Argb”中的各字母分别代表 Alpha（不透明度）、红、绿和蓝，这四个部分共同组成一种颜色。 每个参数的值可介于 0 至 255 之间。 可选择忽略第一个值，此操作将默认选定 255 或 100% 的透明度。

> [!Note]
> 如果使用 C++，则必须使用 [**ColorHelper**](https://docs.microsoft.com/uwp/api/windows.ui.colorhelper) 类创建颜色。

**Color** 最常用作 [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.solidcolorbrush) 的参数，该参数可用于以单一纯色绘制 UI 元素。 通常在 [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) 中定义这些画笔，以便画笔可重复用于多个元素。

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
- 默认值
  * 显示[浅色](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme)和[深色](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)主题中的颜色变化
- 轻型
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
![对比度图示](images/color/illo-contrast.svg)
    :::column-end:::
    :::column span="2":::
**对比度**

确保元素和图像有足够的对比度来区分它们，不管主题色或主题如何。

考虑在应用程序中使用什么颜色时，应主要考虑可访问性。 请遵循以下指南，确保应用程序可供尽可能多的用户访问。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
![对比度图示](images/color/illo-lighting.svg)
    :::column-end:::
    :::column span="2":::
**照明**

请注意，环境照明的变化可能影响应用的可用性。 例如，黑色背景的页面在室外可能因屏幕眩光而变得不可读，而白色背景的页面则可能在黑暗的房间中难以阅读。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
![对比度图示](images/color/illo-colorblindness.svg)
    :::column-end:::
    :::column span="2":::
**色盲**

请注意色盲因素对应用程序的可用性造成的影响。 例如，红绿色盲用户将难以辨别红色和绿色元素。 大约 **8% 的男性**和 **0.5% 的女性**是红绿色盲，因此请避免单独使用此类颜色组合来区分应用程序元素。
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>相关文章

- [XAML 样式](../controls-and-patterns/xaml-styles.md)
- [XAML 主题资源](../controls-and-patterns/xaml-theme-resources.md)

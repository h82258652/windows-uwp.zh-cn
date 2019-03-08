---
description: 了解如何在 UWP 应用中使用主题色和主题。
title: UWP 应用中的颜色
ms.date: 04/7/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: karenmui
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 49d891888e26b6ce4c9f94e92605eaf7d619b6f3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654252"
---
# <a name="color"></a>颜色

![主图](images/header-color.svg)

通过颜色，可在应用中向用户直观传达信息：颜色可用于指示交互性、针对用户操作提供反馈并保证应用界面的视觉连续性。 

在 UWP 应用中，颜色主要取决于主题色和主题。 本文将介绍如何在应用中使用颜色，同时介绍如何使用主题色和主题资源来让 UWP 应用可用于任何主题上下文。 

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

用户还可选择高对比度主题，该主题使用一块小型的对比色调色板，使界面的对比效果更明显。 在此情况下，系统将替代 RequestedTheme。

### <a name="testing-themes"></a>测试主题

如果不为应用请求主题，请确保用浅色和深色主题测试应用，保证在应用在任何情况下均可清晰显示。

**注意**：在 Visual Studio 中，默认 RequestedTheme 是光，因此将需要更改 RequestedTheme 测试同时。

## <a name="theme-brushes"></a>主题画笔

常见控件自动使用[主题画笔](../controls-and-patterns/xaml-theme-resources.md#the-xaml-color-ramp-and-theme-dependent-brushes)调整浅色和深色主题的对比度。

例如，下图介绍了 [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 使用主题画笔的方式：

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

<!-- check this is true --> 您还可以访问以编程方式使用的强调文字颜色调色板[ **UISettings.GetColorValue** ](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UISettings#Windows_UI_ViewManagement_UISettings_GetColorValue_Windows_UI_ViewManagement_UIColorType_)方法和[ **UIColorType** ](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType)枚举。

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

## <a name="scoping-system-colors"></a>作用域的系统颜色

除了应用程序中定义您自己的颜色，可以还的作用域限定到所需区域我们 systematized 的颜色在整个应用通过使用**ColorSchemeResources**标记。 此 API 允许你不仅为着色和控件通过设置少量属性，但还使您很多其他系统的优点，一次的主题大型组通常不会获取与手动定义您自己的自定义颜色：

- 使用设置的任何颜色**ColorSchemeResources**不会影响高对比度
  * 这意味着您的应用程序可以访问更多的人，而无需任何额外的设计或开发人员成本
- 可以轻松地设置颜色的浅色、 深色或普遍在这两个主题通过在 API 上设置一个属性
- 颜色集上**ColorSchemeResources**将向下级联到所有类似的控件还使用该系统颜色
  * 这可确保的将维护自己的品牌的外观时您的应用程序中具有一致的颜色情景
- 影响所有可视状态、 动画和不透明度变体，而无需重新创建模板

### <a name="how-to-use-colorschemeresources"></a>如何使用 ColorSchemeResources

ColorSchemeResources 是告知哪些资源正在的系统范围内的位置的 API。 ColorSchemeResources 必须采取[X:key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute)，这可以是三个选项之一：
- 默认
  * 将颜色更改显示在这种[Light](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme)并[深色](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)主题
- 浅色
  * 将显示您的颜色更改仅在[浅色主题](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme) 
- 深色
  * 将显示您的颜色更改仅在[深色主题](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)

设置该注册表项 x： 将确保您的颜色相应地更改为系统或应用主题，你应该在任一主题时不同的自定义外观。

### <a name="how-to-apply-scoped-colors"></a>如何将应用作用域内的颜色

范围通过资源**ColorSchemeResources**在 XAML 中的 API，可采用任何系统颜色或画笔中我们[主题资源](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-theme-resources)库和页范围内重新定义它们或容器。

例如，如果定义了两种系统颜色的**SystemBaseLowColor**并**SystemBaseMediumLowColor**内一个网格，并将其放在页面上的两个按钮： 一个在该网格内的，一个外部：

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorSchemeResources x:Key="Default" 
        SystemBaseLowColor="LightGreen" 
        SystemBaseMediumLowColor="DarkCyan"/>
    </Grid.Resources>

    <Buton Content="Button_A"/>
</Grid>
<Buton Content="Button_B"/>
```

你会得到**Button_A**与应用的新颜色，并**Button_B**仍将像我们系统默认按钮外观：

![在按钮上的作用域内的系统颜色](images/color/scopedcolors_cyan_button.png)

但是，由于我们的所有系统颜色向下级都联到其他控件过，则将设置**SystemBaseLowColor**并**SystemBaseMediumLowColor**将影响多个只是按钮。 在这种情况下，控件，如**ToggleButton**， **RadioButton**和**滑块**将也会受到影响通过这些系统的颜色更改，将这些控件被放入以上 exampl网格的作用域。
如果你想要一种系统颜色更改作用域*到单个控件，仅*就可以做到定义**ColorSchemeResources**该控件的资源中：

```xaml
<Grid x:Name="Grid_A">
    <Button Content="Button_A">
        <Button.Resources>
            <ColorSchemeResources x:Key="Default" 
                SystemBaseLowColor="LightGreen" 
                SystemBaseMediumLowColor="DarkCyan"/>
        </Button.Resources>
    </Button>
</Grid>
<Button Content="Button_B"/>
```
实质上是具有完全相同的操作为之前，但现在将向网格添加的任何其他控件不会选取颜色更改。 这是因为这些系统颜色都只限于**Button_A**仅。

### <a name="nesting-scoped-resources"></a>嵌套作用域内的资源

嵌套的系统颜色也是有可能，和执行此操作通过将放**ColorSchemeResources**应用布局的标记内的嵌套的元素的资源中：

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorSchemeResources x:Key="Default"
            SystemBaseLowColor="LightGreen"
            SystemBaseMediumLowColor="DarkCyan"/>
    </Grid.Resources>

    <Button Content="Button_A"/>
    <Grid x:Name="Grid_B">
        <Grid.Resources>
            <ColorSchemeResources x:Key="Default"
                SystemBaseLowColor="Goldenrod"
                SystemBaseMediumLowColor="DarkGoldenrod"/>
        </Grid.Resources>

        <Button Content="Nested Button"/>
    </Grid>
</Grid>
```

在此示例中， **Button_A**是在中定义继承的颜色**Grid_A**的资源，并**嵌套按钮**颜色从其继承**Grid_B**的资源。 通过扩展，这意味着任何其他控件放在**Grid_B**将检查或应用**Grid_B**的资源前，之前检查或应用**Grid_A**的资源，以及最后应用我们默认颜色，在页面或应用程序级别定义的任何内容。

这适用于任意数量的嵌套元素的资源具有颜色定义。

### <a name="scoping-with-a-resourcedictionary"></a>使用 ResourceDictionary 作用域

并不局限于一个容器或页面的资源，并还可以在以后可合并在任何范围内通常将合并字典的方法的 ResourceDictionary 中定义这些系统颜色。

#### <a name="mycustomthemexaml"></a>MyCustomTheme.xaml

首先，您需要创建 ResourceDictionary。 然后将放**ColorPaletteResources** ThemeDictionaries 中重写所需的系统颜色和：

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

在包含你的布局页上，只需合并该字典中在所需的作用域：

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

现在，在单个放置所有资源、 主题和自定义颜色**MyCustomTheme**资源字典，而范围限定在需要时无需担心如何布局标记中的额外混乱。

### <a name="other-ways-to-define-color-resources"></a>其他方法来定义颜色资源

ColorSchemeResources 也允许放置种系统颜色和为一个包装，而不在行中直接在其中定义：

``` xaml
<ColorSchemeResources x:Key="Dark">
    <Color x:Key="SystemBaseLowColor">Goldenrod</Color>
</ColorSchemeResources>
```

## <a name="usability"></a>可用性

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-contrast.svg)
    :::column-end:::
    :::column span="2":::
        **Contrast**

        Make sure that elements and images have sufficient contrast to differentiate between them, regardless of the accent color or theme.

        When considering what colors to use in your application, accessiblity should be a primary concern. Use the guidance below to make sure your application is accessible to as many users as possible.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-lighting.svg)
    :::column-end:::
    :::column span="2":::
        **Lighting**

        Be aware that variation in ambient lighting can affect the useability of your app. For example, a page with a black background might unreadable outside due to screen glare, while a page with a white background might be painful to look at in a dark room.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-colorblindness.svg)
    :::column-end:::
    :::column span="2":::
        **Colorblindness**

        Be aware of how colorblindness could affect the useability of your application. For example, a user with red-green colorblindness will have difficulty distinguishing red and green elements from each other. About **8 percent of men** and **0.5 percent of women** are red-green colorblind, so avoid using these color combinations as the sole differentiator between application elements.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>相关文章

- [XAML 样式](../controls-and-patterns/xaml-styles.md)
- [XAML 主题资源](../controls-and-patterns/xaml-theme-resources.md)

---
author: QuinnRadich
description: 了解如何在 UWP 应用中使用主题色和主题。
title: UWP 应用中的颜色
ms.author: quradic
ms.date: 4/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
design-contact: karenmui
ms.localizationpriority: medium
ms.openlocfilehash: 19f4d9cde6ee2bc9615f044f18bc5e8828ca1985
ms.sourcegitcommit: 53ba430930ecec8ea10c95b390fe6e654fe363e1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2018
ms.locfileid: "3421295"
---
# <a name="color"></a>颜色

![主图](images/header-color.svg)

通过颜色，可在应用中向用户直观传达信息：颜色可用于指示交互性、针对用户操作提供反馈并保证应用界面的视觉连续性。 

在 UWP 应用中，颜色主要取决于主题色和主题。 本文将介绍如何在应用中使用颜色，同时介绍如何使用主题色和主题资源来让 UWP 应用可用于任何主题上下文。 

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
        **颜色为个人。**
在 Windows 中，用户可选择要在其整个体验中显示的主题色和浅色/深色主题。 可选择按何种方式将用户的主题色及主题纳入应用程序，进而为用户提供个性化体验。
    :::column-end:::
    :::column:::
        **颜色具有文化性。**
请考虑来自不同文化的人们对所用颜色的解读方式。 例如，蓝色在一些文化中象征着美德和保护，而在另一些文化中代表着哀悼。
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

**注意**：Visual Studio 中的 RequestedTheme 默认为浅色，因此需要更改 RequestedTheme 来测试两种主题。

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
        创建自定义控件的模板时，使用主题画笔，而不是硬编码颜色值。 这样，应用可轻松适应任何主题。

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
        ![用户选择主题色标头](images/color/user-accent.svg)![用户选择主题色](images/color/user-selected-accent.svg)
    :::column-end:::
    :::column:::
        ![自定义主题色标头](images/color/custom-accent.svg)![自定义品牌主题色](images/color/brand-color.svg)
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

![颜色编号](images/color/color-on-color.svg)

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

## <a name="usability"></a>可用性

:::row:::
    :::column:::
        ![对比度图](images/color/illo-contrast.svg)
    :::column-end:::
    ::: 列范围 ="2":::**对比度**

        Make sure that elements and images have sufficient contrast to differentiate between them, regardless of the accent color or theme.

        When considering what colors to use in your application, accessiblity should be a primary concern. Use the guidance below to make sure your application is accessible to as many users as possible.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![对比度图](images/color/illo-lighting.svg)
    :::column-end:::
    ::: 列范围 ="2":::**照明**

        Be aware that variation in ambient lighting can affect the useability of your app. For example, a page with a black background might unreadable outside due to screen glare, while a page with a white background might be painful to look at in a dark room.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![对比度图](images/color/illo-colorblindness.svg)
    :::column-end:::
    ::: 列范围 ="2":::**色盲**

        Be aware of how colorblindness could affect the useability of your application. For example, a user with red-green colorblindness will have difficulty distinguishing red and green elements from each other. About **8 percent of men** and **0.5 percent of women** are red-green colorblind, so avoid using these color combinations as the sole differentiator between application elements.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>相关文章

- [XAML 样式](../controls-and-patterns/xaml-styles.md)
- [XAML 主题资源](../controls-and-patterns/xaml-theme-resources.md)
---
author: jwmsft
description: 使用根据当前处于活动状态的主题检索不同资源的附加系统逻辑，通过计算对某个资源的引用来为任何 XAML 属性提供值。
title: ThemeResource 标记扩展
ms.assetid: 8A1C79D2-9566-44AA-B8E1-CC7ADAD1BCC5
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 024e48380941c0d79eef65780396ec9b89edc3c7
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "7147648"
---
# <a name="themeresource-markup-extension"></a>{ThemeResource} 标记扩展

使用根据当前处于活动状态的主题检索不同资源的附加系统逻辑，通过计算对某个资源的引用来为任何 XAML 属性提供值。 与 [{StaticResource} 标记扩展](staticresource-markup-extension.md)类似，资源在 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 中定义，并且 **ThemeResource** 用法引用 **ResourceDictionary** 中的该资源的键。

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法

``` syntax
<object property="{ThemeResource key}" .../>
```

## <a name="xaml-values"></a>XAML 值

| 术语 | 说明 |
|------|-------------|
| 键 | 所请求资源的键。 此键最初通过 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 分配。 资源键可以是以 XamlName 语法定义的任何字符串。 |

## <a name="remarks"></a>备注

**ThemeResource** 是用于获取在 XAML 资源字典中的其他地方定义的 XAML 属性值的技术。 该标记扩展提供与 [{StaticResource} 标记扩展](staticresource-markup-extension.md)相同的基本用途。 与 {StaticResource} 标记扩展的行为差异在于 **ThemeResource** 引用可以根据系统当前在使用哪个主题动态使用不同的字典作为主要查找位置。

首次启动应用时，将根据启动时正在使用的主题对由 **ThemeResource** 引用执行的任何资源引用进行计算。 但是，如果用户之后在运行时更改了活动主题，则系统将重新计算每个 **ThemeResource** 引用、检索可能不同的特定于主题的资源，并在可视化树中所有合适的位置使用新的资源值重新显示应用。 **StaticResource** 在 XAML 加载时/应用启动时确定，并且在运行时不会重新计算。 （还有可以动态重新加载 XAML 的其他技术，例如可视状态，但是这些技术在比由 [{StaticResource} 标记扩展](staticresource-markup-extension.md)启用的基本资源计算更高的级别上操作）。

**ThemeResource** 接受一个参数，该参数指定所请求的资源的键。 在 Windows 运行时 XAML 中，资源键始终是一个字符串。 有关最初如何指定资源键的详细信息，请参阅 [x:Key 属性](x-key-attribute.md)。

有关如何定义资源和正确使用 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 的详细信息（包括示例代码），请参阅 [ResourceDictionary 和 XAML 资源引用](https://msdn.microsoft.com/library/windows/apps/mt187273)。

**重要提示** 与 **StaticResource** 一样，**ThemeResource** 不应当尝试对在 XAML 文件中定义的在字典顺序上位于更靠后位置的资源进行前向引用。 这样的尝试不受支持。 即使前向引用没有失败，尝试进行这样的引用也会对性能造成不利影响。 为实现最佳效果，请调整你的资源字典的组成，以避免使用前向引用。

尝试为无法解析的键指定 **ThemeResource** 会在运行时引发 XAML 分析异常。 设计工具还可能会提供警告或错误。

在 Windows 运行时 XAML 处理器实现中，没有针对 **ThemeResource** 的后备类表示。 在代码中最接近的等效体是使用 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 的集合 API，例如调用 [**Contains**](https://msdn.microsoft.com/library/windows/apps/jj635925) 或 [**TryGetValue**](https://msdn.microsoft.com/library/windows/apps/jj603139)。

**ThemeResource** 是一个标记扩展。 当需要将属性值转义为除文字值或处理程序名称之外的值时，以及当需求更具全局性而不是仅仅将类型转换器放在某些类型或属性上时，通常需要实现标记扩展。 XAML 中的所有标记扩展在其属性语法中都使用“{”和“}”字符，通过此约定，XAML 处理器可以知道标记扩展必须处理属性。

### <a name="when-and-how-to-use-themeresource-rather-than-staticresource"></a>何时以及如何使用 {ThemeResource} 而不是 {StaticResource}

**ThemeResource** 解析为资源字典中的项时所遵循的规则通常与 **StaticResource** 相同。 **ThemeResource** 查找可以扩展到 [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208807) 集合中引用的 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 文件，但 **StaticResource** 也可以这样做。 区别是 **ThemeResource** 在运行时可以重新计算，而 **StaticResource** 不能。

无论哪个主题处于活动状态，每个主题字典中的键集都应当提供相同的键控资源集。 如果某个给定的键控资源存在于 **HighContrast** 主题字典中，则 **Light** 和 **Default** 中也应当存在具有该名称的另一资源。 如果不是这样，则当用户切换主题时资源查找可能会失败，并且你的应用看起来将不正常。 但是，主题字典可以包含仅从同一范围内引用的键控资源来提供子值；这些不需要在所有主题中都等效。

通常，只有当这些值可以在主题之间更改时或者受更改的值支持时，你才应当将资源放置在主题字典中并使用 **ThemeResource** 引用这些资源。 这适合以下类型的资源：

-   画笔，特别是用于 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) 的颜色。 在默认的 XAML 控件模板 (generic.xaml) 中，这大约占 **ThemeResource** 使用情况的 80%。
-   用于边框、偏移、边距和填充等的像素值。
-   字体属性，例如 **FontFamily** 或 **FontSize**。
-   用于有限数目的控件的完整模板，这些控件通常是系统样式的并且用于动态展示，例如 [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/hh738501) 和 [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/br242919)。
-   文本显示样式（通常更改字体颜色、背景，还可能更改大小）。

Windows 运行时提供了专门由 **ThemeResource** 引用的资源集。 这些资源将全部作为 XAML 文件 themeresources.xaml 的一部分列出，该文件在包含于 Windows 软件开发工具包 (SDK) 的 include/winrt/xaml/design 文件夹中提供。 有关在 themeresources.xaml 中定义的主题画笔和其他样式的文档，请参阅 [XAML 主题资源](https://msdn.microsoft.com/library/windows/apps/mt187274)。 画笔将在表格中记录，该表格将告知你，针对三个可能活动的主题，其中每个画笔将具有哪些颜色值。

每当存在可能会因主题更改而更改的基础资源时，控件模板中的可视状态的 XAML 定义都应当使用 **ThemeResource** 引用。 系统主题更改通常也不会导致可视状态更改。 在这种情况下，资源需要使用 **ThemeResource** 引用，以便可以为仍然处于活动状态的可视状态重新计算值。 例如，如果你具有更改特定 UI 部件的画笔颜色及其属性之一的可视状态，并且该画笔颜色对于每个主题是不同的，则你应当使用 **ThemeResource** 引用在默认模板中提供该属性的值以及对该默认模板进行任何可视状态修改。

可能会在一系列具有依赖关系的值中看到 **ThemeResource** 用法。 例如，由同时还是键控资源的 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) 使用的 [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723)值可能会使用 **ThemeResource** 引用。 但是，任何使用键控 **SolidColorBrush** 资源的 UI 属性也将使用 **ThemeResource** 引用，以便每个启用了动态值的 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 类型属性在主题更改时会随之更改。

**注意**`{ThemeResource}`和主题切换的运行时资源评估是 Windows8.1 XAML 中支持，但在 XAML 中的应用不支持面向 Windows8。

### <a name="system-resources"></a>系统资源

某些主题资源引用系统资源值作为基础子值。 系统资源是不存在于任何 XAML 资源字典中的特殊资源值。 这些值依赖于 Windows 运行时 XAML 支持中的行为转发来自系统本身的值，并以 XAML 资源可以引用的形式表示它们。 例如，有一个名为“SystemColorButtonFaceColor”的系统资源，它提供了一种 RGB 颜色。 此颜色来自非仅特定于 Windows 运行时和 Windows 运行时应用的系统颜色和主题的外表。

系统资源通常是某个高对比度主题的基础值。 用户可以控制其高对比度主题的颜色选择，并且用户使用同样非特定于 Windows 运行时应用的系统功能执行这些选择。 通过将系统资源引用为 **ThemeResource** 引用，Windows 运行时应用的高对比度主题的默认行为可以使用这些由用户控制的且由系统公开的特定于主题的值。 另外，这些引用现在已标记为当系统检测到运行时主题更改时进行重新计算。

### <a name="an-example-themeresource-usage"></a>示例 {ThemeResource} 用法

下面是摘自默认的 generic.xaml 和 themeresources.xaml 文件的某个示例 XAML，它展示了如何使用 **ThemeResource**。 我们将只查看一个模板（默认的 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)）并了解如何将两个属性（[**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) 和 [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414)）声明为对主题更改做出响应。

```xml
    <!-- Default style for Windows.UI.Xaml.Controls.Button -->
    <Style TargetType="Button">
        <Setter Property="Background" Value="{ThemeResource ButtonBackgroundThemeBrush}" />
        <Setter Property="Foreground" Value="{ThemeResource ButtonForegroundThemeBrush}"/>
...
```

在此处，属性接受一个 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 值，并且使用 **ThemeResource** 引用名为 `ButtonBackgroundThemeBrush` 和 `ButtonForegroundThemeBrush` 的 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) 资源。

[**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 的某些可视状态也调整这些相同的属性。 值得注意的是，当单击某个按钮时，背景颜色会更改。 另外，可视状态情节提要中的 [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) 和 [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414) 动画使用 [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/br243132) 对象和对以 **ThemeResource** 作为关键帧值的画笔的引用。

```xml
<VisualState x:Name="Pressed">
  <Storyboard>
    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Border"
        Storyboard.TargetProperty="Background">
      <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonPressedBackgroundThemeBrush}" />
    </ObjectAnimationUsingKeyFrames>
    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter"
         Storyboard.TargetProperty="Foreground">
       <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonPressedForegroundThemeBrush}" />
    </ObjectAnimationUsingKeyFrames>
  </Storyboard>
</VisualState>
```

其中每个画笔都在 generic.xaml 中较早定义：必须在任何使用它们的模板之前定义它们以避免 XAML 前向引用。 下面是针对“Default”主题字典的定义。

```xml
    <ResourceDictionary.ThemeDictionaries>
        <ResourceDictionary x:Key="Default">
...
            <SolidColorBrush x:Key="ButtonBackgroundThemeBrush" Color="Transparent" />
            <SolidColorBrush x:Key="ButtonForegroundThemeBrush" Color="#FFFFFFFF" />
...
            <SolidColorBrush x:Key="ButtonPressedBackgroundThemeBrush" Color="#FFFFFFFF" />
            <SolidColorBrush x:Key="ButtonPressedForegroundThemeBrush" Color="#FF000000" />
...
```

然后，每个其他主题字典也定义了这些画笔，例如：

```xml
        <ResourceDictionary x:Key="HighContrast">
            <!-- High Contrast theme resources -->
...
            <SolidColorBrush x:Key="ButtonBackgroundThemeBrush" Color="{ThemeResource SystemColorButtonFaceColor}" />
            <SolidColorBrush x:Key="ButtonForegroundThemeBrush" Color="{ThemeResource SystemColorButtonTextColor}" />

...
            <SolidColorBrush x:Key="ButtonPressedBackgroundThemeBrush" Color="{ThemeResource SystemColorButtonTextColor}" />
            <SolidColorBrush x:Key="ButtonPressedForegroundThemeBrush" Color="{ThemeResource SystemColorButtonFaceColor}" />
```

在此处，[**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) 值是对某个系统资源的另一个 **ThemeResource** 引用。 如果你引用了某个系统资源，并且希望它发生更改以响应主题更改，则应当使用 **ThemeResource** 进行该引用。

## <a name="windows8-behavior"></a>Windows8 行为

Windows8 不支持**ThemeResource**标记扩展，它是从 Windows8.1 开始提供。 此外，Windows8 不支持动态切换 Windows 运行时应用的主题相关资源。 必须重新启动应用，才能应用针对 XAML 模板和样式的主题更改。 这不是良好的用户体验，因此应用不重新编译和目标 Windows8.1 强烈建议，以便他们可以使用**ThemeResource**用法使用样式和动态切换主题时在用户执行。 编译的应用的 Windows8 Windows8.1 上运行将继续使用 Windows8 行为。

## <a name="design-time-tools-support-for-the-themeresource-markup-extension"></a>设计时工具支持 **{ThemeResource}** 标记扩展

当你在 XAML 页面中使用 **{ThemeResource}** 标记扩展时，Microsoft Visual Studio2013 可以在 Microsoft IntelliSense 下拉菜单中包含可能的键值。 例如，键入“{ThemeResource”时会立即显示来自 [XAML 主题资源](https://msdn.microsoft.com/library/windows/apps/mt187274)的任何资源键。

在资源键作为任何 **{ThemeResource}** 用法的一部分存在后，**转到定义** (F12) 功能可以解析该资源并向你显示设计时 generic.xaml、定义主题资源的位置。 由于多次定义了主题资源（每个主题），“转到定义”**** 会将你转到文件中找到的第一个定义（**Default** 的定义）。 如果你需要其他定义，可以在该文件中搜索键名并找到其他主题的定义。

## <a name="related-topics"></a>相关主题

* [ResourceDictionary 和 XAML 资源引用](https://msdn.microsoft.com/library/windows/apps/mt187273)
* [XAML 主题资源](https://msdn.microsoft.com/library/windows/apps/mt187274)
* [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)
* [x:Key 特性](x-key-attribute.md)
 


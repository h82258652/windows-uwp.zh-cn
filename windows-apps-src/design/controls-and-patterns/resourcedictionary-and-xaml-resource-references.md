---
author: Jwmsft
Description: Explains how to define a ResourceDictionary element and keyed resources, and how XAML resources relate to other resources that you define as part of your app or app package.
MS-HAID: dev\_ctrl\_layout\_txt.resourcedictionary\_and\_xaml\_resource\_references
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: ResourceDictionary 和 XAML 资源引用
ms.assetid: E3CBFA3D-6AF5-44E1-B9F9-C3D3EA8A25CE
label: ResourceDictionary and XAML resource references
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8b5d2a55610b6cec2f9026a5834b00ad7015a9c6
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5920920"
---
# <a name="resourcedictionary-and-xaml-resource-references"></a>ResourceDictionary 和 XAML 资源引用

 

你可以使用 XAML 为你的应用定义 UI 或资源。 资源通常是一些你预期多次使用的对象的定义。 为了在以后引用 XAML 资源，你为资源指定一个键，用于充当其名称。 你可以在整个应用中或从应用中的任意 XAML 页面引用资源。 你可以使用 Windows 运行时 XAML 中的 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 元素定义你的资源。 然后，可使用 [StaticResource 标记扩展](../../xaml-platform/staticresource-markup-extension.md)或 [ThemeResource 标记扩展](../../xaml-platform/themeresource-markup-extension.md)来引用你的资源。

你最希望声明为 XAML 资源的 XAML 元素包括 [Style](https://msdn.microsoft.com/library/windows/apps/br208849)、[ControlTemplate](https://msdn.microsoft.com/library/windows/apps/br209391)、动画组件和 [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush) 子类。 我们在此处介绍如何定义 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 和键控资源，以及 XAML 资源与你定义为应用或应用包一部分的其他资源有何关系。 我们还介绍资源字典高级功能，如 [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801) 和 [ThemeDictionaries](https://msdn.microsoft.com/library/windows/apps/br208807)。

**先决条件**

我们假设你了解 XAML 标记并已阅读 [XAML 概述](https://msdn.microsoft.com/library/windows/apps/mt185595)。

## <a name="define-and-use-xaml-resources"></a>定义和使用 XAML 资源

XAML 资源是多次从标记中引用的对象。 资源在 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 中定义，通常位于单独的文件中或位于标记页面的顶部，如下所示。

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <x:String x:Key="greeting">Hello world</x:String>
        <x:String x:Key="goodbye">Goodbye world</x:String>
    </Page.Resources>

    <TextBlock Text="{StaticResource greeting}" Foreground="Gray" VerticalAlignment="Center"/>
</Page>
```

在本示例中：

-   `<Page.Resources>…</Page.Resources>` - 定义资源字典。
-   `<x:String>` - 定义带有键“greeting”的资源。
-   `{StaticResource greeting}` - 查找带有键“greeting”的资源，它已分配给 [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) 的 [Text](https://msdn.microsoft.com/library/windows/apps/br209676) 属性。

> **注意**&nbsp;&nbsp; [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 相关的概念与“**资源**生成操作、资源 (.resw) 文件或在构建生成应用包的代码项目上下文中讨论的其他“资源”相混淆。

资源不需要作为字符串；它们可以作为任何可共享的对象，例如样式、目标、画笔和颜色。 但是，控件、形状和其他 [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/br208706) 不可共享，因此不能将其声明为可重复使用的资源。 有关共享的详细信息，请参阅本主题后面的 [XAML 资源必须可共享](#xaml-resources-must-be-shareable)部分。

在此处，画笔和字符串都声明为资源，并由页面中的控件使用。

```XAML
<Page
    x:Class="SpiderMSDN.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <SolidColorBrush x:Key="myFavoriteColor" Color="green"/>
        <x:String x:Key="greeting">Hello world</x:String>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource myFavoriteColor}" Text="{StaticResource greeting}" VerticalAlignment="Top"/>
    <Button Foreground="{StaticResource myFavoriteColor}" Content="{StaticResource greeting}" VerticalAlignment="Center"/>
</Page>
```

所有资源都需要有一个键。 通常，键是通过 `x:Key=”myString”` 定义的字符串。 但是，还有几种其他方法可指定键：

-   如果未指定 [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787)，则 [Style](https://msdn.microsoft.com/library/windows/apps/br208849) 和 [ControlTemplate](https://msdn.microsoft.com/library/windows/apps/br209391) 需要 **TargetType**，并且将 **TargetType** 用作键。 在这种情况下，键是实际的 Type 对象，而非字符串。 （请参阅下方示例）
-   如果未指定 [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787)，具有 **TargetType** 的 [DataTemplate](https://msdn.microsoft.com/library/windows/apps/br242348) 资源会将 **TargetType** 用作键。 在这种情况下，键是实际的 Type 对象，而非字符串。
-   [x:Name](https://msdn.microsoft.com/library/windows/apps/mt204788) 可以代替 [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787) 使用。 但是，x:Name 还会为资源生成代码隐藏字段。 因此，x:Name 的效率低于 x:Key，因为该字段需要在页面加载时进行初始化。

[StaticResource 标记扩展](../../xaml-platform/staticresource-markup-extension.md)可以仅使用字符串名称（[x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787) 或 [x:Name](https://msdn.microsoft.com/library/windows/apps/mt204788)）检索资源。 但是，XAML 框架在决定为尚未设置 [Style](https://msdn.microsoft.com/library/windows/apps/br208743) 和 [ContentTemplate](https://msdn.microsoft.com/library/windows/apps/br209369) 或 [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/br242830) 属性的控件使用哪个样式和模板时，还将查找隐式样式资源（使用 **TargetType** 而非 x:Key 或 x:Name 的资源）。

此处的 [Style](https://msdn.microsoft.com/library/windows/apps/br208849) 具有隐式键 **typeof(Button)**，并且由于页面底部的 [Button](https://msdn.microsoft.com/library/windows/apps/br209265) 未指定 [Style](https://msdn.microsoft.com/library/windows/apps/br208743) 属性，它将查找具有 **typeof(Button)** 键的样式：

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <Style TargetType="Button">
            <Setter Property="Background" Value="Red"/>
        </Style>
    </Page.Resources>
    <Grid>
       <!-- This button will have a red background. -->
       <Button Content="Button" Height="100" VerticalAlignment="Center" Width="100"/>
    </Grid>
</Page>
```

有关隐式样式及其工作方式的详细信息，请参阅[设置控件样式](xaml-styles.md)和[控件模板](control-templates.md)。

## <a name="look-up-resources-in-code"></a>查找代码中的资源

像任何其他字典一样，访问资源字典的成员。

> [!WARNING]
> 当你执行在代码中查找资源的操作时，仅找到 `Page.Resources` 字典中的资源。 与 [StaticResource 标记扩展](../../xaml-platform/staticresource-markup-extension.md)不同，如果未在第一个字典中找到这些资源，该代码不会回退到 `Application.Resources` 字典。

 

此示例演示如何在页面的资源字典中检索出 `redButtonStyle` 资源。

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <Style TargetType="Button" x:Key="redButtonStyle">
            <Setter Property="Background" Value="red"/>
        </Style>
    </Page.Resources>
</Page>
```

```CSharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            Style redButtonStyle = (Style)this.Resources["redButtonStyle"];
        }
    }
```

若要在代码中查找应用范围的资源，请使用 **Application.Current.Resources** 获取应用的资源目录，如下所示。

```XAML
<Application
    x:Class="MSDNSample.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SpiderMSDN">
    <Application.Resources>
        <Style TargetType="Button" x:Key="appButtonStyle">
            <Setter Property="Background" Value="red"/>
        </Style>
    </Application.Resources>

</Application>
```

```CSharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            Style appButtonStyle = (Style)Application.Current.Resources["appButtonStyle"];
        }
    }
```

你还可以在代码中添加应用程序资源。

以下是在执行此操作时要记住的两个事项：

-   第一，在任何页面尝试使用资源时，你需要先添加资源。
-   第二，不能在应用的构造函数中添加资源。

如果在 [Application.OnLaunched](https://msdn.microsoft.com/library/windows/apps/br242335) 方法中添加资源，可以避免这两个问题，如下所示。

```CSharp
// App.xaml.cs
    
sealed partial class App : Application
{
    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;
        if (rootFrame == null)
        {
            SolidColorBrush brush = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 0, 255, 0)); // green
            this.Resources["brush"] = brush;
            // … Other code that VS generates for you …
        }
    }
}
```

## <a name="every-frameworkelement-can-have-a-resourcedictionary"></a>每个 FrameworkElement 都可以具有 ResourceDictionary

[FrameworkElement](https://msdn.microsoft.com/library/windows/apps/br208706) 是控件所继承的基类，并且具有 [Resources](https://msdn.microsoft.com/library/windows/apps/br208740) 属性。 因此你可以将本地资源字典添加到任何 **FrameworkElement**。

此时，[Page](https://msdn.microsoft.com/library/windows/apps/br227503) 和 [Border](https://msdn.microsoft.com/library/windows/apps/br209250) 都具有资源字典，并且都具有名为“greeting”的资源。 名为 textBlock2 [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)位于**边框**，因此其资源查找**边框**的资源，则该**页面**的资源，然后[应用程序](https://msdn.microsoft.com/library/windows/apps/br242324)资源。 **TextBlock** 将读取“Hola mundo”。

若要从代码访问元素的资源，请使用该元素的 [Resources](https://msdn.microsoft.com/library/windows/apps/br208740) 属性。 在代码（而非 XAML）中访问 [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/br208706) 的资源，将仅在该字典中查找，而不在父级元素的字典中查找。

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <x:String x:Key="greeting">Hello world</x:String>
    </Page.Resources>
    
    <StackPanel>
        <!-- Displays "Hello world" -->
        <TextBlock x:Name="textBlock1" Text="{StaticResource greeting}"/>

        <Border x:Name="border">
            <Border.Resources>
                <x:String x:Key="greeting">Hola mundo</x:String>
            </Border.Resources>
            <!-- Displays "Hola mundo" -->
            <TextBlock x:Name="textBlock2" Text="{StaticResource greeting}"/>
        </Border>

        <!-- Displays "Hola mundo", set in code. -->
        <TextBlock x:Name="textBlock3"/>
    </StackPanel>
</Page>

```

```CSharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            textBlock3.Text = (string)border.Resources["greeting"];
        }
    }
```

## <a name="merged-resource-dictionaries"></a>合并的资源字典

*合并的资源字典*将一个资源字典合并到通常在其他文件中的另一个字典。

> **提示**&nbsp;&nbsp;可以在 Microsoft Visual Studio 中创建资源字典文件，方法是在**项目**菜单中依次使用**添加 &gt; 新建项目…&gt; 资源字典**选项。

此时，可以在名为 Dictionary1.xaml 的单独 XAML 文件中定义资源字典。

```XAML
<!-- Dictionary1.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="Red"/>

</ResourceDictionary>

```

若要使用该字典，请将它与页面的字典合并：

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml"/>
            </ResourceDictionary.MergedDictionaries>

            <x:String x:Key="greeting">Hello world</x:String>

        </ResourceDictionary>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource brush}" Text="{StaticResource greeting}" VerticalAlignment="Center"/>
</Page>
```

下面是本示例中会出现的结果。 在 `<Page.Resources>` 中，声明 `<ResourceDictionary>`。 当你向 `<Page.Resources>` 添加资源时，XAML 框架将为你隐式创建资源字典；但是在这种情况下，你需要的不仅是任一资源字典，你还需要包含合并字典的资源字典。

因此请声明 `<ResourceDictionary>`，然后将内容添加到其 `<ResourceDictionary.MergedDictionaries>` 集合。 其中每个条目都采用 `<ResourceDictionary Source="Dictionary1.xaml"/>` 形式。 若要添加多个字典，只需在第一个条目后添加 `<ResourceDictionary Source="Dictionary2.xaml"/>` 条目即可。

在 `<ResourceDictionary.MergedDictionaries>…</ResourceDictionary.MergedDictionaries>` 后，你可以选择在主字典中放置其他资源。 你可以使用要合并到的字典中的资源，如同使用常规字典一样。 在上述示例中，`{StaticResource brush}` 在子级/合并字典 (Dictionary1.xaml) 中查找资源，而 `{StaticResource greeting}` 在主页字典中查找其资源。

在资源查找序列中，仅在检查 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 的所有其他键控资源后，才会检查 [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801) 字典。 搜索该级别后，查找会深入到合并后的字典，且对 **MergedDictionaries** 中的每一项进行检查。 如果存在多个合并的字典，会按在 **MergedDictionaries** 属性中声明这些字典的顺序的相反顺序来检查它们。 在以下示例中，如果 Dictionary2.xaml 和 Dictionary1.xaml 声明同一个键，则首先使用来自 Dictionary2.xaml 中的键，因为它排在 **MergedDictionaries** 集的末尾。

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml"/>
                <ResourceDictionary Source="Dictionary2.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource brush}" Text="greetings!" VerticalAlignment="Center"/>
</Page>
```

在任一 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 范围内，均会检查字典中键的唯一性。 但是，这一范围不会扩展到不同 [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801) 文件中的不同项。

可以结合使用查找序列和跨合并字典范围不强制使用唯一键来创建 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 资源的回退值序列。 例如，你可能会使用与应用的状态数据和用户首选项数据同步的资源词典，为序列中最后合并的资源字典中的特殊画笔颜色存储用户首选项。 但是，如果尚不存在任何用户首选项，则可为初始 [**MergedDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208801) 文件中的 ResourceDictionary 资源定义相同的键字符串，并可将其用作回退值。 请记住，始终会在检查合并字典之前检查你在主要资源字典中提供的任何值，所以如果希望使用回退技术，则不要在主要资源字典中定义该资源。

## <a name="theme-resources-and-theme-dictionaries"></a>主题资源和主题字典

[ThemeResource](../../xaml-platform/themeresource-markup-extension.md) 类似于 [StaticResource](../../xaml-platform/staticresource-markup-extension.md)，但资源查找会在主题更改时进行重新评估。

在此示例中，将 [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) 的前景设置为当前主题中的值。

```XAML
<TextBlock Text="hello world" Foreground="{ThemeResource FocusVisualWhiteStrokeThemeBrush}" VerticalAlignment="Center"/>
```

主题字典是一种特殊类型的合并字典，用于保存各种资源，具体资源取决于用户当前在其设备上使用的主题。 例如，“浅色”主题可能使用白色画笔，而“深色”主题可能使用黑色画笔。 画笔会更改它所溶入的资源，但使用该画笔作为资源的控件的组成可能保持不变。 若要在个人模板或样式中重现主题切换行为而不将 [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801) 用作属性以将项目合并到主词典中，请使用 [ThemeDictionaries](https://msdn.microsoft.com/library/windows/apps/br208807) 属性。

[ThemeDictionaries](https://msdn.microsoft.com/library/windows/apps/br208807) 内的每个 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 元素必须具有一个 [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787) 值。 该值是一个字符串，它为相关主题命名，例如 “Default”、“Dark”、“Light”或“HighContrast”。 通常，`Dictionary1` 和 `Dictionary2` 将定义名称相同但值不同的资源。

在此处，将红色文本用于浅色主题，蓝色文本用于深色主题。

```XAML
<!-- Dictionary1.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="Red"/>

</ResourceDictionary>

<!-- Dictionary2.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="blue"/>

</ResourceDictionary>
```

在此示例中，将 [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) 的前景设置为当前主题中的值。

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.ThemeDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml" x:Key="Light"/>
                <ResourceDictionary Source="Dictionary2.xaml" x:Key="Dark"/>
            </ResourceDictionary.ThemeDictionaries>
        </ResourceDictionary>
    </Page.Resources>
    <TextBlock Foreground="{StaticResource brush}" Text="hello world" VerticalAlignment="Center"/>
</Page>
```

对于主题字典，每当使用 [ThemeResource 标记扩展](../../xaml-platform/themeresource-markup-extension.md)进行引用并且系统检测到主题更改时，要用于资源查找的活动字典都会动态更改。 系统执行的查找行为基于将活动主题映射到特定主题字典的 [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787) 操作。

检查主题字典在默认 XAML 设计资源中的构建方式十分有用，这些资源与 Windows 运行时默认用作其控件的模板相对应。 使用文本编辑器或 IDE 打开 \\(Program Files)\\Windows Kits\\10\\DesignTime\\CommonConfiguration\\Neutral\\UAP\\&lt;SDK version&gt;\\Generic 中的 XAML 文件。 请注意主题字典首先在 generic.xaml 中的定义方式，以及每个主题字典定义相同键的方式。 然后每个这样的键由构成各种键控元素的元素所引用，这些键控元素位于主题字典外部并且稍后在 XAML 中定义。 还存在用于设计的单独 themeresources.xaml 文件，该文件仅包含主题资源和额外模板，不包含默认控件模板。 这些主题区域是你将在 generic.xaml 中看到的内容副本。

当你使用 XAML 设计工具以编辑样式和模板的副本时，设计工具会提取 XAML 设计资源词典中的片段并将其作为应用和项目一部分的 XAML 字典元素的本地副本放置。

有关可用于应用的特定于主题的资源和系统资源的详细信息和列表，请参阅 [XAML 主题资源](xaml-theme-resources.md)。

## <a name="lookup-behavior-for-xaml-resource-references"></a>针对 XAML 资源引用的查找行为

*查找行为*是描述 XAML 资源系统如何尝试查找 XAML 资源的术语。 当键从应用的 XAML 的某处被引用为 XAML 资源引用时，即发生查找行为。 首先，资源系统具有可预测行为，它将为此类行为根据作用域检查是否存在资源。 如果在初始作用域中未找到资源，该作用域将展开。 将在应用或系统可能定义了 XAML 资源的位置和作用域上继续查找行为。 如果所有可能的资源查找尝试均失败，通常会导致错误。 通常可以在开发过程中消除这些错误。

针对 XAML 资源引用的查找行为首先从应用实际用法的对象以及它自己的 [Resources](https://msdn.microsoft.com/library/windows/apps/br208740) 属性开始。 如果此处存在 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794)，则会检查该 **ResourceDictionary** 以查看其中是否存在具有所请求键的项。 该第一级查找很少包含相关信息，因为你通常不会在同一个对象上定义某个资源，然后引用该资源。 实际上，**Resources** 属性通常不存在于此处。 你几乎可以在 XAML 中的任何位置进行 XAML 资源引用，而不限于在 [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/br208706) 子类的属性中。

查找序列随后检查应用的运行时对象树中的下一个父对象。 如果存在一个 [FrameworkElement.Resources](https://msdn.microsoft.com/library/windows/apps/br208740) 并且它持有一个 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794)，将请求具有指定键字符串的字典项。 如果找到资源，查找序列就会停止，并将该对象提供到执行引用的位置。 否则，查找行为会向对象树根方向前进到下一个父级对象。 此搜索会继续递归前进，直至到达 XAML 的根元素，从而完成对所有可能直接资源位置的搜索。

> **注意**&nbsp;&nbsp;一种常见的做法是在页面的根级别定义所有直接资源，以利用该资源查找行为并将其用作 XAML 标记样式的一个惯例。

 

如果未在直接资源中找到所请求的资源，下一个查找步骤是检查 [Application.Resources](https://msdn.microsoft.com/library/windows/apps/br242338) 属性。 **Application.Resources** 是放置由应用导航结构中多个页面引用的任何应用特定资源的最佳位置。

在引用查找中，控件模板有另一个可能的位置：主题字典。 主题字典是单个 XAML 文件，它具有一个 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 元素作为其根。 主题字典可能是来自 [Application.Resources](https://msdn.microsoft.com/library/windows/apps/br242338) 的一个合并字典。 主题字典也可能是一个特定于控件的主题字典，用于模板化的自定义控件。

最后，还有一种针对平台资源的资源查找。 平台资源包括为每个系统 UI 主题定义的控件模板，以及用于定义你在 Windows 运行时应用中为所有 UI 控件使用的默认外观的控件模板。 平台资源还包括与系统范围内的外观和主题相关的一组已命名资源。 从技术上来说，这些资源是 [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801) 项，因此在加载应用之后可用于从 XAML 或代码中查找。 例如，系统主题资源包括一个名为 “SystemColorWindowTextColor”的资源，该资源提供一个 [Color](https://msdn.microsoft.com/library/windows/apps/hh673723) 定义以使应用文本颜色与系统窗口的文本颜色（来自操作系统和用户首选项）相匹配。 应用的其他 XAML 样式可以引用该样式，或者你的代码可以获取资源查找值（并将该值转换为示例中的 **Color**）。

有关特定于主题的资源和系统资源（可用于使用 XAML 的 UWP 应用）的详细信息和列表，请参阅 [XAML 主题资源](xaml-theme-resources.md)。

如果请求的键仍未在这其中任何位置中找到，会发生 XAML 分析错误/异常。 某些情况下，XAML 分析异常可能是 XAML 标记编译操作或 XAML 设计环境未检测到的运行时异常。

根据针对资源字典的分层查找行为，你可特意定义多个资源项，每个资源项使用相同的字符串值作为键，只要将每个资源定义在不同的级别上即可。 换言之，尽管键在任何给定的 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 中必须是唯一的，但该唯一性要求不会扩展到整个查找行为序列。 在查找过程中，只有成功检索到的首个此类对象会用于 XAML 资源引用，随即查找便会停止。 你可以利用此行为在应用 XAML 内的各个位置通过键请求相同的 XAML 资源，但会取回不同的资源，具体取决于进行 XAML 资源引用的范围以及特殊查找的行为方式。

##  <a name="forward-references-within-a-resourcedictionary"></a>ResourceDictionary 中的前向引用


一个特定资源字典中的 XAML 资源引用必须引用定义了键的资源，并且从字典顺序来讲，该资源必须出现在资源引用之前。 XAML 资源引用无法解析前向引用。 出于此原因，如果你从另一个资源内使用 XAML 资源引用，则必须设计资源字典结构，以便使其他资源使用的资源在资源字典中首先定义。

在应用级别定义的资源不能引用直接资源。 这相当于尝试前向引用，因为应用资源实际上最先被处理（在应用首次启动和加载任意导航页内容之前）。 但是，任何直接资源都可引用应用资源，这可以作为避免前向引用情形的一种有用技术。

## <a name="xaml-resources-must-be-shareable"></a>XAML 资源必须可共享


要使对象可存在于 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 中，该对象必须*可共享*。

可共享是必要的，因为在构造以及在运行时使用应用的对象树时，对象不能存在于树中的多个位置。 就内部而言，在请求所有 XAML 资源时，资源系统会为在资源的对象图中使用的资源值创建副本。

通常，[ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 和 Windows 运行时 XAML 支持将下列对象用于共享：

-   样式和模板（从 [FrameworkTemplate](https://msdn.microsoft.com/library/windows/apps/br208753) 派生的 [Style](https://msdn.microsoft.com/library/windows/apps/br208849) 和类）
-   画笔和颜色（从 [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush) 派生的类和 [Color](https://msdn.microsoft.com/library/windows/apps/hh673723) 值）
-   动画类型，包括 [Storyboard](https://msdn.microsoft.com/library/windows/apps/br210490)
-   转换（从 [GeneralTransform](https://msdn.microsoft.com/library/windows/apps/br210034) 派生的类）
-   [Matrix](https://msdn.microsoft.com/library/windows/apps/br210127) 和 [Matrix3D](https://msdn.microsoft.com/library/windows/apps/br243266)
-   [Point](https://msdn.microsoft.com/library/windows/apps/br225870) 值
-   其他与 UI 相关的某些结构，例如 [Thickness](https://msdn.microsoft.com/library/windows/apps/br208864) 和 [CornerRadius](https://msdn.microsoft.com/library/windows/apps/br242343)
-   [XAML 固有数据类型](https://msdn.microsoft.com/library/windows/apps/mt186448)

此外，如果遵循必要的实现模式，则可将自定义类型用作可共享资源。 在支持代码（或所含运行时组件）中定义这些类，然后在 XAML 中将其实例化为资源。 示例包括对象数据源和用于数据绑定的 [IValueConverter](https://msdn.microsoft.com/library/windows/apps/br209903) 实现。

自定义类型必须包含默认构造函数，因为 XAML 分析程序须借助该构造函数对类进行实例化。 用作资源的自定义类型不能在其继承中包含 [UIElement](https://msdn.microsoft.com/library/windows/apps/br208911) 类，因为 **UIElement** 无法共享（而只能一直用于表示运行时应用的对象图中某一位置中存在的一个 UI 元素）。

## <a name="usercontrol-usage-scope"></a>UserControl 使用范围


对于资源查找行为，[UserControl](https://msdn.microsoft.com/library/windows/apps/br227647) 元素有一个特殊情况，因为它有一个定义范围和使用范围的固有概念。 从其定义范围内进行 XAML 资源引用的 **UserControl** 必须能够支持在其自己的定义范围查找序列中查找该资源，也就是说，它不能访问应用资源。 在 **UserControl** 使用范围中，将资源引用视为在朝向其使用页面根方向的查询序列内（就像从加载的对象树中的一个对象执行的任何其他资源引用一样），并且可访问应用资源。

## <a name="resourcedictionary-and-xamlreaderload"></a>ResourceDictionary 和 XamlReader.Load

你可以将 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 用作根或用作 [XamlReader.Load](https://msdn.microsoft.com/library/windows/apps/br228048) 方法的 XAML 输入的一部分。 你还可以在该 XAML 中包含 XAML 资源引用（如果所有此类引用都完全自包含在为了加载而提交的 XAML 中）。 **XamlReader.Load** 在不了解任何其他 **ResourceDictionary** 对象（甚至不了解 [Application.Resources](https://msdn.microsoft.com/library/windows/apps/br242338)）的上下文中分析 XAML。 此外，请不要从 XAML 内部使用提交到 **XamlReader.Load** 的 `{ThemeResource}`。

## <a name="using-a-resourcedictionary-from-code"></a>通过代码使用 ResourceDictionary

大部分情况下，[ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 均在 XAML 中专门处理。 你要在 UI 定义文件中将内部的 **ResourceDictionary** 容器和资源声明为 XAML 文件或 XAML 节点集。 然后，使用 XAML 资源引用从 XAML 的其他部分请求这些资源。 不过，在某些特定情况下，你的应用可能需要使用在应用运行时执行的代码来调整 **ResourceDictionary** 的内容，或者至少需要查询 **ResourceDictionary** 的内容以查看是否已定义某个资源。 这些代码调用在 **ResourceDictionary** 实例上执行，所以必须首先通过获得 [**FrameworkElement.Resources**](https://msdn.microsoft.com/library/windows/apps/br208740) 来检索对象树中的即时 ResourceDictionary，或者检索 `Application.Current.Resources`。

在 C\# 或 Microsoft Visual Basic 代码中，你可以使用索引器 ([Item](https://msdn.microsoft.com/library/windows/apps/jj603134)) 引用给定 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 中的资源。 **ResourceDictionary** 是一个字符串键控字典，因此索引器使用字符串键，而不使用整数索引。 在 VisualC + + 组件扩展 (C + + CX) 的代码，请使用[查找](https://msdn.microsoft.com/library/windows/apps/br208800)。

当使用代码检查或更改 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 时，API 的行为（如 [Lookup](https://msdn.microsoft.com/library/windows/apps/br208800) 或 [Item](https://msdn.microsoft.com/library/windows/apps/jj603134)）不会从直接资源遍历到应用资源；这是仅在加载 XAML 页面时发生的 XAML 分析程序行为。 在运行时，键作用域将自包含到此时所使用的 **ResourceDictionary** 实例中。 但是，该作用域不会扩展到 [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801) 中。

另外，如果你请求的键不在 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 中，可能不会出现错误；但返回值可能只会提供为 **null**。 但是，如果你尝试将返回的 **null** 用作值，仍然可能会出现错误。 错误可能来自于属性的设置器，而不是你的 **ResourceDictionary** 调用。 避免错误的唯一方法是属性接受 **null** 作为有效值。 请注意此行为与 XAML 解析期间的 XAML 查找行为有何区别；如果在解析期间无法解析从 XAML 提供的键，会导致 XAML 解析错误，甚至在属性可以接受 **null** 时也是如此。

合并的资源字典包含在主要资源字典的索引范围内，该字典在运行时引用合并字典。 换句话说，可以使用主要字典的 **Item** 或 [Lookup](https://msdn.microsoft.com/library/windows/apps/br208800) 查找在合并字典中实际定义的任何对象。 在这种情况下，该查找行为类似于分析期间的 XAML 查找行为：如果合并字典中存在多个具有相同键的对象，将返回最后添加的字典中的对象。

你可以通过调用 **Add**（C\# 或 Visual Basic）或 [Insert](https://msdn.microsoft.com/library/windows/apps/br208799) (C++/CX) 向现有的 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 添加项。 你可以将这些项添加到直接资源或应用资源。 这些 API 调用中的每一个都需要一个键，这满足 **ResourceDictionary** 中的每个项都必须有一个键的要求。 但是，你在运行时添加到 **ResourceDictionary** 的项与 XAML 资源引用无关。 在加载应用后首次分析 XAML 时，将执行必要的 XAML 资源引用查找。 此时，在运行时添加到集合的资源不可用，而更改 **ResourceDictionary** 并不会使已从中检索到的资源失效，即使更改该资源的值也是如此。

你还可以在运行时从某个 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 删除项、复制部分或所有项，或执行其他操作。 **ResourceDictionary** 的成员列表指出了哪些 API 可用。 请注意，由于 **ResourceDictionary** 有一个投影的 API 可支持其基本集合接口，因此你的 API 选项会有所不同，具体取决于你使用的是 C\# 或 Visual Basic 还是 C++/CX。

## <a name="resourcedictionary-and-localization"></a>ResourceDictionary 和本地化


XAML [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 最初可能包含要本地化的字符串。 如果是这样，将这些字符串存储为项目资源，而不存储在 **ResourceDictionary** 中。 将字符串放在 XAML 外部，而为拥有元素提供一个 [x:Uid 指令](https://msdn.microsoft.com/library/windows/apps/mt204791)值。 然后，在资源文件中定义一个资源。 以 *XUIDValue*.*PropertyName* 的形式提供资源名称，并提供应该本地化的字符串的资源值。

## <a name="custom-resource-lookup"></a>自定义资源查找

对于高级方案，你可以实现其行为不同于本主题中描述的 XAML 资源引用查找行为的类。 要达到此目的，你可实现类 [CustomXamlResourceLoader](https://msdn.microsoft.com/library/windows/apps/br243327)，然后可以通过使用 [CustomResource 标记扩展](https://msdn.microsoft.com/library/windows/apps/mt185580)进行资源引用（而不是使用 [StaticResource](../../xaml-platform/staticresource-markup-extension.md) 或 [ThemeResource](../../xaml-platform/themeresource-markup-extension.md)）来实现该行为。 大部分应用的方案将不会需要此操作。 有关详细信息，请参阅 [CustomXamlResourceLoader](https://msdn.microsoft.com/library/windows/apps/br243327)。

 
## <a name="related-topics"></a>相关主题

* [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794)
* [XAML 概述](https://msdn.microsoft.com/library/windows/apps/mt185595)
* [StaticResource 标记扩展](../../xaml-platform/staticresource-markup-extension.md)
* [ThemeResource 标记扩展](../../xaml-platform/themeresource-markup-extension.md)
* [XAML 主题资源](xaml-theme-resources.md)
* [设置控件的样式](xaml-styles.md)
* [x:Key 特性](https://msdn.microsoft.com/library/windows/apps/mt204787)

 

 




---
author: Xansky
Description: "介绍了为确保通用 Windows 平台 (UWP) 应用在高对比度主题处于活动状态时可用所需的步骤。"
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: "高对比度主题"
label: High-contrast themes
template: detail.hbs
ms.sourcegitcommit: 50c37d71d3455fc2417d70f04e08a9daff2e881e
ms.openlocfilehash: 4201f5a0b08f1fc8d691218da0803ee04ab2c86a

---

# 高对比度主题  

介绍为确保通用 Windows 平台 (UWP) 应用可在高对比度主题处于活动状态时使用所需的步骤。

UWP 应用默认支持高对比度主题。 如果用户已经选择他们希望系统使用系统设置或辅助功能工具中的高对比度主题，则框架将自动使用为 UI 中的控件和组件生成高对比度布局和呈现的颜色和样式设置。

此默认支持假设使用默认主题和模板。 这些主题和模板引用系统颜色作为资源定义，并且资源来源在系统使用高对比度模式时会自动改变。 但是，如果你为控件使用了自定义的模板、主题和样式，请注意不要禁用对高对比度的内置支持。 如果你使用某个适用于 Microsoft Visual Studio 的 XAML 设计器来设置样式，那么，每当你定义与默认模板具有显著区别的模板时，该设计器都会在主要的主题旁边生成一个单独的高对比度主题。 单独的主题字典会进入 [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.resourcedictionary.themedictionaries.aspx) 集合（[**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 元素的一个专门属性）中。

有关主题和控件模板的详细信息，请参阅[快速入门：控件模板](https://msdn.microsoft.com/library/windows/apps/xaml/Hh465374)。 通常，有必要查看特定控件的 XAML 资源字典和主题，并查看这些主题的构造方式以及它们引用资源的方式。对于每个可能的高对比度设置来说，这些资源相似但不同。

## 主题字典

当你需要从其系统默认值更改颜色时，或者需要添加图像作为修饰（如背景图像）时，可为你的应用创建 **ThemeDictionaries** 集合。

* 首先创建正确的管道（如果尚不存在）。 在 App.xaml 中，创建 **ThemeDictionaries** 集合：

``` xaml
 <Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">

            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">

            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources
```

* **HighContrast** 并非唯一的可用键名称。 另外，还有 **HighContrastBlack**、**HighContrastWhite** 和 **HighContrastCustom**。 在大多数情况下，只需使用 **HighContrast**。
* 在 **Default** 中，创建所需的 [**Brush**](http://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.aspx) 类型，通常为 **SolidColorBrush**。 为它提供特定于其用途的 **x:key** 名称：<br/>
    `<SolidColorBrush x:Key="BrandedPageBackground" />`
* 为它指定所需的 **Color**：<br/>
    `<SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />`
* 将 **Brush** 复制到 **HighContrast** 中：

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

* 确定 **Brush** 应采用何种颜色并在 **HighContrast** 中修改它。

针对高对比度确定颜色需要了解相关内容。 前面所创建的管道使其更易于更新。

## 高对比度颜色

用户可以使用设置页面切换到高对比度。 默认情况下，提供 4 个高对比度主题。 在用户选择某一选项后，该页面将显示应用外观的预览。

![高对比度设置](images/high-contrast-settings.png)<br/>
_高对比度设置_

 可以单击预览上的每个方块来更改其值。 每个方块还直接映射到系统资源。

![高对比度资源](images/high-contrast-resources.png)<br/>
_高对比度资源_

如果为上面标出的名称添加 _SystemColor_ 前缀并为它们添加 _Color_ 后缀（例如：**SystemColorWindowTextColor**），它们将动态更新以匹配用户指定的内容。 这样一来，你便无需为高对比度选取特定颜色。 相反，选取对应于颜色用途的系统资源。 在上面的示例中，我们已将我们的页面背景色命名为 **SolidColorBrushBrandedPageBackground**。 由于这将用于背景，因此我们可以在高对比度中将其映射到 **SystemColorWindowColor**：

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="{ThemeResource SystemColorWindowColor}" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

如果你坚持使用 8 个高对比度颜色的调色板，则无需创建任何其他高对比度 **ResourceDictionaries**。 通常，这一有限调色板在表示复杂的视觉状态方面可能会带来巨大挑战。 通常，仅在高对比度中添加某一区域的边框可以帮助阐明相关情况。

### 应做事项和禁止事项

* 及早并经常在高对比度模式下执行测试。
* 针对其预期目的使用已命名的颜色。
* 将 **Color**、**Brush** 和 **Thickness** 之类的基元放置在 **ThemeDictionaries** 内。 避免将更复杂的资源（如 **Style** 元素）放置在其中。 以下示例正常工作：

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="{ThemeResource SystemColorWindowColor}" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>

        <Style x:Key="MyButtonStyle" TargetType="Button">
            <Setter Property="Foreground" Value="{ThemeResource BrandedPageBackground}" />
        </Style>
    </ResourceDictionary>
</Application.Resources>

...

<Button Style="{StaticResource MyButtonStyle}" />
```

* 将高对比度前景色用于前台 UI 元素。
* 将高对比度颜色与其定义的颜色对结合使用。 例如，始终结合使用 **BUTTONTEXT** 与 **BUTTONFACE**，特别是在前台/后台的情况下。
* 将建议的高对比度颜色配对用于特定 UI 元素，以确保满足所需的 14:1 对比率。
* 不要拆分高对比度颜色对或任意混合和匹配高对比度颜色。 这通常能够为至少一个预安装的高对比度主题创建不可见的 UI。
* 不要将创建的任何 **Brush** 对象放置在 **ThemeDictionaries** 集合之外。
* 永远不要使用 **StaticResource** 来引用 **ThemeDictionaries** 集合中的资源。 这似乎有效，直到用户在应用正在运行时更改主题。 改为使用 **ThemeResource**。
* 不要使用硬编码的颜色值。
* 不要仅仅因为喜欢就使用某种颜色。

有关详细信息，请参阅 [XAML 主题资源](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/xaml-theme-resources)。

## 何时使用边框
在高对比度模式下，为 UI 元素添加边框，需要保持项的可识别边界形状。 使用边框可区分导航、操作和内容的内容区域。

![从页面其余部分分离的导航窗格](images/high-contrast-actions-content.png)<br/>
_从页面其余部分分离的导航窗格_

如果默认情况下 UI 元素_不_具有边框或背景，请不要在高对比度模式下向默认状态添加边框或背景。

如果默认情况下 UI 元素_具有_边框，则在高对比度模式下保留该边框。

重叠或相邻颜色应区别于彼此，但它们无需满足 14:1 的颜色对比率。 但是，最佳做法是这些方案类型均使用 3:1 对比率。

如果高对比度背景色用于区分重叠的 UI 元素，可确保这些元素之间的对比度的唯一方法是引入边框。

## 检测高对比度主题何时启用  
使用 [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) 类的成员检测高对比度主题的当前设置。 [**HighContrast**](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.accessibilitysettings.highcontrast) 属性确定高对比度主题当前是否处于选中状态。 如果 **HighContrast** 设置为 **true**，则下一步是检查 [**HighContrastScheme**](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.accessibilitysettings.highcontrastscheme) 属性的值以获取所使用的高对比度主题的名称。 “High Contrast White”和“High Contrast Black”通常是你的代码应响应 **HighContrastScheme** 的值。 XAML 定义的 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 键不能有空格，因此资源字典中这些主题的键通常分别是“HighContrastWhite”和“HighContrastBlack”。 你还应针对当值是其他字符串时的默认高对比主题提供回滚逻辑。 [XAML 高对比度示例](http://go.microsoft.com/fwlink/p/?linkid=254993)显示了这样的回滚逻辑。

> [!NOTE]
> 确保从初始化应用且已经显示内容的范围内调用 [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) 构造函数。

应用可以在运行时通过切换来使用高对比度资源值。 只要在样式或模板 XAML 中使用 [{ThemeResource} 标记扩展](https://msdn.microsoft.com/library/windows/apps/Mt185591)请求了资源，这便有效。 默认主题 (generic.xaml) 都使用此 {ThemeResource} 标记扩展技术，因此如果你使用默认的控件主题，将实现此行为。 如果你还在自定义的模板和样式中使用了此 {ThemeResource} 标记扩展资源技术，则自定义的控件或自定义的控件样式可以实现此行为。

## 相关主题  
* [辅助功能](accessibility.md)
* [UI 对比度和设置示例](http://go.microsoft.com/fwlink/p/?linkid=231539)
* [XAML 辅助功能示例](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [XAML 高对比度示例](http://go.microsoft.com/fwlink/p/?linkid=254993)
* [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)



<!--HONumber=Jun16_HO4-->



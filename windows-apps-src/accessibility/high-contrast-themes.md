---
Description: 介绍了为确保通用 Windows 平台 (UWP) 应用在高对比度主题处于活动状态时可用所需的步骤。
title: 高对比度主题
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
label: High-contrast themes
template: detail.hbs
---

高对比度主题
=============================================================================

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


介绍了为确保通用 Windows 平台 (UWP) 应用在高对比度主题处于活动状态时可用所需的步骤。

UWP 应用默认支持高对比度主题。 如果用户已经选择他们希望系统使用系统设置或辅助功能工具中的高对比度主题，则框架将自动使用为 UI 中的控件和组件生成高对比度布局和呈现的颜色和样式设置。

此默认支持假设使用默认主题和模板。 这些主题和模板引用系统颜色作为资源定义，并且资源来源在系统使用高对比度模式时会自动改变。 但是，如果你为控件使用了自定义的模板、主题和样式，请注意不要禁用对高对比度的内置支持。 如果你使用某个适用于 Microsoft Visual Studio 的 XAML 设计器来设置样式，那么，每当你定义与默认模板具有显著区别的模板时，该设计器都会在主要的主题旁边生成一个单独的高对比度主题。 单独的主题字典会进入 [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/BR208807) 集合（[**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 元素的一个专门属性）中。

有关主题和控件模板的详细信息，请参阅[快速入门：控件模板](https://msdn.microsoft.com/library/windows/apps/xaml/Hh465374)。 通常，有必要查看特定控件的 XAML 资源字典和主题，并查看这些主题的构造方式以及它们引用资源的方式。对于每个可能的高对比度设置来说，这些资源相似但不同。

<span id="Detecting_when_a_high-contrast_theme_is_enabled"> </span> <span id="detecting_when_a_high-contrast_theme_is_enabled"> </span> <span id="DETECTING_WHEN_A_HIGH-CONTRAST_THEME_IS_ENABLED"> </span>检测高对比度主题何时启用
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

UWP 应用可以使用 [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) 类的成员检测当前的高对比度主题设置。 [
            **HighContrast**](https://msdn.microsoft.com/library/windows/apps/BR242237_highcontrast) 属性确定高对比度主题当前是否处于选中状态。 如果 **HighContrast** 设置为 **true**，则下一步是检查 [**HighContrastScheme**](https://msdn.microsoft.com/library/windows/apps/BR242237_highcontrastscheme) 属性的值以获取所使用的高对比度主题的名称。 “High Contrast White”和“High Contrast Black”通常是你的代码应响应 **HighContrastScheme** 的值。 XAML 定义的 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 键不能有空格，因此资源字典中这些主题的键通常分别是“HighContrastWhite”和“HighContrastBlack”。 你还应针对当值是其他字符串时的默认高对比主题提供回滚逻辑。 [XAML 高对比度示例](http://go.microsoft.com/fwlink/p/?linkid=254993)显示了这样的逻辑。

**注意** 确保从应用已初始化且已经显示内容的范围内调用 [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) 构造函数。

 

应用可以在运行时切换到使用高对比度资源值。 只要在样式或模板 XAML 中使用 [{ThemeResource} 标记扩展](https://msdn.microsoft.com/library/windows/apps/Mt185591)请求了资源，这便有效。 默认主题 (generic.xaml) 都使用此 {ThemeResource} 标记扩展技术，因此如果你使用默认的控件主题，将实现此行为。 如果你还在自定义模板和样式中使用了此 {ThemeResource} 标记扩展资源技术，则自定义控件或自定义控件样式设置可以实现此行为。

<span id="related_topics"> </span>相关主题
-----------------------------------------------

* [辅助功能](accessibility.md)
* [UI 对比度和设置示例](http://go.microsoft.com/fwlink/p/?linkid=231539)
* [XAML 辅助功能示例](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [XAML 高对比度示例](http://go.microsoft.com/fwlink/p/?linkid=254993)
* [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)
 

 





<!--HONumber=Mar16_HO3-->



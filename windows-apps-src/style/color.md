---
Description: 颜色提供查找应用的各种级别的信息的直观方法，并用作强化交互模型的重要工具。
title: 颜色
ms.assetid: 3ba7176f-ac47-498c-80ed-4448edade8ad
label: Color
template: detail.hbs
extraBodyClass: style-color
brief: Color provides intuitive wayfinding through an app's various levels of information and serves as a crucial tool for reinforcing the interaction model.<br /><br />In Windows, color is also personal. Users can choose a color and a light or dark theme to be reflected throughout their experience.
---

# 适用于 UWP 应用的颜色
颜色提供查找应用的各种级别的信息的直观方法，并用作强化交互模型的重要工具。

## 主题色

用户可以选取一种称为主题色的颜色。 他们可以从一组 48 种颜色样本中进行选择。


<!-- Alternate version for the dev center. Need to add hex values. -->
<figure>
![Accent colors](images/accentcolorswatch.png)
<figcaption>根据经验，当原始主题色用作背景时，请始终在上面放置白色文本。</figcaption>
</figure>

当用户选择了某种主题色时，它会显示为系统主题的一部分。 受影响的区域有“开始”屏幕、任务栏、窗口镶边、选择的交互状态以及[常用控件](https://dev.windows.com/design/controls-patterns)中的超链接。 每个应用可以通过版式、背景和交互进一步合并主题色，或覆盖它以保留它们的特定品牌。

## 颜色选择

选定主题色后，会根据颜色亮度的 HCL 值创建浅色和深色的主题色。 应用可使用阴影变量创建可视化层次结构和提供交互指示。

![拥有 6 种阴影的单个主题色](images/shades.png)

<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            In XAML, the accent color is exposed as a [theme resource](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx) named `SystemAccentColor`. It's also available programmatically from [UISettings.GetColorValue](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx). You can programmatically access the different shades from [UISettings.GetColorValue](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx), see the [UIColorType](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx) enum.
    </div>
</aside>

## 颜色主题

用户或许可以为系统在浅色主题和深色主题之间进行选择（在手机上可以，但平板电脑和台式机尚不具有该选项，因为它们可以提供应用内设置）。 某些应用选择根据用户喜好更改主题，而其他应用则不会。

使用浅色主题的应用适用于涉及生产性应用的方案。 Microsoft Office 提供的应用套件即是例子。 浅色主题支持在延长的任务时间内轻松阅读长篇大论。

深色主题更能够看到媒体中心式的应用的内容对比或提供给用户丰富的视频或图像的方案对比。 在这些方案中，虽然看电影的体验可能是主要任务，但阅读不一定是，并且它在光线较暗的环境条件下显示。

如果你的应用不完全符合这些描述中的任意描述，请考虑遵循系统主题，让用户决定适合他们的主题。

为使主题设计变得更简单，Windows 提供了自动适应主题的颜色调色板。


<!-- OP version -->
### 浅色主题
#### 基本
![基本浅色主题](images/themes-light-base.png)
#### 备用
![备用浅色主题](images/themes-light-alt.png)
#### 列表
![列表浅色主题](images/themes-light-list.png)
#### 镶边
![镶边浅色主题](images/themes-light-chrome.png)
### 深色主题
#### 基本
![基本深色主题](images/themes-dark-base.png)
#### 备用
![备用深色主题](images/themes-dark-alt.png)
#### 列表
![列表深色主题](images/themes-dark-list.png)
#### 镶边
![镶边深色主题](images/themes-dark-chrome.png)


<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            Each color is available as a XAML [theme resource](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx#the_xaml_color_ramp_and_theme-dependent_brushes) that follows the `System*Color` naming convention (ex: `SystemChromeHighColor`). You can control your app's theme through either [Application.RequestedTheme](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.requestedtheme.aspx) or [FrameworkElement.RequestedTheme](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.requestedtheme.aspx).
    </div>
</aside>

## 辅助功能

我们的调色板已经过优化，可在屏幕上使用。 我们建议保持 4.5:1 的最小文本对比率，以达到最佳可读性。


<!--HONumber=Mar16_HO5-->



---
author: Xansky
Description: 本主题介绍了应用中的文本的最佳辅助功能做法：确保颜色和背景满足必需的对比率。
ms.assetid: BA689C76-FE68-4B5B-9E8D-1E7697F737E6
title: 辅助文本要求
label: Accessible text requirements
template: detail.hbs
---

# 辅助文本要求  




本主题通过确保颜色和背景满足必需的对比率来介绍应用中的文本辅助功能最佳做法。 本主题还讨论了通用 Windows 平台 (UWP) 应用中的文本元素可以具有的 Microsoft UI 自动化角色，以及图形中的文本的最佳做法。

<span id="contrast_rations"/>
<span id="CONTRAST_RATIONS"/>
## 对比率  
尽管用户始终可以选择切换到高对比度模式，但是你的应用的文本设计应当将该选项视为最后的方法。 更好的做法是确保应用文本满足某些为文本及其背景之间对比度级别制定的指导准则。 对比度级别基于不考虑色调的确定性技术进行评估。 例如，如果文本为红色，而背景为绿色，则具有色盲障碍的用户可能无法读取该文本。 检查和更正对比率可以防止出现这些类型的辅助功能问题。

此处记录的文本对比度建议基于 Web 辅助功能标准 [G18：确保文本（和文本的图像）和文本后面的背景之间的对比率至少为 4.5:1](http://go.microsoft.com/fwlink/p/?linkid=221823)。 该指南在 *WCAG 2.0 的 W3C 技术*规范中有说明。

为了考虑辅助功能，可见文本与背景的发光度对比率必须最低为 4.5:1。 例外情况包括徽标和附带文本，例如作为非活动 UI 组件一部分的文本。

装饰性文本且未传递任何信息的文本除外。 例如，如果使用随机字词创建背景，且这些字词可以在不改变含义的情况下进行重新整理或取代，则会将这些字词视为装饰性文本且无需符合此条件。

使用颜色对比工具验证可见文本的对比度是否可接受。 若要了解可以测试对比率的工具，请参阅 [WCAG 2.0 G18 的技术（“资源”部分）](http://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources)。

> [!NOTE]
> “WCAG 2.0 G18 的技术”列出的某些工具无法与 UWP 应用交互使用。 你可能需要在工具中手动输入前景和背景颜色值，或者屏幕捕获应用 UI，然后对屏幕捕获图像运行对比率工具。

<span id="Text_element_roles"/>
<span id="text_element_roles"/>
<span id="TEXT_ELEMENT_ROLES"/>
## 文本元素角色  
UWP 应用可以使用以下默认元素（通常称为 *text* 元素或 *textedit* 控件）：

* [
            **TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)：角色为 [**Text**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [
            **TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)：角色为 [**Edit**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [
            **RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565)（溢出类 [**RichTextBlockOverflow**](https://msdn.microsoft.com/library/windows/apps/BR227565overflow)）：角色为 [**Text**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [
            **RichEditBox**](https://msdn.microsoft.com/library/windows/apps/BR227548)：角色为 [**Edit**](https://msdn.microsoft.com/library/windows/apps/BR209182)

当控件报告它的角色为 [**Edit**](https://msdn.microsoft.com/library/windows/apps/BR209182) 时，辅助技术假设用户可通过多种方法更改这些值。 因此，如果你在 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 中放置静态文本，则说明你向辅助功能用户报告的角色和应用结构有误。

在 XAML 的文本模型中，针对静态文本，主要使用 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 和 [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565) 两种元素。 这两种元素均非 [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) 子类，因此两者均不可通过键盘聚焦，也不可按 Tab 键顺序显示。 但是，这并不意味着辅助技术现在和将来无法读取它们。 屏幕阅读器的设计通常支持多种读取应用内容的模式，包括摆脱聚焦和 Tab 键顺序限制的专用阅读模式或导航模式，例如“虚拟光标”。 因此，不要为能通过 Tab 键顺序向用户显示静态文本而将静态文本输入到可聚焦容器中。 辅助技术用户预期按 Tab 键顺序显示的所有内容均可交互，如果他们在这里看到静态文本，这不但起不到帮助作用，还会令其感到不解。 在使用屏幕阅读器检查应用的静态文本时，你应该通过“讲述人”功能亲自测试，以感受用户在你的应用上获得的体验。

<span id="Text_in_graphics"/>
<span id="text_in_graphics"/>
<span id="TEXT_IN_GRAPHICS"/>
## 图形中的文本  
请尽可能避免在图形中包括文本。 例如，图像源文件中所包括的、在应用中显示为 [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) 元素的任何文本不会自动供辅助技术访问或读取。 如果必须在图形中使用文本，请确保作为“alt text”的等效值提供的 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 值包括该文本或者该文本的含义汇总。 如果要从矢量创建作为 [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355) 一部分的文本字符，或者要使用 [**Glyphs**](https://msdn.microsoft.com/library/windows/apps/BR209921) 创建文本字符，也可以遵循类似的注意事项

<span id="Text_font_size"/>
<span id="text_font_size"/>
<span id="TEXT_FONT_SIZE"/>
## 文本字号  
如果应用中的文本使用的字号太小而使文本难以辨认，许多用户都会难以认出这样的文本。 你可以通过使应用 UI 中的文本在首次出现时合理变大来避免发生此问题。 Windows 中还包含辅助技术，这些技术通过可使用户更改应用或显示器的视图大小。

* 某些用户可采用辅助功能选项方式更改其主显示器的每英寸点数 (dpi) 值。 该选项可从“轻松使用”****的“放大屏幕上显示的内容”****中获得，可重定向到“外观和个性化”**** / “显示器”****的“控制面板”****UI。 究竟有哪些大小设置选项可用可能有所不同，因为这取决于显示设备的功能。
* “放大镜”工具可以放大 UI 的选定区域。 但是，很难使用“放大镜”工具阅读文本。

<span id="Text_scale_factor"/>
<span id="text_scale_factor"/>
<span id="TEXT_SCALE_FACTOR"/>
## 文本缩放比例  
各种文本元素和控件都具有 [**IsTextScaleFactorEnabled**](https://msdn.microsoft.com/library/windows/apps/BR209652_istextscalefactorenabled) 属性。 默认情况下，此属性的值为 **true**。 当其值为 **true** 时，手机上称为“文本缩放”****的设置（“设置”&gt;“轻松使用”****）导致该元素中文本的文字大小被放大。 此缩放对 **FontSize** 较小的文本的影响程度比对 **FontSize** 较大的文本的影响更大。 但是你可以通过将元素的 **IsTextScaleFactorEnabled** 属性设置为 **false** 来禁用自动放大。 试用此标记、调整手机上的**文本大小**设置，然后查看 **TextBlock** 会发生什么情况：

XAML
```xml
<TextBlock Text="In this case, IsTextScaleFactorEnabled has been left set to its default value of true."
    Style="{StaticResource BodyTextBlockStyle}"/>

<TextBlock Text="In this case, IsTextScaleFactorEnabled has been set to false."
    Style="{StaticResource BodyTextBlockStyle}" IsTextScaleFactorEnabled="False"/>
```  

但请勿例行禁用自动放大，因为通常跨所有应用缩放 UI 文本对于用户而言是一项重要的辅助功能体验，他们会以为在你的应用中也有这项功能。

你还可以使用 [**TextScaleFactorChanged**](https://msdn.microsoft.com/library/windows/apps/Dn633867) 事件和 [**TextScaleFactor**](https://msdn.microsoft.com/library/windows/apps/Dn633866) 属性查明对手机上的**“文本大小”**设置的更改。 以下是操作方法：

C#
```csharp
{
    ...
    var uiSettings = new Windows.UI.ViewManagement.UISettings();
    uiSettings.TextScaleFactorChanged += UISettings_TextScaleFactorChanged;
    ...
}

private async void UISettings_TextScaleFactorChanged(Windows.UI.ViewManagement.UISettings sender, object args)
{
    var messageDialog = new Windows.UI.Popups.MessageDialog(string.Format("It's now {0}", sender.TextScaleFactor), "The text scale factor has changed");
    await messageDialog.ShowAsync();
}
```

**TextScaleFactor** 的值是个双数，范围是 \[1,2\]。 最小的文本通过此数量放大。 例如，你可以使用此值缩放图像以匹配文本。 但是请记住，并非所有文本都通过相同的系数来缩放。 一般来说，开始时的文本越大，它受缩放的影响越小。

这些类型具有 **IsTextScaleFactorEnabled** 属性：  
* [**ContentPresenter**](https://msdn.microsoft.com/library/windows/apps/BR209378)
* [
            **Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) 和派生的类
* [**FontIcon**](https://msdn.microsoft.com/library/windows/apps/Dn279514)
* [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565)
* [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)
* [
            **TextElement**](https://msdn.microsoft.com/library/windows/apps/BR209967) 和派生的类

<span id="related_topics"/>
## 相关主题  
* [辅助功能](accessibility.md)
* [基本的辅助功能信息](basic-accessibility-information.md)
* [XAML 文本显示示例](http://go.microsoft.com/fwlink/p/?linkid=238579)
* [XAML 文本编辑示例](http://go.microsoft.com/fwlink/p/?linkid=251417)
* [XAML 辅助功能示例](http://go.microsoft.com/fwlink/p/?linkid=238570)


<!--HONumber=May16_HO2-->



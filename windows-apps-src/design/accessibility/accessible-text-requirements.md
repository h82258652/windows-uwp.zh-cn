---
Description: 本主题介绍了应用中的文本的最佳辅助功能做法：确保颜色和背景满足必需的对比率。
ms.assetid: BA689C76-FE68-4B5B-9E8D-1E7697F737E6
title: 辅助文本要求
label: Accessible text requirements
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3f474ec0c3017c3834d3eadb6f1caa989fc188a7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653332"
---
# <a name="accessible-text-requirements"></a>辅助文本要求  




本主题介绍了应用中的文本的最佳辅助功能做法：确保颜色和背景满足必需的对比率。 本主题还讨论了通用 Windows 平台 (UWP) 应用中的文本元素可以具有的 Microsoft UI 自动化角色，以及图形中的文本的最佳做法。

<span id="contrast_rations"/>
<span id="CONTRAST_RATIONS"/>

## <a name="contrast-ratios"></a>对比率  
尽管用户始终可以选择切换到高对比度模式，但是你的应用的文本设计应当将该选项视为最后的方法。 更好的做法是确保应用文本满足某些为文本及其背景之间对比度级别制定的指导准则。 对比度级别基于不考虑色调的确定性技术进行评估。 例如，如果文本为红色，而背景为绿色，则具有色盲障碍的用户可能无法读取该文本。 检查和更正对比率可以防止出现这些类型的辅助功能问题。

文本对比度此处所述的建议基于 web 的辅助功能标准， [G18:确保有 3:1 的对比度至少文本 （和图像的文本） 之间存在对比度和文本的背景的背景](https://go.microsoft.com/fwlink/p/?linkid=221823)。 该指南在 *WCAG 2.0 的 W3C 技术*规范中有说明。

为了考虑辅助功能，可见文本与背景的发光度对比率必须最低为 4.5:1。 例外情况包括徽标和附带文本，例如作为非活动 UI 组件一部分的文本。

装饰性文本且未传递任何信息的文本除外。 例如，如果使用随机字词创建背景，且这些字词可以在不改变含义的情况下进行重新整理或取代，则会将这些字词视为装饰性文本且无需符合此条件。

使用颜色对比工具验证可见文本的对比度是否可接受。 请参阅 [WCAG 2.0 G18 的技术（“资源”部分）](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources)了解可以测试对比率的工具。

> [!NOTE]
> “WCAG 2.0 G18 的技术”列出的某些工具无法与 UWP 应用交互使用。 你可能需要在工具中手动输入前景和背景颜色值，或者屏幕捕获应用 UI，然后对屏幕捕获图像运行对比率工具。

<span id="Text_element_roles"/>
<span id="text_element_roles"/>
<span id="TEXT_ELEMENT_ROLES"/>

## <a name="text-element-roles"></a>文本元素角色  
UWP 应用可以使用以下默认元素（通常称为 *text* 元素或 *textedit* 控件）：

* [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)： 角色是[**文本**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [**文本框**](https://msdn.microsoft.com/library/windows/apps/BR209683)： 角色是[**编辑**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [**RichTextBlock** ](https://msdn.microsoft.com/library/windows/apps/BR227565) (和 overflow 类[ **RichTextBlockOverflow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.richtextblockoverflow)): 角色是[**文本**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/BR227548)： 角色是[**编辑**](https://msdn.microsoft.com/library/windows/apps/BR209182)

当控件报告它的角色为 [**Edit**](https://msdn.microsoft.com/library/windows/apps/BR209182) 时，辅助技术假设用户可通过多种方法更改这些值。 因此，如果你在 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 中放置静态文本，则说明你向辅助功能用户报告的角色和应用结构有误。

在 XAML 的文本模型中，针对静态文本，主要使用 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 和 [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565) 两种元素。 这两种元素均非 [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) 子类，因此两者均不可通过键盘聚焦，也不可按 Tab 键顺序显示。 但是，这并不意味着辅助技术现在和将来无法读取它们。 屏幕阅读器的设计通常支持多种读取应用内容的模式，包括摆脱聚焦和 Tab 键顺序限制的专用阅读模式或导航模式，例如“虚拟光标”。 因此，不要为能通过 Tab 键顺序向用户显示静态文本而将静态文本输入到可聚焦容器中。 辅助技术用户预期按 Tab 键顺序显示的所有内容均可交互，如果他们在这里看到静态文本，这不但起不到帮助作用，还会令其感到不解。 在使用屏幕阅读器检查应用的静态文本时，你应该通过“讲述人”功能亲自测试，以感受用户在你的应用上获得的体验。

<span id="Auto-suggest_accessibility"/>
<span id="auto-suggest_accessibility"/>
<span id="AUTO-SUGGEST_ACCESSIBILITY"/>

## <a name="auto-suggest-accessibility"></a>自动建议辅助功能  
当用户在输入字段中进行键入后显示可能的建议列表时，此种情形称为“自动建议”。 这在邮件字段中的“收件人:”行、Windows 中的 Cortana 搜索框、Microsoft Edge 中的 URL 输入字段、“天气”应用中的位置输入字段等方面很常见。 如果你使用的是 XAML [**AutosuggestBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.autosuggestbox) 或 HTML 内部控件，则已默认为你关联了此体验。 若要使此体验可访问输入字段，必须关联列表。 这将在[实现自动建议](#implementing_auto-suggest)一节中介绍。

已更新了讲述人，使此类型的体验可访问特殊建议模式。 在高级别上，当正确连接编辑字段和列表时，最终用户将：

* 了解列表是否存在，以及列表关闭的时间
* 了解可用的建议数目
* 了解选定项（如果有）
* 能够将“讲述人”焦点移动到列表
* 能够使用所有其他阅读模式浏览建议

![建议列表](images/autosuggest-list.png)<br/>
_建议列表的示例_

<span id="Implementing_auto-suggest"/>
<span id="implementing_auto-suggest"/>
<span id="IMPLEMENTING_AUTO-SUGGEST"/>

### <a name="implementing-auto-suggest"></a>实现自动建议  
若要使此体验可访问输入字段，必须在 UIA 树中关联列表。 在桌面应用中，通过 [UIA_ControllerForPropertyId](https://msdn.microsoft.com/windows/desktop/ee684017) 属性实现此关联；在 UWP 应用中，通过 [ControlledPeers](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) 属性实现此关联。

在高级别上，存在 2 种类型的自动建议体验。

**默认选择**  
如果在列表中设置了默认选择，则“讲述人”会在桌面应用中查找 [**UIA_SelectionItem_ElementSelectedEventId**](https://msdn.microsoft.com/library/windows/desktop/ee671223) 事件或在 UWP 应用中查找要引发的 [**AutomationEvents.SelectionItemPatternOnElementSelected**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) 事件。 每次选择更改时，如果用户键入其他字母并且建议已更新，或者如果用户浏览该列表，则应引发 **ElementSelected** 事件。

![采用默认选择列表](images/autosuggest-default-selection.png)<br/>
_示例的默认选择_

**没有默认选定内容**  
如果没有默认选择（如在“天气”应用的位置框中），则每次更新列表时，“讲述人”都会在列表上查找 [**UIA_LayoutInvalidatedEventId**](https://msdn.microsoft.com/library/windows/desktop/ee671223 ) 事件（适用于桌面）或查找要引发的 [**LayoutInvalidated**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) 事件（适用于 UWP）。

![列出包含任何默认选定内容](images/autosuggest-no-default-selection.png)<br/>
_示例在没有默认选定内容_

### <a name="xaml-implementation"></a>XAML 实现  
如果你使用的是默认 XAML [**AutosuggestBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.autosuggestbox)，则已为你关联了所有内容。 如果你要使用 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox) 和列表自行实现自动建议体验，则需要在 **TextBox** 上将列表设置为 [**AutomationProperties.ControlledPeers**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.getcontrolledpeers)。 必须在每次添加或删除 [**ControlledPeers**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) 属性时触发该属性的 **AutomationPropertyChanged** 事件，此外还必须引发你自己的 [**SelectionItemPatternOnElementSelected**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) 事件或 [**LayoutInvalidated**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) 事件，具体取决于方案类型（已在本文的前面部分介绍过）。

### <a name="html-implementation"></a>HTML 实现  
如果你使用的是 HTML 中的内部控件，则已为你映射了 UIA 实现。 以下是已为你关联的实现示例。

``` HTML
<label>Sites <input id="input1" type="text" list="datalist1" /></label>
<datalist id="datalist1">
        <option value="http://www.google.com/" label="Google"></option>
        <option value="http://www.reddit.com/" label="Reddit"></option>
</datalist>
```

 如果你要创建自己的控件，则必须设置你自己的 ARIA 控件（将在 W3C 标准中进行介绍）。

<span id="Text_in_graphics"/>
<span id="text_in_graphics"/>
<span id="TEXT_IN_GRAPHICS"/>

## <a name="text-in-graphics"></a>图形中的文本

请尽可能避免在图形中包括文本。 例如，图像源文件中所包括的、在应用中显示为 [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) 元素的任何文本不会自动供辅助技术访问或读取。 如果必须在图形中使用文本，确保你提供为“alt text”的等效值的 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 值包括该文本或者该文本的含义汇总。 如果要从矢量创建作为 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 一部分的文本字符，或者要使用 [**Glyphs**](https://msdn.microsoft.com/library/windows/apps/BR209921) 创建文本字符，也可以遵循类似的注意事项。

<span id="Text_font_size"/>
<span id="text_font_size"/>
<span id="TEXT_FONT_SIZE"/>

## <a name="text-font-size-and-scale"></a>文本字体大小和小数位数

用户可以读取在应用中的文本，只需字体使用时的难度太小，因此请确保你的应用程序中的所有文本都是合理的大小，第一个位置中。

完成操作后很明显，Windows 将包括各种可访问性工具和设置，用户可以充分利用并调整其自己的需求和用于读取文本的首选项。 这些地方包括：

* 放大镜工具，它将放大选定的区域的用户界面。 您应确保应用程序中的文本的布局不会使其难以使用放大镜以进行读取。
* 中的全局规模和解决方法设置**设置-> 系统-> 显示-> 扩展和布局**。 完全有哪些大小调整选项可能有所不同，因为这取决于显示设备的功能。
* 中的文本大小设置**设置-> 轻松访问-> 显示**。 调整**使文本更大**设置支持跨所有应用程序和屏幕 （所有 UWP 文本控件都支持缩放体验，而无任何自定义或模板化的文本） 的控件中指定的文本大小。 
> [!NOTE]
> **保持所有对象的更大**设置可使用户在一般情况下在其主屏幕上指定的文本和应用程序的首选的大小。

各种文本元素和控件都具有 [**IsTextScaleFactorEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.istextscalefactorenabled) 属性。 默认情况下，此属性的值为 **true**。 当 **，则返回 true**，可以缩放该元素中的文本的大小。 缩放会影响有一个较小的文本**FontSize**不是它会影响具有大量的文本是更大**FontSize**。 您可以禁用自动调整大小，通过设置元素的**IsTextScaleFactorEnabled**属性设置为**false**。 

请参阅[文本缩放](https://docs.microsoft.com/windows/uwp/design/input/text-scaling)的更多详细信息。

将以下标记添加到应用程序并运行它。 调整**字号**设置，并了解到每个发生什么情况**TextBlock**。

XAML
```xml
<TextBlock Text="In this case, IsTextScaleFactorEnabled has been left set to its default value of true."
    Style="{StaticResource BodyTextBlockStyle}"/>

<TextBlock Text="In this case, IsTextScaleFactorEnabled has been set to false."
    Style="{StaticResource BodyTextBlockStyle}" IsTextScaleFactorEnabled="False"/>
```  

我们不建议你禁用文本缩放如普遍的所有应用之间的缩放 UI 文本是用户的重要的可访问性体验。

你还可以使用 [**TextScaleFactorChanged**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) 事件和 [**TextScaleFactor**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactor) 属性查明对手机上的 **“文本大小”** 设置的更改。 以下是操作方法：

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

值**TextScaleFactor**是范围中的双精度\[1,2.25\]。 最小的文本通过此数量放大。 例如，你可以使用此值缩放图像以匹配文本。 但是请记住，并非所有文本都通过相同的系数来缩放。 一般来说，开始时的文本越大，它受缩放的影响越小。

这些类型具有 **IsTextScaleFactorEnabled** 属性：  
* [**ContentPresenter**](https://msdn.microsoft.com/library/windows/apps/BR209378)
* [**控件**](https://msdn.microsoft.com/library/windows/apps/BR209390)和派生类
* [**FontIcon**](https://msdn.microsoft.com/library/windows/apps/Dn279514)
* [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565)
* [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)
* [**TextElement** ](https://msdn.microsoft.com/library/windows/apps/BR209967)和派生类

<span id="related_topics"/>

## <a name="related-topics"></a>相关主题  

* [文本缩放](https://docs.microsoft.com/windows/uwp/design/input/text-scaling)
* [辅助功能](accessibility.md)
* [基本的辅助功能信息](basic-accessibility-information.md)
* [XAML 文本显示示例](https://go.microsoft.com/fwlink/p/?linkid=238579)
* [XAML 文本编辑示例](https://go.microsoft.com/fwlink/p/?linkid=251417)
* [XAML 可访问性示例](https://go.microsoft.com/fwlink/p/?linkid=238570) 

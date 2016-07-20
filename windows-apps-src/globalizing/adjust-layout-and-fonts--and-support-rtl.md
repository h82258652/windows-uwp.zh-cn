---
author: DelfCo
Description: "开发你的应用来支持多种语言的布局和字体，包括 RTL（从右到左）排列方向。"
title: "调整布局和字体并支持 RTL"
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: a1b271360b84e670f0b28557ffc499436487ad5f

---

# 调整布局和字体并支持 RTL





开发你的应用来支持多种语言的布局和字体，包括 RTL（从右到左）排列方向。

## <span id="Layout_guidelines"></span><span id="layout_guidelines"></span><span id="LAYOUT_GUIDELINES"></span>布局指南


一些语言（如德语和芬兰语）的文本所需的空间比其对应的英语文本所需的空间更多。 一些语言（如日语）的字体需要更高的高度。 还有一些语言（如阿拉伯语和希伯来语）要求文本布局和应用布局必须采用从右到左 (RTL) 的读取顺序。

使用灵活的布局机制，而不要使用绝对定位、固定宽度或固定高度。 可以根据语言来调整特定 UI 元素（如果必要）。

### <span id="XAML"></span><span id="xaml"></span>XAML

为元素指定 **Uid**：

```XML
<TextBlock x:Uid="Block1">
```

确保你的应用的 ResW 文件具有 Block1.Width 的资源，你可以为每种本地化目标语言设置该资源。

对于使用 C++、C# 或 Visual Basic 的 Windows 应用商店应用，使用 [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 属性（具有对称间距和边距），以便针对其他布局方向进行本地化。

XAML 布局控件（例如 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)）可通过 [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 属性自动缩放和翻转。 将应用中你自己的 **FlowDirection** 属性作为资源显示给本地化人员。

为应用的主页指定 **Uid**：

```XML
<Page x:Uid="MainPage">
```

确保你的应用的 **ResW** 文件具有 MainPage.FlowDirection 的资源，你可以为要本地化为该语言的每种语言设置该资源。

### <span id="HTML"></span><span id="html"></span>HTML

对于使用 JavaScript 的 Windows 应用商店应用，请使用[级联样式表 (CSS)](https://msdn.microsoft.com/library/ms531209) 布局机制，例如 [-ms-grid](https://msdn.microsoft.com/library/windows/apps/hh465453.aspx#g_section) 和 [–ms-box](https://msdn.microsoft.com/library/windows/apps/hh465453.aspx#f_section)。 使用对称填充和边距，以便针对各种布局方向进行本地化。

你的应用还可以使用 [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) 伪类选择器来根据应用的语言调整 CSS 属性，例如特定元素的宽度。 为了实现这一点，应用托管进程将根元素的 **lang** 属性设置为应用语言。

**CSS**
```CSS
.item:-ms-lang(de, fi) { width: 350px; }
```

使用 JavaScript 的 Windows 应用商店应用（可使用 ui-light.css 或 ui-dark.css 样式表）可以根据应用语言自动设置其正文布局方向。 以下 CSS 位于 ui-light 和 ui-dark.css 中，无需自行编写它。

**CSS**
```CSS
body:-ms-lang(ar,he…) { direction: rtl;}
```

这意味着当系统使用从右到左的语言时，大多数应用布局都可正确设置。

与 [WinJS.UI](https://msdn.microsoft.com/library/windows/apps/br229782) 控件类似，你的应用可以使用 [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) 伪类选择器来调整物理 CSS 属性（如 **margin** 和 **padding**）。 你无需调整使用关键字（例如 **after** 和 **before**）的逻辑 CSS 属性。

不要在 HTML 中使用 **align** 属性或特性。 应改用 **direction** 属性控制特定组件的对齐方式。

使用 [**writing-mode**](https://msdn.microsoft.com/library/ms531187) 属性在 CSS 中支持垂直文本布局。

## <span id="Mirroring_images"></span><span id="mirroring_images"></span><span id="MIRRORING_IMAGES"></span>镜像图像


### <span id="XAML"></span><span id="xaml"></span>XAML

如果应用具有必须针对 RTL 进行镜像的图像（即可以翻转相同图像），则可以应用 [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 属性：

```XML
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

### <span id="HTML"></span><span id="html"></span>HTML

如果应用具有必须针对 RTL 进行镜像的图像（即可以翻转相同图像），则可以使用 CSS 变换在呈现时对图像进行镜像，方法是将 .mirrorable 类添加到元素并添加以下 CSS 类：

```CSS
.mirrorable { transform: scaleX(-1); }
```

**对于 XAML 和 HTML：**如果应用需要其他图像来正确翻转该图像，则可以将资源管理系统与 [layoutdir 限定符](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)结合使用。 当[应用程序语言](manage-language-and-region.md)设置为 RTL 语言时，系统会选择名为 file.layoutdir-rtl.png 的图像。 当图像的某一部分翻转而其他部分不翻转时，可能必须使用此方法。

## <span id="Fonts"></span><span id="fonts"></span><span id="FONTS"></span>字体


**对于 XAML 和 HTML：**使用 [**LanguageFont**](https://msdn.microsoft.com/library/windows/apps/br206864) 字体映射 API 以编程方式访问为特定语言建议的字体系列、大小、粗细和样式。 **LanguageFont** 对象提供了对各种类别内容的正确字体信息的访问，这些信息包括 UI 标头、通知、正文文本和用户可编辑的文档正文字体。

### <span id="HTML"></span><span id="html"></span>HTML

使用 JavaScript 的 Windows 应用商店应用（可使用 ui-light.css 或 ui-dark.css 样式表）可以根据应用语言将其字体自动设置为最适合的字体。 应用托管进程将根元素的 **lang** 属性设置为应用语言。

在单页上显示多种语言的应用应分别针对各语言部分设置 **lang** 属性。 [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) 伪类选择器会为页面的每个部分选择正确的字体。

 

 






<!--HONumber=Jul16_HO2-->



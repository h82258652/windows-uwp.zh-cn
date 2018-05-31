---
author: stevewhims
Description: Design your app to support the layouts and fonts of multiple languages, including RTL (right-to-left) flow direction.
title: 调整布局和字体并支持 RTL
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.author: stwhi
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 可本地化性, 本地化, rtl, ltr
ms.localizationpriority: medium
ms.openlocfilehash: cf3a2d781dc916fbda9a9d6386dee4e2e6144873
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2018
ms.locfileid: "1673974"
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>调整布局和字体并支持 RTL

设计应用，使其支持多种语言布局和字体，包括 RTL（从右到左）排列方向。 排列方向是脚本的写入方向和显示方向，页面上的 UI 元素是目视浏览的。

## <a name="layout-guidelines"></a>布局指南

德语和芬兰语等语言使用的字符数通过比英语更多。 远东字体通常需要更高的空间。 阿拉伯语和希伯来语等语言要求布局面板和文本元素按照从右到左 (RTL) 的读取顺序排列。

由于译文长度不尽相同，因此应使用动态 UI 布局机制，而非绝对定位、固定宽度或固定高度。 伪本地化应用将发现存在任何 UI 元素大小与内容不匹配问题的边缘用例。

对于 RTL 语言，请使用 [**FrameworkElement.FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) 属性，并设置对称填充和边距。 布局面板（例如 [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid?branch=live)）可通过设置的 **FlowDirection** 值自动缩放和翻转。

以下示例介绍如何将应用中的 FlowDirection 属性作为本地化人员可相应设置的资源公开。

首先，在应用的资源文件 (.resw) 中，添加一个名为“MainPage.FlowDirection”（而不是“MainPage”，可使用任何你喜欢的名称）的属性标识符。

然后，使用 **X:uid** 将主要**页面**元素与此属性标识符关联。

```xaml
<Page x:Uid="MainPage">
```

有关资源文件 (.resw)、属性标识符和 **x:Uid** 的详细信息，请参阅[本地化 UI 和应用包清单中的字符串](../../app-resources/localize-strings-ui-manifest.md)。

应避免基于语言对任意 UI 元素设置绝对布局值。 但如果这是绝对无法避免的，则创建一个“TitleText.Width”形式的属性标识符。

```xaml
<TextBlock x:Uid="TitleText">
```

## <a name="fonts"></a>字体

使用 [**LanguageFont**](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live) 字体映射类以编程方式访问为特定语言建议的字体系列、大小、粗细和样式。 **LanguageFont** 类提供了对各种类别内容的正确字体信息的访问，这些信息包括 UI 标头、通知、正文文本和用户可编辑的文档正文字体。

## <a name="mirroring-images"></a>镜像图像

如果应用具有必须针对 RTL 进行镜像的图像（即可以翻转相同图像），则可以使用 **FlowDirection**。

```xaml
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

如果应用需要其他图像才能正确地翻转该图像，则可以配合使用资源管理系统和 `LayoutDirection` 限定符（请参阅[定制语言、比例和其他限定符的资源](../../app-resources/tailor-resources-lang-scale-contrast.md#layoutdirection)的 LayoutDirection 部分）。 应用运行时语言（请参阅[了解用户配置文件语言和应用清单语言](manage-language-and-region.md)）设置为 RTL 语言时，系统选择一个名为 `file.layoutdir-rtl.png` 的图像。 当图像的某一部分翻转而其他部分不翻转时，可能必须使用此方法。

## <a name="best-practices-for-handling-right-to-left-rtl-languages"></a>处理从右到左 (RTL) 语言的最佳做法

当为从右到左 (RTL) 语言本地化应用时，请使用 API 为页面的根布局面板设置默认文本方向。 这导致根面板内包含的所有控件将相应地响应默认文本方向。 当支持多种语言时，请将 `LayoutDirection` 用于首选应用运行时语言，以设置 [**FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) 属性（请参阅以下代码示例）。 包含在 Windows 中的大多数控件已使用 **FlowDirection**。 如果在实现自定义控件，这些控件应使用 **FlowDirection** 相应地更改 RTL 和 LTR 语言布局。

```csharp    
this.languageTag = Windows.Globalization.ApplicationLanguages.Languages[0];

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

var flowDirectionSetting = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["LayoutDirection"];
if (flowDirectionSetting == "LTR")
{
    this.layoutRoot.FlowDirection = Windows.UI.Xaml.FlowDirection.LeftToRight;
}
else
{
    this.layoutRoot.FlowDirection = Windows.UI.Xaml.FlowDirection.RightToLeft;
}
```

```cpp
this->languageTag = Windows::Globalization::ApplicationLanguages::Languages->GetAt(0);

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

auto flowDirectionSetting = Windows::ApplicationModel::Resources::Core::ResourceContext::GetForCurrentView()->QualifierValues->Lookup("LayoutDirection");
if (flowDirectionSetting == "LTR")
{
    this->layoutRoot->FlowDirection = Windows::UI::Xaml::FlowDirection::LeftToRight;
}
else
{
    this->layoutRoot->FlowDirection = Windows::UI::Xaml::FlowDirection::RightToLeft;
}
```

### <a name="rtl-faq"></a>RTL 常见问题解答 

**问：** **FlowDirection** 会根据当前语言选择自动设置吗？ 例如，如果我选择英语，它会从左往右显示，而在选择阿拉伯语时，它又会从右往左显示吗？

> **答：****FlowDirection** 不会考虑语言种类。 要为当前显示的语言设置适当的 **FlowDirection**。 查看上述示例代码。

**问：** 我不太熟悉本地化。 资源是否已经包含排列方向？ 能否根据当前语言确定排列方向？

> **答：** 如在使用当前的最佳做法，则资源不会直接包含排列方向。 必须确定当前语言的排列方向。 可通过两种方式执行此操作。
> 
> 首选方法是将 **LayoutDirection** 用于首选语言以设置 RootFrame 的 **FlowDirection** 属性。 RootFrame 中的所有控件均从 RootFrame 继承 FlowDirection。
> 
> 另一种方法是在要针对其进行本地化的 RTL 语言的 .resw 文件中设置 FlowDirection。 例如，你可能拥有阿拉伯语的 resw 文件和希伯来语的 resw file。 在这些文件中，可以使用 x:UID 设置 FlowDirection。 但相比于编程方法，此方法更易于出错。

## <a name="important-apis"></a>重要的 API

* [FrameworkElement.FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection)
* [LanguageFont](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live)

## <a name="related-topics"></a>相关主题

* [对 UI 和应用包清单中的字符串实施本地化](../../app-resources/localize-strings-ui-manifest.md)
* [定制语言、比例和其他限定符的资源](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [了解用户配置文件语言和应用清单语言](manage-language-and-region.md)
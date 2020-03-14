---
Description: 设计应用，使其支持多种语言布局和字体，包括 RTL（从右到左）排列方向。
title: 调整布局和字体并支持 RTL
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.date: 05/11/2018
ms.topic: article
keywords: windows 10, uwp, 可本地化性, 本地化, rtl, ltr
ms.localizationpriority: medium
ms.openlocfilehash: e428dd068337ecd79992e8e27cd193bed112d9c2
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79209833"
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>调整布局和字体并支持 RTL
设计应用，使其支持多种语言布局和字体，包括 RTL（从右到左）排列方向。 排列方向是脚本的写入方向和显示方向，页面上的 UI 元素是目视浏览的。

## <a name="layout-guidelines"></a>布局指南
德语和芬兰语等语言使用的字符数通过比英语更多。 远东字体通常需要更高的空间。 阿拉伯语和希伯来语等语言要求布局面板和文本元素按照从右到左 (RTL) 的读取顺序排列。

由于译文中的这些指标变化，建议不要在用户界面 (UI) 中提供绝对定位、固定宽度或固定高度。 而应使用 Windows UI 元素内置的动态布局机制。 例如内容控件（如按钮）、项目控件（如网格视图和列表视图）以及可自动调整大小和默认重排以适应其内容的布局面板（如网格和 stackpanel）。 伪本地化你的应用，找出任何存在 UI 元素与内容大小不匹配问题的边缘案例。

推荐使用动态布局技术，在大多数情况下都可以使用这种技术。 不太推荐但仍优于在 UI 标记中提供大小的方法是将布局值设置为语言的函数。 以下示例介绍如何将应用中的 Width 属性作为本地化人员可根据语言相应设置的资源公开。 首先，在应用的资源文件 (.resw) 中，添加一个名为“TitleText.Width”（而不是“TitleText”，你可以使用任何喜欢的名称）的属性标识符。 然后，使用一个 **x:Uid** 将一个或多个 UI 元素与此属性标识符关联。

```xaml
<TextBlock x:Uid="TitleText">
```

有关资源文件 (.resw)、属性标识符和 **x:Uid** 的详细信息，请参阅[本地化 UI 和应用包清单中的字符串](../../app-resources/localize-strings-ui-manifest.md)。

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

如果应用需要其他图像才能正确地翻转该图像，则可以配合使用资源管理系统和 `LayoutDirection` 限定符（请参阅[定制语言、比例和其他限定符的资源](../../app-resources/tailor-resources-lang-scale-contrast.md#layoutdirection)的 LayoutDirection 部分）。 应用运行时语言（请参阅`file.layoutdir-rtl.png`了解用户配置文件语言和应用清单语言[）设置为 RTL 语言时，系统选择一个名为 ](manage-language-and-region.md) 的图像。 当图像的某一部分翻转而其他部分不翻转时，可能必须使用此方法。

## <a name="handling-right-to-left-rtl-languages"></a>处理从右到左 (RTL) 语言
当为从右到左 (RTL) 语言本地化应用时，请使用 [**FrameworkElement.FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) 属性，并设置对称填充和边距。 布局面板（例如 [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid?branch=live)）可通过设置的 **FlowDirection** 值自动缩放和翻转。

在页面的根布局面板（或框架）或页面本身上设置 **FlowDirection**。 这将导致包含在其中的所有控件继承该属性。

> [!IMPORTANT]
> 但是，**FlowDirection***不会* 根据用户在 Windows 设置中选择的显示语言自动设置；也不会作为用户切换显示语言的响应动态更改。 例如，如果用户将 Windows 设置从英语切换为阿拉伯语，**FlowDirection** 属性将*不会* 自动由从左到右更改为从右到左。 作为应用开发人员，你必须为你当前显示的语言设置相应的 **FlowDirection**。

编程方法是使用首选用户显示语言的 `LayoutDirection` 属性设置 [**FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) 属性（请参阅下面的代码示例）。 包含在 Windows 中的大多数控件都已使用 **FlowDirection**。 如果要实现自定义控件，则应使用 **FlowDirection** 为 RTL 和 LTR 语言进行相应的布局更改。

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

```cppwinrt
#include <winrt/Windows.ApplicationModel.Resources.Core.h>
#include <winrt/Windows.Globalization.h>
...

m_languageTag = Windows::Globalization::ApplicationLanguages::Languages().GetAt(0);

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

auto flowDirectionSetting = Windows::ApplicationModel::Resources::Core::ResourceContext::GetForCurrentView().QualifierValues().Lookup(L"LayoutDirection");
if (flowDirectionSetting == L"LTR")
{
    layoutRoot().FlowDirection(Windows::UI::Xaml::FlowDirection::LeftToRight);
}
else
{
    layoutRoot().FlowDirection(Windows::UI::Xaml::FlowDirection::RightToLeft);
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

上面的方法将 **FlowDirection** 作为首选用户显示语言的 `LayoutDirection` 属性的函数。 如果出于任何原因不希望使用此逻辑，可以将应用中的 FlowDirection 属性作为本地化人员可根据所翻译到的每种语言相应设置的资源公开。

首先，在应用的资源文件 (.resw) 中，添加一个名为“MainPage.FlowDirection”（而不是“MainPage”，可使用任何你喜欢的名称）的属性标识符。 然后，使用一个 **x:Uid** 将主要 **Page** 元素（或者其根布局面板或框架）与此属性标识符关联。

```xaml
<Page x:Uid="MainPage">
```

这不是适用于所有语言的一行代码，而是取决于翻译人员针对每种翻译语言正确地“翻译”此属性；因此应注意，使用此技术可能出现更多的人为错误。

## <a name="important-apis"></a>重要的 API
* [FrameworkElement. System.windows.flowdirection>](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection)
* [LanguageFont](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live)

## <a name="related-topics"></a>相关主题
* [对 UI 和应用包清单中的字符串进行本地化](../../app-resources/localize-strings-ui-manifest.md)
* [为语言、缩放和其他限定符定制资源](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [了解用户配置文件语言和应用程序清单语言](manage-language-and-region.md)
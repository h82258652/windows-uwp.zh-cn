---
author: DelfCo
Description: "开发你的应用来支持多种语言的布局和字体，包括 RTL（从右到左）排列方向。"
title: "调整布局和字体并支持 RTL"
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 9c700928d2ec0da21b518528289034296637eeff
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>调整布局和字体并支持 RTL
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

开发你的应用来支持多种语言的布局和字体，包括 RTL（从右到左）排列方向。

## <a name="layout-guidelines"></a>布局指南


一些语言（如德语和芬兰语）的文本所需的空间比其对应的英语文本所需的空间更多。 一些语言（如日语）的字体需要更高的高度。 还有一些语言（如阿拉伯语和希伯来语）要求文本布局和应用布局必须采用从右到左 (RTL) 的读取顺序。

使用灵活的布局机制，而不要使用绝对定位、固定宽度或固定高度。 可以根据语言来调整特定 UI 元素（如果必要）。

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


## <a name="mirroring-images"></a>镜像图像

如果应用具有必须针对 RTL 进行镜像的图像（即可以翻转相同图像），则可以应用 [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 属性：

```XML
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```


如果应用需要其他图像来正确翻转该图像，那么你可以将资源管理系统与 [layoutdir qualifier](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324) 结合使用。 当[应用程序语言](manage-language-and-region.md)设置为 RTL 语言时，系统会选择名为 file.layoutdir-rtl.png 的图像。 当图像的某一部分翻转而其他部分不翻转时，可能必须使用此方法。

## <a name="fonts"></a>字体

使用 [**LanguageFont**](https://msdn.microsoft.com/library/windows/apps/br206864) 以编程方式访问为特定语言建议的字体系列、大小、粗细和样式。 **LanguageFont** 对象提供了对各种类别内容的正确字体信息的访问，这些信息包括 UI 标头、通知、正文文本和用户可编辑的文档正文字体。

## <a name="best-practices-for-handling-right-to-left-rtl-languages"></a>处理从右往左 (RTL) 语言的最佳做法

当为从右往左 (RTL) 语言本地化应用时，请使用 API 为 RootFrame 设置默认文本方向。 这将导致 RootFrame 包含的所有控件将相应地响应默认文本方向。  在支持多种语言时，请将 LayoutDirection 用于首选语言以设置 FlowDirection 属性。 包含在 Windows 中的大多数控件已使用 FlowDirection。 如果在实现自定义控件，这些控件应使用 FlowDirection 相应地更改 RTL 和 LTR 语言布局。

C#
```csharp    
// For bidirectional languages, determine flow direction for RootFrame and all derived UI.

    string resourceFlowDirection = ResourceContext.GetForCurrentView().QualifierValues["LayoutDirection"];
    if (resourceFlowDirection == "LTR")
    {
       RootFrame.FlowDirection = FlowDirection.LeftToRight;
    }
    else
    {
       RootFrame.FlowDirection = FlowDirection.RightToLeft;
    }
```

C++：
```
    // Get preferred app language
    m_language = Windows::Globalization::ApplicationLanguages::Languages->GetAt(0);
     
    // Set flow direction accordingly
    m_flowDirection = ResourceManager::Current->DefaultContext->QualifierValues->Lookup("LayoutDirection") != "LTR" ? 
       FlowDirection::RightToLeft : FlowDirection::LeftToRight;
```


### <a name="rtl-faq"></a>RTL 常见问题 

<dl>
  <dt> <p><b>问：</b> <b>FlowDirection</b> 会根据当前语言选择自动设置吗？ 例如，如果我选择英语，它会从左往右显示，而在选择阿拉伯语时，它又会从右往左显示吗？</p></dt>

  <dd><p><b>答：</b> <b>FlowDirection</b> 不会考虑语言种类。 你要为当前显示的语言的选择相应的 <b>FlowDirection</b>。 查看上述示例代码。</p></dd> 

  <dt> <p><b>问：</b> 我不太熟悉本地化。 资源是否已经包含流动方向？ 能否根据当前语言确定流动方向？</p></dt>

  <dd> <p><b>答：</b>如果你在使用当前的最佳做法，资源不会直接包含流动方向。 必须确定当前语言的流动方向。 有两种方式可执行此操作： </p>
   <p>首选方法是将 LayoutDirection 用于首选语言以设置 RootFrame 的 FlowDirection 属性。 RootFrame 中的所有控件均继承自 RootFrame 的 FlowDirection。</p>
   <p>另一种方法是为本地化的 RTL 语言设置 resw 文件中的 FlowDirection。 例如，你可能拥有阿拉伯语的 resw 文件和希伯来语的 resw file。 在这些文件中，可以使用 x:UID 设置 FlowDirection。 但相比于编程方法，此方法更易于出错。</p></dd>
</dl>


## <a name="related-topics"></a>相关主题
[FlowDirection](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.flowdirection.aspx)

---
author: muhsinking
Description: Panning and scrolling allows users to reach content that extends beyond the bounds of the screen.
title: 滚动查看器控件
ms.assetid: 1BFF0E81-BF9C-43F7-95F6-EFC6BDD5EC31
label: Scrollbars
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, uwp
pm-contact: Abarlow, pagildea
design-contact: ksulliv
dev-contact: regisb
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 983f80c705aba44fe096f9dfa05b71b0cf28f294
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2018
ms.locfileid: "5993798"
---
# <a name="scroll-viewer-controls"></a>滚动查看器控件



当 UI 内容超出一个区域的容量时，可以使用滚动查看器控件。

> **重要 API**：[ScrollViewer 类](https://msdn.microsoft.com/library/windows/apps/br209527)，[ScrollBar 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.scrollbar.aspx)

滚动查看器允许内容延伸到视区（可见区域）边界外。 用户可以通过触摸、鼠标滚轮、键盘或游戏板操作滚动查看器图面或者使用鼠标或笔光标操作滚动查看器滚动条查看此内容。 此图显示了滚动查看器控件的几个示例。

![说明标准滚动条控件的屏幕截图](images/ScrollBar_Standard.jpg)

根据情况，滚动查看器的滚动条使用两种不同的可视化效果，如下图所示：平移指示器（左）和传统滚动条（右）。

![标准滚动条和平移指示器控件的外观示例](images/SCROLLBAR.png)

滚动查看器注意用户输入法，并用它来确定以哪种可视化效果显示。

* 当通过触摸等方式不直接操作滚动条滚动区域时，平移指示器就会出现，显示当前滚动位置。
* 将鼠标或笔光标移动到平移指示器上方时，它会变为传统滚动条。  拖动滚动条滑块可操作滚动区域。

<!--
<div class="microsoft-internal-note">
See complete redlines in [UNI]http://uni/DesignDepot.FrontEnd/#/ProductNav/3378/0/dv/?t=Windows|Controls|ScrollControls&f=RS2
</div>
-->

![操作中的滚动条](images/conscious-scroll.gif)

> [!NOTE]
> 当滚动条可见时，它以 16px 叠加在 ScrollViewer 中内容的上方。 为确保好的 UX 设计，你会希望保证这一叠加不会遮挡任何交互式内容。 此外，如果你不希望有 UX 重叠，可以在视区边缘为滚动条保留 16px 的填充。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/ScrollViewer">打开此应用，了解 ScrollViewer 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-scroll-viewer"></a>创建滚动查看器

若要向页面添加垂直滚动，请将页面内容打包在滚动查看器中。

```xaml
<Page
    x:Class="App1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App1">

    <ScrollViewer>
        <StackPanel>
            <TextBlock Text="My Page Title" Style="{StaticResource TitleTextBlockStyle}"/>
            <!-- more page content -->
        </StackPanel>
    </ScrollViewer>
</Page>
```

此 XAML 显示如何在滚动查看器中放置图像并启用缩放。

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10"
              HorizontalScrollMode="Enabled" HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

## <a name="scrollviewer-in-a-control-template"></a>控件模板中的 ScrollViewer

ScrollViewer 控件作为其他控件复合部分的形式存在是普遍情况。 仅在主机控件的布局空间限制为比扩展的内容大小更小的时候，ScrollViewer 部件和提供支持的 [ScrollContentPresenter](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollcontentpresenter.aspx) 类才会显示视口和滚动条。 列表经常会发生此情况，因此 [ListView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx) 和 [GridView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx) 模板始终会包括 ScrollViewer。 [TextBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx) 和 [RichEditBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx) 也在其模板中包括了 ScrollViewer。

当 **ScrollViewer** 部件存在于控件中时，主机控件通常对某些能够使内容滚动的输入事件和操作内置了事件处理。 例如，GridView 解释轻扫手势，而这会使内容水平滚动。 主机控件接收的输入事件和原始操作视作由该控件处理，并且低级别事件（例如 [PointerPressed](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.pointerpressed.aspx)）不会引发，也不会浮升到任何父容器。 你可以更改某些内置控件处理，方法是覆盖控件类和事件的 **On*** 虚拟方法，或重新模板化控件。 但这两种情况要重现原始默认行为都不简单，这种行为通常已存在，以便控件以预期方式对事件以及用户输入操作和手势做出反应。 因此应该考虑是否正需要引发该输入事件。 你可能想调查是否有不受控件处理的其他输入事件或手势，并在应用或控件交互设计中使用它们。

为让包括 ScrollViewer 的控件能够影响 ScrollViewer 部件的某些行为和属性，ScrollViewer 定义了大量能够在样式中设置并在模板绑定中使用的 XAML 附加属性。 有关附加属性的详细信息，请参阅[附加属性概述](../../xaml-platform/attached-properties-overview.md)。

**ScrollViewer XAML 附加属性**

ScrollViewer 定义以下 XAML 附加属性：

- [ScrollViewer.BringIntoViewOnFocusChange](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.bringintoviewonfocuschange.aspx)
- [ScrollViewer.HorizontalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility.aspx)
- [ScrollViewer.HorizontalScrollMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode.aspx)
- [ScrollViewer.IsDeferredScrollingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isdeferredscrollingenabled.aspx)
- [ScrollViewer.IsHorizontalRailEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.ishorizontalrailenabled.aspx)
- [ScrollViewer.IsHorizontalScrollChainingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.ishorizontalscrollchainingenabled.aspx)
- [ScrollViewer.IsScrollInertiaEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isscrollinertiaenabled.aspx)
- [ScrollViewer.IsVerticalRailEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isverticalrailenabled.aspx)
- [ScrollViewer.IsVerticalScrollChainingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isverticalscrollchainingenabled.aspx)
- [ScrollViewer.IsZoomChainingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled.aspx)
- [ScrollViewer.IsZoomInertiaEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled.aspx)
- [ScrollViewer.VerticalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibilityproperty.aspx)
- [ScrollViewer.VerticalScrollMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.verticalscrollmode.aspx)
- [ScrollViewer.ZoomMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.zoommode.aspx)

这些 XAML 附加属性计划用于 ScrollViewer 处于隐式状态的情况，例如 ScrollViewer 存在于 ListView 或 GridView 的默认模板中，并且你想要在不访问模板部件的情况下影响控件的滚动行为。

例如，下面是如何使 ListView 内置滚动查看器的垂直滚动条始终可见的示例。

```xaml
<ListView ScrollViewer.VerticalScrollBarVisibility="Visible"/>
```

对于 XAML 中的 ScrollViewer 为显式的情况（正如示例代码所示），你无需使用附加属性语法。 只需使用属性语法，例如 `<ScrollViewer VerticalScrollBarVisibility="Visible"/>`。


## <a name="dos-and-donts"></a>应做事项和禁止事项

- 尽可能针对垂直滚动（而不是水平）进行设计。
- 对于超出一条视口边界（垂直或水平）的内容区域使用单轴平移。 对于超出两条视口边界（垂直和水平）的内容区域使用双轴平移。
- 在列表视图、网格视图、组合框、列表框、文本输入框和中心控件中使用内置滚动功能。 如果项目过多无法同时显示，用户可以借助这些控件在项目列表中水平或垂直滚动。
- 如果你希望用户可在较大的区域中在两个方向上平移并缩放，请将该图像放置到滚动查看器中。例如，如果你希望用户可以在完整大小的图像（不是调整大小以适应屏幕的图像）中平移和缩放。
- 如果用户将滚动查看一段较长的文本，请配置滚动查看器，使其仅在垂直方向滚动。
- 使用滚动查看器仅包含一个对象。 请注意，该唯一对象可能是版式面板，它反过来包含自身的任意数量的对象。
- 不要将 [Pivot](tabs-pivot.md) 控件放置在滚动查看器内，避免与透视表的滚动逻辑发生冲突。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-topics"></a>相关主题

**对于开发人员 (XAML)**

* [ScrollViewer 类](https://msdn.microsoft.com/library/windows/apps/br209527)

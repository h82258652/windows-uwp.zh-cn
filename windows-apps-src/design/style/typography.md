---
description: 了解如何在应用中使用版式帮助用户轻松了解内容。
title: UWP 应用中的版式
ms.date: 04/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1d162fcf9a0f1023c58792e8c9f7a0e22fac4440
ms.sourcegitcommit: f7ef7e894d7b7fc24483b4485605686abf8f2e93
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2019
ms.locfileid: "68867756"
---
# <a name="typography"></a>版式

![主图](images/header-typography.svg)

作为语言的视觉表示形式，版式的主要任务是传达信息。 它的样式应永远不妨碍该目标。 本文介绍如何在 UWP 应用中设计版式以帮助用户轻松高效地了解内容。

## <a name="font"></a>Font

应在应用的全部 UI 中使用同一种字体，建议始终使用 UWP 应用的默认字体 Segoe UI  。 其设计目的是为保持不同字体大小和像素密度下的最佳可读性，并提供可润色系统内容的清晰、明朗的美学效果。

![Segoe UI 字体的示例文本](images/type/segoe-sample.svg)

若要在应用上显示非英语语言或为应用选择另一种字体，请参阅[语言](#languages)和[字体](#fonts)，了解我们推荐的 UWP 应用字体。

:::row:::
    :::column:::
![应做事项](images/do.svg) 为 UI 选取一种字体。
    :::column-end:::
    :::column:::
![禁止事项](images/dont.svg) 不要混用多种字体。
    :::column-end:::
:::row-end:::

## <a name="size-and-scaling"></a>大小和缩放

UWP 应用中的字号可在所有设备上自动缩放。 该缩放算法确保可从 10 英尺远处识别 Surface Hub 上高 24 像素的字体，正如从几英寸远处识别 5 英寸手机上高 24 像素的字体。

![不同设备的观看距离](images/type/scaling-chart.svg)

因系统缩放的工作原理，设计时采用的是有效像素而非实际物理像素，所以不必更改字体大小来适应不同尺寸的屏幕的分辨率。

:::row:::
    :::column:::
![应做事项](images/do.svg) 遵循 UWP [字体渐变](#type-ramp)大小调整方式。
    :::column-end:::
    :::column:::
![禁止事项](images/dont.svg) 使用小于 12 像素的字号。
    :::column-end:::
:::row-end:::

## <a name="hierarchy"></a>层次结构

:::row:::
    :::column:::
用户在扫描页面时依赖于视觉层次结构：标题用于总结内容，正文文本用于提供更多详细信息。 若要在应用中创建清晰的视觉层次结构，请遵循 UWP 字体渐变。
    :::column-end:::
    :::column:::
![文本块样式](images/type/type-hierarchy.svg)
    :::column-end:::
:::row-end:::

### <a name="type-ramp"></a>字体渐变

UWP 字体渐变在页面上的字型之间建立关键关系，帮助用户轻松阅读内容。 所有大小均以有效像素为单位，并针对所有设备上运行的 UWP 应用进行了优化。

![字体渐变](images/type/type-ramp.png)

### <a name="using-the-type-ramp"></a>使用字体渐变

:::row:::
    :::column:::
可以像访问 XAML [静态资源](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp)那样访问字体渐变的不同层次。 样式遵循 `*TextBlockStyle` 命名约定。
    :::column-end:::
    :::column:::
![文本块样式](images/type/text-block-type-ramp.svg)
    :::column-end:::
:::row-end:::

```XAML
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>
```

:::row:::
    :::column:::
![应做事项](images/do.svg) 对大多数文本使用“Body”。

对空间受限的标题使用“Base”。
    :::column-end:::
    :::column:::
![禁止事项](images/dont.svg) 对主要操作或任何长字符串使用“Caption”。

如果文本需要换行，请使用“Header”或“Subheader”。
    :::column-end:::
:::row-end:::

## <a name="alignment"></a>对齐

默认 [TextAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.textalignment) 是左对齐，在大多数情况下，左边对齐但右边不对齐可提供一致的内容编排效果和统一的布局。 有关 RTL 语言，请参阅[调整布局和字体以支持全球化](../globalizing/adjust-layout-and-fonts--and-support-rtl.md)。

![显示左对齐的文本。](images/type/alignment.svg)

```xaml
<TextBlock TextAlignment="Left">
```

## <a name="character-count"></a>字符数

:::row:::
    :::column:::
![应做事项](images/do.svg) 每行保持 50-60 个字母以便于阅读。
    :::column-end:::
    :::column:::
![禁止事项](images/dont.svg) 每行少于 20 个字符或多于 60 个字符将不便于阅读。
    :::column-end:::
:::row-end:::

## <a name="clipping-and-ellipses"></a>剪切和省略号

当文本数量超出可用空间时，建议剪裁文本，这是大多数 [UWP 文本控件](../controls-and-patterns/text-controls.md)的默认行为。

![显示剪裁一些文字的设备框架](images/type/clipping.svg)

```xaml
<TextBlock TextWrapping="WrapWholeWords" TextTrimming="Clip"/>
```

:::row:::
    :::column:::
![应做事项](images/do.svg) 剪裁文本，如果启用了多行显示则换行。
    :::column-end:::
    :::column:::
![禁止事项](images/dont.svg) 使用省略号以避免视觉上的杂乱感。
    :::column-end:::
:::row-end:::

**注意**：如果未对容器进行完善定义（例如，不区分背景颜色），或者当存在用于查看更多文本的链接时，则使用省略号。

## <a name="languages"></a>语言 

Segoe UI 适用于英语、欧洲语言、希腊语、希伯来语、亚美尼亚语、格鲁吉亚语以及阿拉伯语。 对于其他语言，请参阅以下建议。

### <a name="globalizinglocalizing-fonts"></a>全球化/本地化字体

使用 [LanguageFont 字体映射 API](https://docs.microsoft.com/uwp/api/Windows.Globalization.Fonts.LanguageFont) 以编程方式访问为特定语言建议的字体系列、大小、粗细和样式。 LanguageFont 对象提供了对各种类别内容的正确字体信息的访问，这些信息包括 UI 标头、通知、正文文本和用户可编辑的文档正文字体。 有关详细信息，请参阅[调整布局和字体以支持全球化](../globalizing/adjust-layout-and-fonts--and-support-rtl.md)。

### <a name="fonts-for-non-latin-languages"></a>非拉丁语言字体

<table>
<thead>
<tr class="header">
<th align="left">字体系列</th>
<th align="left">样式</th>
<th align="left">注释</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Embrima;">Ebrima</td>
<td align="left">常规、粗体</td>
<td align="left">非洲语言脚本的用户界面字体（埃塞俄比亚文、西非书面文、索马里文、提芬纳格文、瓦伊文）。</td>
</tr>
<tr class="even">
<td style="font-family: Gadugi;">Gadugi</td>
<td align="left">常规、粗体</td>
<td align="left">北美语言脚本的用户界面字体（加拿大音节文字、切罗基语）</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Leelawadee UI;">Leelawadee UI</td>
<td align="left">常规、半细、粗体</td>
<td align="left">东南亚语言脚本的用户界面字体（布吉斯语、老挝语、泰语）。</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Malgun Gothic;">Malgun Gothic</td>
<td align="left">常规</td>
<td align="left">朝鲜语的用户界面字体。</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft JhengHei UI;">Microsoft JhengHei UI</td>
<td align="left">常规、粗体、细体</td>
<td align="left">繁体中文的用户界面字体。</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft YaHei UI;">Microsoft YaHei UI</td>
<td align="left">常规、粗体、细体</td>
<td align="left">简体中文的用户界面字体。</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Myanmar Text;">Myanmar Text</td>
<td align="left">常规</td>
<td align="left">缅甸文脚本的后备字体。</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Nirmala UI;">Nirmala UI</td>
<td align="left">常规、半细、粗体</td>
<td align="left">南亚语言脚本的用户界面字体（孟加拉语、梵文、古吉拉特语、锡克教文、埃纳德语、马拉雅拉姆语、奥里亚语、欧甘语、僧伽罗语、索拉文、泰米尔语、泰卢固语）</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: SimSun;">SimSun</td>
<td align="left">常规</td>
<td align="left">传统的中文用户界面字体。 </td>
</tr>
<tr class="even">
<td align="left" style="font-family: Yu Gothic UI;">Yu Gothic UI</td>
<td align="left">细体、半细、常规、半粗、粗体</td>
<td align="left">日语的用户界面字体。</td>
</tr>
</tbody>
</table>

## <a name="fonts"></a>字体

### <a name="sans-serif-fonts"></a>Sans-serif 字体

Sans-serif 字体是用于标题和 UI 元素的不错选择。 

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">字体系列</th>
<th align="left">样式</th>
<th align="left">注释</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left" style="font-family: Arial;">Arial</td>
<td align="left">常规、斜体、粗体、粗斜体、黑体</td>
<td align="left">支持欧洲和中东语言脚本（拉丁文、希腊语、西里尔文、阿拉伯语、亚美尼亚语和希伯来语）黑粗体仅支持欧洲语言脚本。</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Calibri;">Calibri</td>
<td align="left">常规、斜体、粗体、粗斜体、细体、细斜体</td>
<td align="left">支持欧洲和中东语言脚本（拉丁文、希腊语、西里尔文、阿拉伯语和希伯来语）。 阿拉伯语仅在竖体中可用。</td>
</tr>
<td style="font-family: Consolas;">Consolas</td>
<td>常规、斜体、粗体、粗斜体</td>
<td>支持欧洲语言脚本（拉丁文、希腊语和西里尔文）的固定宽度字体。</td>
</tr>

<tr>
<td style="font-family: Segoe UI;">Segoe UI</td>
<td>常规、斜体、细斜体、黑斜体、粗体、粗斜体、细体、半细、半粗、黑体</td>
<td>欧洲和中东语言脚本（阿拉伯语、亚美尼亚语、西里尔文、格鲁吉亚语、希腊语、希伯来语、拉丁文）以及傈僳语脚本的用户界面字体。</td>
</tr>

<tr class="even">
<td style="font-family: Selawik;">Selawik</td>
<td align="left">常规、半细、细体、粗体、半粗</td>
<td align="left">计量方面与 Segoe UI 兼容的开源字体，用于其它平台上不希望包含 Segoe UI 的应用。 <a href="https://github.com/Microsoft/Selawik">在 GitHub 上获取 Selawik。</a></td>
</tr>

</tbody>
</table>

### <a name="serif-fonts"></a>Serif 字体

Serif 字体适合用于显示大量文本。 

<table>
<thead>
<tr class="header">
<th align="left">字体系列</th>
<th align="left">样式</th>
<th align="left">注释</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Cambria;">Cambria</td>
<td align="left">常规</td>
<td align="left">支持欧洲语言脚本（拉丁文、希腊语、西里尔文）的 Serif 字体。</td>
</tr>
<tr class="even">
<td style="font-family: Courier New;">Courier New</td>
<td align="left">常规、斜体、粗体、粗斜体</td>
<td align="left">支持欧洲和中东语言脚本（拉丁文、希腊语、西里尔文、阿拉伯语、亚美尼亚语和希伯来语）的 Serif 固定宽度字体。</td>
</tr>
<tr class="odd">
<td style="font-family: Georgia;">格鲁吉亚</td>
<td align="left">常规、斜体、粗体、粗斜体</td>
<td align="left">支持欧洲语言脚本（拉丁文、希腊语和西里尔文）。</td>
</tr>

<tr class="even">
<td style="font-family: Times New Roman;">Times New Roman</td>
<td align="left">常规、斜体、粗体、粗斜体</td>
<td align="left">支持欧洲语言脚本（拉丁文、希腊语、西里尔文、阿拉伯语、亚美尼亚语、希伯来语）的传统字体。</td>
</tr>

</tbody>
</table>

### <a name="symbols-and-icons"></a>符号和图标

<table>
<thead>
<tr class="header">
<th align="left">字体系列</th>
<th align="left">样式</th>
<th align="left">注释</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Segoe MDL2 Assets</td>
<td align="left">常规</td>
<td align="left">应用图标的用户界面字体。 有关详细信息，请参阅 <a href="segoe-ui-symbol-font.md">Segoe MDL2 Assets 文章</a>。</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Emoji</td>
<td align="left">常规</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">常规</td>
<td align="left">符号的后备字体</td>
</tr>
</tbody>
</table>

## <a name="related-articles"></a>相关文章

* [文本控件](../controls-and-patterns/text-controls.md)
* [XAML 主题资源](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp)
* [XAML 样式](../controls-and-patterns/xaml-styles.md)
* [Microsoft 板式](https://docs.microsoft.com/typography/)

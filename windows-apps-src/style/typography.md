---
author: mijacobs
description: "作为语言的可视化表示形式，版式的主要任务是清楚简洁。 它的样式应永远不妨碍该目标。 但是版式作为布局组件（对设计的密度和复杂程度有着强大的影响）对设计的用户体验有着重大作用。"
title: "版式"
ms.assetid: ca35f78a-e4da-423d-9f5b-75896e0b8f82
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 8338b4ebcdd73f1b7ebf1dedafe68d861cd9d93b
ms.openlocfilehash: 481c66e3edd42722cfd59bf420fe5b6286706245

---

# 版式

作为语言的视觉表现形式，版式的主要任务是清晰。 它的样式应永远不妨碍该目标。 但是版式作为布局组件（对设计的密度和复杂程度有着强大的影响）对设计的用户体验有着重大作用。

## 字样

我们选择了 Segoe UI 以用于所有 Microsoft 数字设计。 Segoe UI 提供了各种各样的字符，其设计目的是在不同大小和像素密度之间保持最佳的易读性。 它提供了简洁清雅、落落大方的美感，从而丰富了系统内容。

![Segoe UI 字体的示例文本](images/segoe-sample.png)

## 粗细

我们使版式一眼望去非常简洁且有效率。 我们选择使用一种粗细和大小均为最小的字样和清晰的层次结构。 排版和对齐遵循给定语言的默认样式。 英语版本的顺序是从左往右，从上到下。 文本和图像之间的关系清晰明了。

![显示了支持的字体粗细。 细体、半细体、常规、半粗体和粗体](images/weights.png)

## 行距

![行距为 125% 的示例](images/line-spacing.png)

行距应为字体大小的 125%，如有必要，则舍入为最近的 4 的倍数。 以 15 像素的 Segoe UI 为例，15 像素的 125% 就是 18.75 像素。 我们建议四舍五入，将行高设置为 20 像素以保持在 4 像素网格上。 这将确保良好的阅读体验，并且为变音符提供了足够的空间。 有关具体示例，请参阅下面的“字体渐变”部分。

在较小字体上堆叠较大字体时，较大字体的最后一个基线与较小字体的第一个基线之间的距离应与较大字体的行高相等。

![显示较大字体如何在较小字体上进行堆叠](images/line-height-stacking.png)

在 XAML 中，可通过将两个 [TextBlocks](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) 堆叠起来并设置适当的边距来实现这一点。

```xml
<StackPanel Width="200">
    <!-- Setting a bottom margin of 3px on the header
         puts the baseline of the body text exactly 24px
         below the baseline of the header. 24px is the
         recommended line height for a 20px font size,
         which is what's set in SubtitleTextBlockStyle.
         The bottom margin will be different for
         different font size pairings. -->
    <TextBlock
        Style="{StaticResource SubtitleTextBlockStyle}"
        Margin="0,0,0,3"
        Text="Header text" />
    <TextBlock
        Style="{StaticResource BodyTextBlockStyle}"
        TextWrapping="Wrap"
        Text="This line of text should be positioned where the above header would have wrapped." />
</StackPanel>
```



## 字距调整和字距

Segoe 是一种人性化的字样，有着柔和友好的外观和基于手写文本的有机开放形式。 为了确保最佳的易读性并保持其人性化的完整性，字距调整和字距必须具有特定的值。

字距调整应设置为“标准”，字距应设置为“0”。


![显示字距调整和字距之间的差异](images/kerning-tracking.png)



## 单词和字母间距

类似于字距调整和字距，单词间距和字母间距使用特定设置来确保最佳的易读性和人性化的完整性。

默认情况下，单词间距始终为 100%，而字母间距应设置为“0”。


![显示单词和字母间距之间的差异](images/word-letter.png)

**注意**&nbsp;&nbsp;在 XAML 文本控件中，[Typogrphy.Kerning](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.documents.typography.kerning.aspx) 用于控制字距调整，而 [FontStretch](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.control.fontstretch.aspx) 用于控制字距。 Typography.Kerning 的默认设置为“true”，FontStretch 的默认设置为“Normal”，它们都是推荐值。




## 对齐

通常情况下，我们建议字体的可视元素和列应左对齐。 在大多数情况下，这种左边对齐但右边未对齐的方法可提供一致的内容编排和统一的布局。


![显示左对齐的文本](images/alignment.png)



## 行尾

当版式未按照左边对齐但右边非对齐的形式进行排版时，请尝试确保行尾整齐，避免有连字符。


![显示整齐的行尾](images/line-endings.png)

## 段落

若要确保列边缘对齐，应通过无缩进跳行指示段落。

![显示段落之间的完整行距](images/paragraphs.png)

## 字符数

如果一行太短，眼睛将需要过于频繁地左右来回扫视，从而破坏读者的节奏。 如果可以，每行最好有 50 到 60 个字母以便于阅读。

Segoe 提供了各种各样的字符，其设计目的是在小字体和大字体以及低像素密度和高像素密度之间保持最佳的易读性。 在文本列行中使用最佳的字母数量可确保在应用程序中有着良好的易读性。

行太长会使眼睛疲劳，并且可能会使用户感到迷惑。 行太短会强制读者的眼睛过多地来回扫视，并且会导致眼睛疲劳。

![显示 3 个行的长度不同的段落](images/character-count.png)

## 悬挂式文字对齐

可以通过方式处理带有文字的图标的水平对齐，具体取决于图标的大小和文字的数量。 当文字（不论是单行还是多行）符合图标的高度时，文本应该垂直居中。

如果文字的高度超出了图标的高度，则文字的第一行应垂直对齐，其他文字应自然顺流下来。 当与大写字母一起使用时（无论高度是升序还是降序），应仔细查看相同的对齐指南。

![显示若干图标和文字的配对](images/hanging-text-alignment.png)

**注意**&nbsp;&nbsp;XAML 的 [TextBlock.TextLineBounds](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.textlinebounds.aspx) 属性提供了对大写字母高度和基线字体指标的访问权限。 它可以用于使文字在视觉上垂直居中或顶部对齐。

## 剪切和省略号

默认进行剪裁：假定文字将自动换行，除非另有去除规定。 在使用非换行文字时，我们建议使用剪裁，而不是使用省略号。 剪裁可能会发生在容器、设备、滚动条等的边缘处。

例外情况：对于未明确定义的容器（例如，不区分背景颜色），可以去除非换行文字而使用省略号“…”。

![显示剪裁一些文字的设备框架](images/clipping.png)

## 字体渐变
类型渐变建立了从标题行到正文文本的关键设计关系，并确保不同层次之间具有清晰和易于理解的层次结构。 此层次结构会生成一个使用户可以轻松地通过书面交流进行导航的结构。

![显示字体渐变](images/type-ramp.png) 所有大小都是以有效像素为单位。 


**注意**&nbsp;&nbsp;大多数级别的渐变都可用作遵循 `*TextBlockStyle` 命名约定（`HeaderTextBlockStyle` 除外）的 XAML [静态资源](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx#the_xaml_type_ramp)。


## 主要文字和次要文字

若要在字体渐变之外创建其他层次结构，请将次要文字的不透明度设置为 60%。 在[“主题调色板”](color.md#color-themes)中，可使用 BaseMedium。 主要文字的不透明度应始终为 100% 或 BaseHigh。


## 全部大写标题

某些页面标题应采用“ALL CAPS”以便添加其他维度的层次结构。 这些标题应使用 BaseAlt，字符间距设置为字体高度的 7.5%。 也可以使用这种处理方式帮助应用导航。

但是，在某些语言中，专有名词大写会更改其含义，因此所有基于名称或用户输入的页面标题*不*应转换为全部大写。


**应做事项**



* 对大部分文字使用“Body”
* 对空间受限的标题使用“Base”
* 合并 SubtitleAlt 以便通过强调顶级内容来创建对比和层次结构



**禁止事项**



* 请勿对长字符串或任何主要操作使用“大写”
* 如果文字需要换行，请勿使用“Header”或“Subheader”
* 请勿在同一页面上合并 Subtitle 和 SubtitleAlt



## 相关文章

* [文本控件](../controls-and-patterns/text-controls.md)



<!--HONumber=Aug16_HO3-->



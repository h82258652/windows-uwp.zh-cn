---
Description: 全球化就是在无需做任何更改或自定义的情况下设计和开发你的应用以便适应不同的国际市场的过程。
Search.SourceType：视频
title: 全球化和本地化
ms.assetid: c0791eec-5bb8-4a13-8977-61d7d98e35ce
label: 简介
template: detail.hbs
---

# 全球化和本地化


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Windows 的使用遍及世界各地，用户的文化背景、区域及语言也各不相同。 用户可能会说任意一种语言，甚至多种语言。 用户可能位于世界的任何地方，可能在任何地方说任何语言。 通过使用*全球化*和*本地化*对应用进行设计使其更具有适应性，可以拓展其潜在市场。

**全球化** 就是在无需做任何更改或自定义的情况下设计和开发你的应用以便适应不同的国际市场的过程。

例如，你可以：

-   设计应用的布局以适应标签和文本字符串中不同的文本长度和其他语言的字体大小。
-   从可适应于不同当地市场的资源中检索文本和与文化相关的图像，而非将它们硬编码到你的应用编码或标记中。
-   使用全球化 API 来显示按不同区域进行不同格式化的数据，例如数值、日期、时间以及货币。

**本地化** 就是对你的应用进行改编以满足特定当地市场的语言、文化和政治要求的过程。

例如：

-   翻译应用的文本和标签以适应新市场，并且为其语言创建单独的资源。
-   修改任何与文化相关的图像（如有必要），并且放在单独的资源中。

观看此视频以简要了解如何准备在全球发布的应用：[全球化和本地化简介](https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-globalization-and-localization)。

## 文章
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">文章</th>
<th align="left">说明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Do's and don'ts](guidelines-and-checklist-for-globalizing-your-app.md)</p></td>
<td align="left"><p>在将你的应用全球化使其适用于更广泛的用户以及将你的应用本地化使其适用于特定市场时，请遵循这些最佳实践。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Use global-ready formats](use-global-ready-formats.md)</p></td>
<td align="left"><p>通过适当设置日期、时间、数字和货币的格式，开发全球通用的应用。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Manage language and region](manage-language-and-region.md)</p></td>
<td align="left"><p>通过使用 Windows 提供的各种语言和区域设置，控制 Windows 如何选择 UI 资源和设置应用的 UI 元素的格式。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Use patterns to format dates and times](use-patterns-to-format-dates-and-times.md)</p></td>
<td align="left"><p>使用 [<strong>Windows.Globalization.DateTimeFormatting</strong>](https://msdn.microsoft.com/library/windows/apps/br206859) API 和自定义模式严格按照所需模式显示日期和时间。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Adjust layout and fonts, and support RTL](adjust-layout-and-fonts--and-support-rtl.md)</p></td>
<td align="left"><p>开发你的应用来支持多种语言的布局和字体，包括 RTL（从右到左）排列方向。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Prepare your app for localization](prepare-your-app-for-localization.md)</p></td>
<td align="left"><p>准备将你的应用以将其本地化到其他市场、语言或地区。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Put UI strings into resources](put-ui-strings-into-resources.md)</p></td>
<td align="left"><p>将你的 UI 中的字符串资源放入资源文件中。 随后你可从代码或标记引用这些字符串。</p></td>
</tr>
</tbody>
</table>

 

另请参阅最初为 Windows 8.x 创建但仍然适用于通用 Windows 平台 (UWP) 应用和 Windows 10 的文档。

-   [全球化你的应用](https://msdn.microsoft.com/library/windows/apps/xaml/hh965328)
-   [语言匹配](https://msdn.microsoft.com/library/windows/apps/xaml/jj673578.aspx)
-   [NumeralSystem 值](https://msdn.microsoft.com/library/windows/apps/xaml/jj236471.aspx)
-   [国际字体](https://msdn.microsoft.com/library/windows/apps/xaml/dn263115.aspx)
-   [应用资源和本地化](https://msdn.microsoft.com/library/windows/apps/xaml/hh710212.aspx)

 

 





<!--HONumber=Mar16_HO1-->



---
author: Jwmsft
Description: "在为 UWP 应用选择字体并指定字体大小和颜色时，请遵循以下指南。"
title: "适用于 UWP 应用的字体"
ms.assetid: 1B8B90AD-CDC4-4997-ACDE-871C1E94A929
label: Fonts
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: b258771c887d4422433522344b11130b7e9ed1e6
ms.openlocfilehash: 0b25dc91a5ec82a83ae24a41854e9eeab8990128

---


# <a name="fonts-for-uwp-apps"></a>适用于 UWP 应用的字体

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

本文列出了为 UWP 应用推荐的字体。 这些字体保证可用于支持 UWP 应用的所有 Windows 10 版本。

<div class="important-apis" >
<b>重要的 API</b><br/>
<ul>
<li>[**FontFamily 属性**](https://msdn.microsoft.com/library/windows/apps/br209655)</li>
</ul>
</div>

[UWP 版式指南](typography.md)建议，应用应使用 Segoe UI 字体，虽然对大多数应用而言，Segoe UI 是一项不错的选择，但无需将此字体用于所有应用。 你可以将其他字体用于某些场景，例如阅读，或使用非英语语言显示文本的情况。 
 
## <a name="sans-serif-fonts"></a>Sans-serif 字体

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
<th align="left">注意</th>
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

<tr class="odd">
<td>Segoe UI Historic</td>
<td align="left">常规</td>
<td align="left">历史脚本的后备字体</td>
</tr>

<tr class="even">
<td style="font-family: Selawik;">Selawik</td>
<td align="left">常规、半细、细体、粗体、半粗</td>
<td align="left">计量方面与 Segoe UI 兼容的开源字体，用于其它平台上不希望包含 Segoe UI 的应用。 [在 GitHub 上获取 Selawik。](https://github.com/Microsoft/Selawik)</td>
</tr>

<tr class="even">
<td style="font-family: Verdana;">Verdana</td>
<td align="left">常规、斜体、粗体、粗斜体</td>
<td align="left">支持欧洲语言脚本（拉丁文、希腊语、西里尔文和亚美尼亚语）。</td>
</tr>

</tbody>
</table>


## <a name="serif-fonts"></a>Serif 字体

Serif 字体适合用于显示大量文本。 

<table>
<thead>
<tr class="header">
<th align="left">字体系列</th>
<th align="left">样式</th>
<th align="left">注意</th>
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
<td style="font-family: Georgia;">Georgia</td>
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

## <a name="symbols-and-icons"></a>符号和图标


<table>
<thead>
<tr class="header">
<th align="left">字体系列</th>
<th align="left">样式</th>
<th align="left">注意</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Segoe MDL2 Assets</td>
<td align="left">常规</td>
<td align="left">应用图标的用户界面字体。 有关详细信息，请参阅 [Segoe MDL2 Assets 文章](segoe-ui-symbol-font.md)。</td>
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



## <a name="fonts-for-non-latin-languages"></a>非拉丁语言字体

虽然其中很多字体提供拉丁字符，但还是要使用这些字体。

<table>
<thead>
<tr class="header">
<th align="left">字体系列</th>
<th align="left">样式</th>
<th align="left">注意</th>
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
<tr class="even">
<td align="left">爪哇语文本 常规 爪哇文脚本的后备字体</td>
<td align="left">常规</td>
<td align="left">爪哇文脚本的后备字体</td>
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
<td align="left" style="font-family: Microsoft Himalaya;">Microsoft Himalaya</td>
<td align="left">常规</td>
<td align="left">藏语脚本的后备字体。</td>
</tr>
<!--
<tr class="odd">
<td align="left" style="font-family: Microsoft JhengHei;">Microsoft JhengHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
-->
<tr class="even">
<td align="left" style="font-family: Microsoft JhengHei UI;">Microsoft JhengHei UI</td>
<td align="left">常规、粗体、细体</td>
<td align="left">繁体中文的用户界面字体。</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft New Tai Lue;">Microsoft New Tai Lue</td>
<td align="left">常规</td>
<td align="left">西双版纳新傣文脚本的后备字体。</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft PhagsPa;">Microsoft PhagsPa</td>
<td align="left">常规</td>
<td align="left">八思巴文脚本的后备字体。</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft Tai Le;">Microsoft Tai Le</td>
<td align="left">常规</td>
<td align="left">德宏傣文脚本的后备字体。</td>
</tr>
<!--
<tr class="even">
<td align="left" style="font-family: Microsoft YaHei;">Microsoft YaHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
-->
<tr class="odd">
<td align="left" style="font-family: Microsoft YaHei UI;">Microsoft YaHei UI</td>
<td align="left">常规、粗体、细体</td>
<td align="left">简体中文的用户界面字体。</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft Yi Baiti;">Microsoft Yi Baiti</td>
<td align="left">常规</td>
<td align="left">彝语脚本的后备字体。</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Mongolian Baiti;">Mongolian Baiti</td>
<td align="left">常规</td>
<td align="left">蒙古语脚本的后备字体。</td>
</tr>
<tr class="even">
<td align="left" style="font-family: MV Boli;">MV Boli</td>
<td align="left">常规</td>
<td align="left">马尔代夫书写体脚本的后备字体。</td>
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
<tr class="odd">
<td align="left" style="font-family: Yu Gothic;">Yu Gothic</td>
<td align="left">中</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left" style="font-family: Yu Gothic UI;">Yu Gothic UI</td>
<td align="left">常规</td>
<td align="left">日语的用户界面字体。</td>
</tr>
</tbody>
</table>


## <a name="globalizinglocalizing-fonts"></a>全球化/本地化字体
使用 [LanguageFont 字体映射 API](https://msdn.microsoft.com/library/windows/apps/br206864) 以编程方式访问为特定语言建议的字体系列、大小、粗细和样式。 LanguageFont 对象提供了对各种类别内容的正确字体信息的访问，这些信息包括 UI 标头、通知、正文文本和用户可编辑的文档正文字体。 有关详细信息，请参阅[调整布局和字体以支持全球化](https://msdn.microsoft.com/windows/uwp/globalizing/adjust-layout-and-fonts--and-support-rtl)。


## <a name="get-the-samples"></a>获取示例

* [下载字体示例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlCloudFontIntegration)
* [UI 入门示例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)
* [使用 DirectWrite 划分行距的示例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/DWriteLineSpacingModes) 

## <a name="related-articles"></a>相关文章

* [调整布局和字体以支持全球化](https://msdn.microsoft.com/windows/uwp/globalizing/adjust-layout-and-fonts--and-support-rtl)
* [Segoe MDL2](segoe-ui-symbol-font.md)
* [文本控件](../controls-and-patterns/text-controls.md)
* [XAML 主题资源](https://msdn.microsoft.com/library/windows/apps/mt187274)

 

 







<!--HONumber=Dec16_HO2-->



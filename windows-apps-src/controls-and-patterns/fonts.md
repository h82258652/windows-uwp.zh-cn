---
author: Jwmsft
Description: "在选择字体并指定字体大小和颜色时，应遵循以下指南。"
title: "字体"
ms.assetid: 1B8B90AD-CDC4-4997-ACDE-871C1E94A929
label: Fonts
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 268884c011caa18f3afb6d8b0d9cfda1ec27f51e

---

# 字体指南

**重要的 API**

-   [**FontFamily 属性**](https://msdn.microsoft.com/library/windows/apps/br209655)

合理地使用字体大小、粗细、颜色、轨迹和间距可帮助使你的通用 Windows 平台 (UWP) 应用具有干净整洁的外观，更便于使用。 在选择字体并指定字体大小和颜色时，应遵循以下指南。

如果你要查找 Segoe UI Symbol 图标列表，请参阅 [**Segoe UI Symbol 图标指南**](segoe-ui-symbol-font.md)。

## <span id="The_Windows_10_type_ramp"></span><span id="the_windows_10_type_ramp"></span><span id="THE_WINDOWS_10_TYPE_RAMP"></span>Windows 10 类型渐变


类型渐变建立了从标题行到正文文本的关键设计关系，并确保不同层次之间具有清晰和易于理解的层次结构。 用户可以立即了解在何处找到信息以及如何分析页面。

下面是建议用于 UWP App 的类型渐变：

| 文本样式 | 字样 | 粗细    | 大小 (epx) | 行距 (epx) | 字间距 | 轨迹 (1/1000 em) | XAML 样式键          |
|------------|----------|-----------|------------|--------------------|--------------|----------------------|-------------------------|
| 标头     | Segoe UI | 细体     | 46         | 56                 | 100%         | 0                    | HeaderTextBlockStyle    |
| 副标题  | Segoe UI | 细体     | 34         | 40                 | 100%         | 0                    | SubheaderTextBlockStyle |
| 标题      | Segoe UI | 半细 | 24         | 28                 | 100%         | 0                    | TitleTextBlockStyle     |
| 副标题   | Segoe UI | 常规   | 20         | 24                 | 100%         | 0                    | SubtitleTextBlockStyle  |
| 基本       | Segoe UI | 半粗  | 15         | 20                 | 100%         | 0                    | BaseTextBlockStyle      |
| 正文       | Segoe UI | 常规   | 15         | 20                 | 100%         | 0                    | BodyTextBlockStyle      |
| 标题    | Segoe UI | 常规   | 12         | 14                 | 100%         | 0                    | CaptionTextBlockStyle   |

 

## <span id="Recommended_fonts"></span><span id="recommended_fonts"></span><span id="RECOMMENDED_FONTS"></span>建议的字体


你无需将 Segoe UI 字体用于所有内容。 你可以将其他字体用于特定应用场景，例如阅读，或使用非英语语言显示文本的情况。

下表列出了保证可用于支持 UWP 应用的所有 Windows 10 版本的字体。

**注意** 如果你使用了不在此列表中的字体，你的应用可能会触发从 Microsoft 服务自动下载该字体数据。 这可能会带来性能或其他影响，从而产生问题，对于移动设备而言尤其如此。 特别要注意的是，这可能会使用用户的一些移动流量套餐或产生移动数据使用费用。 将用于移动设备的 UWP 应用绝不应为 UI 内容使用除此列表中字体之外的其他字体。

 

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
<th align="left">备注</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Arial</td>
<td align="left">常规、斜体、粗体、粗斜体、黑体</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Calibri</td>
<td align="left">常规、斜体、粗体、粗斜体、细体、细斜体</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Cambria</td>
<td align="left">常规</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Cambria Math</td>
<td align="left">常规</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Comic Sans MS</td>
<td align="left">常规、斜体、粗体、粗斜体</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Courier New</td>
<td align="left">常规、斜体、粗体、粗斜体</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Ebrima</td>
<td align="left">常规、粗体</td>
<td align="left">非洲语言脚本的用户界面字体（埃塞俄比亚文、西非书面文、索马里文、提芬纳格文、瓦伊文）</td>
</tr>
<tr class="even">
<td align="left">Gadugi</td>
<td align="left">常规</td>
<td align="left">北美语言脚本的用户界面字体（加拿大音节文字、切罗基语）</td>
</tr>
<tr class="odd">
<td align="left">Georgia</td>
<td align="left">常规、斜体、粗体、粗斜体</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">爪哇语文本 常规 爪哇文脚本的后备字体</td>
<td align="left">常规</td>
<td align="left">爪哇文脚本的后备字体</td>
</tr>
<tr class="odd">
<td align="left">Leelawadee UI</td>
<td align="left">常规、半细、粗体</td>
<td align="left">东南亚语言脚本的用户界面字体（布吉斯语、老挝语、泰语）</td>
</tr>
<tr class="even">
<td align="left">宋体</td>
<td align="left">常规</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Malgun Gothic</td>
<td align="left">常规</td>
<td align="left">韩文的用户界面字体</td>
</tr>
<tr class="even">
<td align="left">Microsoft Himalaya</td>
<td align="left">常规</td>
<td align="left">藏语脚本的后备字体</td>
</tr>
<tr class="odd">
<td align="left">Microsoft JhengHei</td>
<td align="left">常规</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Microsoft JhengHei UI</td>
<td align="left">常规</td>
<td align="left">繁体中文的用户界面字体</td>
</tr>
<tr class="odd">
<td align="left">Microsoft New Tai Lue</td>
<td align="left">常规</td>
<td align="left">西双版纳新傣文脚本的后备字体</td>
</tr>
<tr class="even">
<td align="left">Microsoft PhagsPa</td>
<td align="left">常规</td>
<td align="left">八思巴文脚本的后备字体</td>
</tr>
<tr class="odd">
<td align="left">Microsoft Tai Le</td>
<td align="left">常规</td>
<td align="left">德宏傣文脚本的后备字体</td>
</tr>
<tr class="even">
<td align="left">Microsoft YaHei</td>
<td align="left">常规</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Microsoft YaHei UI</td>
<td align="left">常规</td>
<td align="left">简体中文的用户界面字体</td>
</tr>
<tr class="even">
<td align="left">Microsoft Yi Baiti</td>
<td align="left">常规</td>
<td align="left">彝语脚本的后备字体</td>
</tr>
<tr class="odd">
<td align="left">Mongolian Baiti</td>
<td align="left">常规</td>
<td align="left">蒙古语脚本的后备字体</td>
</tr>
<tr class="even">
<td align="left">MV Boli</td>
<td align="left">常规</td>
<td align="left">马尔代夫书写体脚本的后备字体</td>
</tr>
<tr class="odd">
<td align="left">Myanmar Text</td>
<td align="left">常规</td>
<td align="left">缅甸文脚本的后备字体</td>
</tr>
<tr class="even">
<td align="left">Nirmala UI</td>
<td align="left">常规、半细、粗体</td>
<td align="left">南亚语言脚本的用户界面字体（孟加拉语、梵文、古吉拉特语、锡克教文、埃纳德语、马拉雅拉姆语、奥里亚语、欧甘语、僧伽罗语、索拉文、泰米尔语、泰卢固语）</td>
</tr>
<tr class="odd">
<td align="left">Segoe MDL2 Assets</td>
<td align="left">常规</td>
<td align="left">应用图标的用户界面字体</td>
</tr>
<tr class="even">
<td align="left">Segoe Print</td>
<td align="left">常规</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Segoe UI</td>
<td align="left">常规、斜体、粗体、粗斜体、细体、半细、半粗、粗体</td>
<td align="left">欧洲和中东语言脚本（阿拉伯语、亚美尼亚语、西里尔文、格鲁吉亚语、希腊语、希伯来语拉丁文）以及傈僳语脚本的用户界面字体</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Emoji</td>
<td align="left">常规</td>
<td align="left">Windows Phone 上随附的版本包括围绕每个图释的白色轮廓，以确保其在任何颜色背景上均会显示。 它与 Windows 随附的版本在计量方面兼容。</td>
</tr>
<tr class="odd">
<td align="left">Segoe UI Historic</td>
<td align="left">常规</td>
<td align="left">历史脚本的后备字体</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">常规</td>
<td align="left">符号的后备字体</td>
</tr>
<tr class="odd">
<td align="left">SimSun</td>
<td align="left">常规</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Times New Roman</td>
<td align="left">常规、斜体、粗体、粗斜体</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Trebuchet MS</td>
<td align="left">常规、斜体、粗体、粗斜体</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">宋体, Verdana</td>
<td align="left">常规、斜体、粗体、粗斜体</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Webdings</td>
<td align="left">常规</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Wingdings</td>
<td align="left">常规</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Yu Gothic</td>
<td align="left">中</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Yu Gothic UI</td>
<td align="left">常规</td>
<td align="left">日语的用户界面字体</td>
</tr>
</tbody>
</table>

 

## <span id="related_topics"></span>相关主题

**面向设计人员**
* [标签（或文本块）](labels.md)
* [Segoe UI Symbol 图标](segoe-ui-symbol-font.md) 
           **对于开发人员 (XAML)**
* [XAML 主题资源](https://msdn.microsoft.com/library/windows/apps/mt187274)
* [设置应用页面的布局](https://msdn.microsoft.com/library/windows/apps/hh872191)
* [Segoe UI Symbol 图标](segoe-ui-symbol-font.md)
* [**TextBlock.FontFamily 属性**](https://msdn.microsoft.com/library/windows/apps/br209655)

**示例**
* [XAML 文本显示示例](http://go.microsoft.com/fwlink/p/?linkid=238578)
* [CSS 样式：打造应用品牌示例](http://go.microsoft.com/fwlink/p/?linkid=231641)
* [语言字体映射示例](http://go.microsoft.com/fwlink/p/?linkid=231603)
 

 







<!--HONumber=Jun16_HO4-->



---
author: Karl-Bridge-Microsoft
Description: This topic describes the use of contact geometry for touch targeting and provides best practices for targeting in Windows Runtime apps.
title: 目标
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bad800f3858cdfdf3def3a9a04854f078b3af399
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6271056"
---
# <a name="guidelines-for-targeting"></a>目标指南


在 Windows 中确定触摸目标需使用触控数字化器检测到的每个手指的全部接触区域。 确定用户的预期（或最可能）目标时，数字化器报告的输入数据集越大、越复杂，精度越高。

> **重要 API**：[**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)、[**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)、[**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)

本主题介绍使用接触几何图形来确定触摸目标并提供在 UWP 应用中确定目标的最佳实践。

## <a name="measurements-and-scaling"></a>度量和缩放


若要在不同的屏幕大小和像素密度之间保持一致性，所有目标大小都要以物理单位（毫米）表示。 使用以下等式将物理单位转换为像素：

像素 = 像素密度 × 度量

以下示例使用此公式计算每英寸 (PPI) 135 像素的屏幕上目标大小为 9 mm 且缩放倍数为 1x 的像素大小：

像素 = 135 PPI × 9 mm

像素 = 135 PPI × (0.03937 英寸/mm × 9 mm)

像素 = 135 PPI × 0.35433 英寸

像素 = 48 像素

必须根据系统定义的每个缩放停滞调整该结果。

## <a name="thresholds"></a>阈值


距离和时间阈值可以用来确定交互的结果。

例如，当检测到向下触摸摸式，如果从向下触摸点拖动对象的距离小于 2.7 mm，并且向下触摸的抬起时间不超过 0.1 秒，则注册为点击。 移动手指超过 2.7 mm 的阈值会使对象被拖动、被选中或被移动（有关详细信息，请参阅[交叉滑动指南](guidelines-for-cross-slide.md)）。 根据你的应用，按下手指超过 0.1 秒可能会使系统进行自我显示交互（有关详细信息，请参阅[视觉反馈指南](guidelines-for-visualfeedback.md)）。

## <a name="target-sizes"></a>目标大小


通常，请将触摸目标大小设置为 9 mm 正方形或更大（1.0x 缩放倍数的 135 PPI 屏幕上为 48x48 像素）。 避免使用小于 7 mm 正方形的触摸目标。

下图显示了目标大小通常如何由可视目标、实际目标大小，以及实际目标和其他可能目标之间的填充组合而成。

![显示可视目标、实际目标以及填充的建议大小的图表。](images/targeting-size.png)

下表列出了触摸目标的组件的最小大小和建议大小。

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">目标组件</th>
<th align="left">最小大小</th>
<th align="left">建议大小</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">填充</td>
<td align="left">2 mm</td>
<td align="left">不适用。</td>
</tr>
<tr class="even">
<td align="left">可视目标大小</td>
<td align="left">&lt;实际大小的 60%</td>
<td align="left">实际大小的 90-100%
<p>如果视觉目标是小于 4.2 mm（7 mm 的推荐最小目标大小的 60%）的正方形，大部分用户不会意识它可触摸。</p></td>
</tr>
<tr class="odd">
<td align="left">实际目标大小</td>
<td align="left">7 mm 正方形</td>
<td align="left">大于或等于 9 mm 正方形 (48 x 48 px @ 1x)</td>
</tr>
<tr class="even">
<td align="left">总目标大小</td>
<td align="left">11 x 11 mm （大约 60 像素：三个 20 像素的网格单位 @ 1x）</td>
<td align="left">13.5 x 13.5 mm （72 x 72 像素 @ 1x）
<p>这表示实际目标与填充的组合大小应该大于其各自的最小值。</p></td>
</tr>
</tbody>
</table>

 

可以根据你的特定方案的需要调整这些目标大小的建议。 这些建议中需要考虑的一些注意事项包括：

-   触摸频率：考虑使重复或经常按的目标大于最小大小。
-   错误后果：由于触摸错误而产生严重后果的目标应该具有较大的填充，并且应该放置在距离内容区域边缘较远的位置。 尤其是经常触摸的目标更是如此。
-   放置在内容区域中
-   组成要素和屏幕大小
-   手指姿势
-   触摸可视化
-   硬件和触摸数字化器

## <a name="targeting-assistance"></a>目标协助


Windows 提供目标协助以支持此处提供的最小大小或填充建议不适用的方案；例如，网页上的超链接、日历控件、下拉列表和组合框，或者文本选择。

这些目标平台改进和用户界面行为与视觉反馈（消除歧义 UI）结合使用以致力于提高用户的准确性和信心。 有关详细信息，请参阅[视觉反馈指南](guidelines-for-visualfeedback.md)。

如果可触摸元素必须小于建议的最小目标大小，则可以使用以下技术来让产生的目标问题减到最少。

## <a name="tethering"></a>叠接


叠接是一种可视提示（从接触点到对象边界矩形的连接器），用于指示用户他们已连接到并且正在与对象进行交互，甚至包括通过输入接触都不能与之联系的对象。 如果符合下列条件，可能会发生这种情况：

-   在对象的某个邻近阈值之内首先检测到接触点，而此对象被识别为最有可能的接触目标。
-   接触点移出某个对象，但接触仍然位于邻近阈值之内。

此功能不会向使用 JavaScript 的 UWP 应用开发人员显示。

## <a name="scrubbing"></a>清理


推移表示触摸目标领域内的任意位置并滑动以选择所需目标，在到达所需目标上之前不抬起手指。 该手势也称为“离开激活”，即当手指抬离屏幕时激活最后一个触摸的对象。

设计推移交互时使用以下指南：

-   将推移与消除歧义 UI 一起使用。 有关详细信息，请参阅[视觉反馈指南](guidelines-for-visualfeedback.md)。
-   推移触摸目标的建议最小大小为 20 像素（3.75 mm @ 1x 大小）。
-   在可平移表面（如网页）上执行操作时，推移具有优先权。
-   推移目标应该彼此靠近。
-   当用户拖动手指离开推移目标时，操作取消。
-   如果目标执行的操作没有破坏性（如在日历上的日期之间切换），则指定叠接到推移目标。
-   在单个方向（水平或垂直）中指定叠接。

## <a name="related-articles"></a>相关文章


**示例**
* [基本输入示例](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延迟输入示例](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [用户交互模式示例](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [焦点视觉示例](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**存档示例**
* [输入：XAML 用户输入事件示例](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [输入：设备功能示例](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [输入：触摸点击测试示例](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML 滚动、平移以及缩放示例](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [输入：简化的墨迹示例](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [输入：Windows 8 手势示例](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [输入：操作和手势 (C++) 示例](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX 触控输入示例](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 





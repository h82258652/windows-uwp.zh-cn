---
title: 响应式设计的屏幕大小和断点
description: 我们建议针对一些关键宽度类别（称为“断点”）进行设计，而不是为 Windows 10 生态系统中的很多设备优化 UI。
ms.date: 08/30/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: fce2c9230add569c4494b01546f1b3ced81d488b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612922"
---
#  <a name="screen-sizes-and-breakpoints"></a>屏幕大小和断点

UWP 应用可以在运行 Windows 10 的任何设备上运行，包括手机、平板电脑、台式机、电视等。 鉴于 Windows 10 生态系统中海量的设备类型和屏幕尺寸，我们建议不要针对每种设备优化 UI，而是针对几个关键宽度类别（也称作“断点”）进行优化： 
- 小（640px 以下）
- 中等（641px 到 1007px）
- 大（1008px 和以上）

> [!TIP]
> 在针对特定断点进行设计时，请针对应用窗口的可用空间大小（而不是屏幕大小）进行设计。 当应用全屏运行时，应用窗口的大小与屏幕的大小相同；但当应用不全屏运行时，窗口的大小小于屏幕的大小。

## <a name="breakpoints"></a>断点
此表描述了不同的大小级别和断点。

![响应式设计断点](images/breakpoints/size-classes.svg)

<table>
<thead>
<tr class="header">
<th align="left">大小级别</th>
<th align="left">断点</th>
<th align="left">典型屏幕大小（对角线）</th>
<th align="left">设备</th>
<th align="left">窗口大小</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td style="vertical-align:top;">小</td>
<td style="vertical-align:top;">640px 或更小</td>
<td style="vertical-align:top;">4&quot; 到 6&quot;；20&quot; 到 65&quot;</td>
<td style="vertical-align:top;">手机、电视</td>
<td style="vertical-align:top;">320x569、360x640、480x854</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">中等</td>
<td style="vertical-align:top;">641px 到 1007px</td>
<td style="vertical-align:top;">7&quot; 到 12&quot;</td>
<td style="vertical-align:top;">平板电脑</td>
<td style="vertical-align:top;">960x540</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">大</td>
<td style="vertical-align:top;">1008px 或更大</td>
<td style="vertical-align:top;">13&quot; 以及更大</td>
<td style="vertical-align:top;">电脑、笔记本电脑、Surface Hub</td>
<td style="vertical-align:top;">1024x640、1366x768、1920x1080</td>
</tr>
</tbody>
</table>

## <a name="why-are-tvs-considered-small"></a>为什么将电视归入“小”类别？ 

虽然大多数电视屏幕面积相当大（一般为 40 到 65 英寸）并且具有高分辨率（HD 或 4k），但在 10 英尺远处观看的 1080P 电视采取的设计与坐在 1 英尺远的桌前使用的 1080p 显示器是不同的。 考虑到距离，1080 像素电视的观看效果相当于 540 像素的显示器。

UWP 的有效像素系统会自动替你解决观看距离问题。 当你为控件或断点范围指定大小时，实际上使用的是“有效”像素。 例如，为 1080 像素或更高像素创建响应代码时，1080 像素显示器将使用该代码，但 1080p 电视不会 - 因为虽然 1080p 电视具有 1080 个物理像素，但它的有效像素数只有 540 个。 这使得面向电视的设计类似于面向手机的设计。

## <a name="effective-pixels-and-scale-factor"></a>有效像素和缩放比例

UWP 应用会自动缩放 UI，以保证应用在所有 Windows 10 设备上都清晰可见。 Windows 会根据其 DPI（每英寸点数）和设备的观看距离为每台显示器自动缩放。 用户可以通过转到**设置** > **显示器** > **缩放和布局**设置页覆盖默认值。 


## <a name="general-recommendations"></a>常规建议

### <a name="small"></a>小
- 将左右窗口边距设置为 12px 以在应用窗口的左侧和右侧边缘之间创建可视间隔。
- 将[应用栏](../controls-and-patterns/app-bars.md)放置在应用窗口底部以改进可访问性。
- 一次使用一个列/区域。
- 使用图标表示搜索（不显示搜索框）。
- 使[导航窗格](../controls-and-patterns/navigationview.md)处于覆盖模式，以节省屏幕空间。
- 如果你使用的是[大纲细节模式](../controls-and-patterns/master-details.md)，请使用堆叠演示模式来节省屏幕空间。

### <a name="medium"></a>中等
- 将左右窗口边距设置为 24px 以在应用窗口的左侧和右侧边缘之间创建可视间隔。
- 将命令元素（如[应用栏](../controls-and-patterns/app-bars.md)）放置在应用窗口顶部。
- 使用最多两个列/区域。
- 显示搜索框。
- 使[导航窗格](../controls-and-patterns/navigationview.md)处于紧缩模式，以便始终在狭长的条带区域内显示图标。
- 请考虑针对[电视体验](https://go.microsoft.com/fwlink/?LinkId=760736)进行进一步定制。

### <a name="large"></a>大
- 将左右窗口边距设置为 24px 以在应用窗口的左侧和右侧边缘之间创建可视间隔。
- 将命令元素（如[应用栏](../controls-and-patterns/app-bars.md)）放置在应用窗口顶部。
- 使用最多三个列/区域。
- 显示搜索框。
- 使[导航窗格](../controls-and-patterns/navigationview.md)处于停靠模式，以使其始终显示。

>[!TIP] 
> 使用[手机版 Continuum](https://go.microsoft.com/fwlink/p/?LinkID=699431) 时，用户可以将监视器、 鼠标和键盘连接到兼容的 Windows 10 移动设备上，让移动设备像笔记本电脑一样工作。针对特定断点进行设计时请记住这一新功能 - 手机将不会始终保持在固定的尺寸级别。



---
author: mijacobs
title: "响应式设计的屏幕大小和断点"
description: "。"
ms.assetid: BF42E810-CDC8-47D2-9C30-BAA19DCBE2DA
label: Screen sizes and break points
template: detail.hbs
op-migration-status: ready
translationtype: Human Translation
ms.sourcegitcommit: b258771c887d4422433522344b11130b7e9ed1e6
ms.openlocfilehash: 7189987209e894d81b2574aff576c64fb5f7a97a

---

#  <a name="screen-sizes-and-break-points-for-responsive-design"></a>响应式设计的屏幕大小和断点

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Windows 10 生态系统上的设备目标和屏幕大小的数量非常惊人，无法针对每台设备来优化你的 UI。 我们建议应针对一些关键的宽度（也称为“断点”）进行设计：360、640、1024 和 1366 epx。

> [!TIP]
> 在针对特定断点进行设计时，请针对应用（应用窗口）的屏幕可用空间大小进行设计。 当应用全屏运行时，应用窗口大小与屏幕大小相同，但在其他情况下则较小一些。
 

此表介绍了不同的大小级别，并为针对这些大小级别进行定制提供了一般建议。

![响应式设计断点](images/rsp-design/rspd-breakpoints.png)

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">大小级别</th>
<th align="left">小型</th>
<th align="left">中型</th>
<th align="left">大型</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;">典型屏幕大小（对角线）</td>
<td style="vertical-align:top;">4&quot; 到 6&quot;</td>
<td style="vertical-align:top;">7&quot; 到 12&quot;，或电视</td>
<td style="vertical-align:top;">13&quot; 以及更大</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">典型设备</td>
<td style="vertical-align:top;">手机</td>
<td style="vertical-align:top;">平板手机、平板电脑、电视</td>
<td style="vertical-align:top;">电脑、笔记本电脑、Surface Hub</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">常用窗口大小以有效像素为单位</td>
<td style="vertical-align:top;">320x569、360x640、480x854</td>
<td style="vertical-align:top;">960x540、1024x640</td>
<td style="vertical-align:top;">1366x768、1920x1080</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">窗口宽度断点以有效像素为单位</td>
<td style="vertical-align:top;">640px 或更小</td>
<td style="vertical-align:top;">641px 到 1007px</td>
<td style="vertical-align:top;">1008px 或更大</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">常规建议</td>
<td style="vertical-align:top;"><ul>
<li>中心选项卡元素。</li>
<li>将左右窗口边距设置为 12px 以在应用窗口的左侧和右侧边缘之间创建一个可视间隔。</li>
<li>窗口底部的扩展坞[应用栏](../controls-and-patterns/app-bars.md)用于改进可访问性</li>
<li>一次使用一个列/区域</li>
<li>使用图标表示搜索（不显示搜索框）。</li>
<li>使[导航窗格](../controls-and-patterns/nav-pane.md)处于覆盖模式，以节省屏幕空间。</li>
<li>如果你使用的是[大纲细节模式](../controls-and-patterns/master-details.md)，请使用堆叠演示模式来节省屏幕空间。</li>
</ul></td>
<td style="vertical-align:top;"><ul>
<li>使选项卡元素左对齐。</li>
<li>将左右窗口边距设置为 24px 以在应用窗口的左侧和右侧边缘之间创建一个可视间隔。</li>
<li>将命令元素（如[应用栏](../controls-and-patterns/app-bars.md)）放置在应用窗口顶部。</li>
<li>最多两个列/区域</li>
<li>显示搜索框。</li>
<li>使[导航窗格](../controls-and-patterns/nav-pane.md)处于长条模式，以便始终显示窄带的图标。</li>
<li>请考虑针对[电视体验](http://go.microsoft.com/fwlink/?LinkId=760736)进行进一步定制。</li>
</ul></td>
<td style="vertical-align:top;"><ul>
<li>使选项卡元素左对齐。</li>
<li>将左右窗口边距设置为 24px 以在应用窗口的左侧和右侧边缘之间创建一个可视间隔。</li>
<li>将命令元素（如[应用栏](../controls-and-patterns/app-bars.md)）放置在应用窗口顶部。</li>
<li>最多三个列/区域</li>
<li>显示搜索框。</li>
<li>使[导航窗格](../controls-and-patterns/nav-pane.md)处于停靠模式，以使其始终显示。</li>
</ul></td>
</tr>
</tbody>
</table>

[**适用于手机的 Continuum**](http://go.microsoft.com/fwlink/p/?LinkID=699431) 是兼容 Windows 10 移动版设备的新体验，用户可以使用它将手机连接到监视器、鼠标和键盘来使手机像笔记本电脑一样工作。 针对特定断点进行设计时请记住这一新功能 - 移动手机将不会始终保持在较小尺寸级别。
 



<!--HONumber=Dec16_HO2-->



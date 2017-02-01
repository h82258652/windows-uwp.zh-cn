---
author: Karl-Bridge-Microsoft
Description: "本主题介绍用于旋转的新 Windows UI，并提供在 Windows 应用商店应用中使用这个新交互机制时应该考虑的用户体验指南。"
title: "旋转"
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: b258771c887d4422433522344b11130b7e9ed1e6
ms.openlocfilehash: 3217dd6bda6d9692ff450133af23002a7040347b

---

# <a name="rotation"></a>旋转
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

本文介绍用于旋转的新 Windows UI，并提供在 UWP 应用中使用这个新交互机制时应该考虑的用户体验指南。

<div class="important-apis" >
<b>重要的 API</b><br/>
<ul>
<li>[**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)</li>
<li>[**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)</li>
</ul>
</div>

## <a name="dos-and-donts"></a>应做事项和禁止事项

-   使用旋转可帮助用户直接旋转 UI 元素。

## <a name="additional-usage-guidance"></a>其他使用指南


**旋转概述**

旋转是 UWP 应用使用的一种触摸优化技术，允许用户以环形方向（顺时针或逆时针）旋转对象。

根据输入设备，采用以下方式执行旋转交互：

-   使用鼠标或主动笔/触笔移动所选对象的旋转控制手柄。
-   直接触摸或者使用被动笔/触笔通过旋转手势沿所需方向旋转对象。

**何时使用旋转**

使用旋转可帮助用户直接旋转 UI 元素。 下图显示了支持旋转交互的某些手指位置。

![演示旋转支持的各种手指姿势的图表。](images/ux-rotate-positions.png)

**注意**  
从直观上来说，多数情况下，旋转点是两个触摸点之一，除非用户指定与接触点无关的旋转点（例如在绘图或布局应用程序中）。 以下图像演示了不采用这种方式限制旋转点时如何降级用户体验。

首张图片显示第一个（拇指）和第二个（食指）接触点：食指接触树，拇指接触树枝。

![显示旋转手势的两个初始接触点的图像。](images/ux-rotate-points1.png)
在这第二张图片中，旋转围绕第一个（拇指）接触点进行。 旋转之后，食指仍然接触树干，并且拇指仍然接触树枝（旋转点）。

![显示已旋转图片的图像，其中旋转点被限制为两个初始接触点中的一个。](images/ux-rotate-points2.png)
在这第三张图片中，应用程序已将旋转中心定义为图片的中心点（或由用户设置）。 旋转之后，由于图片并未围绕其中一个手指旋转，因此不能直接操作（除非用户已选择该设置）。

![显示已旋转图片的图像，其中旋转点被限制为图片的中心，而不是两个初始接触点中的任何一个。](images/ux-rotate-points3.png)
在这最后一张图片中，应用程序已将旋转中心定义为图片左边缘的中点（或由用户设置）。 而且，除非用户已选择该设置，否则在这种情况下不能直接操作。

![显示已旋转图片的图像，其中旋转点被限制为图片最左侧的中心，而不是两个初始接触点中的任何一个。](images/ux-rotate-points4.png)

 

Windows 8 支持三种类型的旋转：自由、受限以及组合。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">类型</th>
<th align="left">说明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">自由旋转</td>
<td align="left"><p>自由旋转允许用户将内容 360 度自由旋转到任意位置。 当用户释放该对象时，对象仍然处于所选的位置。 自由旋转对于绘图和布局应用程序非常有用，如 Microsoft PowerPoint、Word、Visio 和 Paint 以及 Adobe Photoshop、Illustrator 和 Flash。</p></td>
</tr>
<tr class="even">
<td align="left">受限旋转</td>
<td align="left"><p>受限旋转支持在操作期间自由旋转，但释放时强制对齐点以 90 度（0、90、180 和 270）递增。 当用户释放对象时，对象自动旋转到最近的对齐点。</p>
<p>受限旋转是最常用的旋转方法，并且它的工作方式与滚动内容类似。 用户通过对齐点虽然不够精确，但仍能实现其目标。 受限旋转对于诸如 Web 浏览器和相册之类的应用程序非常有用。</p></td>
</tr>
<tr class="odd">
<td align="left">组合旋转</td>
<td align="left"><p>组合旋转支持包含区域（类似于[平移指南](guidelines-for-panning.md)中的围栏）的自由旋转，这些区域位于受限旋转强制的每个 90 度对齐点上。 如果用户在其中一个 90 度区域之外释放对象，则对象仍然在该位置；否则，对象会自动旋转到一个对齐点。</p>
<div class="alert">
<strong>注意</strong>  用户界面围栏是目标周围的某个区域限制向特定值或位置的移动，从而影响其选择的一项功能。
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## <a name="related-topics"></a>相关主题


**示例**
* [基本输入示例](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延迟输入示例](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [用户交互模式示例](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [焦点视觉示例](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**存档示例**
* [输入：XAML 用户输入事件示例](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [输入：设备功能示例](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [输入：触摸点击测试示例](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML 滚动、平移以及缩放示例](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [输入：简化的墨迹示例](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [输入：使用 GestureRecognizer 的笔势和操作](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [输入：操作和笔势 (C++) 示例](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX 触控输入示例](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 







<!--HONumber=Dec16_HO2-->



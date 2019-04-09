---
Description: 本主题介绍了新的 Windows 用户界面的旋转，并提供在 UWP 应用中使用此新的交互机制时应考虑的用户体验指南。
title: 旋转
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d58587c8a7e391c51dc3267dd6ebb069170604a4
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244273"
---
# <a name="rotation"></a>旋转


本文介绍用于旋转的全新 Windows UI，并提供在 UWP 应用中使用这个新交互机制时应该考虑的用户体验指南。

> **重要的 API**：[**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)， [ **Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)

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

**请注意**  除非用户可以指定无关 （例如，在绘图或布局的应用程序） 联系点的旋转点直观地说，并在大多数情况下，旋转点是两个触摸点之一。 以下图像演示了不采用这种方式限制旋转点时如何降级用户体验。

首张图片显示第一个（拇指）和第二个（食指）接触点：食指接触树，拇指接触树枝。

![显示旋转手势的两个初始接触点的图像。](images/ux-rotate-points1.png)
在这第二张图片中，旋转围绕第一个（拇指）接触点进行。 旋转之后，食指仍然接触树干，并且拇指仍然接触树枝（旋转点）。

![显示已旋转图片的图像，其中旋转点被限制为两个初始接触点中的一个。](images/ux-rotate-points2.png)
在这第三张图片中，应用程序已将旋转中心定义为图片的中心点（或由用户设置）。 旋转之后，由于图片并未围绕其中一个手指旋转，因此不能直接操作（除非用户已选择该设置）。

![显示已旋转图片的图像，其中旋转点被限制为图片的中心，而不是两个初始接触点中的任何一个。](images/ux-rotate-points3.png)
在这最后一张图片中，应用程序已将旋转中心定义为图片左边缘的中点（或由用户设置）。 而且，除非用户已选择该设置，否则在这种情况下不能直接操作。

![显示已旋转图片的图像，其中旋转点被限制为图片最左侧的中心，而不是两个初始接触点中的任何一个。](images/ux-rotate-points4.png)

 

Windows 10 支持三种类型的旋转： 免费版、 约束、 和组合。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">在任务栏的搜索框中键入</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">自由旋转</td>
<td align="left"><p>自由旋转允许用户将内容 360 度自由旋转到任意位置。当用户释放该对象时，对象仍然处于所选的位置。 自由旋转对于绘图和布局应用程序非常有用，如 Microsoft PowerPoint、Word、Visio 和 Paint 以及 Adobe Photoshop、Illustrator 和 Flash。</p></td>
</tr>
<tr class="even">
<td align="left">受限旋转</td>
<td align="left"><p>受限旋转支持在操作期间自由旋转，但释放时强制对齐点以 90 度（0、90、180 和 270）递增。 当用户释放对象时，对象自动旋转到最近的对齐点。</p>
<p>受限旋转是最常用的旋转方法，并且它的工作方式与滚动内容类似。 用户通过对齐点虽然不够精确，但仍能实现其目标。 受限旋转对于诸如 Web 浏览器和相册之类的应用程序非常有用。</p></td>
</tr>
<tr class="odd">
<td align="left">组合旋转</td>
<td align="left"><p>组合旋转支持包含区域（类似于<a href="guidelines-for-panning.md">平移指南</a>中的围栏）的自由旋转，这些区域位于受限旋转强制的每个 90 度对齐点上。 如果用户在其中一个 90 度区域之外释放对象，则对象仍然在该位置；否则，对象会自动旋转到一个对齐点。</p>
<div class="alert">
<strong>请注意</strong>  用户界面导轨是在其中一个目标周围区域约束针对某些特定值或位置来影响其所选内容移动的功能。
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## <a name="related-topics"></a>相关主题


**示例**
* [基本输入示例](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延迟输入示例](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [用户交互模式示例](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [焦点视觉示例](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**存档示例**
* [输入：XAML 用户输入的事件示例](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [输入：设备功能示例](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [输入：触控命中测试示例](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML 滚动、平移以及缩放示例](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [输入：简化的墨迹示例](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [输入：笔势和 GestureRecognizer 操作](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [输入：操作和手势 (C++) 示例](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX 触控输入示例](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 





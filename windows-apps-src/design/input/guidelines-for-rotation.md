---
Description: 本主题介绍了新的 Windows 用户界面以进行旋转，并提供了在 UWP 应用中使用此新交互机制时应考虑的用户体验指南。
title: 旋转
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 71a09d304b0d68bf01012166173360ec6304fb2c
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257930"
---
# <a name="rotation"></a>旋转


本文介绍用于旋转的全新 Windows UI，并提供在 UWP 应用中使用这个新交互机制时应该考虑的用户体验指南。

> **重要 API**：[**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)、[**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

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

**请注意**   直观的，在大多数情况下，旋转点是两个触摸点之一，除非用户可以指定与接触点无关的旋转点（例如，在绘图或布局应用程序中）。 以下图像演示了不采用这种方式限制旋转点时如何降级用户体验。

首张图片显示第一个（拇指）和第二个（食指）接触点：食指接触树，拇指接触树枝。

显示旋转笔势的两个初始触控点 ![图像。](images/ux-rotate-points1.png)
在这第二张图片中，旋转围绕第一个（拇指）接触点进行。 旋转之后，食指仍然接触树干，并且拇指仍然接触树枝（旋转点）。

![图像显示旋转的图片，旋转点受限于两个初始触控点之一。](images/ux-rotate-points2.png)
在这第三张图片中，应用程序已将旋转中心定义为图片的中心点（或由用户设置）。 旋转之后，由于图片并未围绕其中一个手指旋转，因此不能直接操作（除非用户已选择该设置）。

![图像显示旋转的图片，旋转点被约束到图片的中心，而不是两个初始触控点中的任何一个。](images/ux-rotate-points3.png)
在这最后一张图片中，应用程序已将旋转中心定义为图片左边缘的中点（或由用户设置）。 而且，除非用户已选择该设置，否则在这种情况下不能直接操作。

![显示已旋转图片的图像，其中旋转点被限制为图片最左侧的中心，而不是两个初始接触点中的任何一个。](images/ux-rotate-points4.png)

 

Windows 10 支持三种类型的旋转：自由、约束和组合。

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
<strong>请注意</strong>  用户界面滑轨是一项功能，在该功能中，目标周围的某个区域会约束到某个特定值或位置，以影响其选择。
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## <a name="related-topics"></a>相关主题


**示例**
* [基本输入示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [低延迟输入示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [用户交互模式示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [焦点视觉对象示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

**存档示例**
* [输入： XAML 用户输入事件示例](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [输入：设备功能示例](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [输入：触控命中测试示例](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
* [XAML 滚动、平移和缩放示例](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [输入：简化墨迹示例](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
* [输入：与 GestureRecognizer 的手势和操作](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [输入：操作和手势（C++）示例](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
* [DirectX 触摸输入示例](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
 

 





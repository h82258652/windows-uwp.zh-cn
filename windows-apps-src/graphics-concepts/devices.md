---
title: 设备
description: Direct3D 设备是 Direct3D 的渲染组件。 设备封装并存储渲染状态，执行转换和照明操作，并将图像光栅化到表面。
ms.assetid: BC903462-A32A-46BA-8411-FB294F5D2CD9
keywords:
- 设备
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d1c35af3dd1f8826fbd61268c5c47cef9d77146a
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2018
ms.locfileid: "7173199"
---
# <a name="devices"></a>设备


Direct3D 设备是 Direct3D 的渲染组件。 设备封装并存储渲染状态，执行转换和照明操作，并将图像光栅化到表面。

体系结构上，Direct3D 设备包含转换模块、光线模块和光栅模块，如下图所示。

![direct3d 设备体系结构的图示](images/d3ddev.png)

Direct3D 支持以下两种主要类型的 Direct3D 设备：

-   支持硬件加速光栅和着色以及硬件和软件顶点处理的 hal 设备
-   参考设备

这些设备都两个独立的驱动程序。 软件和参考设备由驱动程序表示，而 hal 设备由硬件驱动程序表示。 利用这些设备的最常见方式为：使用 hal 设备传送应用程序，使用参考设备进行功能测试。 这些设备由第三方提供，用于模仿特定设备，例如尚未发布的开发硬件。

应用程序创建的 Direct3D 设备必须与运行该应用程序的硬件的功能一致。 Direct3D 提供渲染功能，方式是通过访问计算机中安装的 3D 硬件或模仿软件中 3D 硬件的功能。 因此，Direct3D 提供设备以访问硬件和模仿软件。

硬件加速设备的性能远远高于软件设备。 所有支持 Direct3D 的图形适配器均可使用此 hal 设备类型。 在大多数情况下，应用程序瞄准具有硬件加速且依赖软件模拟来容纳低端计算机的计算机。

软件设备并不总是支持与硬件设备相同的功能，但参考设备除外。 若要确定支持哪些功能，应用程序应始终查询设备功能。

由于与 Direct3D 9 一起提供的软件和参考设备的行为与 hal 设备的相同，编写的用于 hal 设备的应用程序代码将不经修改直接用于软件或参考设备。 提供的软件或参考设备的行为与 hal 设备的相同，但设备功能各不相同，并且特定的软件设备可以实现更小的功能集。

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>本节内容


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="device-types.md">设备类型</a></p></td>
<td align="left"><p>Direct3D 设备类型包括硬件抽象层 (hal) 设备和参考光栅器。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="windowed-vs--full-screen-mode.md">窗口与全屏模式</a></p></td>
<td align="left"><p>Direct3D 应用程序可以在窗口模式或全屏模式中运行。 在<em>窗口模式</em>中，此应用程序与所有运行的应用共享可用的桌面屏幕空间。 在<em>全屏模式</em>中，应用程序运行于的窗口将覆盖整个桌面，并隐藏所有运行的应用（包括你的开发环境）。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="lost-devices.md">丢失的设备</a></p></td>
<td align="left"><p>Direct3D 设备可以处于运行状态或丢失状态。 <em>运行</em>状态是设备的正常状态（设备正在运行），并能如预期般呈现所有内容。 当出现全屏应用程序失去键盘焦点而导致无法呈现内容等事件时，设备会转换到<em>丢失</em>状态。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="swap-chains.md">交换链</a></p></td>
<td align="left"><p>交换链是用于向用户显示帧的缓冲区集合。 应用程序每次提供要显示的新帧时，交换链中的第一个缓冲区将替代已显示的缓冲区。 此过程称为<em>交换</em>或<em>翻转</em>。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="introduction-to-rasterization-rules.md">光栅化规则简介</a></p></td>
<td align="left"><p>通常，为顶点指定的点不与屏幕上的像素精确匹配。 发生这种情况时，Direct3D 应用三角形光栅化规则来决定将哪些像素应用到给定的三角形。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[Direct3D 图形学习指南](index.md)

 

 





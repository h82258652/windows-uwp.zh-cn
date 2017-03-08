---
title: "Direct3D 图形学习指南"
description: "介绍 Microsoft Direct3D 所基于的图形概念。"
ms.assetid: c3850a92-4d05-4f72-bf0f-6a0c79e841eb
keywords:
- "Direct3D 图形学习指南"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: e62f9cfde35580dd384ef69fe6e5658d927ce3d8
ms.lasthandoff: 02/07/2017

---

# <a name="direct3d-graphics-learning-guide"></a>Direct3D 图形学习指南


介绍 Microsoft Direct3D 所基于的图形概念。 本文档集在很大程度上独立于任何 Direct3D 版本，可为图形开发人员提供超出特定于版本的 API 文档说明范围的更多背景信息。

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
<td align="left"><p>[坐标系统和几何结构](coordinate-systems-and-geometry.md)</p></td>
<td align="left"><p>进行 Direct3D 应用程序编程需要熟练掌握 3D 几何原理。 本节介绍创建 3D 场景所需的最重要的几何概念。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[顶点和索引缓冲区](vertex-and-index-buffers.md)</p></td>
<td align="left"><p><em>顶点缓冲区</em>是包含顶点数据的内存缓冲区；处理顶点缓冲区中的顶点以执行转换、照明和剪切。 <em>索引缓冲区</em>是包含索引数据的内存缓冲区，索引数据是到顶点缓冲区的整数偏移量，用于渲染基元。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[设备](devices.md)</p></td>
<td align="left"><p>Direct3D 设备是 Direct3D 的渲染组件。 设备封装并存储渲染状态，执行转换和照明操作，并将图像光栅化到表面。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[照明](lights-and-materials.md)</p></td>
<td align="left"><p>光照用于照亮场景中的对象。 每个对象顶点的颜色基于当前纹理贴图、顶点颜色和光源。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[深度和模板缓冲区](depth-and-stencil-buffers.md)</p></td>
<td align="left"><p><em>深度缓冲区</em>存储深度信息，以控制渲染（而不是从视图中隐藏）哪些多边形区域。 <em>模板缓冲区</em>用于遮罩图像中的像素，以产生特殊效果，包括合成，贴花，溶解、淡入淡出和轻扫，轮廓与剪影，以及双面模板。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[纹理](textures.md)</p></td>
<td align="left"><p>纹理是在计算机生成的 3D 图像中创建逼真效果的强大工具。 Direct3D 支持广泛的纹理功能集，使开发人员能够轻松使用高级纹理技术。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[图形管道](graphics-pipeline.md)</p></td>
<td align="left"><p>Direct3D 图形管线旨在为实时游戏应用程序生成图形。 数据通过各个可配置或可编程的阶段从输入流到输出。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[视图](views.md)</p></td>
<td align="left"><p>术语&quot;视图&quot;用于表示&quot;采用所需格式的数据&quot;。 例如，常量缓冲区视图 (CBV) 是正确格式化的常量缓冲区数据。 本节介绍最常见和最有用的视图。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[计算管道](compute-pipeline.md)</p></td>
<td align="left"><p>Direct3D 计算管道旨在处理大部分可与图形管道并行完成的计算。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[资源](resources.md)</p></td>
<td align="left"><p>资源是内存中可由 Direct3D 管道访问的区域。 为了使管道能够高效地访问内存，提供给管道的数据（如输入几何图形、着色器资源和纹理）必须存储在资源中。 所有 Direct3D 资源都来自于两种资源类型：缓冲区或纹理。 每个管道阶段最多可以有 128 个活动资源。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[流式资源](streaming-resources.md)</p></td>
<td align="left"><p><em>流式资源</em>是使用少量物理内存的大型逻辑资源。 整个大型资源不是一次传入，而是在需要时流式传输小部分资源。 流式资源以前称作<em>平铺资源</em>。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[附录](appendix.md)</p></td>
<td align="left"><p>以下部分提供了深层次的技术细节。</p></td>
</tr>
</tbody>
</table>

 

 

 


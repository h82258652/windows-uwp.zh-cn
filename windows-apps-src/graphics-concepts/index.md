---
title: Direct3D 图形词汇表
description: 定义由 Microsoft Direct3D 图形术语。
ms.assetid: c3850a92-4d05-4f72-bf0f-6a0c79e841eb
keywords:
- Direct3D 图形词汇表
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0e325494cefeab4cf3ed40939f5b24de5e921b68
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "7438145"
---
# <a name="direct3d-graphics-glossary"></a>Direct3D 图形词汇表


定义 Microsoft Direct3D 图形术语。 此词汇表定义，在高级别、 一般 3D 计算机图形条款的开发中使用 Direct3D 游戏和应用。

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>本部分内容


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">说明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="coordinate-systems-and-geometry.md">坐标系和几何图形</a></p></td>
<td align="left"><p>进行 Direct3D 应用程序编程需要熟练掌握 3D 几何原理。 本节介绍创建 3D 场景所需的最重要的几何概念。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vertex-and-index-buffers.md">顶点和索引缓冲区</a></p></td>
<td align="left"><p><em>顶点缓冲区</em>是包含顶点数据的内存缓冲区；将处理顶点缓冲区中的顶点以执行转换、照明和剪裁。 <em>索引缓冲区</em>是包含索引数据的内存缓冲区，索引数据是到顶点缓冲区的整数偏移量，用于渲染基元。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="devices.md">设备</a></p></td>
<td align="left"><p>Direct3D 设备是 Direct3D 的渲染组件。 设备封装并存储渲染状态，执行转换和照明操作，并将图像光栅化到表面。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="lights-and-materials.md">照明</a></p></td>
<td align="left"><p>光照用于照亮场景中的对象。 每个对象顶点的颜色基于当前纹理贴图、顶点颜色和光源。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="depth-and-stencil-buffers.md">深度和模板缓冲区</a></p></td>
<td align="left"><p><em>深度缓冲区</em>存储深度信息，以控制渲染（而不是从视图中隐藏）哪些多边形区域。 <em>模板缓冲区</em>用于遮罩图像中的像素，以产生特殊效果，包括合成；贴纸；溶解、淡化和滑动；轮廓描绘和剪影，以及双面模板。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="textures.md">纹理</a></p></td>
<td align="left"><p>纹理是在计算机生成的 3D 图像中创建逼真效果的强大工具。 Direct3D 支持广泛的纹理功能集，使开发人员能够轻松使用高级纹理技术。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="graphics-pipeline.md">图形管道</a></p></td>
<td align="left"><p>Direct3D 图形管道旨在为实时游戏应用程序生成图形。 数据通过各个可配置或可编程的阶段从输入流到输出。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="views.md">视图</a></p></td>
<td align="left"><p>术语&quot;视图&quot;用于表示&quot;采用所需格式的数据&quot;。 例如，常量缓冲区视图 (CBV) 是正确格式化的常量缓冲区数据。 本节介绍最常见和最有用的视图。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="compute-pipeline.md">计算管道</a></p></td>
<td align="left"><p>Direct3D 计算管道旨在处理大部分可与图形管道并行完成的计算。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="resources.md">资源</a></p></td>
<td align="left"><p>资源是内存中可由 Direct3D 管道访问的区域。 为了使管道能够高效地访问内存，提供给管道的数据（如输入几何图形、着色器资源和纹理）必须存储在资源中。 所有 Direct3D 资源都存储在两种类型的资源中：缓冲区或纹理。 每个管道阶段最多可以有 128 个活动资源。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources.md">流式资源</a></p></td>
<td align="left"><p><em>流式资源</em>是使用少量物理内存的大型逻辑资源。 整个大型资源不是一次传入，而是在需要时流式传输小部分资源。 流式资源以前称作<em>平铺资源</em>。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="appendix.md">附录</a></p></td>
<td align="left"><p>以下部分提供了深层次的技术细节。</p></td>
</tr>
</tbody>
</table>

 

 

 

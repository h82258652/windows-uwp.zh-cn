---
title: 坐标系统和几何结构
description: 进行 Direct3D 应用程序编程需要熟练掌握 3D 几何原理。 本部分介绍创建 3D 场景所需的最重要的几何概念。
ms.assetid: E82EB0A9-0678-496B-96B3-8993BA580099
keywords:
- 坐标系统和几何结构
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ce3b1ebfc2f18a06ff4fa960c91749f61f5011d9
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5765689"
---
# <a name="coordinate-systems-and-geometry"></a>坐标系统和几何结构


进行 Direct3D 应用程序编程需要熟练掌握 3D 几何原理。 本部分介绍创建 3D 场景所需的最重要的几何概念。

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
<td align="left"><p><a href="coordinate-systems.md">坐标系统</a></p></td>
<td align="left"><p>通常，3D 图形应用程序使用两种类型的笛卡尔坐标系统之一：左手坐标系统或右手坐标系统。 在这两个坐标系统中，正 x 轴指向右侧，正 y 轴指向上方。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="primitives.md">基元</a></p></td>
<td align="left"><p>3D <em>基元</em>是单个 3D 实体中的顶点的集合。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="face-and-vertex-normal-vectors.md">面和顶点的法向矢量</a></p></td>
<td align="left"><p>网格的每个面都有垂直单位法向矢量。 矢量方向取决于顶点的定义顺序和坐标系统是左手坐标还是右手坐标。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="rectangles.md">矩形</a></p></td>
<td align="left"><p>在整个 Direct3D 和 Windows 编程中，将根据边界矩形引用屏幕上的对象。 边界矩形的边始终与屏幕的边平行，始终可通过两个点（左上角和右下角）来描述矩形。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="triangle-interpolation.md">三角形插值</a></p></td>
<td align="left"><p>在渲染期间，管道会在每个三角形中插入顶点数据。 顶点数据可以是各种各样的数据并可以包括（但不限于）：漫射颜色、反射颜色、漫射 alpha（三角形不透明度）、反射 alpha 和雾化系数。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vectors--vertices--and-quaternions.md">矢量、顶点和四元数</a></p></td>
<td align="left"><p>顶点会在整个 Direct3D 中描述位置和方向。 基元中的每个顶点都由矢量进行描述，矢量会提供其位置、颜色、纹理坐标和提供其方向的法向矢量。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="transforms.md">转换</a></p></td>
<td align="left"><p>将几何图形推过固定函数几何图形管道的 Direct3D 部分是转换引擎。 它会找到世界中的模型和查看器、投影显示在屏幕上的顶点，并将顶点剪切到视区上。 转换引擎还会执行照明计算来确定每个顶点的漫射和反射组件。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="viewports-and-clipping.md">视区和剪切</a></p></td>
<td align="left"><p><em>视区</em>是将 3D 场景投影到其中的二维 (2D) 矩形。 在 Direct3D 中，矩形在系统作为渲染目标使用的 Direct3D 表面内作为坐标存在。 投影转换将顶点转换为用于视区的坐标系统。 视区还用于指定渲染目标表面（场景将渲染到其中）的深度值范围（通常为 0.0 至 1.0）。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[Direct3D 图形学习指南](index.md)

 

 





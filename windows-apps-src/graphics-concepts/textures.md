---
title: 纹理
description: 纹理是在计算机生成的 3D 图像中创建逼真效果的强大工具。 Direct3D 支持广泛的纹理功能集，使开发人员能够轻松访问高级纹理技术。
ms.assetid: B9E85C9E-B779-4852-9166-6FA2240B7046
keywords:
- 纹理
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 81d77b262cc77c23d859cf76227a34bc72b15b96
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7693328"
---
# <a name="textures"></a>纹理


纹理是在计算机生成的 3D 图像中创建逼真效果的强大工具。 Direct3D 支持广泛的纹理功能集，使开发人员能够轻松访问高级纹理技术。

为提升性能，可以考虑使用动态纹理。 动态纹理可以锁定、写入以及解锁每个帧。

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
<td align="left"><p><a href="introduction-to-textures.md">纹理简介</a></p></td>
<td align="left"><p>纹理资源是存储纹素的数据结构，纹素是可以读取或写入的纹理的最小单位。 在着色器读取纹理时，可以通过纹理采样器对纹理进行筛选。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="basic-texturing-concepts.md">基本纹理概念</a></p></td>
<td align="left"><p>早期的计算机生成的 3D 图像，尽管在当时通常很先进，往往有一个闪亮的塑料外观。 这类图像缺少磨损、裂缝、指纹和污迹等标记类型，所以 3D 对象不具有真实的视觉复杂性。 为了增强计算机生成的 3D 图像的真实性，纹理变得非常流行。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="texture-addressing-modes.md">纹理寻址模式</a></p></td>
<td align="left"><p>Direct3D 应用程序可以将纹理坐标分配至任何基元的任何顶点。 通常，你分配至某个顶点的 u 纹理以及 v 纹理坐标的范围介于 0.0-1.0（包含这两个值）。 但如果在此范围之外分配纹理坐标，则可以创造某些特殊的纹理效果。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-filtering.md">纹理筛选</a></p></td>
<td align="left"><p>当通过将 3D 基元映射到 2D 屏幕的形式呈现基元时，纹理筛选针对基元的 2D 呈现图像中的每个像素生成一种颜色。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="texture-resources.md">纹理资源</a></p></td>
<td align="left"><p>纹理是一种用于呈现的资源。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-wrapping.md">纹理环绕</a></p></td>
<td align="left"><p>纹理环绕通过使用为每个顶点指定的纹理坐标，改变 Direct3D 光栅化纹理多边形的基本方式。 实行多边形光栅化时，系统会内插入每个多边形顶点的纹理坐标之间，以确定应用于每个多边形像素的纹素。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="texture-blending.md">纹理混合</a></p></td>
<td align="left"><p>Direct3D 可以在单程内最多将八个纹理融合到基元。 使用多个纹理混合可以显著增加 Direct3D 应用程序的帧速率。 应用程序可通过多个纹理混合，在单程内应用纹理、阴影、镜面光、漫射照明以及其他的特殊效果。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="light-mapping-with-textures.md">使用纹理的光映射</a></p></td>
<td align="left"><p>光照贴图是包含关于 3D 场景中照明信息的纹理或纹理组。 光照图将光和阴影的区域映射到基元上。 多纹理混合使你的应用程序能够以比着色技术更逼真的外观渲染场景。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="compressed-texture-resources.md">压缩的纹理资源</a></p></td>
<td align="left"><p>纹理映射是在三维图形上绘制以添加视觉细节的数字化图像。 它们在光栅化期间被贴图到这些形状，而此过程会占用大量系统总线和内存。 为了降低纹理占用的内存量，Direct3D 支持压缩纹理曲面。 某些 Direct3D 设备支持在本地压缩的纹理表面。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[Direct3D 图形学习指南](index.md)

 

 





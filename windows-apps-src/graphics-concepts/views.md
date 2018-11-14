---
title: 视图
description: 术语“视图”用于表示“所需格式的数据”。 例如，常量缓冲区视图 (CBV) 是格式正确的常量缓冲区数据。 本部分介绍最常见和最有用的视图。
ms.assetid: 0C7FB99F-7391-472F-BA53-576888DFC171
keywords:
- 视图
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 77ac16c55bb4292ea7c5362d007ff9569812954f
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6664327"
---
# <a name="views"></a>视图


术语“视图”用于表示“所需格式的数据”。 例如，常量缓冲区视图 (CBV) 是格式正确的常量缓冲区数据。 本部分介绍最常见和最有用的视图。

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
<td align="left"><p><a href="constant-buffer-view--cbv-.md">常量缓冲区视图 (CBV)</a></p></td>
<td align="left"><p>常量缓冲区包含着色器常量数据。 其优点在于数据会持久保存，并且可由任何 GPU 着色器访问，直到需要更改数据。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vertex-buffer-view--vbv-.md">顶点缓冲区视图 (VBV) 和索引缓冲区视图 (IBV)</a></p></td>
<td align="left"><p>顶点缓冲区保存顶点列表的数据。 每个顶点的数据都可能包含位置、颜色、法向矢量、纹理坐标等。 索引缓冲区将整数索引（偏移量）保存到顶点缓冲区中，并用于定义和呈现组成完整的顶点列表的一部分的对象。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="shader-resource-view--srv-.md">着色器资源视图 (SRV) 和无序的访问视图 (UAV)</a></p></td>
<td align="left"><p>着色器资源视图通常以方便着色器访问纹理的方式围绕纹理。 无序的访问视图提供类似的功能，但支持以任何顺序读取和写入到纹理（或其他资源）。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="sampler.md">采样器</a></p></td>
<td align="left"><p>采样是读取纹理中的输入值或读取其他资源的过程。 &quot;采样器&quot;是从资源中读取的任何对象。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-target-view--rtv-.md">呈现目标视图 (RTV)</a></p></td>
<td align="left"><p>呈现目标使场景能够呈现到临时中间缓冲区，而不是呈现到要呈现到屏幕的后台缓冲区。 利用此功能，可在图形管道中将可呈现的复杂场景用作反射纹理或用于其他用途，也可将其用于在呈现前向场景添加其他像素着色器效果。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="depth-stencil-view--dsv-.md">深度模板视图 (DSV)</a></p></td>
<td align="left"><p>深度模板视图提供用于保存深度和模板信息的格式和缓冲区。 深度缓冲区用于剔除由较近的对象对其进行遮挡时，对查看者不可见的像素的绘制。 模板缓冲区可用于剔除定义的形状外部的所有绘制。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="stream-output-view--sov-.md">流输出视图 (SOV)</a></p></td>
<td align="left"><p>流输出视图支持将顶点、分割和几何图形着色器附带的顶点信息流式传输回应用程序以供进一步使用。 例如，已经由这些着色器导致失真的对象可以写回到应用程序，从而为物理引擎或其他引擎提供更准确的输入。 但在实践中，流输出视图是图形管道中不常使用的功能。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="rasterizer-ordered-view--rov-.md">光栅器排序视图 (ROV)</a></p></td>
<td align="left"><p>利用光栅器有序视图，可以消除一些深度缓冲区限制，特别是使多个包含透明度的纹理应用于同一像素。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[Direct3D 图形学习指南](index.md)

 

 





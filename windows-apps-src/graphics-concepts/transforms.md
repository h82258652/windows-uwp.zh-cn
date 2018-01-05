---
title: "转换"
description: "转换引擎是 Direct3D 的一部分，用于将几何图形穿过固定函数几何图形管道。"
ms.assetid: 0DF2A99A-335C-4D14-9720-6D7996DD635A
keywords: "转换"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: e5eb63a41ad04577196b6a622736211c1d1576f9
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/22/2017
---
# <a name="transforms"></a>转换


转换引擎是 Direct3D 的一部分，用于将几何图形穿过固定函数几何图形管道。 它会找到世界中的模型和查看器、投影显示在屏幕上的顶点，并将顶点剪切到视区上。 转换引擎还会执行照明计算来确定每个顶点的漫射和反射组件。

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
<td align="left"><p>[转换概述](transform-overview.md)</p></td>
<td align="left"><p>矩阵转换将处理 3D 图形的大量低级数学运算。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[世界转换](world-transform.md)</p></td>
<td align="left"><p>世界转换将坐标从模型空间（其中相对于模型的本地原点定义顶点）更改为世界空间。 在世界空间中，相对于场景中所有对象的共用原点定义顶点。 世界转换将模型放入世界中。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[视图转换](view-transform.md)</p></td>
<td align="left"><p><em>视图转换</em> 在世界空间中定位查看器，并将顶点转换为相机空间。 在相机空间中，相机或查看器位于原点，并面向正 z 方向。 视图矩阵会在围绕相机的位置（相机空间的原点）和方向的世界中重新定位各个对象。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[投影转换](projection-transform.md)</p></td>
<td align="left"><p><em>投影转换</em> 控制相机的内部结构，如选择相机的镜头。 这是三种转换类型中最复杂的类型。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[坐标系和几何图形](coordinate-systems-and-geometry.md)

 

 





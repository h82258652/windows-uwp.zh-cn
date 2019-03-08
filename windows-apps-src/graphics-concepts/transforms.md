---
title: 转换
description: 将几何图形推过固定函数几何图形管道的 Direct3D 部分是转换引擎。
ms.assetid: 0DF2A99A-335C-4D14-9720-6D7996DD635A
keywords:
- 转换
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a29d42a9254ca47402a38ea71c8c1ef69de5c6c7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639642"
---
# <a name="transforms"></a>转换


将几何图形推过固定函数几何图形管道的 Direct3D 部分是转换引擎。 它会找到世界中的模型和查看器、投影显示在屏幕上的顶点，并将顶点剪切到视区上。 转换引擎还会执行照明计算来确定每个顶点的漫射和反射组件。

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>本部分中的内容


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
<td align="left"><p><a href="transform-overview.md">转换概述</a></p></td>
<td align="left"><p>矩阵转换将处理 3D 图形的大量低级数学运算。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="world-transform.md">世界转换</a></p></td>
<td align="left"><p>世界转换将坐标从相对模型的本地原点定义顶点的世界空间更改到世界空间。 在世界空间中，顶点是相对场景中所有对象共同的原点定义的。 世界转换将模型放入世界空间中。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="view-transform.md">视图转换</a></p></td>
<td align="left"><p><em>视图转换</em> 在世界空间中定位查看器，并将顶点转换为相机空间。 在相机空间中，相机或查看器位于原点，并面向正 z 方向。 视图矩阵会在围绕相机的位置（相机空间的原点）和方向的世界空间中重新定位各个对象。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="projection-transform.md">投影转换</a></p></td>
<td align="left"><p><em>投影转换</em> 控制相机的内部结构，如选择相机的镜头。 这是三种转换类型中最复杂的类型。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[坐标系和坐标 geometry](coordinate-systems-and-geometry.md)

 

 





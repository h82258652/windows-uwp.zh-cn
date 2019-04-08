---
title: 基元
description: 3D 基元是单个 3D 实体中的顶点的集合。
ms.assetid: A1FE6F49-B0D4-4665-90E1-40AD98E668B1
keywords:
- 基元
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6c2bf3aa2f421efa7061f3003e6cab8c9f7b8c58
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648662"
---
# <a name="primitives"></a>基元


3D *基元*是单个 3D 实体中的顶点的集合。

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
<td align="left"><p><a href="point-lists.md">点列表</a></p></td>
<td align="left"><p>点列表是作为隔离的点呈现的顶点的集合。 你的应用程序可以在星图的 3D 场景中使用点列表，或在多边形的图面上使用虚线。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="line-lists.md">行列表</a></p></td>
<td align="left"><p>线列表是隔离的直线段的列表。 线列表用于向 3D 场景添加雨夹雪或暴雨之类的任务。 应用程序通过填充一组顶点来创建线列表。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="line-strips.md">条带线</a></p></td>
<td align="left"><p>线条带是由互连的线段组成的基元。 你的应用程序可使用线条带创建不封闭的多边形。 封闭多边形是通过线段将最后一个顶点连接到第一个顶点的多边形。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="triangle-lists.md">三角形列表</a></p></td>
<td align="left"><p>三角形列表是隔离的三角形的列表。 各个隔离的三角形可能相隔很近，也可能相隔不近。 三角形列表必须至少拥有 3 个顶点，并且顶点总数必须可以被 3 整除。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="triangle-strips.md">三角形条带</a></p></td>
<td align="left"><p>三角形带是一系列连接的三角形。 由于这些三角形是连接的，因此应用程序不需要为每个三角形重复指定所有三个顶点。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[坐标系统和几何结构](coordinate-systems-and-geometry.md)

 

 





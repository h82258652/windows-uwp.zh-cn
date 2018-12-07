---
title: 顶点和索引缓冲区
description: 顶点缓冲区是包含顶点数据的内存缓冲区；将处理顶点缓冲区中的顶点以执行转换、照明和剪裁。
ms.assetid: 8A39CD23-85FB-4424-9AC3-37919704CD68
keywords:
- 顶点和索引缓冲区
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d2ea6ce4060957ade5dd1007389be51176440f04
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8799168"
---
# <a name="vertex-and-index-buffers"></a>顶点和索引缓冲区


*顶点缓冲区*是包含顶点数据的内存缓冲区；将处理顶点缓冲区中的顶点以执行转换、照明和剪裁。 *索引缓冲区*是包含索引数据的内存缓冲区，索引数据是到顶点缓冲区的整数偏移量，用于渲染基元。

顶点缓冲区可包含可呈现的任何顶点类型（已转换或未转换、已亮起或未亮起）。 你可以处理顶点缓冲区中的顶点以执行转换、照明、生成剪裁标志等操作。 转换始终会执行。

顶点缓冲区的灵活性使其成为重复使用已转换几何图形的理想暂存点。 你可以创建单顶点缓冲区，对其中的顶点进行转换、照明和剪裁，以及将场景中的模型呈现所需的次数而不对其进行再转换，甚至在内插的呈现状态更改时也是如此。 当呈现使用多个纹理的模型时，这很有用：几何图形只转换一次，然后它的各个部分可以呈现所需的次数，并与所需的纹理更改交错。 在处理顶点后进行的呈现状态更改在下次处理顶点时生效。

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
<td align="left"><p><a href="introduction-to-buffers.md">缓冲区简介</a></p></td>
<td align="left"><p>缓冲区资源是一系列完全类型化的数据，被分组到元素中。 缓冲区将存储数据，如<em>顶点缓冲区</em> 中的纹理坐标、<em>索引缓冲区</em> 中的索引、<em>常量缓冲区</em> 中的着色器常量数据、位置矢量、法向矢量或设备状态。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="index-buffers.md">索引缓冲区</a></p></td>
<td align="left"><p><em>索引缓冲区</em>是包含索引数据的内存缓冲区，索引数据是到顶点缓冲区的整数偏移量，用于渲染基元。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[Direct3D 图形学习指南](index.md)

 

 





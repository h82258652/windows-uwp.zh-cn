---
title: 纹理筛选
description: 当通过将 3D 基元映射到 2D 屏幕的形式呈现基元时，纹理筛选针对基元的 2D 呈现图像中的每个像素生成一种颜色。
ms.assetid: 1CCF4138-5D48-4B07-9490-996844F994D8
keywords:
- 纹理筛选
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 449a31d92235efc50119bcd0db11b3532f523cd2
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8474781"
---
# <a name="texture-filtering"></a>纹理筛选


当通过将 3D 基元映射到 2D 屏幕的形式呈现基元时，纹理筛选针对基元的 2D 呈现图像中的每个像素生成一种颜色。

当 Direct3D 呈现基元时，会将 3D 基元映射到 2D 屏幕。 如果基元有纹理，则 Direct3D 必须使用该纹理，以针对基元的 2D 呈现图像中的每个像素生成一种颜色。 基元屏幕图像中的每个像素都必须从纹理获得颜色值。 此过程称作*纹理筛选*。

执行纹理筛选操作时，所使用的纹理通常会被放大或缩小。 也就是说，该纹理会被映射到大于或小于自身的基元图像。 放大纹理可使多个像素映射到一个纹素。 由此可能生成大块的图像。 放大纹理通常指单个像素被映射到多个纹素。 由此获得的图像可能模糊不清或混叠。 要解决这些问题，必须进行一些纹素颜色的混合，以混合出一种适合的像素颜色。

Direct3D 使复杂的纹理筛选过程变得简单。 Direct3D 提供三类纹理筛选，即线性筛选、各向异性筛选以及 mipmap 筛选。 如果你未选择任何纹理筛选，则 Direct3D 会使用最近点采样技术。

每类纹理筛选各有利弊。 例如，线性纹理筛选可能产生锯齿形边缘或者大块的最终图像。 但从计算角度，这是一个低开销的纹理筛选方法。 mipmap 筛选的效果最好，与各向异性筛选结合使用时尤为如此。 但这对内存容量的要求最高，以存储 Direct3D 支持的技术。

## <a name="span-idtypes-of-texture-filteringspanspan-idtypes-of-texture-filteringspanspan-idtypes-of-texture-filteringspantypes-of-texture-filtering"></a><span id="Types-of-texture-filtering"></span><span id="types-of-texture-filtering"></span><span id="TYPES-OF-TEXTURE-FILTERING"></span>纹理筛选的类型


Direct3D 支持以下纹理筛选方法。

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
<td align="left"><p><a href="nearest-point-sampling.md">最近点采样</a></p></td>
<td align="left"><p>应用程序不需要使用纹理筛选。 Direct3D 经过设置，可以计算纹素的地址（通常不是整数）并复制整数地址最近的纹素的颜色。 此过程称作<em>最近点采样</em>。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="bilinear-texture-filtering.md">双线性纹理筛选</a></p></td>
<td align="left"><p><em>双线性筛选</em>计算最接近取样点的 4 个纹素的加权平均值。 与最近点筛选相比，此筛选方法更加准确和通用。 此方法是借助现代图形硬件实施的，因此十分有效。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="anisotropic-texture-filtering.md">各向异性纹理筛选</a></p></td>
<td align="left"><p><em>各向异性</em>是可见于 3D 对象的纹素中的失真，此 3D 对象的表面面向与屏幕平面相对的角度。 当各向异性基元中的一个像素被映射到纹素时，会发生变形。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-filtering-with-mipmaps.md">使用 mipmap 进行纹理筛选</a></p></td>
<td align="left"><p><em>mipmap</em> 是一种纹理序列，每个纹理都以逐步降低的分辨率表示同一图像。 mipmap 中各图像或各级的高度和宽度都比上一级小二次方。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[纹理](textures.md)

 

 





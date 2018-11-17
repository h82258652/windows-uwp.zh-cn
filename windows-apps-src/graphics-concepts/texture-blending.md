---
title: 纹理混合
description: Direct3D 可以在单程内最多将八个纹理融合到基元。
ms.assetid: 9AD388FA-B2B9-44A9-B73E-EDBD7357ACFB
keywords:
- 纹理混合
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d4121bd402b048ee6102ed3be30b94a66e274273
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2018
ms.locfileid: "7152977"
---
# <a name="texture-blending"></a>纹理混合


Direct3D 可以在单程内最多将八个纹理融合到基元。 使用多个纹理混合可以显著增加 Direct3D 应用程序的帧速率。 应用程序可通过多个纹理混合，在单程内应用纹理、阴影、镜面光、漫射照明以及其他的特殊效果。

若要使用纹理混合，应用程序首先应检查用户的硬件是否支持纹理混合。

## <a name="span-idtexture-stages-and-the-texture-blending-cascadespanspan-idtexture-stages-and-the-texture-blending-cascadespanspan-idtexture-stages-and-the-texture-blending-cascadespantexture-stages-and-the-texture-blending-cascade"></a><span id="Texture-Stages-and-the-Texture-Blending-Cascade"></span><span id="texture-stages-and-the-texture-blending-cascade"></span><span id="TEXTURE-STAGES-AND-THE-TEXTURE-BLENDING-CASCADE"></span>纹理层以及纹理混合层叠


Direct3D 支持通过使用纹理层实现单程内的多个纹理混合。 纹理层取两个参数并对其执行混合操作，然后传递结果以进行进一步处理或光栅化。 你可以查看下图所示的纹理层。

![纹理层图示](images/texstg.png)

如上图所示，纹理层使用指定的运算符将两个参数混合。 常见操作包括简单的调制，或者添加参数的颜色或 α 值，但此外还有二十多个操作也受支持。 纹理层参数可以是一个相关的纹理、重复的颜色或 α 值（在高洛德着色过程中重复）、任意的颜色或 α 值，也可以是从之前纹理层获得的结果。

**注意** Direct3D 将颜色混合与 α 混合区分开来。 应用程序针对颜色和 α 单独设置混合操作以及相关参数，所以这些设置的结果是相互独立的。

 

将多个混合层所用的参数以及操作相结合，可以定义一种基于流的简单混合语言。 从某个纹理层得出的结果流到另一层，再从这一层流到下一层，依此类推。 结果在各纹理层之间流动并最终在多边形上光栅化的概念通常被称作纹理混合层叠。 下图显示了单个纹理层如何形成纹理混合层叠。

![纹理混合层叠的纹理层图示](images/tcascade.png)

设备中的各层都采用以零为基准的索引。 Direct3D 最多支持八个混合层，不过你始终应该检查设备功能，以确定当前硬件支持多少层。 第一个混合层使用索引 0，第二个混合层使用索引 1，依此类推，第八个混合层使用索引 7。 系统以索引升序的形式混合各层。

只使用所需数量的层；不用的混合层默认处于禁用状态。 因此，如果应用程序只使用前两层，则只需要设置纹理层 0 和 1 的相关操作及参数。 系统将混合这两层并忽略禁用的层。

如果应用程序在不同情况下使用不同数量的层（比如针对某些对象使用四层，针对其他对象只使用两层），则你无需明确禁用所有之前使用的层。 一种方法是禁用第一个不使用层的颜色操作，这样颜色操作就不会应用到具有更高索引的所有层。 另一种方法是通过将第一个纹理层（纹理层 0）的颜色操作设置为禁用状态，统一禁用纹理映射。

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
<td align="left"><p><a href="blending-stages.md">混合层</a></p></td>
<td align="left"><p>混合层是定义如何混合纹理的一组纹理操作及相关参数。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="multipass-texture-blending.md">多通道纹理混合</a></p></td>
<td align="left"><p>通过在多通道呈现的过程中将不同纹理应用到基元，Direct3D 应用程序可以实现多个特殊效果。 我们通用将此称为<em>多通道纹理混合</em>。 多通道纹理混合通常用于：通过应用几种不同纹理的多种颜色来模拟复杂照明及着色模型的效果。 比如<em>光照映射</em>。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[纹理](textures.md)

 

 





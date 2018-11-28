---
title: 照明的数学运算
description: Direct3D 光模型涵盖环境、漫射、高光和放射照明。 它足够灵活，可解决广泛的照明情况。 场景中光的总量称作全局照明。
ms.assetid: D0521F56-050D-4EDF-9BD1-34748E94B873
keywords:
- 照明的数学运算
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 38a65a3532fe401f31fbf0da656aff1a141fa71a
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7851297"
---
# <a name="mathematics-of-lighting"></a>照明的数学运算


Direct3D 光模型涵盖环境、漫射、高光和放射照明。 它足够灵活，可解决广泛的照明情况。 场景中光的总量称作*全局照明*。

全局照明按如下方式计算：

```
Global Illumination = Ambient Light + Diffuse Light + Specular Light + Emissive Light 
```

[环境光](ambient-lighting.md)是恒定照明。 环境光在所有方向都是恒定的，并且会无差别地为某个对象的所有像素上色。 这是一种快速计算，但会让对象看上去扁平且不逼真。

[漫射照明](diffuse-lighting.md)取决于光的方向和物体表面的法线。 光方向和表面法向矢量的改变会导致漫射照明在物体表面上发生变化。 计算漫射照明需要更长的时间，因为它随每个对象顶点而变化，但使用它的好处在于它能够为对象着色并赋予它们三维 (3D) 深度。

[高光照明](specular-lighting.md)可标识光照射至物体表面且反射至相机时产生的明亮反射高光。 高光照明比漫射光更强，且可更快照射至物体表面。 虽然计算高光照明的时间长于漫射照明，但高光照明具有可将重要细节添加至物体表面的优势。

[放射照明](emissive-lighting.md)是由物体发出的光；例如，辉光。 放射会使渲染对象看起来是自发光的。 放射会影响对象的颜色，使暗材质显得更亮并在一定程度上呈现出放射光的颜色。

可以通过向 3D 场景中应用这些照明类型中的每一种来实现逼真的照明效果。 为环境、放射和漫射分量计算的值输出为漫射顶点颜色；高光照明分量的值输出为高光顶点颜色。 环境光、漫射光和反射光值可能受到给定光的衰减和聚光因素的影响。 请参阅[衰减和聚焦因素](attenuation-and-spotlight-factor.md)。

为实现更逼真的照明效果，你可以添加更多的光；但这会增加场景的渲染时间。 为实现设计者想达到的所有效果，一些游戏使用了比通常可用的更高的 CPU 功率。 在这种情况下，通常在使用纹理贴图的同时通过使用光照图和环境贴图来向场景中添加照明，从而将照明计算的数量减到最少。

照明在相机空间中计算。 请参阅[相机空间转换](camera-space-transformations.md)。 当存在特殊条件（法线矢量已标准化、不需要顶点混合并且变换矩阵是正交的）时，可以在模型空间中计算优化的照明。

通过使用世界矩阵的逆将光源的位置和方向以及相机位置转换到模型空间，可在模型空间中进行所有照明计算。 因此，如果世界矩阵或视图矩阵引入了非一致性的缩放，则所得到的照明可能不准确。

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
<td align="left"><p><a href="ambient-lighting.md">环境光</a></p></td>
<td align="left"><p>环境光为场景提供恒定照明。 它会无差别地点亮所有对象顶点，因为它不取决于任意其他照明因素，例如，顶点法线、光线方向、光线位置、范围或衰减。 环境光在所有方向都是恒定的，并且会无差别地为某个对象的所有像素上色。 这是一种快速计算，但会让对象看上去扁平且不逼真。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="diffuse-lighting.md">漫射照明</a></p></td>
<td align="left"><p><em>漫射照明</em>取决于光的方向和物体表面的法线。 光方向和表面法向矢量的改变会导致漫射照明在物体表面上发生变化。 计算漫射照明需要更长的时间，因为它随每个对象顶点而变化，但使用它的好处在于它能够为对象着色并赋予它们三维 (3D) 深度。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="specular-lighting.md">高光照明</a></p></td>
<td align="left"><p><em>高光照明</em>可标识光照射至物体表面且反射至相机时产生的明亮反射高光。 高光照明比漫射光更强，且可更快照射至物体表面。 虽然计算高光照明的时间长于漫射照明，但高光照明具有可将重要细节添加至物体表面的优势。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="emissive-lighting.md">放射照明</a></p></td>
<td align="left"><p><em>放射照明</em>是由物体发出的光；例如，辉光。 放射会使渲染对象看起来是自发光的。 放射会影响对象的颜色，使暗材质显得更亮并在一定程度上呈现出放射光的颜色。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="camera-space-transformations.md">相机空间转换</a></p></td>
<td align="left"><p>通过使用通用视图矩阵转换对象顶点来计算相机空间中的顶点。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="attenuation-and-spotlight-factor.md">衰减和聚焦因素</a></p></td>
<td align="left"><p>全局照明公式的漫射和反射光组件包含介绍光线衰减和聚焦锥形的术语。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[光和材质](lights-and-materials.md)

 

 





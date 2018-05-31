---
title: 使用纹理的光映射
description: 光照图是包含关于 3D 场景中照明信息的纹理或纹理组。
ms.assetid: 5C7518D2-AC92-4A97-B7AF-4469D213D7BD
keywords:
- 使用纹理的光映射
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: d5d4a5e5a9fa656737ae86451a38a00edbe99c92
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
ms.locfileid: "1652446"
---
# <a name="light-mapping-with-textures"></a>使用纹理的光映射


光照图是包含关于 3D 场景中照明信息的纹理或纹理组。 光照图将光和阴影的区域映射到基元上。 多通道及多纹理混合使你的应用程序能够以比着色技术更逼真的外观渲染场景。

要逼真地渲染 3D 场景，应用程序必须考虑光源对场景外观的影响。 虽然平面着色、高氏着色等技术在这方面是很有价值的工具，但它们可能仍不满足你的需求。 Direct3D 支持多纹理混合。 这些功能使你的应用程序能够渲染出比单单使用着色技术更逼真的场景。 通过应用一个或多个光照图，你的应用程序可以将光和阴影的区域映射到其基元。

光照图是包含关于 3D 场景中照明信息的纹理或纹理组。 你可以将光照信息存储在光照图的 Alpha 值或颜色值中，或同时存储在这两者之中。

如果使用多通道纹理混合实现光映射，你的应用程序应在第一遍渲染中将光照图渲染到其基元上。 它应使用第二遍渲染来渲染基本纹理。 高光映射是个例外。 在这种情况下，应先渲染基本纹理，然后再添加光照图。

多纹理混合使你的应用程序能够在一遍渲染中渲染光照图和基本纹理。 如果用户的硬件了提供多纹理混合功能，你的应用程序应在执行光映射时使用它。 这可显著提高应用程序的性能。

借助光照图，Direct3D 应用程序可以在渲染基元时实现各种照明效果。 它不仅能够映射场景中的单色光和彩色光，而且还可以添加反射高光和漫射照明等细节。

有关使用 Direct3D 纹理混合执行光映射的信息，请参阅以下主题。

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
<td align="left"><p><a href="monochrome-light-maps.md">单色光映射</a></p></td>
<td align="left"><p>当较旧的 3D 加速器板不支持使用目标像素 Alpha 值的纹理混合时，单色光映射使较旧的适配器能够执行多通道纹理混合。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="color-light-maps.md">彩色光映射</a></p></td>
<td align="left"><p>彩色光映射使用光映射中的 RGB 数据展示光线信息。 如果应用程序使用彩色光映射，则其通常可以更真实地呈现 3D 场景。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="specular-light-maps.md">高光映射</a></p></td>
<td align="left"><p>通过光源照亮时，采用高反光材质的闪光对象会接收反射高光。 有时，相较于使用照明模块产生的反射高光，你可通过将高光映射应用至基元获取更精确的高光。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="diffuse-light-maps.md">漫射光映射</a></p></td>
<td align="left"><p>亚光表面会反射漫射光。 漫反射光的亮度取决于与光源的距离以及表面法线与光源方向矢量之间的角度。 纹理光照图可以模拟复杂的漫射照明。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[纹理](textures.md)

 

 





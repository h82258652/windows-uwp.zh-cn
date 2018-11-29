---
title: 光属性
description: 光属性描述光源的类型（点光、定向光、聚光）、衰减、颜色、方向、位置和范围。
ms.assetid: E832C3FD-9921-41C4-87B8-056E16B61B77
keywords:
- 光属性
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 86b8627461251a5d43762facc18c8a414a117fc9
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "8193259"
---
# <a name="light-properties"></a>光属性


光属性描述光源的类型（点光、定向光、聚光）、衰减、颜色、方向、位置和范围。 根据所用的光的类型，光可以具有衰减和范围属性，或聚光效果属性。 并非所有光类型都使用所有属性。

位置、范围和衰减属性定义了光在世界空间中的位置，以及其发射的光如何随距离而发生变化。

## <a name="span-idlightattenuationspanspan-idlightattenuationspanspan-idlightattenuationspanlight-attenuation"></a><span id="Light_Attenuation"></span><span id="light_attenuation"></span><span id="LIGHT_ATTENUATION"></span>光衰减


衰减控制光的强度如何随距离属性指定的最大距离而减弱。 有时使用三个浮点值来表示光衰减：Attenuation0、Attenuation1 和 Attenuation2。 这些浮点值的范围是从 0.0 到无穷大，用于控制光的衰减。 有些应用程序将 Attenuation1 成员设为 1.0，将其他的设为 0.0，导致光强度按 1 / D 发生变化，其中的 D 是从光到顶点的距离。 光强度在光源处最大，在光的范围内减弱到 1 / (光范围)。

虽然应用程序通常将 Attenuation0 设为 0.0、将 Attenuation1 设为恒定值并将 Attenuation2 设为 0.0，但我们可以通过更改此设置来实现各种光照效果。 你可以组合衰减值以获得更复杂的衰减效果。 或者，你也可以将它们设为超出正常范围的值，以营造光怪陆离的衰减效果。 但是，不允许使用负衰减值。 请参阅[衰减和聚焦因素](attenuation-and-spotlight-factor.md)。

## <a name="span-idlightcolorspanspan-idlightcolorspanspan-idlightcolorspanlight-color"></a><span id="Light_Color"></span><span id="light_color"></span><span id="LIGHT_COLOR"></span>光颜色


在 Direct3D 中，光发出在系统照明计算中独立使用的三种颜色：漫射颜色、环境颜色和高光颜色。 它们由 Direct3D 照明模块合并，与来自当前材质的对应物进行交互，以产生在渲染中使用的最终颜色。 漫射颜色与当前材质的漫射属性相互作用，高光颜色与材质的高光反射属性相互作用，依次类推。 有关 Direct3D 如何应用这些颜色的详细信息，请参阅[照明的数学运算](mathematics-of-lighting.md)。

在 Direct3D 应用程序中，通常有三种颜色值：漫射、环境和高光，它们用于定义正在发出的颜色。

漫射颜色是对系统计算最为重要的颜色类型。 最常见的漫射颜色是白色（R：1.0、G：1.0、B：1.0），但你可以根据需要创建颜色以实现所需的效果。 例如，你可以为壁炉使用红光，或者为“通行”交通信号灯使用绿光。

我们通常将光颜色分量设为介于 0.0 和 1.0（包括端值）之间的值，但这不是必需的。 例如，你可以将所有分量设为 2.0，以创造出“比白色更亮”的光。 当你使用常量以外的衰减设置时，此类设置可能特别有用。

虽然 Direct3D 为光使用了 RGBA 值，但它不使用 Alpha 颜色分量。

通常，材质颜色用于照明。 但是，你可以指定将被漫射或高光顶点颜色覆盖的材质颜色：放射、环境、漫射和高光。

Alpha/透明度值始终只来自漫射颜色的 Alpha 通道。

雾值始终只来自高光颜色的 Alpha 通道。

## <a name="span-idlightdirectionspanspan-idlightdirectionspanspan-idlightdirectionspanlight-direction"></a><span id="Light_Direction"></span><span id="light_direction"></span><span id="LIGHT_DIRECTION"></span>光方向


光的方向属性决定了物体发出的光在世界空间中行进的方向。 方向使用矢量描述，并且只有定向光和聚光使用方向。

将光方向设置为矢量。 方向矢量被描述为与逻辑原点的距离，与光在场景中的位置无关。 因此，不管其位置被定义为何处，沿正 z 轴直线指向场景的聚光具有 &lt;0,0,1&gt; 的方向矢量。 同样，你可以使用方向为 &lt;0,-1,0&gt; 的定向光模拟直接照射在场景中的阳光。 你不必创建沿坐标轴闪耀的光；你可以混合和匹配值，以创建在更有趣的角度闪耀的光。

虽然你不需要标准化光的方向矢量，但请务必确保它有度量值。 也就是说，不要使用 &lt;0,0,0&gt; 方向矢量。

## <a name="span-idlightpositionspanspan-idlightpositionspanspan-idlightpositionspanlight-position"></a><span id="Light_Position"></span><span id="light_position"></span><span id="LIGHT_POSITION"></span>光位置


光位置使用矢量结构描述。 假设世界空间中有 x、y 和 z 坐标。 定向光是唯一一种不使用位置属性的光类型。

## <a name="span-idlightrangespanspan-idlightrangespanspan-idlightrangespanlight-range"></a><span id="Light_Range"></span><span id="light_range"></span><span id="LIGHT_RANGE"></span>光范围


光的范围属性决定在世界空间中的场景中的哪些网格处不再接收由该物体发射的光的距离。 定向光不使用范围属性。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[光和材质](lights-and-materials.md)

 

 





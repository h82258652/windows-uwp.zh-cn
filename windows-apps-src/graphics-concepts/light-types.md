---
title: 光类型
description: 光类型属性定义所用光源的类型。 Direct3D 中有三种类型的光 - 点光、聚光和定向光。
ms.assetid: 57748CAF-6F08-4D1D-9ED6-8FAA8C5FE314
keywords:
- 光类型
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9898a050131813b7b2431f8fc11397eee7c7942c
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5838263"
---
# <a name="light-types"></a>光类型


光类型属性定义所用光源的类型。 Direct3D 中有三种类型的光 - 点光、聚光和定向光。 每种类型以不同的方式照亮场景中的对象，它们具有不同的计算开销水平。

## <a name="span-idpointlightspanspan-idpointlightspanspan-idpointlightspanpoint-light"></a><span id="Point_Light"></span><span id="point_light"></span><span id="POINT_LIGHT"></span>点光


点光在场景中具有颜色和位置，但没有单一方向。 它们向所有方向均匀发光，如下图所示。

![点光示意图](images/ptlight.png)

灯泡是点光的良好示例。 点光受衰减和范围的影响，以顶点为基础照亮网格。 在照明期间，Direct3D 使用点光在世界空间中的位置和被点亮的顶点的坐标来生成光的方向矢量和光行进的距离。 两者与顶点法线一起用于计算光对表面照明的贡献。

## <a name="span-iddirectionallightspanspan-iddirectionallightspanspan-iddirectionallightspandirectional-light"></a><span id="Directional_Light"></span><span id="directional_light"></span><span id="DIRECTIONAL_LIGHT"></span>定向光


定向光只有颜色和方向，没有位置。 它们发射平行光。 这意味着由定向光产生的所有光线在相同方向上穿过场景。 你可以将定向光想象成位于近乎无限远处的光源，如太阳。 定向光不受衰减或范围的影响，因此，你指定的方向和颜色是 Direct3D 计算顶点颜色时考虑的唯一因素。 由于照明因素较少，它们是可以使用的计算密集程度最低的光。

## <a name="span-idspotlightspanspan-idspotlightspanspan-idspotlightspanspotlight"></a><span id="SpotLight"></span><span id="spotlight"></span><span id="SPOTLIGHT"></span>聚光


聚光具有颜色、位置和发光方向。 从聚光发射的光由明亮的内锥和较大的外锥组成，光强度在两者之间减弱，如下图所示。

![带有内锥和外锥的聚焦的示意图](images/spotlt.png)

聚光受减退、衰减和范围的影响。 在计算场景中对象的照明效果时，需要考虑这些因素以及光到每个顶点的射程。 由于需要为每个顶点计算这些效果，聚光在 Direct3D 中是计算耗时最长的光类型。

只有聚光使用 Falloff、Theta 和 Phi 值。 这些值控制聚光对象内锥和外锥的大小，以及它们之间光的减弱程度。

Theta 是聚光内锥的弧度角，Phi 值是聚光外锥的角度。 Falloff 控制光强度在内锥的外边缘和外锥的内边缘之间的减弱程度。 大多数应用程序将 Falloff 设为 1.0，以在两个锥体之间营造均匀的衰减效果，但你可以根据需要设置其他的值。

下图显示了这些值之间的关系，以及它们如何影响聚光的内锥和外锥。

![说明 Phi 和 Theta 值与聚光锥体的关系的示意图](images/spotlt2.png)

聚光发出的光锥包括两个部分：一个明亮的内锥和一个外锥。 光在内锥中最亮，在外锥之外不存在，光强度在这两个区域之间逐渐衰减。 这种类型的衰减通常被称为减退。

顶点接收的光量取决于其在内锥或外锥中的位置。 Direct3D 计算聚光方向矢量 (L) 和从光到顶点的矢量 (D) 的点积。 该值等于两个矢量之间角度的余弦，并用作顶点位置的指示器。它可以与光的锥角进行比较，以确定顶点可能位于内锥或外锥中的何处。 下图提供了这两个矢量之间关联的图形化表示。

![聚光方向矢量和从顶点到聚光的矢量的示意图](images/spotalg1.png)

系统将此值与聚光的内锥角和外锥角的余弦值进行比较。 光的 Theta 和 Phi 值表示内锥和外锥的总锥角。 当顶点远离照明中心（而不是在总锥角上）时会发生衰减现象，因此，运行时会先将这些锥角分成两半，然后再计算它们的余弦值。

如果矢量 L 和 D 的点积小于或等于外锥角的余弦值，则顶点位于外锥之外且不接收光线。 如果 L 和 D 的点积大于内锥角的余弦值，则顶点位于内锥中且接收最大的光量，此时需要考虑距离变化导致的衰减。 如果顶点位于两个区域之间的某处，则使用下面的公式计算减退。

![发生减退后顶点处光强度的计算公式](images/falloff.png)

其中：

-   I<sub>f</sub> 是减退后的光强度
-   Alpha 是矢量 L 和 D 之间的角度
-   Theta 是内锥角
-   Phi 是外锥角
-   p 是减退

此公式生成一个介于 0.0 和 1.0 之间的值，用于缩放光在顶点处的强度，从而营造减退效果。 此外，还需要根据顶点与光的距离应用衰减。 下图显示了不同的减退值对减退曲线的影响。

![光强度与从顶点到光的距离的示意图](images/fallgraf.png)

各种减退值对实际照明的影响很微小，将不为 1.0 的减退值生成减退曲线会产生较小的性能损失。 出于上述原因，此值通常设为 1.0。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[光和材质](lights-and-materials.md)

 

 





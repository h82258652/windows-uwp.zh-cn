---
title: 照明概述
description: 使用 Direct3D 照明时，Direct3D 负责为你处理照明细节。 如果需要，高级用户可以自行执行照明。
ms.assetid: FCBF6A92-4EAC-4CCC-A76C-79985AF348AE
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6eca73beae6634d1809c0e9e779d80a43b495a65
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5943462"
---
# <a name="lighting-overview"></a>照明概述

使用 Direct3D 照明时，Direct3D 负责为你处理照明细节。 如果需要，高级用户可以自行执行照明。

启用照明时，Direct3D 基于以下各项的组合计算每个对象顶点的颜色：

-   相关纹理贴图中的纹素。
-   顶点处的漫射和高光颜色（如果已指定）。
-   由场景中的光源或场景的环境光水平产生的光的颜色和强度。

你使用照明和材质的方式会使已渲染场景的外观出现较大的差异。 材质定义光如何从表面反射。 直射光和环境光水平定义被反射的光。 如果启用了照明，则必须使用材质来渲染场景。

光对于渲染场景来说不是必需的，但在没有光的情况下已渲染的场景中的细节是不可见的。 如果对无照明的场景进行渲染，充其量只会显露出场景中物体的轮廓。 这对大多数情况来说不够详细。

## <a name="span-iddirectlightvsambientlightspanspan-iddirectlightvsambientlightspandirect-light-vs-ambient-light"></a><span id="direct_light_vs._ambient_light"></span><span id="DIRECT_LIGHT_VS._AMBIENT_LIGHT"></span>直射光与环境光


虽然直射光和环境光都能照亮场景中的对象，但它们彼此独立，具有非常不同的效果，并且要求你以完全不同的方式使用它们。

*直射光*直接照射对象。 直射光总是具有方向和颜色，并且是着色算法（如高氏着色）的要素之一。 不同类型的光以不同的方式发射直射光，从而产生特殊的衰减效果。

*环境光*在场景中无处不在。 环境光是填充整个场景的一般水平的光，它与对象及对象在场景中的位置无关。 环境光没有位置或方向，只有颜色和强度。 场景中的所有光加总形成整体环境光。

环境光颜色采用 RGBA 值的形式，其中的每个分量都是从 0 到 255 的整数值。 这不同于 Direct3D 中的大多数颜色值。

红色、绿色和蓝色分量组合起来形成环境光的最终颜色。 Alpha 分量控制颜色的透明度。 当使用硬件加速或 RGB 仿真时，Alpha 分量会被忽略。

## <a name="span-iddirect3dlightmodelvsnaturespanspan-iddirect3dlightmodelvsnaturespandirect3d-light-model-vs-nature"></a><span id="direct3d_light_model_vs._nature"></span><span id="DIRECT3D_LIGHT_MODEL_VS._NATURE"></span>Direct3D 光模型与现实世界


在自然环境中，当光从光源发出后，会被成百上千个对象反射，然后才到达人眼。 每次反射时，都有一些光被表面吸收，一些朝随机方向散射，剩余的光到达另一个表面或进入人眼。 这一过程不断进行，直到光被完全削减，或者被用户感知。

显然，完全模拟光的自然行为所需的计算对于实时的 Direct3D 图形应用来说太过耗时。 因此，出于速度方面的考虑，Direct3D 光模型以近似于光在自然界中的行为方式工作。 Direct3D 用红色、绿色和蓝色分量描述光，这些分量组合起来形成最终的颜色。

在 Direct3D 中，当光被表面反射时，光颜色与表面本身发生数学作用，从而呈现出最终显示在屏幕上的颜色。 有关 Direct3D 使用的算法的详细信息，请参阅[照明的数学运算](mathematics-of-lighting.md)。

Direct3D 光模型将光归纳为两种类型：环境光和直射光。 它们具有不同的属性，并且以不同的方式与表面的材质进行交互。 环境光是经过多次散射的光，其方向和光源是不确定的：它在任意位置保持低水平的光强度。 摄影师使用的间接照明是环境光的一个绝佳示例。

与自然界一样，Direct3D 中的环境光没有真正的方向或光源，只有颜色和强度。 事实上，环境光水平完全独立于场景中产生光的任何物体。 环境光不会导致镜面反射。

直射光是由场景中的光源产生的光；它始终具有颜色和强度，并且在指定方向上行进。 直射光与表面的材质相互作用以产生反射高光，其方向用作着色算法（包括高氏着色）中的要素之一。 直射光被反射时，它不会对场景中的环境光水平产生任何影响。 场景中产生直射光的光源具有不同的特性，这些特性会对其如何照亮场景产生影响。

此外，多边形的材质具有影响多边形如何反射其接收的光的属性。 你设置描述材质如何反射环境光的单一反射率特性，并设置各个特性以确定材质的镜面反射率和漫射率。

## <a name="span-idcolorvaluesforlightsandmaterialsspanspan-idcolorvaluesforlightsandmaterialsspanspan-idcolorvaluesforlightsandmaterialsspancolor-values-for-lights-and-materials"></a><span id="Color_Values_for_Lights_and_Materials"></span><span id="color_values_for_lights_and_materials"></span><span id="COLOR_VALUES_FOR_LIGHTS_AND_MATERIALS"></span>光和材质的颜色值


Direct3D 用四个分量（红、绿、蓝和 Alpha）描述颜色，这四个分量组合起来形成最终的颜色。 每个分量的范围是从 0.0 到 1.0。 虽然光和材质使用相同的结构来描述颜色，但光和材质在使用值的方式上略有不同。

光源的颜色值表示其发射的特定光分量的量。 光不使用 Alpha 分量，它只使用颜色的红色、绿色和蓝色分量。 你可以将三个分量想象成投影电视上的红色、绿色和蓝色透镜。 每个透镜可以关闭（相应成员的值为 0.0），可以达到最高亮度（值为 1.0），或者介于两者之间。

透过透镜的颜色组合起来就形成了光的最终颜色。 R(1.0)、G(1.0)、B(1.0) 组合会产生白光，而 R(0.0)、G(0.0)、B(0.0) 根本不发光。 你可以使光只发射一种分量，以产生纯红、纯绿或纯蓝色的光；或者，光也可以使用组合来发射黄色、紫色等颜色。 你甚至可以设置负颜色分量值，以创造出实际上会从场景中消除光的“暗光”。 或者，你也可以将分量设为大于 1.0 的某个值，以创建异常明亮的光。

另一方面，对于材质，颜色值表示使用该材质渲染的表面会反射多少光分量。 颜色分量为 R(1.0)、G(1.0)、B(1.0)、A(1.0) 的材质会反射照射在其上的所有光。 同样，颜色分量为 R(0.0)、G(1.0)、B(0.0)、A(1.0) 的材质会反射照射在其上的所有绿光。 材质具有多个反射率值，用以营造多种类型的效果。

请参阅[光类型](light-types.md)和[光属性](light-properties.md)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[光和材质](lights-and-materials.md)

 

 





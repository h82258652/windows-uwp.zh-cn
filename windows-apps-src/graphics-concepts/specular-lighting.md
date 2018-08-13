---
title: 高光照明
description: 高光照明可标识光照射至物体表面且反射至相机时产生的明亮反射高光。
ms.assetid: 71F87137-B00F-48CE-8E6A-F98A139A742A
keywords:
- 高光照明
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 305924b19914bf4e85366695add2476c3c81c281
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044806"
---
# <a name="specular-lighting"></a>高光照明


*高光照明*可标识光照射至物体表面且反射至相机时产生的明亮反射高光。 高光照明比漫射光更强，且可更快照射至物体表面。 虽然计算高光照明的时间长于漫射照明，但高光照明具有可将重要细节添加至物体表面的优势。

建立镜面反射模型要求系统了解光线照射的方向及观看者眼睛所对方向。 系统采用了简化版的 Phong 镜面反射模型，这种模型使用中间矢量，可获得近似于镜面反射的强度。

默认的照明状态不会计算反射高光。

## <a name="span-idspecularlightingequationspanspan-idspecularlightingequationspanspan-idspecularlightingequationspanspecular-lighting-equation"></a><span id="Specular_Lighting_Equation"></span><span id="specular_lighting_equation"></span><span id="SPECULAR_LIGHTING_EQUATION"></span>高光照明公式


高光照明可用以下公式说明。

|                                                                             |
|-----------------------------------------------------------------------------|
| 高光照明 = Cₛ \* sum\[Lₛ \* (N · H)<sup>P</sup> \* Atten \* Spot\] |

 

变量及其类型和范围如下所列：

| 参数    | 默认值 | 类型                                                             | 说明                                                                                            |
|--------------|---------------|------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| Cₛ           | (0,0,0,0)     | 红色、绿色、蓝色和 alpha 透明度（浮点值） | 反射颜色。                                                                                        |
| sum          | 不适用           | 不适用                                                              | 每束光线的反射组件的总和。                                                          |
| N            | 不适用           | 3D 矢量（x、y 和 z 浮点值）                    | 顶点法线。                                                                                         |
| H            | 不适用           | 3D 矢量（x、y 和 z 浮点值）                    | 中间矢量。 请参阅有关中间矢量的部分。                                                |
| <sup>P</sup> | 0.0           | 浮点                                                   | 镜面反射能量。 范围为 0 至正无穷                                                     |
| Lₛ           | (0,0,0,0)     | 红色、绿色、蓝色和 alpha 透明度（浮点值） | 光线反射颜色。                                                                                  |
| Atten        | 不适用           | 浮点                                                   | 灯光衰减值。 请参阅[衰减和聚焦因素](attenuation-and-spotlight-factor.md)。 |
| Spot         | 不适用           | 浮点                                                   | 聚光因素。 请参阅[衰减和聚焦因素](attenuation-and-spotlight-factor.md)。        |

 

Cₛ 的值为：

-   顶点颜色 1，前提为镜面材料来源是漫射顶点颜色，且在顶点声明中提供第一个顶点的颜色。
-   顶点颜色 2，前提为镜面材料来源是反射顶点颜色，且在顶点声明中提供第二个顶点的颜色。
-   材料反射颜色

**注意**：如果采用了任何一种反射材料来源选项且未提供顶点颜色，则使用材料反射颜色。

 

单独处理和内插所有灯光后，将反射组件的范围限制为 0 至 255。

## <a name="span-idthehalfwayvectorspanspan-idthehalfwayvectorspanspan-idthehalfwayvectorspanthe-halfway-vector"></a><span id="The_Halfway_Vector"></span><span id="the_halfway_vector"></span><span id="THE_HALFWAY_VECTOR"></span>中间向量


中间向量 (H) 存在于以下两个向量中间：从物体顶点到光源的向量，及从物体顶点到相机位置的向量。 Direct3D 提供两种计算中间向量的方式。 如果启用相机相关的反射高光（而不是正交反射高光），则系统通过相机位置和顶点位置，及灯光的方向矢量计算中间矢量。 下面的公式对此进行了说明。

|                                           |
|-------------------------------------------|
| H = norm(norm(Cₚ - Vₚ) + L<sub>dir</sub>) |

 

| 参数       | 默认值 | 类型                                          | 说明                                                  |
|-----------------|---------------|-----------------------------------------------|--------------------------------------------------------------|
| Cₚ              | 不适用           | 3D 矢量（x、y 和 z 浮点值） | 相机位置。                                             |
| Vₚ              | 不适用           | 3D 矢量（x、y 和 z 浮点值） | 顶点位置。                                             |
| L<sub>dir</sub> | 不适用           | 3D 矢量（x、y 和 z 浮点值） | 从顶点位置到光线位置的方向矢量。 |

 

以这种方式确定中间矢量可能是计算密集型操作。 或者，使用正交反射高光（而不是相机相关的反射高光）指示系统进行操作，就像 Z 轴上的视角距离为无限远。 通过以下公式来反映。

|                                     |
|-------------------------------------|
| H = norm((0,0,1) + L<sub>dir</sub>) |

 

这种设置的计算密集程度较低，但精确度大大降低，所以最适合采用正交投影的应用使用。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>示例


在此示例中，物体的颜色由场景反射光颜色和材料反射颜色决定。

根据公式，生成的对象顶点颜色是材料颜色和光颜色的组合。

以下两个图例显示了反射材料颜色（灰色）和反射光颜色（白色）。

![灰色球体的图例](images/amb1.jpg)![白色球体的图例](images/lightwhite.jpg)

生成的反射高光如下图所示。

![反射高光图例](images/lights.jpg)

反射高光与环境和漫射照明结合产生的结果如下图所示。 借助三种所应用的照明所产生结果将与真实对象更相似。

![反射高光、环境照明和漫射照明结合图例](images/lightads.jpg)

高光照明的计算强度高于漫射照明。 通常可用于提供有关表面材料的视觉线索。 反射高光因表面材料的大小和颜色而异。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[照明的数学运算](mathematics-of-lighting.md)

 

 





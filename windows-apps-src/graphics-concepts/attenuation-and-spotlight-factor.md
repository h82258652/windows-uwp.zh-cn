---
title: 衰减和聚焦因素
description: 全局照明公式的漫射和反射光分量涉及介绍光线衰减和聚焦锥形的术语。
ms.assetid: F61D4ACB-09AB-4087-9E2D-224E472D6196
keywords:
- 衰减和聚焦因素
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 65b9f6700ddd11c41193820a5247a90c2382c98b
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6257820"
---
# <a name="attenuation-and-spotlight-factor"></a>衰减和聚焦因素


全局照明公式的漫射和反射光分量涉及介绍光线衰减和聚焦锥形的术语。 下面介绍了这些术语。

## <a name="span-idattenuationspanspan-idattenuationspanspan-idattenuationspanattenuation"></a><span id="Attenuation"></span><span id="attenuation"></span><span id="ATTENUATION"></span>衰减


光线的衰减取决于光线的类型和光线与顶点位置之间的距离。 若要计算衰减，请使用以下公式之一。

Atten = 1/( att0<sub>i</sub> + att1<sub>i</sub> \* d + att2<sub>i</sub> \* d²)

其中：

| 参数        | 默认值 | 类型           | 描述                                     | 范围          |
|------------------|---------------|----------------|-------------------------------------------------|----------------|
| att0<sub>i</sub> | 0.0           | 浮点 | 恒定衰减系数                     | 0 至正无穷 |
| att1<sub>i</sub> | 0.0           | 浮点 | 线性衰减因素                       | 0 至正无穷 |
| att2<sub>i</sub> | 0.0           | 浮点 | 二次衰减因素                    | 0 至正无穷 |
| d.                | 不适用           | 浮点 | 顶点位置至光线位置的距离 | 不适用            |

 

-   如果光线是定向光，则 Atten = 1。
-   如果光线与顶点之间的距离超出光线范围，则 Atten = 0。

光线与顶点位置之间的距离始终为正。

d = |L<sub>dir</sub> |

其中：

| 参数       | 默认值 | 类型                                             | 描述                                                 |
|-----------------|---------------|--------------------------------------------------|-------------------------------------------------------------|
| L<sub>dir</sub> | 不适用           | 含 x、y 和 z 浮点值的 3D 矢量 | 从顶点位置到光线位置的方向矢量 |

 

如果 d 大于光线范围，Direct3D 不再进行衰减计算，也不再对从光线到顶点的区域应用效果。

衰减常量充当公式中的系数 - 你可以通过对其进行简单的调整生成各种衰减曲线。 你可以将 Attenuation1 设置为 1.0，以创建不会衰减但仍然受制于范围的光线，也可以测试不同的值以实现不同的衰减效果。

光线的最大范围处的衰减不是 0.0。 若要阻止光线范围内的光线突然出现，应用程序可以扩大光线范围。 或者，应用程序可以设置衰减常量，以便光线范围内的衰减因素接近 0.0。 将衰减值乘以光线颜色的红色、绿色和蓝色分量，以缩放作为光线到达顶点的距离因素的光线强度。

## <a name="span-idspotlight-factorspanspan-idspotlight-factorspanspan-idspotlight-factorspanspotlight-factor"></a><span id="Spotlight-Factor"></span><span id="spotlight-factor"></span><span id="SPOTLIGHT-FACTOR"></span>聚焦因素


以下公式指定了聚焦因素。

![聚焦因素的公式](images/dx8light9.png)

| 参数         | 默认值 | 类型           | 描述                              | 范围                    |
|-------------------|---------------|----------------|------------------------------------------|--------------------------|
| rho<sub>i</sub>   | 不适用           | 浮点 | 聚焦 i 的余弦（角度）            | 不适用                      |
| phi<sub>i</sub>   | 0.0           | 浮点 | 聚焦 i 的 Penumbra 角度（以弧度为单位） | \[theta<sub>i</sub>, pi) |
| theta<sub>i</sub> | 0.0           | 浮点 | 聚焦 i 的 Umbra 角度（以弧度为单位）    | \[0, pi)                 |
| 衰减           | 0.0           | 浮点 | 衰减因素                           | （负无穷，正无穷）   |

 

其中：

rho = norm(L<sub>dcs</sub>)<sup>.</sup>norm(L<sub>dir</sub>)

且：

| 参数       | 默认值 | 类型                                             | 描述                                                 |
|-----------------|---------------|--------------------------------------------------|-------------------------------------------------------------|
| L<sub>dcs</sub> | 不适用           | 含 x、y 和 z 浮点值的 3D 矢量 | 相机空间中光线方向的负数         |
| L<sub>dir</sub> | 不适用           | 含 x、y 和 z 浮点值的 3D 矢量 | 从顶点位置到光线位置的方向矢量 |

 

计算光线衰减后，为计算顶点的漫射和反射分量，Direct3D 还会考虑聚焦效果（如果适用）、光线从表面反射的角度和当前材料的反射率。 在[光线类型](light-types.md)中，请参阅“聚焦”。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[照明的数学运算](mathematics-of-lighting.md)

 

 





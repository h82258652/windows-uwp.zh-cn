---
title: 漫射照明
description: 漫射照明取决于光的方向和物体表面的法线。
ms.assetid: 8AF78742-76B1-4BBB-86E3-94AE6F48B847
keywords:
- 漫射照明
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5846edda167823b7ae161d332fbde450ccf20d72
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5817700"
---
# <a name="diffuse-lighting"></a>漫射照明


*漫射照明*取决于光的方向和物体表面的法线。 光方向和表面法向矢量的改变会导致漫射照明在物体表面上发生变化。 计算漫射照明需要更长的时间，因为它随每个对象顶点而变化，但使用它的好处在于它能够为对象着色并赋予它们三维 (3D) 深度。

在调整光线强度以实现任意衰减效果后，光线引擎基于顶点法线的角度和入射光线的方向计算从顶点折射了多少剩余光线。 光线引擎会跳到此步处理定向光，因为它们不会随距离衰减。 系统会考虑两种反射类型，即漫射和反射，并使用不同的公式确定分别为它们反射多少光。

在计算反射多少光后，Direct3D 将这些新值应用于当前材料的漫射和镜面反射属性。 产生的颜色值为漫射和反射分量，光栅器用此来产生高氏着色和反射高光。

漫射照明可用以下公式说明。

Diffuse Lighting = sum\[C<sub>d</sub>\*L<sub>d</sub>\*(N<sup>.</sup>L<sub>dir</sub>)\*Atten\*Spot\]

| 参数       | 默认值 | 类型          | 描述                                                                                      |
|-----------------|---------------|---------------|--------------------------------------------------------------------------------------------------|
| 总和             | 不适用           | 不适用           | 每束光线的漫射分量的总和。                                                     |
| C<sub>d</sub>   | (0,0,0,0)     | D3DCOLORVALUE | 漫射颜色。                                                                                   |
| L<sub>d</sub>   | (0,0,0,0)     | D3DCOLORVALUE | 光漫射颜色。                                                                             |
| N               | 不适用           | D3DVECTOR     | 顶点法线                                                                                    |
| L<sub>dir</sub> | 不适用           | D3DVECTOR     | 从物体顶点到光线的方向矢量。                                                |
| Atten           | 不适用           | 浮点数         | 光衰减。 请参阅[衰减和聚焦因素](attenuation-and-spotlight-factor.md)。 |
| Spot            | 不适用           | 浮点数         | 聚焦因素。 请参阅[衰减和聚焦因素](attenuation-and-spotlight-factor.md)。  |

 

若要计算衰减 (Atten) 或聚焦特征 (Spot)，请参阅[衰减和聚焦因素](attenuation-and-spotlight-factor.md)。

单独处理和内插所有灯光后，将漫射分量的范围限制为 0 至 255。 生成的漫射照明值是环境、漫射和放射光值的组合。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>示例


在此示例中，使用光漫射颜色和材料漫射颜色为对象设置颜色。

根据公式，生成的对象顶点颜色是材料颜色和光颜色的组合。

下面两个图例显示了材料颜色（灰色）和光颜色（鲜红色）。

![灰色球体的图例](images/amb1.jpg)![红色球体的图例](images/lightred.jpg)

生成的场景显示在以下图例中。 场景中的唯一对象是一个球体。 漫射光线计算使用材料和光漫射颜色，并使用点产品按光线方向和顶点法线之间的角度对其进行修改。 因此，球体背面会变得更暗，因为球体曲线表面远离光线。

![有漫射照明的球体图示](images/lightd.jpg)

组合漫射光与上一示例中的环境光可以使对象的整个表面变暗。 环境光使整个表面变暗，而漫射光帮助显露对象的 3D 形状，如下图所示。

![有漫射照明和环境照明的球体图示](images/lightad.jpg)

漫射照明的计算强度高于环境照明。 因为这取决于顶点法线和光线方向，你可以在 3D 空间中查看对象几何结构，这会产生比环境照明更逼真的照明。 你可以使用反射高光达到更逼真的外观。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[照明的数学运算](mathematics-of-lighting.md)

 

 





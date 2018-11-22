---
title: 放射照明
description: 放射照明是由物体发出的光；例如，辉光。
ms.assetid: 262EB9E2-DF96-401F-93D6-51A7BB60074B
keywords:
- 放射照明
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9ce2082479261cd96fb1c5bafd5f2df06bf6f239
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "7575503"
---
# <a name="emissive-lighting"></a>放射照明


*放射照明*是由物体发出的光；例如，辉光。 放射会使渲染对象看起来是自发光的。 放射会影响对象的颜色，使暗材质显得更亮并在一定程度上呈现出放射光的颜色。

放射照明是用单个术语来描述的。

放射照明 = Cₑ

其中：

| 参数 | 默认值 | 类型                                                                 | 描述     |
|-----------|---------------|----------------------------------------------------------------------|-----------------|
| Cₑ        | (0,0,0,0)     | 红色、绿色、蓝色和 alpha 透明度（所有浮点值） | 放射颜色。 |

 

Cₑ 的值为颜色 1 或颜色 2。 如果未提供顶点颜色，则使用材料放射颜色。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>示例


在此示例中，使用场景环境光和材料环境光为对象着色。

根据公式，生成的对象顶点颜色是材料颜色。

下图显示了材料颜色（绿色）。 放射光使用相同的颜色点亮所有对象顶点。 它不取决于顶点法线或光线方向。 因此，球体看起来像 2D 圆形，因为对象表面周围的着色没有差别。

![绿色球体的图示](images/lighte.jpg)

下图显示了放射光与其他三种类型的光混合的方式。 在球体右侧，混合了绿色放射光和红色环境光。 在球体左侧，绿色放射光与产生红色渐变效果的红色环境光和漫射光混合。 随着反射光值显著降低、脱离混合起来制造黄色的环境光、漫射光和放射光值，中心为白色的反射高光并形成黄色环形。

![带放射光的绿色球体的图示](images/lightadse.jpg)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[照明的数学运算](mathematics-of-lighting.md)

 

 





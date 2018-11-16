---
title: 各向异性纹理筛选
description: 各向异性是可见于 3D 对象的纹素中的失真，此 3D 对象的表面面向与屏幕平面相对的角度。 当各向异性基元中的像素映射到纹素时，其形状就会失真。
ms.assetid: 58923809-EF76-4C16-BCE7-922A66425F83
keywords:
- 各向异性纹理筛选
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6e91c707b31de859d61ae926518c40812758320e
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6838865"
---
# <a name="anisotropic-texture-filtering"></a>各向异性纹理筛选


*各向异性*是可见于 3D 对象的纹素中的失真，此 3D 对象的表面面向与屏幕平面相对的角度。 当各向异性基元中的像素映射到纹素时，其形状就会失真。 Direct3D 测量像素的各向异性，作为反向映射到纹理空间的屏幕像素的拉伸，即，长度除以宽度。

你可以将各向异性纹理筛选与线性纹理筛选或 mipmap 纹理筛选结合使用，以改进呈现结果。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[纹理筛选](texture-filtering.md)

 

 





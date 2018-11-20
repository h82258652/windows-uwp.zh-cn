---
title: 相机空间转换
description: 通过使用通用视图矩阵转换对象顶点来计算相机空间中的顶点。
ms.assetid: 86EDEB95-8348-4FAA-897F-25251B32B076
keywords:
- 相机空间转换
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9de6759fb15aef4b32a5e9022a27cab09af300f8
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7280615"
---
# <a name="camera-space-transformations"></a>相机空间转换


通过使用通用视图矩阵转换对象顶点来计算相机空间中的顶点。

V = V \* wvMatrix

通过使用通用视图矩阵的反向转置转换对象法线来计算相机空间中的顶点法线。 通用视图矩阵可能正交，也可能不正交。

N = N \* (wvMatrix⁻¹)<sup>T</sup>

矩阵反转和矩阵转置在 4 x 4 矩阵上进行。 乘法组合了法线与生成的 4 x 4 矩阵的 3 x 3 部分。

如果呈现状态被设置为标准化法线，则在转换为相机空间后，顶点法线矢量会被标准化，如下所示：

N = norm(N)

通过使用视图矩阵转换光源位置来计算相机空间中的光线位置。

Lₚ = Lₚ \ * vMatrix

将光源方向乘以视图矩阵，标准化，然后对结果求反，从而计算相机空间中定向光的光线方向。

L<sub>dir</sub> = -norm(L<sub>dir</sub> \* wvMatrix)

对于点光和聚光灯，按如下所述计算光线方向：

L<sub>dir</sub> = norm(V \* Lₚ)，其中参数在下表中进行定义。

| 参数       | 默认值 | 类型                                          | 描述                                               |
|-----------------|---------------|-----------------------------------------------|-----------------------------------------------------------|
| L<sub>dir</sub> | 不适用           | 3D 矢量（x、y 和 z 浮点值） | 从对象顶点到光线的方向矢量          |
| V               | 不适用           | 3D 矢量（x、y 和 z 浮点值） | 相机空间中的顶点位置                           |
| wvMatrix        | Identity      | 浮点值的 4 x 4 矩阵           | 包含通用和视图转换的复合矩阵 |
| N               | 不适用           | 3D 矢量（x、y 和 z 浮点值） | 顶点法线                                             |
| Lₚ              | 不适用           | 3D 矢量（x、y 和 z 浮点值） | 相机空间中的光线位置                            |
| vMatrix         | Identity      | 浮点值的 4 x 4 矩阵           | 包含视图转换的矩阵                      |

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[照明的数学运算](mathematics-of-lighting.md)

 

 





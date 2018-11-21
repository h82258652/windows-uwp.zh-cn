---
title: 转换概述
description: 矩阵转换将处理 3D 图形的大量低级数学运算。
ms.assetid: B5220EE8-2533-4B55-BF58-A3F9F612B977
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 32f55b0a387221b792e37072f129edddf285195b
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "7442726"
---
# <a name="transform-overview"></a>转换概述


矩阵转换将处理 3D 图形的大量低级数学运算。

几何图形管道会将顶点用作输入。 转换引擎将对顶点应用世界转换、视图转换和投影转换，剪裁结果并将所有内容传递到光栅器。

| 转换和空间                           | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|-----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 模型空间中的模型坐标              | 在管道的前端，相对于本地坐标系声明模型的顶点。 这是本地源和方向。 此坐标方向通常称为*模型空间*。 各个坐标称为*模型坐标*。                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 到世界空间的世界转换              | 几何图形管道的第一个阶段会将模型的顶点从其本地坐标系转换到由场景中的所有对象使用的坐标系。 重定向顶点的过程称为[世界转换](world-transform.md)，此过程从模型空间转换到称为*世界空间*的新方向。 世界空间中的每个顶点均使用*世界坐标*进行声明。                                                                                                                                                                                                                                                                                                                           |
| 到视图空间（相机空间）的视图转换 | 在下一个阶段，相对于相机确定描述你的 3D 世界的顶点的方向。 也就是说，你的应用程序选择场景的视角，重定位世界空间坐标并使其围绕相机视图旋转，将世界空间转换为*视图空间*（也称为*相机空间*）。 这是[视图转换](view-transform.md)，它将世界空间转换为视图空间。                                                                                                                                                                                                                                                                                                                        |
| 到投影空间的投影转换    | 下一个阶段是[投影转换](projection-transform.md)，它从视图空间转换为投影空间。 在管道的此部分中，通常会根据对象与查看器的距离来按比例缩放对象，以便为场景提供深度错觉，即近距离对象看上去比远距离对象大。 为简单起见，本文档将投影转换后顶点所在的空间称为*投影空间*。 在一些图形书籍中，可能会将投影空间称为*后透视齐性空间*。 并非所有投影转换都会按比例缩放场景中对象的大小。 此类投影有时称为*仿射*或*正交投影*。 |
| 屏幕空间中的剪裁                      | 在管道的最后一个部分中，将删除屏幕上所有不可见的顶点，这样一来，光栅器便无需花时间计算不可见项的颜色和着色。 此过程称为*剪裁*。 在剪裁后，剩余顶点将根据视口参数进行缩放并转换为屏幕坐标。 生成的顶点（光栅化场景时可在屏幕上看到）存在于*屏幕空间*中。                                                                                                                                                                                                                                                    |

 

转换用于将对象几何图形从一个坐标空间转换到另一个坐标空间。 Direct3D 使用矩阵执行 3D 转换。 矩阵创建 3D 转换。 你可组合矩阵来生成一个包含多个转换的矩阵。

你可在模型空间、世界空间和视图空间之间转换坐标。

-   [世界转换](world-transform.md) - 将模型空间转换为世界空间。
-   [视图转换](view-transform.md) - 从世界空间转换为视图空间。
-   [投影转换](projection-transform.md) - 将视图空间转换为投影空间。

## <a name="span-idmatrixtransformsspanspan-idmatrixtransformsspanspan-idmatrixtransformsspanmatrix-transforms"></a><span id="Matrix_Transforms"></span><span id="matrix_transforms"></span><span id="MATRIX_TRANSFORMS"></span>矩阵转换


在处理 3D 图形的应用程序中，你可使用几何转换来执行以下操作：

-   表示一个对象相对于另一个对象的位置。
-   旋转对象并设置对象的大小。
-   更改查看位置、方向和角度。

你可使用 4x4 矩阵将任意点 (x,y,z) 转换为另一个点 (x', y', z')，如以下等式所示。

![将任意点转换为另一个点的等式](images/matmult.png)

执行 (x, y, z) 与矩阵的等式以生成点 (x', y', z')。

![新点的等式](images/matexpnd.png)

最常见的转换是转换、旋转和缩放。 你可将产生这些效果的矩阵组合到一个矩阵中来一次性计算多个转换。 例如，你可生成一个矩阵来转换和旋转一系列点。

矩阵是按行-列顺序编写的。 均匀缩放每个轴上的顶点（称为“统一缩放”）的矩阵由使用数学符号的以下矩阵表示。

![用于统一缩放的矩阵的等式](images/matrix.png)

在 C++ 中，Direct3D 使用矩阵结构将矩阵声明为二维数组。 以下示例说明如何初始化 [**D3DMATRIX**](https://msdn.microsoft.com/library/windows/desktop/bb172573) 结构以充当统一缩放矩阵（比例系数“s”）。

```
D3DMATRIX scale = {
    5.0f,            0.0f,            0.0f,            0.0f,
    0.0f,            5.0f,            0.0f,            0.0f,
    0.0f,            0.0f,            5.0f,            0.0f,
    0.0f,            0.0f,            0.0f,            1.0f
};
```

## <a name="span-idtranslatespanspan-idtranslatespanspan-idtranslatespantranslate"></a><span id="Translate"></span><span id="translate"></span><span id="TRANSLATE"></span>转换


以下等式将点 (x, y, z) 转换为新点 (x', y', z')。

![新点的转换矩阵的等式](images/transl8.png)

你可在 C++ 中手动创建转换矩阵。 以下示例显示了创建用于转换顶点的矩阵的函数的源代码。

```
D3DXMATRIX Translate(const float dx, const float dy, const float dz) {
    D3DXMATRIX ret;

    D3DXMatrixIdentity(&ret);
    ret(3, 0) = dx;
    ret(3, 1) = dy;
    ret(3, 2) = dz;
    return ret;
}    // End of Translate
```

## <a name="span-idscalespanspan-idscalespanspan-idscalespanscale"></a><span id="Scale"></span><span id="scale"></span><span id="SCALE"></span>缩放


以下等式按 x 轴、y 轴和 z 轴方向的任意值将点 (x, y, z) 缩放到新点 (x', y', z')。

![新点的缩放矩阵的等式](images/matscale.png)

## <a name="span-idrotatespanspan-idrotatespanspan-idrotatespanrotate"></a><span id="Rotate"></span><span id="rotate"></span><span id="ROTATE"></span>旋转


此处描述的转换是针对左手坐标系的，因此可能不同于你在其他地方看到的转换矩阵。

以下等式围绕 x 轴旋转点 (x, y, z)，从而产生新点 (x', y', z')。

![新点的 x 旋转矩阵的等式](images/matxrot.png)

以下等式围绕 y 轴旋转此点。

![新点的 y 旋转矩阵的等式](images/matyrot.png)

以下等式围绕 z 轴旋转点。

![新点的 z 旋转矩阵的等式](images/matzrot.png)

在这些示例矩阵中，希腊字母 θ 标识旋转角度（以弧度为单位）。 在顺着面向原点的旋转轴观察时，角度是按顺时针方向测量的。

以下代码演示用于处理围绕 X 轴的旋转的函数。

```
    // Inputs are a pointer to a matrix (pOut) and an angle in radians.
    float sin, cos;
    sincosf(angle, &sin, &cos);  // Determine sin and cos of angle

    pOut->_11 = 1.0f; pOut->_12 =  0.0f;   pOut->_13 = 0.0f; pOut->_14 = 0.0f;
    pOut->_21 = 0.0f; pOut->_22 =  cos;    pOut->_23 = sin;  pOut->_24 = 0.0f;
    pOut->_31 = 0.0f; pOut->_32 = -sin;    pOut->_33 = cos;  pOut->_34 = 0.0f;
    pOut->_41 = 0.0f; pOut->_42 =  0.0f;   pOut->_43 = 0.0f; pOut->_44 = 1.0f;

    return pOut;
}
```

## <a name="span-idconcatenatingmatricesspanspan-idconcatenatingmatricesspanspan-idconcatenatingmatricesspanconcatenating-matrices"></a><span id="Concatenating_Matrices"></span><span id="concatenating_matrices"></span><span id="CONCATENATING_MATRICES"></span>连接矩阵


使用矩阵的一个优势在于，你可通过将两个或两个以上的矩阵相乘来组合其效果。 这意味着，要旋转模型并将其转换到某个位置，你无需应用两个矩阵。 相反，你将旋转矩阵和转换矩阵相乘以产生包含其所有效果的复合矩阵。 此过程称为矩阵连接，可使用以下等式进行编写。

![矩阵连接的等式](images/matrxcat.png)

在此等式中，C 是要创建的复合矩阵，而 M₁ 到 Mₙ 是独立的矩阵。 在大多数情况下，仅连接两个或三个矩阵，但没有限制。

进行矩阵乘法运算的顺序很关键。 上述等式反映矩阵连接的从左到右规则。 也就是会说，你用于创建复合矩阵的矩阵的可见效果是按从左到右的顺序出现的。 以下示例中显示了典型的世界矩阵。 假设你要为常规飞碟创建世界矩阵。 你可能需要围绕飞碟中心（模型空间的 y 轴）旋转飞碟，并将它转换到场景中的某个其他位置。 要完成此效果，您先创建一个旋转矩阵，然后将它与转换矩阵相乘，如以下等式所示。

![基于旋转矩阵和转换矩阵旋转的等式](images/wrldexpl.png)

在此公式中，R<sub>y</sub> 是围绕 Y 轴的旋转的矩阵，T<sub>w</sub> 是到世界坐标中某个位置的转换。

矩阵相乘的顺序很重要，因为与两个标量值相乘不同，矩阵相乘是不可交换的。 按相反顺序将矩阵相乘会产生以下视觉效果：将飞碟转换到其世界空间位置，然后围绕世界原点旋转飞碟。

无论你创建哪种类型的矩阵，请记住从左到右规则以确保实现预期效果。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[转换](transforms.md)

 

 





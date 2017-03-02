---
title: "纹理简介"
description: "纹理资源是存储纹素的数据结构，纹素是可以读取或写入的纹理的最小单位。 在着色器读取纹理时，可以通过纹理采样器对纹理进行过滤。"
ms.assetid: 6F3C76A8-F762-4296-AE02-BFBD6476A5A8
keywords:
- "纹理简介"
author: mtoepke
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ccdca1396673b0eed9ef74c5f0ce3a6f97e02b6d
ms.lasthandoff: 02/07/2017

---

# <a name="introduction-to-textures"></a>纹理简介


纹理资源是存储纹素的数据结构，纹素是可以读取或写入的纹理的最小单位。 在着色器读取纹理时，可以通过纹理采样器对纹理进行过滤。

纹理资源是设计用于存储纹素的结构化数据集合。 纹素表示可以被管道读取或写入的纹理的最小单位。 纹理不同于缓冲区，在着色器单元读取纹理时，它可以被纹理采样器过滤。 纹理的类型会影响到纹理的过滤方式。 每个纹素包含 1 到 4 个部分，它们按 DXGI\_FORMAT 枚举定义的 DXGI 格式之一排列。

纹理被创建成具有已知大小的结构化资源。 但是，只要在将纹理绑定到管道时使用视图完全指定类型，每个纹理资源在创建时有无类型均可。

## <a name="span-idtexturetypesspanspan-idtexturetypesspanspan-idtexturetypesspantexture-types"></a><span id="Texture_Types"></span><span id="texture_types"></span><span id="TEXTURE_TYPES"></span>纹理类型


Direct3D 支持多种浮点表示。 所有浮点计算都依照 IEEE 754 32 位单精度浮点规则的既定子集运行。

纹理有以下几种类型：1D、2D、3D，它们中的每一种都可以在有或没有 mipmap 的情况下创建。 Direct3D 还支持纹理数组和多重采样纹理。

-   [1D 纹理](#texture1d-resource)
-   [1D 纹理数组](#texture1d-array-resource)
-   [2D 纹理和 2D 纹理数组](#texture2d-resource)
-   [3D 纹理](#texture3d-resource)

### <a name="span-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1d-resourcespan1d-textures"></a><span id="Texture1D_Resource"></span><span id="texture1d_resource"></span><span id="TEXTURE1D_RESOURCE"></span><span id="texture1d-resource"></span>1D 纹理

最简单的 1D 纹理包含可以用单个纹理坐标寻址的纹理数据；它可以可视化为纹素数组，如下图所示。

下图显示了一个 1D 纹理：

![一个 1D 纹理](images/d3d10-1d-texture.png)

每个纹素包含多个颜色分量，具体视存储的数据格式而定。 再复杂一些的话，您可以创建具有 mipmap 级别的 1D 纹理，如下图所示。

![具有 mipmap 级别的 1D 纹理](images/d3d10-resource-texture1d.png)

mipmap 级别是一个比它上面的级别小二次幂的纹理。 最上面的级别包含的细节最多，下面的每个级别逐渐减少。 对于 1D mipmap，最小的级别包含一个纹素。 此外，MIP 级别总是减小到 1:1。

在为奇数大小的纹理生成 mipmap 时，下一个较低级别始终为偶数大小（最低级别达到 1 时除外）。 例如，该图显示了一个 5x1 的纹理，其下一个较低级别为 2x1 纹理，再下一个（也就是最后一个）mip 级别为 1x1 大小的纹理。 这些级别通过称作 LOD（细节级别）的索引进行标识，用于在渲染离摄像机较远的几何图形时访问较小的纹理。

### <a name="span-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1d-array-resourcespan1d-texture-arrays"></a><span id="Texture1D_Array_Resource"></span><span id="texture1d_array_resource"></span><span id="TEXTURE1D_ARRAY_RESOURCE"></span><span id="texture1d-array-resource"></span>1D 纹理数组

Direct3D 还支持纹理数组。 1D 纹理数组在概念上类似下图所示。

![1D 纹理数组](images/d3d10-resource-texture1darray.png)

此纹理数组包含三个纹理。 三个纹理都是宽度为 5（第一层中的元素数量）的纹理。 每个纹理还包含一个 3 层 mipmap。

Direct3D 中的所有纹理数组都是同类纹理数组；这意味着纹理数组中的每个纹理必须具有相同的数据格式和大小（包括纹理宽度和 mipmap 级别的数量）。 你可以创建具有不同大小的纹理数组，只要每个数组中的所有纹理大小相同即可。

### <a name="span-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2d-resourcespan2d-textures-and-2d-texture-arrays"></a><span id="Texture2D_Resource"></span><span id="texture2d_resource"></span><span id="TEXTURE2D_RESOURCE"></span><span id="texture2d-resource"></span>2D 纹理和 2D 纹理数组

Texture2D 资源包含 2D 纹素网格。 每个纹素可以通过 u、v 矢量进行寻址。 由于是纹理资源，它可能包含 mipmap 级别和子资源。 完全填充的 2D 纹理资源如下图所示。

![2D 纹理资源](images/d3d10-resource-texture2d.png)

此纹理资源包含具有三个 mipmap 级别的单个 3x5 纹理。

2D 纹理数组资源是一个同类 2D 纹理数组；也就是说，每个纹理具有相同的数据格式和维度（包括 mipmap 级别）。 它具有与 1D 纹理数组相似的布局，只不过纹理现在包含的是 2D 数据，如下图所示。

![2D 纹理数组](images/d3d10-resource-texture2darray.png)

此纹理数组包含三个纹理；每个纹理是具有两个 mipmap 级别的 3x5 纹理。

### <a name="span-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanusing-a-2d-texture-array-as-a-texture-cube"></a><span id="Texture2DArray_Resource_as_a_Texture_Cube"></span><span id="texture2darray_resource_as_a_texture_cube"></span><span id="TEXTURE2DARRAY_RESOURCE_AS_A_TEXTURE_CUBE"></span>使用 2D 纹理数组作为立方纹理

立方纹理是一个包含 6 个纹理的 2D 纹理数组，其中的每个纹理代表立方体的一个面。 完全填充的立方纹理如下图所示。

![代表立方纹理的 2D 纹理数组](images/d3d10-resource-texturecube.png)

使用立方体纹理视图将包含 6 个纹理的 2D 纹理数组绑定到管道后，可以使用立方体贴图内部函数在着色器中读取它。 立方纹理使用从立方纹理的中心指向外侧的 3D 矢量从着色器寻址。

### <a name="span-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3d-resourcespan3d-textures"></a><span id="Texture3D_Resource"></span><span id="texture3d_resource"></span><span id="TEXTURE3D_RESOURCE"></span><span id="texture3d-resource"></span>3D 纹理

3D 纹理资源（也称作体积纹理）包含纹素的 3D 体积。 由于是纹理资源，它可能包含 mipmap 级别。 完全填充的 3D 纹理如下图所示。

![3D 纹理资源](images/d3d10-resource-texture3d.png)

当 3D 纹理 mipmap 片被绑定为渲染目标输出（具有渲染目标视图）时，3D 纹理的行为与具有 n 个片的 2D 纹理数组相同。 从几何着色器阶段中选择特定的渲染片。

没有 3D 纹理数组的概念；因此 3D 纹理子资源是单个 mipmap 级别。

Direct3D 的坐标系是为像素和纹素定义的。

## <a name="span-idpixelspanspan-idpixelspanspan-idpixelspanpixel-coordinate-system"></a><span id="Pixel"></span><span id="pixel"></span><span id="PIXEL"></span>像素坐标系


Direct3D 中的像素坐标系将渲染目标的原点定义在左上角处，如下图所示。 像素中心从整数位置偏移 (0.5f,0.5f)。

![Direct3D 10 中像素坐标系的示意图](images/d3d10-coordspix10.png)

## <a name="span-idtexelspanspan-idtexelspanspan-idtexelspantexel-coordinate-system"></a><span id="Texel"></span><span id="texel"></span><span id="TEXEL"></span>纹素坐标系


纹素坐标系的原点位于纹理的左上角处，如下图所示。 这使得渲染屏幕对齐纹理变得不再重要，因为像素坐标系与纹素坐标系是对齐的。

![纹素坐标系的示意图](images/d3d10-coordstex10.png)

纹理坐标用标准化数或比例尺数表示；每个纹理坐标被映射到特定的纹素，如下所述：

对于标准化坐标：

-   点采样：纹素 \# = floor(U \* 宽度)
-   线性采样：左纹素 \# = floor(U \* 宽度)，右纹素 \# = 左纹素 \# + 1

对于比例尺坐标：

-   点采样：纹素 \# = floor(U)
-   线性采样：左纹素 \# = floor(U - 0.5)，右纹素 \# = 左纹素 \# + 1

其中的宽度是指纹理的宽度（单位为纹素）。

纹理地址包装在计算纹素位置后进行。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[纹理](textures.md)


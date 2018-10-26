---
title: BC6H 格式
description: BC6H 格式是一种纹理压缩格式，设计用于支持源数据中的高动态范围 (HDR) 颜色空间。
ms.assetid: 6781D967-9262-4EE7-B354-7A6D0EA0498E
keywords:
- BC6H 格式
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: be88f06cd5893f2f67697a54754826440bdf7d18
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5563862"
---
# <a name="bc6h-format"></a>BC6H 格式


BC6H 格式是一种纹理压缩格式，设计用于支持源数据中的高动态范围 (HDR) 颜色空间。

## <a name="span-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanabout-bc6hdxgiformatbc6h"></a><span id="About-BC6H-DXGI-FORMAT-BC6H"></span><span id="about-bc6h-dxgi-format-bc6h"></span><span id="ABOUT-BC6H-DXGI-FORMAT-BC6H"></span>关于 BC6H/DXGI\_FORMAT\_BC6H


BC6H 格式为使用三个 HDR 颜色通道的图像提供高质量的压缩，对于该值 (16:16:16) 的每个颜色通道，使用一个 16 位值。 不存在任何对 alpha 通道的支持。

BC6H 由以下 DXGI \ _FORMAT 枚举值指定：

-   **DXGI\_FORMAT\_BC6H\_TYPELESS**。
-   **DXGI\_FORMAT\_BC6H\_UF16**。 此 BC6H 格式不使用 16 位浮点颜色通道值中的符号位。
-   **DXGI\_FORMAT\_BC6H\_SF16**。 此 BC6H 格式使用 16 位浮点颜色通道值中的符号位。

**注意** 16 位浮点颜色通道格式通常称为"半"浮点格式。 此格式具有以下位布局：
|                       |                                                 |
|-----------------------|-------------------------------------------------|
| UF16（无符号浮点） | 5 个指数位 + 11 个尾数位              |
| SF16（带符号浮点）   | 1 个符号位 + 5 个指数位 + 10 个尾数位 |

 

 

BC6H 格式可用于 [Texture2D](https://msdn.microsoft.com/library/windows/desktop/bb205277)（包括数组）、Texture3D 或 TextureCube（包括阵列）纹理资源。 同样，此格式适用于与这些资源有关的任何 MIP 贴图表面。

BC6H 使用 16 字节（128 位）的固定块大小和 4×4 纹素的固定磁贴大小。 与先前的 BC 格式一样，大于支持的磁贴大小 (4x4) 的纹理图像是通过使用多个块来压缩的。 此寻址标识也适用于三维图像、MIP 贴图、立方体贴图和纹理阵列。 所有图像磁贴必须具有相同的格式。

BC6H 格式的几点注意事项：

-   BC6H 支持浮点非规范化，但不支持 INF（无穷大）和 NaN（非数字）。 例外是 BC6H 的带符号模式 (DXGI\_FORMAT\_BC6H\_SF16)，它支持 -INF（负无穷大）。 这种对 -INF 的支持只是格式本身的一个项目，并且并不受这种格式的编码器专门支持。 通常，当编码器遇到 INF（正或负）或 NaN 输入数据时，编码器应该将该数据转换为最大允许的非 INF 表示值，并且在压缩之前将 NaN 映射为 0。
-   BC6H 不支持 alpha 通道。
-   BC6H 解码器在执行纹理筛选之前执行解压缩。
-   BC6H 解压缩必须是位精度的；也就是说，硬件必须返回与本文档中所述的解码器相同的结果。

## <a name="span-idbc6h-implementationspanspan-idbc6h-implementationspanspan-idbc6h-implementationspanbc6h-implementation"></a><span id="BC6H-implementation"></span><span id="bc6h-implementation"></span><span id="BC6H-IMPLEMENTATION"></span>BC6H 实现


BC6H 块由模式位、压缩终结点、压缩索引和可选分区索引组成。 此格式指定 14 种不同的模式。

终结点颜色被存储为 RGB 三元色。 BC6H 在跨越多个定义的颜色终结点的近似线上定义颜色调色板。 此外，根据模式，可以将磁贴划分为两个区域或者视为单个区域，其中两区域磁贴的每个区域有一组单独的颜色终结点。 BC6H 为每个纹理存储一个调色板索引。

在两区域情形下，会存在 32 个可能的分区。

## <a name="span-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspandecoding-the-bc6h-format"></a><span id="Decoding-the-BC6H-format"></span><span id="decoding-the-bc6h-format"></span><span id="DECODING-THE-BC6H-FORMAT"></span>解码 BC6H 格式


下面的伪代码显示了在给定 16 字节 BC6H 块的情况下在 (x,y) 处解压缩像素的步骤。

``` syntax
decompress_bc6h(x, y, block)
{
    mode = extract_mode(block);
    endpoints;
    index;
    
    if(mode.type == ONE)
    {
        endpoints = extract_compressed_endpoints(mode, block);
        index = extract_index_ONE(x, y, block);
    }
    else //mode.type == TWO
    {
        partition = extract_partition(block);
        region = get_region(partition, x, y);
        endpoints = extract_compressed_endpoints(mode, region, block);
        index = extract_index_TWO(x, y, partition, block);
    }
    
    unquantize(endpoints);
    color = interpolate(index, endpoints);
    finish_unquantize(color);
}
```

下表包含 BC6H 块 14 种可能格式中每一种的位计数和值。

| 模式 | 分区索引 | Partition | 颜色终结点                  | 模式位      |
|------|-------------------|-----------|----------------------------------|----------------|
| 1    | 46 位           | 5 位    | 75 位 (10.555, 10.555, 10.555) | 2 位 (00)    |
| 2    | 46 位           | 5 位    | 75 位 (7666, 7666, 7666)       | 2 位 (01)    |
| 3    | 46 位           | 5 位    | 72 位 (11.555, 11.444, 11.444) | 5 位 (00010) |
| 4    | 46 位           | 5 位    | 72 位 (11.444, 11.555, 11.444) | 5 位 (00110) |
| 5    | 46 位           | 5 位    | 72 位 (11.444, 11.444, 11.555) | 5 位 (01010) |
| 6    | 46 位           | 5 位    | 72 位 (9555, 9555, 9555)       | 5 位 (01110) |
| 7    | 46 位           | 5 位    | 72 位 (8666, 8555, 8555)       | 5 位 (10010) |
| 8    | 46 位           | 5 位    | 72 位 (8555, 8666, 8555)       | 5 位 (10110) |
| 9    | 46 位           | 5 位    | 72 位 (8555, 8555, 8666)       | 5 位 (11010) |
| 10   | 46 位           | 5 位    | 72 位 (6666, 6666, 6666)       | 5 位 (11110) |
| 11   | 63 位           | 0 位    | 60 位 (10.10, 10.10, 10.10)    | 5 位 (00011) |
| 12   | 63 位           | 0 位    | 60 位 (11.9, 11.9, 11.9)       | 5 位 (00111) |
| 13   | 63 位           | 0 位    | 60 位 (12.8, 12.8, 12.8)       | 5 位 (01011) |
| 14   | 63 位           | 0 位    | 60 位 (16.4, 16.4, 16.4)       | 5 位 (01111) |

 

此表中的每个格式可以由模式位唯一地进行标识。 前十个模式用于两区域磁贴，而模式位字段的长度可以是两位或五位。 这些块还有用于压缩颜色终结点（72 位或 75 位）、分区（5 位）和分区索引（46 位）的字段。

对于压缩的颜色终结点，上表中的值会记录存储的 RGB 终结点的精度，以及用于每个颜色值的位数。 例如，模式 3 指定颜色终结点精度级别 11，和用于存储红色、蓝色和绿色的已转换终结点的增量值的位数（分别为 5、4 和 4）。 模式 10 不使用增量压缩，而是显式地存储全部四个颜色终结点。

最后四个块模式用于单区域磁贴，其中模式字段为 5 位。 这些块具有用于终结点（60 位）和压缩索引（63 位）的字段。 模式 11（与模式 10 类似）不使用增量压缩，而是显式地存储全部两个颜色终结点。

模式 10011、10111、11011 和 11111（不显示）会被保留。 请勿在你的编码器中使用这些模式。 如果硬件是使用指定的这些模式之一的被传递的块，那么产生的解压缩块必须包含除 alpha 通道之外的所有通道中的所有零。

对于 BC6H，不管模式如何，alpha 通道必须始终返回 1.0。

### <a name="span-idbc6h-partition-setspanspan-idbc6h-partition-setspanspan-idbc6h-partition-setspanbc6h-partition-set"></a><span id="BC6H-partition-set"></span><span id="bc6h-partition-set"></span><span id="BC6H-PARTITION-SET"></span>BC6H 分区集

一个两区域磁贴存在 32 个可能的分区集，这些分区集在下表中进行了定义。 每个 4 x 4 块代表一个形状。

![bc6h 分区集表](images/bc6h-partition-sets.png)

在此分区集表中，粗体和带下划线的条目是子集 1（使用一个较少位指定）的固定索引的位置。 子集 0 的固定索引始终为索引 0，因为始终会对分区进行排列，使得索引 0 始终处于子集 0 中。 分区顺序是从左上到右下，从左到右，然后从上到下。

## <a name="span-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanbc6h-compressed-endpoint-format"></a><span id="BC6H-compressed-endpoint-format"></span><span id="bc6h-compressed-endpoint-format"></span><span id="BC6H-COMPRESSED-ENDPOINT-FORMAT"></span>BC6H 压缩终结点格式


![bc6h 压缩终结点格式的位字段](images/bc6h-headers-med.png)

此表将压缩终结点的位字段显示为终结点格式的一个函数，其中每一列指定一个编码，而每一行指定一个位字段。 对于两区域磁贴，此方法占用 82 位，而对于单区域磁贴，此方法占用 65 位。 比如，上面的单区域 \[16 4\] 编码的前 5 位（特别是最右边的列）是位 m\[4:0\]，接下来的 10 位是 rw\[9:0\]，以此类推，最后 6 位包含 bw\[10:15\]。

上表中的字段名称定义如下：

| 字段 | 变量          |
|-------|-------------------|
| m     | mode              |
| d     | 形状索引       |
| rw    | endpt\[0\].A\[0\] |
| rx    | endpt\[0\].B\[0\] |
| ry    | endpt\[1\].A\[0\] |
| rz    | endpt\[1\].B\[0\] |
| gw    | endpt\[0\].A\[1\] |
| gx    | endpt\[0\].B\[1\] |
| gy    | endpt\[1\].A\[1\] |
| gz    | endpt\[1\].B\[1\] |
| bw    | endpt\[0\].A\[2\] |
| bx    | endpt\[0\].B\[2\] |
| by    | endpt\[1\].A\[2\] |
| bz    | endpt\[1\].B\[2\] |

 

Endpt\[i\]，其中 i 要么是 0 要么是 1，分别指第 0 个或第 1 个终结点集。
## <a name="span-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspansign-extension-for-endpoint-values"></a><span id="Sign-extension-for-endpoint-values"></span><span id="sign-extension-for-endpoint-values"></span><span id="SIGN-EXTENSION-FOR-ENDPOINT-VALUES"></span>终结点值的符号扩展


对于两区域磁贴，存在四个可以进行符号扩展的终结点值。 仅当该格式为带符号格式时，Endpt\[0\].A 才是带符号的；仅当该终结点被转换或该格式为带符号格式时，其他终结点才是带符号的。 下面的代码演示了用于扩展两区域终结点值的符号的算法。

``` syntax
static void sign_extend_two_region(Pattern &p, IntEndpts endpts[NREGIONS_TWO])
{
    for (int i=0; i<NCHANNELS; ++i)
    {
      if (BC6H::FORMAT == SIGNED_F16)
        endpts[0].A[i] = SIGN_EXTEND(endpts[0].A[i], p.chan[i].prec);
      if (p.transformed || BC6H::FORMAT == SIGNED_F16)
      {
        endpts[0].B[i] = SIGN_EXTEND(endpts[0].B[i], p.chan[i].delta[0]);
        endpts[1].A[i] = SIGN_EXTEND(endpts[1].A[i], p.chan[i].delta[1]);
        endpts[1].B[i] = SIGN_EXTEND(endpts[1].B[i], p.chan[i].delta[2]);
      }
    }
}
```

对于单区域磁贴，同样也是如此，只是 endpt\[1\] 被移除。

``` syntax
static void sign_extend_one_region(Pattern &p, IntEndpts endpts[NREGIONS_ONE])
{
    for (int i=0; i<NCHANNELS; ++i)
    {
    if (BC6H::FORMAT == SIGNED_F16)
        endpts[0].A[i] = SIGN_EXTEND(endpts[0].A[i], p.chan[i].prec);
    if (p.transformed || BC6H::FORMAT == SIGNED_F16) 
        endpts[0].B[i] = SIGN_EXTEND(endpts[0].B[i], p.chan[i].delta[0]);
    }
}
```

## <a name="span-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspantransform-inversion-for-endpoint-values"></a><span id="Transform-inversion-for-endpoint-values"></span><span id="transform-inversion-for-endpoint-values"></span><span id="TRANSFORM-INVERSION-FOR-ENDPOINT-VALUES"></span>终结点值的转换反转


对于两区域磁贴，转换会应用差分编码的反转，将 endpt\[0\].A 处的基值添加到三个其他条目以获得总计 9 个添加操作。 在下图中，基值表示为“A0”，并且具有最高的浮点精度。 “A1”、“B0”和“B1”都是从定位点值计算出的增量，并且这些增量值是用较低的精度表示的。 （A0 与 endpt\[0\].A 相对应，B0 与 endpt\[0\].B 相对应，A1 与 endpt\[1\].A 相对应，B1 与 endpt\[1\].B 相对应。）

![转换反转终结点值的计算](images/bc6h-transform-inverse.png)

对于单区域磁贴，仅存在一个增量偏移，因此只有 3 个添加操作。

解压缩器必须确保逆转换的结果不会溢出 endpt\[0\].a 的精度。 在溢出的情况下，从逆转换得到的值必须包含在相同的位数内。 如果 A0 的精度是“p”位，则转换算法为：

`B0 = (B0 + A0) & ((1 << p) - 1)`

对于带符号格式，增量计算的结果也必须进行符号扩展。 如果符号扩展操作考虑扩展全部两个符号，其中 0 是正的，1 是负的，那么 0 的符号扩展会处理上面的固定。 同样，在上述固定之后，只有 1（负）的一个值需要进行符号扩展。

## <a name="span-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanunquantization-of-color-endpoints"></a><span id="Unquantization-of-color-endpoints"></span><span id="unquantization-of-color-endpoints"></span><span id="UNQUANTIZATION-OF-COLOR-ENDPOINTS"></span>颜色终结点的非量化


对于未压缩的终结点，下一步是执行颜色终结点的初始非量化。 这包括以下三个步骤：

-   调色板的非量化
-   调色板的内插
-   非量化终止

与调色板内插之前的完全非量化过程相比，将该非量化过程分离成两部分（内插之前的调色板非量化和内插之后的最终非量化）会减少所需的乘法运算次数。

下面的代码说明了检索原始 16 位颜色值的估计值，然后使用提供的权重值将 6 个附加颜色值添加到调色板的过程。 在每个通道上执行相同的操作。

``` syntax
int aWeight3[] = {0, 9, 18, 27, 37, 46, 55, 64};
int aWeight4[] = {0, 4, 9, 13, 17, 21, 26, 30, 34, 38, 43, 47, 51, 55, 60, 64};

// c1, c2: endpoints of a component
void generate_palette_unquantized(UINT8 uNumIndices, int c1, int c2, int prec, UINT16 palette[NINDICES])
{
    int* aWeights;
    if(uNumIndices == 8)
        aWeights = aWeight3;
    else  // uNumIndices == 16
        aWeights = aWeight4;

    int a = unquantize(c1, prec); 
    int b = unquantize(c2, prec);

    // interpolate
    for(int i = 0; i < uNumIndices; ++i)
        palette[i] = finish_unquantize((a * (64 - aWeights[i]) + b * aWeights[i] + 32) >> 6);
}
```

下一个代码示例演示了插值过程，其中包含以下观察结果：

-   由于 **unquantize** 函数（下方）的颜色值的完整范围为 -32768 至 65535，内插器是使用 17 位带符号算术实现的。
-   在内插之后，会将值传递给 **finish\_unquantize** 函数（在本节的第三个示例中介绍），该函数会应用最终缩放。
-   所有硬件解压缩器都需要返回带有这些函数的位精度结果。

``` syntax
int unquantize(int comp, int uBitsPerComp)
{
    int unq, s = 0;
    switch(BC6H::FORMAT)
    {
    case UNSIGNED_F16:
        if(uBitsPerComp >= 15)
            unq = comp;
        else if(comp == 0)
            unq = 0;
        else if(comp == ((1 << uBitsPerComp) - 1))
            unq = 0xFFFF;
        else
            unq = ((comp << 16) + 0x8000) >> uBitsPerComp;
        break;

    case SIGNED_F16:
        if(uBitsPerComp >= 16)
            unq = comp;
        else
        {
            if(comp < 0)
            {
                s = 1;
                comp = -comp;
            }

            if(comp == 0)
                unq = 0;
            else if(comp >= ((1 << (uBitsPerComp - 1)) - 1))
                unq = 0x7FFF;
            else
                unq = ((comp << 15) + 0x4000) >> (uBitsPerComp-1);

            if(s)
                unq = -unq;
        }
        break;
    }
    return unq;
}
```

在调色板内插之后会调用 **finish\_unquantize**。 对于带符号的终结点，**unquantize** 函数会按 31/32 延迟缩放，而对于无符号的终结点，会按 31/64 延迟缩放。 在完成调色板内插之后，为了减少需要的乘法次数，需要此行为来使最终值进入有效半范围 (-0x7BFF ~ 0x7BFF)。 **finish\_unquantize** 会应用最终缩放并返回一个**无符号短整型**值，该值会被重新解释为 **half**。

``` syntax
unsigned short finish_unquantize(int comp)
{
    if(BC6H::FORMAT == UNSIGNED_F16)
    {
        comp = (comp * 31) >> 6;                                         // scale the magnitude by 31/64
        return (unsigned short) comp;
    }
    else // (BC6H::FORMAT == SIGNED_F16)
    {
        comp = (comp < 0) ? -(((-comp) * 31) >> 5) : (comp * 31) >> 5;   // scale the magnitude by 31/32
        int s = 0;
        if(comp < 0)
        {
            s = 0x8000;
            comp = -comp;
        }
        return (unsigned short) (s | comp);
    }
}
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[纹理块压缩](texture-block-compression.md)

 

 





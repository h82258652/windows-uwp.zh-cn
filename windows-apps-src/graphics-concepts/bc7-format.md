---
title: BC7 格式
description: BC7 格式是一种用于对 RGB 和 RGBA 数据进行高质量压缩的纹理压缩格式。
ms.assetid: 788B6E8C-9A1F-45F9-BE49-742285E8D8A6
keywords:
- BC7 格式
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 70380dd0bd07cfe0c81e8339f8606029663b47d4
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5937571"
---
# <a name="bc7-format"></a>BC7 格式


BC7 格式是一种用于对 RGB 和 RGBA 数据进行高质量压缩的纹理压缩格式。

有关 BC7 格式的块模式的信息，请参阅 [BC7 格式模式参考](https://msdn.microsoft.com/library/windows/desktop/hh308954)。

## <a name="span-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanabout-bc7dxgiformatbc7"></a><span id="About-BC7-DXGI-FORMAT-BC7"></span><span id="about-bc7-dxgi-format-bc7"></span><span id="ABOUT-BC7-DXGI-FORMAT-BC7"></span>关于 BC7/DXGI\_FORMAT\_BC7


BC7 由以下 DXGI\_FORMAT 枚举值指定：

-   **DXGI\_FORMAT\_BC7\_TYPELESS**。
-   **DXGI\_FORMAT\_BC7\_UNORM**。
-   **DXGI\_FORMAT\_BC7\_UNORM\_SRGB**。

BC7 格式可用于 [Texture2D](https://msdn.microsoft.com/library/windows/desktop/bb205277)（包括阵列）、Texture3D 或 TextureCube（包括阵列）纹理资源。 同样，此格式适用于与这些资源相关联的任何 MIP 贴图表面。

BC7 使用 16 字节（128 位）的固定块大小和 4×4 纹素的固定磁贴大小。 与先前的 BC 格式一样，大于支持的磁贴大小 (4x4) 的纹理图像会通过使用多个块进行压缩。 此寻址标识也适用于三维图像和 MIP 贴图、立方体贴图和纹理阵列。 所有图像磁贴必须使用相同的格式。

BC7 可对三通道 (RGB) 和四通道 (RGBA) 固定点数据图像进行压缩。 通常，源数据是每颜色分量（通道）8 位，尽管该格式能够以每颜色分量更高位对源数据进行编码。 所有图像磁贴必须使用相同的格式。

BC7 解码器在应用纹理筛选之前执行解压缩。

BC7 解压缩硬件必须是位精确的；也就是说，该硬件返回的结果必须与此文档中所述的解码器返回的结果相同。

## <a name="span-idbc7-implementationspanspan-idbc7-implementationspanspan-idbc7-implementationspanbc7-implementation"></a><span id="BC7-Implementation"></span><span id="bc7-implementation"></span><span id="BC7-IMPLEMENTATION"></span>BC7 实现


BC7 实现可以使用在 16 字节（128 位）块的最低有效位中指定的模式指定 8 种模式之一。 该模式由零或更多位进行编码，使用值 0，后面跟着 1。

BC7 块可以包含多个终结点对。 与某个终结点对对应的索引集可能会称为“子集”。 同样，在某些块模式下，终结点表示是采用可称为“RBGP”的某种形式进行编码的，其中“P”位代表终结点的颜色分量的共享最低有效位。 例如，如果该格式的终结点表示是“RGB 5.5.5.1”，则会将终结点解释为一个 RGB 6.6.6 值，其中 P 位的状态定义每个分量的最低有效位。 同样，对于使用某个 alpha 通道的源数据，如果该格式的表示是“RGBAP 5.5.5.5.1”，那么会将该终结点解释为 RGBA 6.6.6.6。 根据块模式，你可以单独为某个子集的全部两个终结点指定共享最低有效位（每个子集 2 个 P 位），或者为在某个子集的终结点之间共享的终结点指定共享最低有效位（每个子集 1 个 P 位）。

对于未显式编码 alpha 分量的 BC7 块，一个 BC7 块由多个模式位、多个分区位、多个压缩的终结点、多个压缩的索引和一个可选 P 位组成。 在这些块中，终结点有一个仅 RGB 表示，并且对于源数据中的所有纹素，alpha 分量被解码为 1.0。

对于已组合了颜色分量和 alpha 分量的 BC7 块，一个块由多个模式位、多个压缩的终结点、多个压缩的索引以及多个可选分区位和一个 P 位组成。 在这些块中，终结点颜色是用 RGBA 格式表示的，而 alpha 分量值会与颜色分量值一起以内插值替换。

对于具有单独的颜色和 alpha 分量的 BC7 块，一个块由多个模式位、多个旋转位、多个压缩的终结点、多个压缩的索引和一个可选索引选择器位组成。 这些块有一个有效的 RGB 矢量 \[R, G, B\] 和一个单独进行编码的标量 alpha 通道 \[A\]。

下表列出了每个块类型的分量。

| BC7 块包含的项目     | 多个模式位 | 多个旋转位 | 索引选择器位 | 多个分区位 | 多个压缩终结点 | P 位    | 多个压缩的索引 |
|---------------------------|-----------|---------------|--------------------|----------------|----------------------|----------|--------------------|
| 仅颜色分量     | 必需  | 不适用           | 不适用                | 必需       | 必需             | 可选 | 必需           |
| 组合的颜色 + alpha    | 必需  | 不适用           | 不适用                | 可选       | 必需             | 可选 | 必需           |
| 单独的颜色和 alpha | 必需  | 必需      | 可选           | 不适用            | 必需             | 不适用      | 必需           |

 

BC7 在两个终结点之间的近似线上定义一个调色板。 该模式值确定每个块的内插终结点对的数量。 BC7 为每个纹素存储一个调色板索引。

对于与一对终结点对应的每个索引子集，编码器会修复该子集的压缩索引数据的一位的状态。 它完成此操作的方法是：选择一个终结点顺序，该顺序允许指定的“固定”索引的索引将其最高有效位设置为 0，然后就可以丢弃该最高有效位，为每个子集保存一位。 对于仅有一个子集的块模式，固定索引始终为索引 0。

## <a name="span-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspandecoding-the-bc7-format"></a><span id="Decoding-the-BC7-Format"></span><span id="decoding-the-bc7-format"></span><span id="DECODING-THE-BC7-FORMAT"></span>解码 BC7 格式


以下伪代码概述了对于 16 字节 BC7 块，解压缩 (x,y) 处的像素的步骤。

``` syntax
decompress_bc7(x, y, block)
{
    mode = extract_mode(block);
    
    //decode partition data from explicit partition bits
    subset_index = 0;
    num_subsets = 1;
    
    if (mode.type == 0 OR == 1 OR == 2 OR == 3 OR == 7)
    {
        num_subsets = get_num_subsets(mode.type);
        partition_set_id = extract_partition_set_id(mode, block);
        subset_index = get_partition_index(num_subsets, partition_set_id, x, y);
    }
    
    //extract raw, compressed endpoint bits
    UINT8 endpoint_array[num_subsets][4] = extract_endpoints(mode, block);
    
    //decode endpoint color and alpha for each subset
    fully_decode_endpoints(endpoint_array, mode, block);
    
    //endpoints are now complete.
    UINT8 endpoint_start[4] = endpoint_array[2 * subset_index];
    UINT8 endpoint_end[4]   = endpoint_array[2 * subset_index + 1];
        
    //Determine the palette index for this pixel
    alpha_index     = get_alpha_index(block, mode, x, y);
    alpha_bitcount  = get_alpha_bitcount(block, mode);
    color_index     = get_color_index(block, mode, x, y);
    color_bitcount  = get_color_bitcount(block, mode);

    //determine output
    UINT8 output[4];
    output.rgb = interpolate(endpoint_start.rgb, endpoint_end.rgb, color_index, color_bitcount);
    output.a   = interpolate(endpoint_start.a,   endpoint_end.a,   alpha_index, alpha_bitcount);
    
    if (mode.type == 4 OR == 5)
    {
        //Decode the 2 color rotation bits as follows:
        // 00 – Block format is Scalar(A) Vector(RGB) - no swapping
        // 01 – Block format is Scalar(R) Vector(AGB) - swap A and R
        // 10 – Block format is Scalar(G) Vector(RAB) - swap A and G
        // 11 - Block format is Scalar(B) Vector(RGA) - swap A and B
        rotation = extract_rot_bits(mode, block);
        output = swap_channels(output, rotation);
    }
    
}
```

以下伪代码概述了对于 16 字节 BC7 块，为每个子集完全解码终结点颜色分量和 alpha 分量的步骤。

``` syntax
fully_decode_endpoints(endpoint_array, mode, block)
{
    //first handle modes that have P-bits
    if (mode.type == 0 OR == 1 OR == 3 OR == 6 OR == 7)
    {
        for each endpoint i
        {
            //component-wise left-shift
            endpoint_array[i].rgba = endpoint_array[i].rgba << 1;
        }
        
        //if P-bit is shared
        if (mode.type == 1) 
        {
            pbit_zero = extract_pbit_zero(mode, block);
            pbit_one = extract_pbit_one(mode, block);
            
            //rgb component-wise insert pbits
            endpoint_array[0].rgb |= pbit_zero;
            endpoint_array[1].rgb |= pbit_zero;
            endpoint_array[2].rgb |= pbit_one;
            endpoint_array[3].rgb |= pbit_one;  
        }
        else //unique P-bit per endpoint
        {  
            pbit_array = extract_pbit_array(mode, block);
            for each endpoint i
            {
                endpoint_array[i].rgba |= pbit_array[i];
            }
        }
    }

    for each endpoint i
    {
        // Color_component_precision & alpha_component_precision includes pbit
        // left shift endpoint components so that their MSB lies in bit 7
        endpoint_array[i].rgb = endpoint_array[i].rgb << (8 - color_component_precision(mode));
        endpoint_array[i].a = endpoint_array[i].a << (8 - alpha_component_precision(mode));

        // Replicate each component's MSB into the LSBs revealed by the left-shift operation above
        endpoint_array[i].rgb = endpoint_array[i].rgb | (endpoint_array[i].rgb >> color_component_precision(mode));
        endpoint_array[i].a = endpoint_array[i].a | (endpoint_array[i].a >> alpha_component_precision(mode));
    }
        
    //If this mode does not explicitly define the alpha component
    //set alpha equal to 1.0
    if (mode.type == 0 OR == 1 OR == 2 OR == 3)
    {
        for each endpoint i
        {
            endpoint_array[i].a = 255; //i.e. alpha = 1.0f
        }
    }
}
```

要为每个子集生成每个内插分量，请使用以下算法：让“c”成为要生成的分量；让“e0”成为子集的终结点 0 的分量；并让“e1”成为子集的终结点 1 的分量。

``` syntax
UINT16 aWeight2[] = {0, 21, 43, 64};
UINT16 aWeight3[] = {0, 9, 18, 27, 37, 46, 55, 64};
UINT16 aWeight4[] = {0, 4, 9, 13, 17, 21, 26, 30, 34, 38, 43, 47, 51, 55, 60, 64};

UINT8 interpolate(UINT8 e0, UINT8 e1, UINT8 index, UINT8 indexprecision)
{
    if(indexprecision == 2)
        return (UINT8) (((64 - aWeights2[index])*UINT16(e0) + aWeights2[index]*UINT16(e1) + 32) >> 6);
    else if(indexprecision == 3)
        return (UINT8) (((64 - aWeights3[index])*UINT16(e0) + aWeights3[index]*UINT16(e1) + 32) >> 6);
    else // indexprecision == 4
        return (UINT8) (((64 - aWeights4[index])*UINT16(e0) + aWeights4[index]*UINT16(e1) + 32) >> 6);
}
```

以下伪代码说明如何提取颜色分量和 alpha 分量的索引和位计数。 具有单独的颜色和 alpha 的块也有两组索引数据：一组用于矢量通道，一组用于标量通道。 对于模式 4，这些索引具有不同的宽度（2 位或 3 位），并且存在一个一位选择器，该选择器指定矢量或标量数据是否使用 3 位索引。 （提取 alpha 位计数类似于提取颜色位计数，但根据 **idxMode** 位，其行为是相反的。）

``` syntax
bitcount get_color_bitcount(block, mode)
{
    if (mode.type == 0 OR == 1)
        return 3;
    
    if (mode.type == 2 OR == 3 OR == 5 OR == 7)
        return 2;
    
    if (mode.type == 6)
        return 4;
        
    //The only remaining case is Mode 4 with 1-bit index selector
    idxMode = extract_idxMode(block);
    if (idxMode == 0)
        return 2;
    else
        return 3;
}
```

## <a name="span-idbc7-format-mode-referencespanspan-idbc7-format-mode-referencespanspan-idbc7-format-mode-referencespanbc7-format-mode-reference"></a><span id="BC7-format-mode-reference"></span><span id="bc7-format-mode-reference"></span><span id="BC7-FORMAT-MODE-REFERENCE"></span>BC7 格式模式参考


此部分包含一个用于 BC7 纹理压缩格式块的 8 个块模式和位分配的列表。

块内的每个子集的颜色由两个显式终结点颜色和它们之间的一组内插颜色表示。 根据块的索引精度，每个子集可以具有 4 种、8 种或 16 种可能的颜色。

### <a name="span-idmode-0spanspan-idmode-0spanspan-idmode-0spanmode-0"></a><span id="Mode-0"></span><span id="mode-0"></span><span id="MODE-0"></span>模式 0

BC7 模式 0 具有以下特征：

-   仅颜色分量（无 alpha）
-   每个块 3 个子集
-   每个终结点有一个唯一的 P 位的 RGBP 4.4.4.1 终结点
-   3 位索引
-   16 个分区

![模式 0 位布局](images/bc7-mode0.png)

### <a name="span-idmode-1spanspan-idmode-1spanspan-idmode-1spanmode-1"></a><span id="Mode-1"></span><span id="mode-1"></span><span id="MODE-1"></span>模式 1

BC7 模式 1 具有以下特征：

-   仅颜色分量（无 alpha）
-   每个块 2 个子集
-   每个子集有一个共享 P 位的 RGBP 6.6.6.1 终结点）
-   3 位索引
-   64 个分区

![模式 1 位布局](images/bc7-mode1.png)

### <a name="span-idmode-2spanspan-idmode-2spanspan-idmode-2spanmode-2"></a><span id="Mode-2"></span><span id="mode-2"></span><span id="MODE-2"></span>模式 2

BC7 模式 2 具有以下特征：

-   仅颜色分量（无 alpha）
-   每个块 3 个子集
-   RGB 5.5.5 终结点
-   2 位索引
-   64 个分区

![模式 2 位布局](images/bc7-mode2.png)

### <a name="span-idmode-3spanspan-idmode-3spanspan-idmode-3spanmode-3"></a><span id="Mode-3"></span><span id="mode-3"></span><span id="MODE-3"></span>模式 3

BC7 模式 3 具有以下特征：

-   仅颜色分量（无 alpha）
-   每个块 2 个子集
-   每个子集有一个唯一 P 位的 RGBP 7.7.7.1 终结点）
-   2 位索引
-   64 个分区

![模式 3 位布局](images/bc7-mode3.png)

### <a name="span-idmode-4spanspan-idmode-4spanspan-idmode-4spanmode-4"></a><span id="Mode-4"></span><span id="mode-4"></span><span id="MODE-4"></span>模式 4

BC7 模式 4 具有以下特征：

-   具有单独的 alpha 分量的颜色分量
-   每个块 1 个子集
-   RGB 5.5.5 颜色终结点
-   6 位 alpha 终结点
-   16 x 2 位索引
-   16 x 3 位索引
-   2 位分量旋转
-   1 位索引选择器（无论是使用 2 位索引还是 3 位索引）

![模式 4 位布局](images/bc7-mode4.png)

### <a name="span-idmode-5spanspan-idmode-5spanspan-idmode-5spanmode-5"></a><span id="Mode-5"></span><span id="mode-5"></span><span id="MODE-5"></span>模式 5

BC7 模式 5 具有以下特征：

-   具有单独的 alpha 分量的颜色分量
-   每个块 1 个子集
-   RGB 7.7.7 颜色终结点
-   6 位 alpha 终结点
-   16 x 2 位颜色索引
-   16 x 2 位 alpha 索引
-   2 位分量旋转

![模式 5 位布局](images/bc7-mode5.png)

### <a name="span-idmode-6spanspan-idmode-6spanspan-idmode-6spanmode-6"></a><span id="Mode-6"></span><span id="mode-6"></span><span id="MODE-6"></span>模式 6

BC7 模式 6 具有以下特征：

-   组合的颜色和 alpha 分量
-   每个块一个子集
-   RGBAP 7.7.7.7.1 颜色（和 alpha）终结点（每个终结点一个唯一的 P 位）
-   16 x 4 位索引

![模式 6 位布局](images/bc7-mode6.png)

### <a name="span-idmode-7spanspan-idmode-7spanspan-idmode-7spanmode-7"></a><span id="Mode-7"></span><span id="mode-7"></span><span id="MODE-7"></span>模式 7

BC7 模式 7 具有以下特征：

-   组合的颜色和 alpha 分量
-   每个块 2 个子集
-   RGBAP 5.5.5.5.1 颜色（和 alpha）终结点（每个终结点一个唯一的 P 位）
-   2 位索引
-   64 个分区

![模式 7 位布局](images/bc7-mode7.png)

### <a name="span-idremarksspanspan-idremarksspanspan-idremarksspanremarks"></a><span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>注释

模式 8（最低有效字节设置为 0x00）会被保留。 请勿在你的编码器中使用此模式。 如果你将此模式传递给硬件，则会返回初始化为全部零的一个块。

在 BC7 中，你可以使用以下方式之一编码 alpha 分量：

-   未进行显式 alpha 分量编码的块类型。 在这些块中，颜色终结点有一个仅 RGB 编码，并且对于所有纹素，alpha 分量被解码为 1.0。
-   具有组合的颜色和 alpha 分量的块类型。 在这些块中，终结点颜色值是用 RGBA 格式指定的，而 alpha 分量值会与颜色值一起以内插值替换。
-   具有单独的颜色和 alpha 分量的块类型。 在这些块中，颜色和 alpha 值是单独指定的，每个都有它们自己的索引集。 因此，它们有一个有效矢量和一个单独编码的标量通道，其中矢量通常指定颜色通道 \[R, G, B\]，而标量指定 alpha 通道 \[A\]。 为支持此方法，在编码中会提供一个单独的 2 位字段，该字段允许将单独通道编码指定为一个标量值。 因此，块可以具有此 alpha 编码的以下四种不同表示之一（由 2 位字段指示）：
    -   RGB|A：单独的 alpha 通道
    -   AGB|R：单独的“红”色通道
    -   RAB|G：单独的“绿”色通道
    -   RGA|B：单独的“蓝”色通道

    解码器在解码之后将通道顺序重新排序回到 RGBA，这样，内部块格式会对于开发者不可见。 具有单独的颜色和 alpha 分量的块也有两组索引数据：一组用于矢量通道集，一组用于标量通道。 （在模式 4 的情况下，这些索引具有不同的宽度 \[2 或 3 位\]。 模式 4 还包含一个 1 位选择器，该选择器指定矢量或标量通道是否使用 3 位索引。）

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[纹理块压缩](texture-block-compression.md)

 

 





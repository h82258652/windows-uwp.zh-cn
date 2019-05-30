---
title: 块压缩
description: 块压缩是一种有损纹理压缩技术，用于减小纹理大小并减少内存占用，从而提高性能。 块压缩纹理可以比每种颜色 32 位的纹理小。
ms.assetid: 2FAD6BE8-C6E4-4112-AF97-419CD27F7C73
keywords:
- 块压缩
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2ac7f0785894849cffe09cd902f459015f1f7b6b
ms.sourcegitcommit: ea15237291ae3ade0bf22e38bd292c3a23947a03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/30/2019
ms.locfileid: "66377324"
---
# <a name="block-compression"></a>块压缩

块压缩是一种有损纹理压缩技术，用于减小纹理大小并减少内存占用，从而提高性能。 块压缩纹理可以比每种颜色 32 位的纹理小。

块压缩是一种纹理压缩技术，用于减少纹理大小。 与每种颜色 32 位的纹理相比，块压缩纹理最多可以小 75%。 由于块压缩的内存占用量较少，因此使用块压缩时，应用程序的性能通常会提高。

尽管有损，但块压缩运行良好并推荐用于按管道转换并筛选的所有纹理。 由于项目更为明显，因此直接映射到屏幕的纹理（如图标和文本等 UI 元素）对于压缩而言不是很好的选择。

在所有尺寸上创建的块压缩纹理必须是大小 4 的倍数，且不能用作管道的输出。

## <a name="span-idbasicsspanspan-idbasicsspanspan-idbasicsspanhow-block-compression-works"></a><span id="Basics"></span><span id="basics"></span><span id="BASICS"></span>如何阻止压缩的工作原理

块压缩是一种用来减少存储颜色数据所需的内存的技术。 通过将某些颜色按其原始大小存储，而其他颜色使用编码模式，你可以大幅减少存储图像所需的内存。 由于硬件会自动解码压缩的数据，因此使用压缩的纹理不会导致性能下降。

若要了解压缩的工作原理，请看下面两个示例。 第一个示例介绍存储未压缩的数据所用的内存量；第二个示例介绍存储压缩的数据所用的内存量。

- [存储未压缩的数据](#storing-uncompressed-data)
- [存储压缩的数据](#storing-compressed-data)

### <a name="span-idstoringuncompresseddataspanspan-idstoringuncompresseddataspanspan-idstoringuncompresseddataspanspan-idstoring-uncompressed-dataspanstoring-uncompressed-data"></a><span id="Storing_Uncompressed_Data"></span><span id="storing_uncompressed_data"></span><span id="STORING_UNCOMPRESSED_DATA"></span><span id="storing-uncompressed-data"></span>存储未压缩的数据

下图显示了未压缩的 4×4 纹理。 假设每种颜色包含一个单色分量（例如红色），并且存储在一字节的内存中。

![未压缩的 4x4 纹理](images/d3d10-block-compress-1.png)

未压缩的数据在内存中按顺序排列且需要 16 字节，如下图所示。

![顺序内存中的未压缩数据](images/d3d10-block-compress-2.png)

### <a name="span-idstoringcompresseddataspanspan-idstoringcompresseddataspanspan-idstoringcompresseddataspanspan-idstoring-compressed-dataspanstoring-compressed-data"></a><span id="Storing_Compressed_Data"></span><span id="storing_compressed_data"></span><span id="STORING_COMPRESSED_DATA"></span><span id="storing-compressed-data"></span>存储压缩的数据

你已经了解了未压缩的图像使用的内存量，现在来看看压缩的图像节省的内存量。 [BC1](#bc1) 压缩格式存储 2 种颜色（每种颜色 1 字节）和用于在纹理中内插原始颜色的 16 个 3 位指数（48 位，或者 6 字节），如下图中所示。

![bc1 压缩格式](images/d3d10-block-compress-3.png)

存储压缩数据所需的空间总量为 8 字节，与未压缩的示例相比，可节省 50% 的内存量。 如果使用多个颜色分量，节省的内存量会更多。

块压缩节省的大量内存可以让性能提升。 此性能是以牺牲图像质量（因颜色内插导致）为代价的；不过，图像质量的降低通常不是很明显。

下一节介绍 Direct3D 如何实现在应用程序中使用块压缩。

## <a name="span-idusingblockcompressionspanspan-idusingblockcompressionspanspan-idusingblockcompressionspanusing-block-compression"></a><span id="Using_Block_Compression"></span><span id="using_block_compression"></span><span id="USING_BLOCK_COMPRESSION"></span>使用块压缩

创建块压缩纹理和未压缩纹理一样，唯一区别是要指定块压缩格式。

接下来，创建一个视图以将纹理绑定到管道。由于块压缩纹理只能用作着色器阶段的输入，所以你要创建着色器资源视图。

请以使用未压缩纹理的方式来使用块压缩纹理。 如果你的应用程序将获取指向块压缩数据的内存指针，你需要考虑 mipmap 中的内存填充，它会导致声明的大小与实际大小不同。

- [与物理大小的虚拟大小](#virtual-size-versus-physical-size)

### <a name="span-idvirtualsizespanspan-idvirtualsizespanspan-idvirtualsizespanspan-idvirtual-size-versus-physical-sizespanvirtual-size-versus-physical-size"></a><span id="Virtual_Size"></span><span id="virtual_size"></span><span id="VIRTUAL_SIZE"></span><span id="virtual-size-versus-physical-size"></span>与物理大小的虚拟大小

如果你的应用程序代码使用内存指针遍历块压缩纹理的内存，则你的应用程序代码中会有一个可能需要修改的重要事项。 因为块压缩算法在 4x4 纹素块上操作，所以所有尺寸中的块压缩纹理必须是 4 的倍数。 对于原始尺寸可以被 4 整除、但细分尺寸无法被 4 整除的 mipmap，这将是个问题。 下图显示了每个 mipmap 级别的虚拟（声明）大小和物理（实际）大小之间的区域差。

![未压缩和压缩的 mipmap 级别](images/d3d10-block-compress-pad.png)

图示左侧显示为未压缩的 60×40 纹理生成的 mipmap 级别大小。 最高级别的大小取自生成纹理的 API 调用；每个后续级别是上一个级别大小的一半。 对于未压缩的纹理，虚拟（声明）大小和物理（实际）大小之间没有区别。

图示右侧显示了为使用压缩的同一 60×40 纹理生成的 mipmap 级别大小。 注意，第二个和第三个级别均有内存填充，以使每个级别上的大小为 4 的因数。 这很有必要，以便算法可以在 4×4 纹素块上操作。 这就显得如果考虑 mipmap 级别小于 4 × 4;这些非常小的 mipmap 级别的大小将向上舍入到最接近原来的四分之一纹理内存分配时。

采样硬件使用虚拟大小；如果对纹理采样，将忽略内存填充。 对于小于 4×4 的 mipmap 级别，只有前四个纹素将用于 2×2 的映射，并且只有第一个纹素将由 1×1 的块使用。 但是，没有公开物理大小（包括内存填充）的 API 结构。

总之，复制包含块压缩数据的区域时，请小心使用对齐的内存块。 若要在获取内存指针的应用程序中执行此操作，请确保指针使用图面间距以考虑物理内存大小。

## <a name="span-idcompressionalgorithmsspanspan-idcompressionalgorithmsspanspan-idcompressionalgorithmsspancompression-algorithms"></a><span id="Compression_Algorithms"></span><span id="compression_algorithms"></span><span id="COMPRESSION_ALGORITHMS"></span>压缩算法

Direct3D 中的块压缩技术将未压缩的纹理数据分成 4×4 的块，压缩每个块，然后存储数据。 因此，预计要压缩的纹理的纹理尺寸必须是 4 的倍数。

![块压缩](images/d3d10-compression-1.png)

上图显示了一个纹理分成多个纹素块。 第一个块显示了标记为 a-p 的 16 个纹素的布局，但是每个块的数据组织相同。

Direct3D 实施了多个压缩方案，每个方案在存储的分量数、每个分量的位数，以及使用的内存量之间进行了不同的权衡。 使用此表可帮助选择最适合数据类型的格式以及最适合你的应用程序的数据分辨率。

| 源数据                     | 数据压缩分辨率（位) | 选择此压缩格式 |
|---------------------------------|---------------------------------------|--------------------------------|
| 三分量颜色和 alpha | 颜色 (5:6:5)，Alpha (1) 或没有 alpha  | [BC1](#bc1)                    |
| 三分量颜色和 alpha | 颜色 (5:6:5)，Alpha (4)              | [BC2](#bc2)                    |
| 三分量颜色和 alpha | 颜色 (5:6:5)，Alpha (8)              | [BC3](#bc3)                    |
| 单分量颜色             | 单分量 (8)                     | [BC4](#bc4)                    |
| 双分量颜色             | 双分量 (8:8)                  | [BC5](#bc5)                    |

- [BC1](#bc1)
- [BC2](#bc2)
- [BC3](#bc3)
- [BC4](#bc4)
- [BC5](#bc5)

### <a name="span-idbc1spanspan-idbc1spanbc1"></a><span id="BC1"></span><span id="bc1"></span>BC1

使用第一个块压缩格式 (BC1) (任一 DXGI\_格式\_BC1\_TYPELESS、 DXGI\_格式\_BC1\_UNORM 或 DXGI\_BC1\_UNORM\_SRGB) 来存储三个组件颜色数据使用 5:6:5 颜色 （红色的 5 位，绿色的 6 位，蓝色的 5 位）。 即使该数据还包含 1 位 alpha 也是如此。 假设 4×4 纹理使用最大的数据格式，BC1 格式会将所需的内存从 48 内存字节（16 种颜色 × 3 个分量/颜色 × 1 字节/分量）减少到 8 内存字节。

该算法在 4×4 纹素块上操作。 而不是存储 16 种颜色，该算法将保存 2 种引用颜色 (颜色\_0 和颜色\_1） 和 16 2 位颜色索引 (块 a – p)，如以下关系图中所示。

![bc1 压缩的布局](images/d3d10-compression-bc1.png)

颜色指数 (a–p) 用于从颜色表中查找原始颜色。 颜色表包含 4 种颜色。 前两个颜色，颜色\_0 和颜色\_1-最小值和最大颜色。 其他两种颜色，颜色\_2 和颜色\_3，将使用线性内插计算的中间颜色。

```cpp
color_2 = 2/3*color_0 + 1/3*color_1
color_3 = 1/3*color_0 + 2/3*color_1
```

这四种颜色会获分 2 位指数值，指数值将保存在块 a–p 中。

```cpp
color_0 = 00
color_1 = 01
color_2 = 10
color_3 = 11
```

最后，会将块 a–p 中的颜色与颜色表中的这四种颜色进行比较，最接近的颜色的指数将存储在 2 位的块中。

该算法还会将自己借给包含 1 位 alpha 的数据。 唯一的区别是该颜色\_3 设置为 0 （它表示透明的颜色） 和颜色\_2 是线性的颜色混合\_0 和颜色\_1。

```cpp
color_2 = 1/2*color_0 + 1/2*color_1;
color_3 = 0;
```

### <a name="span-idbc2spanspan-idbc2spanbc2"></a><span id="BC2"></span><span id="bc2"></span>BC2

使用 BC2 格式 (任一 DXGI\_格式\_BC2\_TYPELESS、 DXGI\_格式\_BC2\_UNORM 或 DXGI\_BC2\_UNORM\_SRGB) 到包含了颜色和 alpha 数据的低一致性的数据存储 (使用[BC3](#bc3)高度一致的 alpha 数据)。 BC2 格式将 RGB 数据存储为 5:6:5 颜色（5 位红色、6 位绿色、5 位蓝色），将 alpha 存储为单独的 4 位值。 假设 4×4 纹理使用最大的数据格式，该压缩技术会将所需的内存从 64 内存字节（16 种颜色 × 4 分量/颜色 × 1 字节/分量）减少为 16 内存字节。

BC2 格式将具有相同位数和数据布局的颜色存储为 [BC1](#bc1) 格式；但是，BC2 需要额外 64 位内存来存储 alpha 数据，如下图所示。

![bc2 压缩的布局](images/d3d10-compression-bc2.png)

### <a name="span-idbc3spanspan-idbc3spanbc3"></a><span id="BC3"></span><span id="bc3"></span>BC3

使用 BC3 格式 (任一 DXGI\_格式\_BC3\_TYPELESS、 DXGI\_格式\_BC3\_UNORM 或 DXGI\_BC3\_UNORM\_SRGB) 到存储高度一致的颜色数据 (使用[BC2](#bc2)与不连贯的 alpha 数据)。 BC3 格式使用 5:6:5 颜色（5 位红色、6 位绿色、5 位蓝色）存储颜色数据，使用 1 字节存储 alpha 数据。 假设 4×4 纹理使用最大的数据格式，该压缩技术会将所需的内存从 64 内存字节（16 种颜色 × 4 分量/颜色 × 1 字节/分量）减少为 16 内存字节。

BC3 格式将具有相同位数和数据布局的颜色存储为 [BC1](#bc1) 格式；但是，BC3 需要额外的 64 位内存来存储 alpha 数据。 BC3 格式通过存储两个参考值并在它们之间进行内插（类似于 BC1 存储 RGB 颜色的方式）来处理 alpha。

该算法在 4×4 纹素块上操作。 而不是存储 16 alpha 值，该算法将存储 2 个引用 alpha (alpha\_0 和 alpha\_1) 和 16 3 位颜色索引 (alpha 通过 p)，如以下关系图中所示。

![bc3 压缩的布局](images/d3d10-compression-bc3.png)

BC3 格式使用 alpha 指数 (a–p) 从包含 8 个值的查找表中查找原始颜色。 前两个值 — alpha\_0 和 alpha\_1 — 是最小和最大值; 其他六个中间值使用线性插值进行计算。

该算法通过检查两个参考 alpha 值确定内插的 alpha 值的数量。 如果 alpha\_0 大于 alpha\_1，则 BC3 内插 6 alpha 值; 否则，它内插 4。 当 BC3 仅内插 4 个 alpha 值时，它会设置两个额外的 alpha 值（0 表示完全透明，255 表示完全不透明）。 BC3 通过存储与最匹配指定纹理的原始 alpha 的内插 alpha 值对应的位代码，将 alpha 值压缩在 4×4 纹理区域中。

```cpp
if( alpha_0 > alpha_1 )
{
  // 6 interpolated alpha values.
  alpha_2 = 6/7*alpha_0 + 1/7*alpha_1; // bit code 010
  alpha_3 = 5/7*alpha_0 + 2/7*alpha_1; // bit code 011
  alpha_4 = 4/7*alpha_0 + 3/7*alpha_1; // bit code 100
  alpha_5 = 3/7*alpha_0 + 4/7*alpha_1; // bit code 101
  alpha_6 = 2/7*alpha_0 + 5/7*alpha_1; // bit code 110
  alpha_7 = 1/7*alpha_0 + 6/7*alpha_1; // bit code 111
}
else
{
  // 4 interpolated alpha values.
  alpha_2 = 4/5*alpha_0 + 1/5*alpha_1; // bit code 010
  alpha_3 = 3/5*alpha_0 + 2/5*alpha_1; // bit code 011
  alpha_4 = 2/5*alpha_0 + 3/5*alpha_1; // bit code 100
  alpha_5 = 1/5*alpha_0 + 4/5*alpha_1; // bit code 101
  alpha_6 = 0;                         // bit code 110
  alpha_7 = 255;                       // bit code 111
}
```

### <a name="span-idbc4spanspan-idbc4spanbc4"></a><span id="BC4"></span><span id="bc4"></span>BC4

使用 BC4 格式存储单分量颜色数据（每种颜色使用 8 位）。 由于更高的准确性 (相比[BC1](#bc1))，BC4 非常适合用于存储浮点数据的范围内\[0 到 1\]使用 DXGI\_格式\_BC4\_UNORM 格式和\[-1 到 + 1\]使用 DXGI\_格式\_BC4\_SNORM 格式。 假设 4×4 纹理使用最大的数据格式，该压缩技术会将所需的内存从 16 字节（16 种颜色 × 1 分量/颜色 × 1 字节/分量）减少为 8 字节。

该算法在 4×4 纹素块上操作。 而不是存储 16 种颜色，该算法将存储 2 种引用颜色 (红\_为 0，红色\_1) 和 16 3 位颜色索引 (红色通过红色 p)，如以下关系图中所示。

![bc4 压缩的布局](images/d3d10-compression-bc4.png)

该算法使用 3 位指数从包含 8 种颜色的颜色表中查找颜色。 前两个颜色-红色\_为 0，红色\_1-最小值和最大颜色。 该算法使用线性内插计算剩余颜色。

该算法通过检查两个参考值确定内插的颜色值的数量。 如果红色\_0 大于 red\_1，则 BC4 内插 6 颜色值; 否则，它内插 4。 如果 BC4 仅内插 4 个颜色值，它将设置两个额外的颜色值（0.0f 表示完全透明，1.0f 表示完全不透明）。 BC4 通过存储与最匹配指定纹理的原始 alpha 的内插 alpha 值对应的位代码，将 alpha 值压缩在 4×4 纹理区域中。

- [BC4\_UNORM](#bc4-unorm)
- [BC4\_SNORM](#bc4-snorm)

### <a name="span-idbc4unormspanspan-idbc4unormspanspan-idbc4-unormspanbc4unorm"></a><span id="BC4_UNORM"></span><span id="bc4_unorm"></span><span id="bc4-unorm"></span>BC4\_UNORM

以下代码示例显示了单分量数据的内插完成情况。

```cpp
unsigned word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = 0.0f;                     // bit code 110
  red_7 = 1.0f;                     // bit code 111
}
```

参考颜色将会获分 3 位指数（000–111，因为有 8 个值），在压缩过程中将会保存在红色 a 块至红色 p 块中。

### <a name="span-idbc4snormspanspan-idbc4snormspanspan-idbc4-snormspanbc4snorm"></a><span id="BC4_SNORM"></span><span id="bc4_snorm"></span><span id="bc4-snorm"></span>BC4\_SNORM

DXGI\_格式\_BC4\_SNORM 是完全相同，不同之处在于数据编码 SNORM 范围内以及时 4 个颜色值的内插。 以下代码示例显示了单分量数据的内插完成情况。

```cpp
signed word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = -1.0f;                    // bit code 110
  red_7 =  1.0f;                    // bit code 111
}
```

参考颜色将会获分 3 位指数（000–111，因为有 8 个值），在压缩过程中将会保存在红色 a 块至红色 p 块中。

### <a name="span-idbc5spanspan-idbc5spanbc5"></a><span id="BC5"></span><span id="bc5"></span>BC5

使用 BC5 格式存储双分量颜色数据（每种颜色使用 8 位）。 由于更高的准确性 (相比[BC1](#bc1))，BC5 非常适合用于存储浮点数据的范围内\[0 到 1\]使用 DXGI\_格式\_BC5\_UNORM 格式和\[-1 到 + 1\]使用 DXGI\_格式\_BC5\_SNORM 格式。 假设 4×4 纹理使用最大的数据格式，该压缩技术会将所需的内存从 32 字节（16 种颜色 × 2 分量/颜色 × 1 字节/分量）减少为 16 字节。

- [BC5\_UNORM](#bc5-unorm)
- [BC5\_SNORM](#bc5-snorm)

该算法在 4×4 纹素块上操作。 而不是存储 16 种颜色为这两个组件，该算法将存储为每个组件的 2 个引用颜色 (红\_0，红色\_月 1 日，绿色\_为 0，，绿色\_1) 和每个组件的 16 3 位颜色索引 (红色通过 p 红色和绿色到绿色 p)，如以下关系图中所示。

![bc5 压缩的布局](images/d3d10-compression-bc5.png)

该算法使用 3 位指数从包含 8 种颜色的颜色表中查找颜色。 前两个颜色-红色\_为 0，红色\_1 (或绿色\_为 0，绿色\_1)-最小值和最大颜色。 该算法使用线性内插计算剩余颜色。

该算法通过检查两个参考值确定内插的颜色值的数量。 如果红色\_0 大于 red\_1，则 BC5 内插 6 颜色值; 否则，它内插 4。 如果 BC5 仅内插 4 个颜色值，它将设置 0.0f 和 1.0f 上的其余两个颜色值。

### <a name="span-idbc5unormspanspan-idbc5unormspanspan-idbc5-unormspanbc5unorm"></a><span id="BC5_UNORM"></span><span id="bc5_unorm"></span><span id="bc5-unorm"></span>BC5\_UNORM

以下代码示例显示了单分量数据的内插完成情况。 绿色分量的计算类似。

```cpp
unsigned word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = 0.0f;                     // bit code 110
  red_7 = 1.0f;                     // bit code 111
}
```

参考颜色将会获分 3 位指数（000–111，因为有 8 个值），在压缩过程中将会保存在红色 a 块至红色 p 块中。

### <a name="span-idbc5snormspanspan-idbc5snormspanspan-idbc5-snormspanbc5snorm"></a><span id="BC5_SNORM"></span><span id="bc5_snorm"></span><span id="bc5-snorm"></span>BC5\_SNORM

DXGI\_格式\_BC5\_SNORM 是完全相同，只不过对数据编码 SNORM 范围内，并且时 4 个数据值的内插，两个其他的值为-1.0 f 和 1.0f。 以下代码示例显示了单分量数据的内插完成情况。 绿色分量的计算类似。

```cpp
signed word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = -1.0f;                    // bit code 110
  red_7 =  1.0f;                    // bit code 111
}
```

参考颜色将会获分 3 位指数（000–111，因为有 8 个值），在压缩过程中将会保存在红色 a 块至红色 p 块中。

## <a name="span-iddifferencesspanspan-iddifferencesspanspan-iddifferencesspanformat-conversion"></a><span id="Differences"></span><span id="differences"></span><span id="DIFFERENCES"></span>格式转换

Direct3D 支持在预先构建的特型纹理和具有相同位宽的块压缩纹理之间进行复制。

你可以在几种格式类型之间复制资源。 此类复制操作执行一种将资源数据重新解释为不同格式类型的格式转换。 看看下面的示例，它介绍了采用更典型的转换的行为类型重新解释数据之间的差异：

```cpp
FLOAT32 f = 1.0f;
UINT32 u;
```

若要将“f”重新解释为类型“u”，请使用 [memcpy](https://docs.microsoft.com/cpp/c-runtime-library/reference/memcpy-wmemcpy)：

```cpp
memcpy( &u, &f, sizeof( f ) ); // 'u' becomes equal to 0x3F800000.
```

在上一重新解释中，数据的基础值不变；[memcpy](https://docs.microsoft.com/cpp/c-runtime-library/reference/memcpy-wmemcpy) 将浮点数重新解释为无符号整数。

若要执行更典型的转换类型，请使用分配：

```cpp
u = f; // 'u' becomes 1.
```

在上一转换中，数据的基础值发生变化。

下表列出了你可以在这种重新解释类型的格式转换中使用的允许源和目标格式。 你必须对值进行正确编码，重新解释才能按预期运行。

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">位宽</th>
<th align="left">未压缩的资源</th>
<th align="left">块压缩资源</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">32</td>
<td align="left"><p>DXGI_FORMAT_R32_UINT</p>
<p>DXGI_FORMAT_R32_SINT</p></td>
<td align="left">DXGI_FORMAT_R9G9B9E5_SHAREDEXP</td>
</tr>
<tr class="even">
<td align="left">64</td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UINT</p>
<p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<p>DXGI_FORMAT_R32G32_UINT</p>
<p>DXGI_FORMAT_R32G32_SINT</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC4_UNORM</p>
<p>DXGI_FORMAT_BC4_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left">128</td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_UINT</p>
<p>DXGI_FORMAT_R32G32B32A32_SINT</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC3_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC5_UNORM</p>
<p>DXGI_FORMAT_BC5_SNORM</p></td>
</tr>
</tbody>
</table>

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题

[压缩的纹理资源](compressed-texture-resources.md)

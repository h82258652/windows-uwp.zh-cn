---
title: 不透明和 1 位 alpha 纹理
description: 纹理格式 BC1 用于不透明或透明色为一种的纹理。
ms.assetid: 8C53ACDD-72ED-4307-B4F3-2FCF9A9F53EC
keywords:
- 不透明和 1 位 alpha 纹理
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 672d7b2ddc913ea3a966fbd0a095367521a27d7c
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5868391"
---
# <a name="span-iddirect3dconceptsopaqueand1-bitalphatexturesspanopaque-and-1-bit-alpha-textures"></a><span id="direct3dconcepts.opaque_and_1-bit_alpha_textures"></span>不透明和 1 位 alpha 纹理


纹理格式 BC1 用于不透明或透明色为一种的纹理。

对于每个不透明或 1 位 Alpha 块，存储两个 16 位值（RGB 5:6:5 格式）和每像素 2 位的 4x4 位图。 总数为：16 个纹素为 64 位，每个纹素为 4 位。 在块位图中，每个纹素有 2 位，以便在四种颜色（其中两种以编码数据存储）中作出选择。 其他两种颜色通过线性内插从这些存储的颜色中派生。 布局如下图所示。

![位图布局示意图](images/colors1.png)

通过比较存储在块中的两个 16 位颜色值，可以将 1 位的 Alpha 格式与不透明格式区分开。 它们被视为无符号整数。 如果第一种颜色大于第二种，则意味着只定义了不透明纹素。 这意味着四种颜色都用于表示纹素。 在四色编码中，存在两种派生颜色，所有四种颜色均匀分布在 RGB 颜色空间中。 此格式类似于 RGB 5:6:5 格式。 否则，对于 1 位 Alpha 透明度，使用三种颜色，保留第四种颜色以表示透明纹素。

在三色编码中，存在一种派生颜色，保留第四个 2 位代码以指示透明纹素（Alpha 信息）。 此格式类似于 RGBA 5:5:5:1，其中的最后一位用于对 Alpha 掩码进行编码。

下面的代码示例演示了用于确定选择三色编码还是四色编码的算法：

```
if (color_0 > color_1) 
{
    // Four-color block: derive the other two colors. 
    
    // 00 = color_0, 01 = color_1, 10 = color_2, 11 = color_3
    // These 2-bit codes correspond to the 2-bit fields 
    // stored in the 64-bit block.
    color_2 = (2 * color_0 + color_1 + 1) / 3;
    color_3 = (color_0 + 2 * color_1 + 1) / 3;
}    
else
{ 
    // Three-color block: derive the other color.
    // 00 = color_0,  01 = color_1,  10 = color_2,  
    // 11 = transparent.
    // These 2-bit codes correspond to the 2-bit fields 
    // stored in the 64-bit block. 
    color_2 = (color_0 + color_1) / 2;    
    color_3 = transparent;    

}
```

建议在混合前将透明度像素的 RGBA 分量设为零。

以下表格显示了 8 字节块的内存布局。 假设第一个索引对应 y 坐标，第二个索引对应 x 坐标。 例如，Texel\[1\]\[2\] 指的是位于 (x,y) = (2,1) 处的纹理贴图像素。

以下是 8 字节（64 位）块的内存布局：

| 字地址 | 16 位字    |
|--------------|----------------|
| 0            | Color\_0       |
| 1            | Color\_1       |
| 2            | Bitmap Word\_0 |
| 3            | Bitmap Word\_1 |

 

Color\_0 和 Color\_1 是处于两个极值的颜色，其布局如下：

| 位        | 颜色                 |
|-------------|-----------------------|
| 4:0 (LSB\*) | 蓝色分量  |
| 10:5        | 绿色分量 |
| 15:11       | 红色分量   |

 

\*最低有效位

位图 Word\_0 的布局如下：

| 位          | 纹素           |
|---------------|-----------------|
| 1:0 (LSB)     | Texel\[0\]\[0\] |
| 3:2           | Texel\[0\]\[1\] |
| 5:4           | Texel\[0\]\[2\] |
| 7:6           | Texel\[0\]\[3\] |
| 9:8           | Texel\[1\]\[0\] |
| 11:10         | Texel\[1\]\[1\] |
| 13:12         | Texel\[1\]\[2\] |
| 15:14 (MSB\*) | Texel\[1\]\[3\] |

 

\*最高有效位 (MSB)

位图 Word\_1 的布局如下：

| 位        | 纹素           |
|-------------|-----------------|
| 1:0 (LSB)   | Texel\[2\]\[0\] |
| 3:2         | Texel\[2\]\[1\] |
| 5:4         | Texel\[2\]\[2\] |
| 7:6         | Texel\[2\]\[3\] |
| 9:8         | Texel\[3\]\[0\] |
| 11:10       | Texel\[3\]\[1\] |
| 13:12       | Texel\[3\]\[2\] |
| 15:14 (MSB) | Texel\[3\]\[3\] |

 

## <a name="span-idexampleofopaquecolorencodingspanspan-idexampleofopaquecolorencodingspanspan-idexampleofopaquecolorencodingspanexample-of-opaque-color-encoding"></a><span id="Example_of_Opaque_Color_Encoding"></span><span id="example_of_opaque_color_encoding"></span><span id="EXAMPLE_OF_OPAQUE_COLOR_ENCODING"></span>不透明颜色编码示例


作为不透明编码的示例，假设红色和黑色处于极值。 红色为 color\_0，黑色为 color\_1。 有四种内插颜色形成在它们之间均匀分布的梯度。 要确定 4x4 位图的值，请使用以下方式计算：

```
00 ? color_0
01 ? color_1
10 ? 2/3 color_0 + 1/3 color_1
11 ? 1/3 color_0 + 2/3 color_1
```

位图如下图所示。

![扩展位图布局示意图](images/colors2.png)

这看起来像下面说明的一系列颜色。

**注意**在图像中，像素 (0，0) 显示在左上角。

 

![不透明编码梯度示意图](images/redsquares.png)

## <a name="span-idexampleof1bitalphaencodingspanspan-idexampleof1bitalphaencodingspanspan-idexampleof1bitalphaencodingspanexample-of-1-bit-alpha-encoding"></a><span id="Example_of_1_Bit_Alpha_Encoding"></span><span id="example_of_1_bit_alpha_encoding"></span><span id="EXAMPLE_OF_1_BIT_ALPHA_ENCODING"></span>1 位 Alpha 编码示例


当无符号 16 位整数 (color\_0) 小于无符号 16 位整数 (color\_1) 时，将选择此格式。 此格式可用于显示位于蓝天背景下的树上的树叶。 部分纹素可以标记为透明，三种绿色色调仍用于树叶。 两种颜色确定极值，第三种为内插颜色。

下图是此类图片的示例。

![1 位 Alpha 编码示意图](images/greenthing.png)

图像显示为白色，纹素将被编码成透明。 透明纹素的 RGBA 分量在混合前应设为零。

颜色和透明度的位图编码使用以下计算方法确定。

```
00 ? color_0
01 ? color_1
10 ? 1/2 color_0 + 1/2 color_1
11   ?   Transparent
```

位图如下图所示。

![扩展位图布局示意图](images/colors3.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[压缩的纹理资源](compressed-texture-resources.md)

 

 





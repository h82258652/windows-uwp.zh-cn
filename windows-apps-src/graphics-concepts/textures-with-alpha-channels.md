---
title: 使用 alpha 通道的纹理
description: 有两种方式对展示更复杂的透明度的纹理映射进行编码。
ms.assetid: 768A774A-4F21-4DDE-B863-14211DA92926
keywords:
- 使用 alpha 通道的纹理
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 88d150383d2be219e7f382e0e690771acbc9d2ee
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "8328548"
---
# <a name="textures-with-alpha-channels"></a>使用 alpha 通道的纹理


有两种方法可用于对展现更复杂透明度的纹理贴图进行编码。 在每种情况下，描述透明度的块都在已描述的 64 位块之前。 透明度以 4x4 位图表示，每个像素 4 位（显示编码）或者具有更少位和线性内插（与用于颜色编码的类似）。

透明数据块和颜色块的排列方式如下表所示。

| Word 地址 | 64 位块                      |
|--------------|-----------------------------------|
| 3:0          | 透明数据块                |
| 7:4          | 之前所述的 64 位块 |

 

## <a name="span-idexplicit-texture-encodingspanspan-idexplicit-texture-encodingspanspan-idexplicit-texture-encodingspanexplicit-texture-encoding"></a><span id="Explicit-Texture-Encoding"></span><span id="explicit-texture-encoding"></span><span id="EXPLICIT-TEXTURE-ENCODING"></span>显式纹理编码


对于显式纹理编码（BC2 格式），描述透明度的纹素的 alpha 组件采用 4x4 位图编码，每个像素为 4 位。 这些四位组可以通过多种方式实现，比如抖动或通过使用四个最高有效位的 alpha 数据。 但它们只能产生并且按本身用途使用，不得有任何形式的内插。

以下图表显示 64 位透明数据块。

![64 位透明数据块图示](images/colors4.png)

**注意**的 Direct3D 压缩法使用四个最高有效位。

 

下表描述了 alpha 信息（16 位词）如何在内存中分布。

word 0 的布局：

| 位          | Alpha      |
|---------------|------------|
| 3:0 (LSB\*)   | \[0\]\[0\] |
| 7:4           | \[0\]\[1\] |
| 11:8          | \[0\]\[2\] |
| 15:12 (MSB\*) | \[0\]\[3\] |

 

\*最低有效位，最高有效位 (MSB)

word 1 的布局：

| 位        | Alpha      |
|-------------|------------|
| 3:0 (LSB)   | \[1\]\[0\] |
| 7:4         | \[1\]\[1\] |
| 11:8        | \[1\]\[2\] |
| 15:12 (MSB) | \[1\]\[3\] |

 

word 2 的布局：

| 位        | Alpha      |
|-------------|------------|
| 3:0 (LSB)   | \[2\]\[0\] |
| 7:4         | \[2\]\[1\] |
| 11:8        | \[2\]\[2\] |
| 15:12 (MSB) | \[2\]\[3\] |

 

word 3 的布局：

| 位        | Alpha      |
|-------------|------------|
| 3:0 (LSB)   | \[3\]\[0\] |
| 7:4         | \[3\]\[1\] |
| 11:8        | \[3\]\[2\] |
| 15:12 (MSB) | \[3\]\[3\] |

 

在 BC1 中用于确定纹素是否透明的颜色对比并没有使用这种格式。 假定在没有颜色对比的情况下，始终将颜色数据处理成 4 颜色模式。

## <a name="span-idthree-bit-linear-alpha-interpolationspanspan-idthree-bit-linear-alpha-interpolationspanspan-idthree-bit-linear-alpha-interpolationspanthree-bit-linear-alpha-interpolation"></a><span id="Three-Bit-Linear-Alpha-Interpolation"></span><span id="three-bit-linear-alpha-interpolation"></span><span id="THREE-BIT-LINEAR-ALPHA-INTERPOLATION"></span>三位线性 alpha 内插


BC3 格式的透明度编码概念与针对颜色的线性编码的相似。 两个 8 位 alpha 值以及一个 4x4 位图（每个像素为 3 位）保存在块的前八个字节中。 典型的 alpha 值用于插入中间 alpha 值。 其他信息则在两个 alpha 值保存的过程中提供。 如果 alpha\_0 大于 alpha\_1，则六个中间 alpha 值可以通过插入的形式创建。 或者，四个中间 alpha 值可以插入指定的 alpha 极值之间。 其他两个隐式 alpha 值分别为 0（完全透明）以及 255（完全不透明）。

以下代码示例描述了此算法。

```
// 8-alpha or 6-alpha block?    
if (alpha_0 > alpha_1) {    
    // 8-alpha block:  derive the other six alphas.    
    // Bit code 000 = alpha_0, 001 = alpha_1, others are interpolated.
    alpha_2 = (6 * alpha_0 + 1 * alpha_1 + 3) / 7;    // bit code 010
    alpha_3 = (5 * alpha_0 + 2 * alpha_1 + 3) / 7;    // bit code 011
    alpha_4 = (4 * alpha_0 + 3 * alpha_1 + 3) / 7;    // bit code 100
    alpha_5 = (3 * alpha_0 + 4 * alpha_1 + 3) / 7;    // bit code 101
    alpha_6 = (2 * alpha_0 + 5 * alpha_1 + 3) / 7;    // bit code 110
    alpha_7 = (1 * alpha_0 + 6 * alpha_1 + 3) / 7;    // bit code 111  
}    
else {  
    // 6-alpha block.    
    // Bit code 000 = alpha_0, 001 = alpha_1, others are interpolated.
    alpha_2 = (4 * alpha_0 + 1 * alpha_1 + 2) / 5;    // Bit code 010
    alpha_3 = (3 * alpha_0 + 2 * alpha_1 + 2) / 5;    // Bit code 011
    alpha_4 = (2 * alpha_0 + 3 * alpha_1 + 2) / 5;    // Bit code 100
    alpha_5 = (1 * alpha_0 + 4 * alpha_1 + 2) / 5;    // Bit code 101
    alpha_6 = 0;                                      // Bit code 110
    alpha_7 = 255;                                    // Bit code 111
}
```

alpha 块的内存布局如下所示：

| 字节 | Alpha                                                          |
|------|----------------------------------------------------------------|
| 0    | Alpha\_0                                                       |
| 1    | Alpha\_1                                                       |
| 2    | \[0\]\[2\]（2 个 MSB）、\[0\]\[1\]、\[0\]\[0\]                    |
| 3    | \[1\]\[1\] (1 MSB)、\[1\]\[0\]、\[0\]\[3\]、\[0\]\[2\]（1 个 LSB） |
| 4    | \[1\]\[3\]、\[1\]\[2\]、\[1\]\[1\]（2 个 LSB）                    |
| 5    | \[2\]\[2\]（2 个 MSB）、\[2\]\[1\]、\[2\]\[0\]                    |
| 6    | \[3\]\[1\]（1 个 MSB）、\[3\]\[0\]、\[2\]\[3\]、\[2\]\[2\]（1 个 LSB） |
| 7    | \[3\]\[3\]、\[3\]\[2\]、\[3\]\[1\]（2 个 LSB）                    |

 

在 BC1 中用于确定纹素是否透明的颜色对比并没有使用这些格式。 假定在没有颜色对比的情况下，始终将颜色数据处理成 4 颜色模式。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[压缩的纹理资源](compressed-texture-resources.md)

 

 





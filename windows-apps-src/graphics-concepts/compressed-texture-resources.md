---
title: 压缩的纹理资源
description: 纹理贴图是指在三维图形上绘制以添加视觉细节的数字化图像。
ms.assetid: 2DD5FF94-A029-4694-B103-26946C8DFBC1
keywords:
- 压缩的纹理资源
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e808bf0fe1f521a60aa347efd148ede96be95964
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6187735"
---
# <a name="compressed-texture-resources"></a>压缩的纹理资源


纹理贴图是指在三维图形上绘制以添加视觉细节的数字化图像。 它们在光栅化期间被贴图到这些形状，而此过程会占用大量系统总线和内存。 为了降低纹理占用的内存量，Direct3D 支持压缩纹理曲面。 某些 Direct3D 设备支持本机压缩纹理曲面。 在这些设备上，当创建压缩曲面并载入数据后，此曲面可像任何其他纹理曲面一样在 Direct3D 中使用。 纹理被贴图到 3D 对象时，Direct3D 将处理解压缩。

## <a name="span-idstorage-efficiency-and-texture-compressionspanspan-idstorage-efficiency-and-texture-compressionspanspan-idstorage-efficiency-and-texture-compressionspanstorage-efficiency-and-texture-compression"></a><span id="Storage-Efficiency-and-Texture-Compression"></span><span id="storage-efficiency-and-texture-compression"></span><span id="STORAGE-EFFICIENCY-AND-TEXTURE-COMPRESSION"></span>存储效率和纹理压缩


所有纹理压缩格式都是二的幂。 虽然这并不意味着纹理一定是平方数，但它意味着 x 和 y 都是二的幂。 例如，如果纹理最初为 512 x 128 字节，下一个 Mip 贴图将为 256 x 64，依此类推，每个级别降低二次方。 在较低级别时，纹理被筛选为 16 x 2 和 8 x 1，由于压缩块始终为 4 x 4 纹素块，因此会出现浪费的位。 块中未使用部分将被填充。

尽管最低级别有浪费的位，但是整体增益仍然可观。 从理论上说，最糟糕的情况是 2K x 1 纹理（2⁰ 幂）。 此处，仅对每个块的单行像素进行编码，而未使用块的剩余部分。

## <a name="span-idmixing-formats-within-a-single-texturespanspan-idmixing-formats-within-a-single-texturespanspan-idmixing-formats-within-a-single-texturespanmixing-formats-within-a-single-texture"></a><span id="Mixing-Formats-Within-a-Single-Texture"></span><span id="mixing-formats-within-a-single-texture"></span><span id="MIXING-FORMATS-WITHIN-A-SINGLE-TEXTURE"></span>混合单个纹理中的格式


任何一个纹理必须指定其数据存储为 64 位还是 128 位（每个 16 纹素组）。 如果纹理使用 64 位块（即块压缩格式 BC1），则可以对同一纹理中的块混合使用透明度和 1 位 alpha 格式。 换言之，分别为每个 16 纹素块比较 color\_0 和 color\_1 的无符号整数度量值。

如果使用 128 位块，必须在显式（格式 BC2）或内插（格式 BC3）模式下为整个纹理指定 alpha 通道。 与颜色一样，选择内插模式（格式 BC3）时，块可以使用八个内插 alpha 模式或六个内插 alpha 模式。 同样，分别为每个块比较 alpha\_0 和 alpha\_1 的度量值。

Direct3D 提供了压缩用于对 3D 模型进行纹理处理的曲面的服务。 此部分提供了有关如何在压缩纹理曲面中创建和操作数据的信息。

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>本节内容


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="opaque-and-1-bit-alpha-textures.md">不透明和 1 位 alpha 纹理</a></p></td>
<td align="left"><p>纹理格式 BC1 适用于不透明或具有一种透明色的纹理。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="textures-with-alpha-channels.md">使用 alpha 通道的纹理</a></p></td>
<td align="left"><p>有两种方法可用于对展现更复杂透明度的纹理贴图进行编码。 在每种情况下，描述透明度的块都在已描述的 64 位块之前。 透明度以 4 x 4 位图表示，其中每个像素具有 4 位（显式编码）或者具有更少位和线性插值（与用于颜色编码的值类似）。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="block-compression.md">块压缩</a></p></td>
<td align="left"><p>块压缩是一种有损纹理压缩技术，用于减小纹理大小并减少内存占用，从而提高性能。 块压缩纹理可以比每种颜色 32 位的纹理小。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="compressed-texture-formats.md">压缩的纹理格式</a></p></td>
<td align="left"><p>此部分包含有关压缩纹理格式的内部组织的信息。 使用压缩的纹理时无需这些详细信息，因为你可以使用 Direct3D 功能转换为压缩格式和从压缩格式转换。 但是，如果你想要直接处理压缩的曲面数据，则此信息很有用。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[纹理](textures.md)

 

 





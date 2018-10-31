---
title: 纹理块压缩
description: Direct3D 11 中的纹理块压缩 (BC) 支持已经过扩展，以包含 BC6H 和 BC7 算法。
ms.assetid: 63506C46-BF14-464B-B20C-8B8F359E7AFE
keywords:
- 纹理块压缩
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 46c58da3dbe425b055855423aa9e9cebaa06f929
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5836979"
---
# <a name="texture-block-compression"></a>纹理块压缩


Direct3D 11 中的纹理块压缩 (BC) 支持已经过扩展，以包含 BC6H 和 BC7 算法。 BC6H 支持高动态范围颜色源数据，而 BC7 以较少的标准 RGB 源数据项目提供优于平均质量的压缩。

有关 Direct3D 11 之前纹理块压缩算法支持的更多详细信息（包括针对 BC1 至 BC5 格式的支持），请参见[纹理块压缩 (Direct3D 10)](https://msdn.microsoft.com/library/windows/desktop/bb694531)。

**有关文件格式的注释：** BC6H 和 BC7 纹理压缩格式可用于存储纹理压缩的数据的 DDS 文件格式。 要了解更多信息，请参见 [DDS 编程指南](https://msdn.microsoft.com/library/windows/desktop/bb943991)。

## <a name="span-idblockcompressionformatssupportedindirect3d11spanspan-idblockcompressionformatssupportedindirect3d11spanspan-idblockcompressionformatssupportedindirect3d11spanblock-compression-formats-supported-in-direct3d-11"></a><span id="Block_Compression_Formats_Supported_in_Direct3D_11"></span><span id="block_compression_formats_supported_in_direct3d_11"></span><span id="BLOCK_COMPRESSION_FORMATS_SUPPORTED_IN_DIRECT3D_11"></span>Direct3D 11 中支持的纹理块压缩格式


| 源数据                                  | 数据压缩分辨率最低要求                              | 推荐格式 | 支持的最低功能级别 |
|----------------------------------------------|---------------------------------------------------------------------------|--------------------|---------------------------------|
| alpha 通道可选三个通道颜色       | 三种颜色的通道（5 位：6 位：5 位），alpha 通道为 0 或 1 位  | BC1                | Direct3D 9.1                    |
| alpha 通道可选三个通道颜色       | 三种颜色的通道（5 位：6 位：5 位），alpha 通道为 4 位         | BC2                | Direct3D 9.1                    |
| alpha 通道可选三个通道颜色       | 三种颜色的通道（5 位：6 位：5 位），alpha 通道为 8 位          | BC3                | Direct3D 9.1                    |
| 单通道的颜色                            | 单色通道（8 位）                                                | BC4                | Direct3D 10                     |
| 两个通道的颜色                            | 双色通道（8 位：8 位）                                        | BC5                | Direct3D 10                     |
| 三个信道高动态范围 (HDR) 颜色 | 三种颜色的通道（16 位：16 位：16 位），采用半精度浮点数格式\* | BC6H               | Direct3D 11                     |
| alpha 通道可选三个通道颜色  | 三种颜色的通道（每个通道为 4 位到 7 位），alpha 通道为 0 至 8 位  | BC7                | Direct3D 11                     |

 

\*半精度浮点数是一个 16 位值，由一个可选符号位、一个 5 位偏置指数以及一个 10 位或 11 位的尾数组成。
## <a name="span-idbc1bc2andb3formatsspanspan-idbc1bc2andb3formatsspanspan-idbc1bc2andb3formatsspanbc1-bc2-and-b3-formats"></a><span id="BC1__BC2__and_B3_Formats"></span><span id="bc1__bc2__and_b3_formats"></span><span id="BC1__BC2__AND_B3_FORMATS"></span>BC1、BC2 以及 B3 格式


BC1、BC2 以及 BC3 格式相当于 Direct3D 9 DXTn 纹理压缩格式，并且与对应的 Direct3D 10 BC1、BC2 以及 BC3 格式相同。 所有功能级别（D3D\_FEATURE\_LEVEL\_9\_1、D3D\_FEATURE\_LEVEL\_9\_2、D3D\_FEATURE\_LEVEL\_9\_3、D3D\_FEATURE\_LEVEL\_10\_0、D3D\_FEATURE\_LEVEL\_10\_1 以及 D3D\_FEATURE\_LEVEL\_11\_0）都必须支持这三种格式。

| 纹理块压缩格式 | DXGI 格式                                                                           | 与 Direct3D 9 相等的格式                               | 每个 4x4 像素块的字节数 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------------------------------------|---------------------------|
| BC1                      | DXGI\_FORMAT\_BC1\_UNORM、DXGI\_FORMAT\_BC1\_UNORM\_SRGB、DXGI\_FORMAT\_BC1\_TYPELESS | D3DFMT\_DXT1、FourCC="DXT1"                                | 8                         |
| BC2                      | DXGI\_FORMAT\_BC2\_UNORM、DXGI\_FORMAT\_BC2\_UNORM\_SRGB、DXGI\_FORMAT\_BC2\_TYPELESS | D3DFMT\_DXT2\*、FourCC="DXT2"、D3DFMT\_DXT3、FourCC="DXT3" | 16                        |
| BC3                      | DXGI\_FORMAT\_BC3\_UNORM、DXGI\_FORMAT\_BC3\_UNORM\_SRGB、DXGI\_FORMAT\_BC3\_TYPELESS | D3DFMT\_DXT4\*、FourCC="DXT4"、D3DFMT\_DXT5、FourCC="DXT5" | 16                        |

 

\*这些压缩方案（DXT2 和 DXT4）并不区分 Direct3D 9 预乘 alpha 格式以及标准 alpha 格式。 在呈现时，不必须使用可编程的着色器进行此区分。

## <a name="span-idbc4andbc5formatsspanspan-idbc4andbc5formatsspanspan-idbc4andbc5formatsspanbc4-and-bc5-formats"></a><span id="BC4_and_BC5_Formats"></span><span id="bc4_and_bc5_formats"></span><span id="BC4_AND_BC5_FORMATS"></span>BC4 和 BC5 格式


| 纹理块压缩格式 | DXGI 格式                                                                     | 与 Direct3D 9 相等的格式 | 每个 4x4 像素块的字节数 |
|--------------------------|---------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC4                      | DXGI\_FORMAT\_BC4\_UNORM、DXGI\_FORMAT\_BC4\_SNORM、DXGI\_FORMAT\_BC4\_TYPELESS | FourCC="ATI1"                | 8                         |
| BC5                      | DXGI\_FORMAT\_BC5\_UNORM、DXGI\_FORMAT\_BC5\_SNORM、DXGI\_FORMAT\_BC5\_TYPELESS | FourCC="ATI2"                | 16                        |

 

## <a name="span-idbc6hformatspanspan-idbc6hformatspanspan-idbc6hformatspanbc6h-format"></a><span id="BC6H_Format"></span><span id="bc6h_format"></span><span id="BC6H_FORMAT"></span>BC6H 格式


如需了解有关此格式的更多详细信息，请参见 [BC6H 格式](https://msdn.microsoft.com/library/windows/desktop/hh308952)文档。

| 纹理块压缩格式 | DXGI 格式                                                                      | 与 Direct3D 9 相等的格式 | 每个 4x4 像素块的字节数 |
|--------------------------|----------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC6H                     | DXGI\_FORMAT\_BC6H\_UF16、DXGI\_FORMAT\_BC6H\_SF16、DXGI\_FORMAT\_BC6H\_TYPELESS | 不适用                          | 16                        |

 

BC6H 格式可以针对每个 4x4 像素块选择不同的编码模式。 总共有 14 种不同的编码模式可选，在每种模式下最终显示的纹理的视觉效果都略有不同。 选择合适的模式有助于硬件快速解码（根据源内容选择或调整质量级别），但这也极大地增加了搜索空间的复杂性。

## <a name="span-idbc7formatspanspan-idbc7formatspanspan-idbc7formatspanbc7-format"></a><span id="BC7_Format"></span><span id="bc7_format"></span><span id="BC7_FORMAT"></span>BC7 格式


如需了解有关此格式的更多详细信息，请参见 [BC7 格式](https://msdn.microsoft.com/library/windows/desktop/hh308953)文档。

| 纹理块压缩格式 | DXGI 格式                                                                           | 与 Direct3D 9 相等的格式 | 每个 4x4 像素块的字节数 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC7                      | DXGI\_FORMAT\_BC7\_UNORM、DXGI\_FORMAT\_BC7\_UNORM\_SRGB、DXGI\_FORMAT\_BC7\_TYPELESS | 不适用                          | 16                        |

 

BC7 格式可以针对每个 4x4 像素块选择不同的编码模式。 总共有 8 种不同的编码模式可选，在每种模式下最终显示的纹理的视觉效果都略有不同。 选择合适的模式有助于硬件快速解码（根据源内容选择或调整质量级别），但这也极大地增加了搜索空间的复杂性。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[附录](appendix.md)

[纹理](https://msdn.microsoft.com/library/windows/desktop/ff476902)

 

 





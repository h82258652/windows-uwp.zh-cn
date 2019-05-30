---
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: 本文列出了可用于 BitmapEncoder 的编码选项。
title: BitmapEncoder 选项参考
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 34e2ac1531b496f076abbe8e23ffa1c0cb318373
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359044"
---
# <a name="bitmapencoder-options-reference"></a>BitmapEncoder 选项参考


本文列出了可用于 [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) 的编码选项。 编码选项由其名称（其名称是一个字符串）和一个采用特定数据类型 ([**Windows.Foundation.PropertyType**](https://docs.microsoft.com/uwp/api/Windows.Foundation.PropertyType)) 的值定义。 有关使用图像的信息，请参阅[创建、编辑和保存位图图像](imaging.md)。

| 名称                    | PropertyType | 使用说明                                                                                        | 有效格式 |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | 单个       | 有效值从 0 到 1.0。 较高的值表示较高的质量                                 | JPEG、JPEG-XR |
| CompressionQuality      | 单个       | 有效值从 0 到 1.0。 较高的值表示较高的效率和较低的压缩模式 | TIFF          |
| 无损                | boolean      | 如果它设置为 true，将忽略 ImageQuality 选项                                        | JPEG-XR       |
| InterlaceOption         | boolean      | 是否隔行扫描图像                                                                    | PNG           |
| FilterOption            | uint8        | 使用 [**PngFilterMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.PngFilterMode) 枚举                                | PNG           |
| TiffCompressionMethod   | uint8        | 使用 [**TiffCompressionMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.TiffCompressionMode) 枚举                    | TIFF          |
| 亮度               | uint32Array  | 包含亮度量化常数的 64 个元素的数组                               | JPEG          |
| 色度             | uint32Array  | 包含色度量化常数的 64 个元素的数组                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | 使用 [**JpegSubsamplingMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.JpegSubsamplingMode) 枚举                    | JPEG          |
| SuppressApp0            | boolean      | 是否取消 App0 元数据块的创建                                        | JPEG          |
| EnableV5Header32bppBGRA | boolean      | 是否编码到支持 alpha 的版本 5 BMP                                         | BMP           |

 

## <a name="related-topics"></a>相关主题

* [创建、编辑和保存位图图像](imaging.md)
* [支持的编解码器](supported-codecs.md)

 





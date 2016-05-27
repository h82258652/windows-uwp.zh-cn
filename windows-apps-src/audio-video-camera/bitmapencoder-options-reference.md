---
author: drewbatgit
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: 本文列出了可用于 BitmapEncoder 的编码选项。
title: BitmapEncoder 选项参考
---

# BitmapEncoder 选项参考

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本文列出了可用于 [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206) 的编码选项。 编码选项由其名称（其名称是一个字符串）和一个采用特定数据类型 ([**Windows.Foundation.PropertyType**](https://msdn.microsoft.com/library/windows/apps/br225871)) 的值定义。 有关处理图像的信息，请参阅[图像处理](imaging.md)。

| 名称                    | PropertyType | 使用说明                                                                                        | 有效格式 |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | 单个       | 有效值从 0 到 1.0。 较高的值表示较高的质量。                                 | JPEG、JPEG-XR |
| CompressionQuality      | 单个       | 有效值从 0 到 1.0。 较高的值表示较高的效率和较低的压缩模式。 | TIFF          |
| 无损                | boolean      | 如果此参数设置为 true，则将忽略 ImageQuality 选项。                                        | JPEG-XR       |
| InterlaceOption         | boolean      | 是否隔行扫描图像。                                                                    | PNG           |
| FilterOption            | uint8        | 使用 [**PngFilterMode**](https://msdn.microsoft.com/library/windows/apps/br226389) 枚举。                                | PNG           |
| TiffCompressionMethod   | uint8        | 使用 [**TiffCompressionMode**](https://msdn.microsoft.com/library/windows/apps/br226399) 枚举。                    | TIFF          |
| 亮度               | uint32Array  | 包含亮度量化常数的 64 个元素的数组。                               | JPEG          |
| 色度             | uint32Array  | 包含色度量化常数的 64 个元素的数组。                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | 使用 [**JpegSubsamplingMode**](https://msdn.microsoft.com/library/windows/apps/br226386) 枚举。                    | JPEG          |
| SuppressApp0            | boolean      | 是否取消 App0 元数据块的创建。                                        | JPEG          |
| EnableV5Header32bppBGRA | boolean      | 是否编码到支持 alpha 的版本 5 BMP。                                         | BMP           |

 

## 相关主题

* [图像处理](imaging.md)
 

 






<!--HONumber=May16_HO2-->



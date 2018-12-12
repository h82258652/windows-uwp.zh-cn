---
title: 流式资源创建参数
description: 可以作为流式资源创建的 Direct3D 资源类型存在某些限制。
ms.assetid: 6FC5AD93-6F47-479E-947C-895C99B427BC
keywords:
- 流式资源创建参数
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1ddb150e570e25af7162a50309b9b0fc30cedf60
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8936044"
---
# <a name="streaming-resource-creation-parameters"></a>流式资源创建参数


可以作为流式资源创建的 Direct3D 资源类型存在某些限制。

<span id="Supported-Resource-Type"></span><span id="supported-resource-type"></span><span id="SUPPORTED-RESOURCE-TYPE"></span>**受支持的资源类型**  
Texture2D\[Array\]（包括 TextureCube\[Array\]，这是 Texture2D\[Array\] 的一种变体）或缓冲区。

**不受支持：** Texture1D\ [Array\]。

<span id="Supported-Resource-Usage"></span><span id="supported-resource-usage"></span><span id="SUPPORTED-RESOURCE-USAGE"></span>**受支持的资源用法**  
默认用法。

**不受支持：** 动态、 暂存或不可变。

<span id="Supported-Resource-Misc-Flags"></span><span id="supported-resource-misc-flags"></span><span id="SUPPORTED-RESOURCE-MISC-FLAGS"></span>**受支持的资源杂项标记**  
平铺式，即流式处理（按照定义）、纹理立方体、绘图间接参数、允许存在原始视图的缓冲区、结构化缓冲区、资源夹或生成 mips。

**不受支持：** 共享、 共享键控互斥体、 可兼容的 GDI 共享 NT 句柄、 限制内容、 限制共享的资源、 限制共享的资源的驱动程序、 受保护，或磁贴池。

<span id="Supported-Bind-Flags"></span><span id="supported-bind-flags"></span><span id="SUPPORTED-BIND-FLAGS"></span>**受支持的绑定标志**  
作为着色器资源、呈现目标、深度模板或无序的访问绑定。

**不受支持：** 绑定作为常量缓冲区、 顶点缓冲区 （绑定平铺的缓冲区作为 SRV/UAV/rtv）、 索引缓冲区、 流输出、 解码器或视频编码器。

<span id="Supported-Formats"></span><span id="supported-formats"></span><span id="SUPPORTED-FORMATS"></span>**受支持的格式**  
除一些例外情况外，所有格式均适用于给定配置（无论是否平铺）。

<span id="Supported-Sample-Description--Multisample-count--quality-"></span><span id="supported-sample-description--multisample-count--quality-"></span><span id="SUPPORTED-SAMPLE-DESCRIPTION--MULTISAMPLE-COUNT--QUALITY-"></span>**受支持的示例说明（多重采样计数、质量）**  
在任何情况下均支持给定配置（无论是否平铺），除一些例外情况外。

<span id="Supported-Width-Height-MipLevels-ArraySize"></span><span id="supported-width-height-miplevels-arraysize"></span><span id="SUPPORTED-WIDTH-HEIGHT-MIPLEVELS-ARRAYSIZE"></span>**受支持的宽度/高度/MIP 级别/数组大小**  
Direct3D 支持的完整范围。 流式资源对非流式资源所应用的总内存大小不设限。 流式资源仅受限于整体虚拟地址空间限制。 请参阅[适用于流式资源的地址空间](address-space-available-for-streaming-resources.md)。

磁贴池内存中的初始内容尚未确定。

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>本节内容


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">说明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="address-space-available-for-streaming-resources.md">可用于流式资源的地址空间</a></p></td>
<td align="left"><p>本部分指定了可用于流式资源的虚拟地址空间。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[创建流式资源](creating-streaming-resources.md)

 

 





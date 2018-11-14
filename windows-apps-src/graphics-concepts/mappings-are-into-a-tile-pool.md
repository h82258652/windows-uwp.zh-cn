---
title: 映射到磁贴池
description: 作为流式资源创建资源时，构成资源的磁贴来自于磁贴池中各位置处的指针。 磁贴池是内存池（由一个或多个分配在后台提供支持 - 对应用程序不可见）。
ms.assetid: 58B8DBD5-62F5-4B94-8DD1-C7D57A812185
keywords:
- 映射到磁贴池
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 24c8787efd108acb2353f6705dbb65a34d358ef2
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2018
ms.locfileid: "6453861"
---
# <a name="mappings-are-into-a-tile-pool"></a>映射到磁贴池


作为流式资源创建资源时，构成资源的磁贴来自于磁贴池中各位置处的指针。 磁贴池是内存池（由一个或多个分配在后台提供支持 - 对应用程序不可见）。 操作系统和显示驱动程序负责管理这个内存池，内存占用很容易被应用程序所理解。 流式资源通过指向磁贴池中的位置来映射 64KB 区域。 此设置有一个副作用：它允许多个资源共享和重复使用相同的磁贴，并允许在资源中的不同位置重复使用相同的磁贴（如果需要）。

为磁贴池以外的资源填充磁贴具有较高的灵活性，但这是有代价的：该资源必须完成定义和维护映射工作，即磁贴池中的哪些磁贴代表该资源所需的磁贴。 可以更改磁贴映射。 此外，不必一次性映射资源中的所有磁贴；资源可以有 **NULL** 映射。 **NULL** 映射定义了从访问磁贴的资源角度来看不可用的磁贴。

可以创建多个磁贴池，可以在同一时间将任意数量的流式资源映射到任意给定磁贴池中。 磁贴池还可以增大或收缩。 有关详细信息，请参阅[磁贴池调整大小](tile-pool-resizing.md)。 简化显示驱动程序和运行时实现存在一个约束：给定流式资源每次只能映射到最多一个磁贴池（而不是同时映射到多个磁贴池）。

与流式资源本身相关的存储量（即独立的磁贴池内存）大致与在任何给定时间实际映射到该池的磁贴数成正比。 在硬件中，这一事实归结为页表存储的内存占用情况大致上与被映射的磁贴数量对应（例如，可以视情况使用多级页表方案）。

你可以将磁贴池想象成一种完全的软件抽象，它使得 Direct3D 应用程序能够有效地在图形处理单元 (GPU) 上编写页表，而不必知道低级的实现细节（或不必直接处理指针地址）。 磁贴池不会在硬件中应用任何额外的间接级别。 使用页目录等结构对单级页表进行的优化独立于磁贴池概念。

我们来讨论一下在最坏的情况下页表本身需要多少存储（实际上，实现只需要与映射内容大致成正比的存储）。

假设每个页表条目为 64 位。

对于而言，最坏的页表单个表面，给定 Direct3D11 中的资源限制，假设流式资源创建使用 128 位 / 元素格式 （如 RGBA 浮点值），因此 64KB 磁贴大小包含仅 4096 个像素。 最大支持的 [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) 大小为 16384\*16384\*2048（但只有一个 mipmap），如果使用 64 位表条目完全填充（不包括 mipmap），则其在页表中需要约 1GB 的存储空间。 添加 mipmap 会使完全映射（最坏情况）的页表存储增长约三分之一，达到约 1.3GB。

这种情况需要访问约 10.6 TB 的可寻址内存。 但可寻址内存的量可能存在限制，这将减少这些数量，可能缩减至约 TB 范围。

另一个需要考虑的情况是大小为 16384\*16384 的单个 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 流式资源（32 位/元素格式，包括 mipmap）。 在完全填充的页表中，其需要约 170KB 的空间（64 位表条目）。

最后，考虑一个使用 BC 格式的示例，假设有一个 128 位的 BC7，每个磁贴为 4x4 像素。 每个像素占一个字节。 包括 mipmap 的大小为 16384\*16384\*2048 的 [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) 将需要约 85MB 的空间才能在页表中完全填充此内存。 考虑到这允许一个流式资源覆盖 5500 亿个像素（在这种情况下为 512 GB 的内存），结果还不错。

实际上，受可用物理内存容量的限制，每次能够映射和引用的位置要少得多，远远达不到完全映射的程度。 但借助磁贴池，应用程序可以选择重复使用磁贴（举个简单的示例，对于图像中的大块黑色区域，可以重复使用“黑色”磁贴），从而有效地将磁贴池（也就是页表映射）当作内存压缩工具使用。

对于所有条目，页表的初始内容为 **NULL**。 应用程序也不能为表面的内存内容传递初始数据，因为它开始时没有内存支持。

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
<td align="left"><p><a href="tile-pool-creation.md">创建磁贴池</a></p></td>
<td align="left"><p>应用程序可以为每个 Direct3D 设备创建一个或多个磁贴池。 每个磁贴池的总大小仅限于 Direct3D11 的资源大小限制，约为 GPU RAM 的 1/4。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-pool-resizing.md">磁贴池调整大小</a></p></td>
<td align="left"><p>如果应用程序需要更多工作集以容纳流式资源映射，可以扩大磁贴池，如果需要更少空间，可以缩小磁贴池。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="hazard-tracking-versus-tile-pool-resources.md">危险跟踪与磁贴池资源</a></p></td>
<td align="left"><p>对于非流式资源，Direct3D 可以在渲染期间避免某些危险情况；但对于流式资源，危险跟踪将在磁贴级别进行，因而在渲染流式资源期间跟踪危险情况的代价可能极其高昂。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[创建流式资源](creating-streaming-resources.md)

 

 





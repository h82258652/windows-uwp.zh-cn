---
title: "创建流式资源"
description: "创建资源时通过指定标志即可创建流式资源，指明该资源为流式资源。"
ms.assetid: B3F3E43C-54D4-458C-9E16-E13CB382C83F
keywords:
- "创建流式资源"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 86f19b2afd580a16510fee8e97acfc2a388cad01
ms.lasthandoff: 02/07/2017

---

# <a name="creating-streaming-resources"></a>创建流式资源


创建资源时通过指定标志即可创建流式资源，指明该资源为流式资源。

对何时创建作为流式资源的资源的限制在[流式资源创建参数](streaming-resource-creation-parameters.md)中进行了描述。

创建资源时，在图形系统中对非流式资源的存储进行分配，例如对 2D 纹理数组的分配。

创建流式资源时，图形系统不会为资源内容分配存储。 相反，应用程序创建流式资源时，图形系统仅为磁贴图面区域预留地址空间，然后允许应用程序控制磁贴映射。 磁贴“映射”只是内存中的物理位置，其中资源中的逻辑磁贴会指向此位置（或者对未映射磁贴来说为 **NULL**）。

不要将此概念与映射 Direct3D 资源以实现 CPU 访问混淆，虽然两者使用同一个名称，但是完全独立的。 在知道一个图面的所有磁贴无需一次映射完的情况下，你将能够定义并根据需要更改每个磁贴的映射，从而有效利用可用内存量。

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
<td align="left"><p>[映射到磁贴池](mappings-are-into-a-tile-pool.md)</p></td>
<td align="left"><p>作为流式资源创建资源时，构成资源的磁贴来自于磁贴池中各位置处的指针。 磁贴池是内存池（由一个或多个分配在后台提供支持 - 对应用程序不可见）。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[流式资源创建参数](streaming-resource-creation-parameters.md)</p></td>
<td align="left"><p>您可以创建为流式资源的 Direct3D 资源类型存在某些限制。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[磁贴池创建参数](tile-pool-creation-parameters.md)</p></td>
<td align="left"><p>创建缓冲区时，使用此部分中的参数定义磁贴池。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[跨进程和设备共享的流式资源](streaming-resource-cross-process-and-device-sharing.md)</p></td>
<td align="left"><p>磁贴池可与传统资源等其他进程共享。 涉及无法跨设备和进程共享磁贴池的流式资源。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[可用于流式资源的操作](operations-available-on-streaming-resources.md)</p></td>
<td align="left"><p>此部分列出了可对流式资源执行的操作。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[可用于磁贴池的操作](operations-available-on-tile-pools.md)</p></td>
<td align="left"><p>可对磁贴池执行的操作包括调整磁贴池的大小、提供资源（为整个磁贴池临时向系统给予内存）以及回收资源。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[流式资源区域的平铺方式](how-a-streaming-resource-s-area-is-tiled.md)</p></td>
<td align="left"><p>创建流式资源时，尺寸、格式元素大小以及 mipmap 和/或数组切片的数量（如适用）决定了支持整个图面区域所需的磁贴数量。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[流式资源](streaming-resources.md)

 

 






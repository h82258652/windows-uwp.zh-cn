---
title: 流式资源
description: 流式资源是使用少量物理内存的大型逻辑资源。 整个大型资源不是一次传入，而是在需要时流式传输小部分资源。 流式资源以前称作平铺资源。
ms.assetid: 04F0486E-4B71-4073-88DA-2AF505F4F0C1
keywords:
- 流式资源
- 资源，流式
- 资源，平铺
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dac89fc678e35b1e3a39d26d836f03c18d3c4684
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6046343"
---
# <a name="streaming-resources"></a>流式资源


*流式资源*是使用少量物理内存的大型逻辑资源。 整个大型资源不是一次传入，而是在需要时流式传输小部分资源。 流式资源以前称作*平铺资源*。

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
<td align="left"><p><a href="the-need-for-streaming-resources.md">需要流式资源</a></p></td>
<td align="left"><p>流式资源可保证 GPU 内存不会浪费无法访问的表面存储区域，并告知硬件如何跨相邻的磁贴进行过滤。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="creating-streaming-resources.md">创建流式资源</a></p></td>
<td align="left"><p>创建资源时通过指定标志即可创建流式资源，指明该资源为流式资源。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pipeline-access-to-streaming-resources.md">对流式资源的管道访问</a></p></td>
<td align="left"><p>流式资源可用于着色器资源视图 (SRV)、呈现目标视图 (RTV)、深度模板视图 (DSV) 和无序访问视图 (UAV)，以及未使用视图的某些盲点（如顶点缓冲区绑定）。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="streaming-resources-features-tiers.md">流式资源功能层</a></p></td>
<td align="left"><p>Direct3D 具有三层功能，可为流式资源提供支持。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[Direct3D 图形学习指南](index.md)

[资源](resources.md)

 

 





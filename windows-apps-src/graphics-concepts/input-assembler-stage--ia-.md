---
title: 输入装配器 (IA) 阶段
description: 输入装配器 (IA) 阶段向管道提供基元和邻接数据，例如三角形、线和点，包括语义 ID，以减少对尚未处理基元的处理，从而提高着色器的效率。
ms.assetid: AF1DC611-C872-47F1-BF1A-92C68C8903E6
keywords:
- 输入装配器 (IA) 阶段
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8c5e1c294da2b4ef24ff8f62b686890cb8c69c06
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8483899"
---
# <a name="input-assembler-ia-stage"></a>输入装配器 (IA) 阶段


输入装配器 (IA) 阶段向管道提供基元和邻接数据，例如三角形、线和点，包括语义 ID，以减少对尚未处理基元的处理，从而提高着色器的效率。

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>用途和用法


输入装配器 (IA) 阶段的目的是从用户填充缓冲区读取基元数据（点、线和三角形），并将数据装配成将被其他管道阶段使用的基元，并附加[系统生成的值](https://msdn.microsoft.com/library/windows/desktop/bb509647)，以帮助提高着色器的效率。 系统生成的值是又称作语义的文本字符串。 可编程着色器阶段是从使用系统生成值（如基元 ID、实例 ID 或顶点 ID）的常用着色器核心构建的，这使得着色器阶段可以将处理减少到仅限尚未处理的基元、实例或顶点。

IA 阶段可以将顶点装配成多种不同的[基元类型](primitive-topologies.md)（如线列表、三角形带或邻接的基元）。 基元类型（如邻接的三角形列表和邻接的线列表）支持[几何着色器 (GS) 阶段](geometry-shader-stage--gs-.md)。

邻接信息仅在几何着色器中对应用程序可见。 例如，如果用包含邻接的三角形调用几何着色器，则输入数据将包含每个三角形的 3 个顶点和每个三角形邻接数据的 3 个顶点。

当 IA 阶段被请求输出邻接数据时，输入数据必须包含邻接数据。 这可能需要提供虚拟顶点（形成退化三角形），或在某个顶点属性中标记该顶点是否存在。 这也需要由几何着色器检测和处理，虽然光栅化阶段会剔除退化几何图形。

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>输入


IA 阶段从内存读取数据：来自用户填充缓冲区的基元数据（点、线和/或三角形）。

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>输出


IA 阶段将数据装配成基元并附加系统生成的值，然后将其输出为被[顶点着色器 (VS) 阶段](vertex-shader-stage--vs-.md)及之后的其他管道阶段使用的基元。

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
<td align="left"><p><a href="primitive-topologies.md">基元拓扑</a></p></td>
<td align="left"><p>Direct3D 支持多种基元拓扑，后者定义管道将如何解释和渲染顶点，如点列表、线列表和三角形带。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="using-system-generated-values.md">使用系统生成的值</a></p></td>
<td align="left"><p>输入装配器 (IA) 阶段（基于用户提供的输入<a href="https://msdn.microsoft.com/library/windows/desktop/bb509647">语义</a>）生成系统生成值，以帮助提高着色器操作的效率。 通过附加数据，如实例 ID（对<a href="vertex-shader-stage--vs-.md">顶点着色器 (VS) 阶段</a>可见）、顶点 ID（对 VS 可见）或基元 ID（对<a href="geometry-shader-stage--gs-.md">几何着色器 (GS) 阶段</a>/<a href="pixel-shader-stage--ps-.md">像素着色器 (PS) 阶段</a>可见），后续的着色器阶段可以查找这些系统值，从而对当前阶段的处理进行优化。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[图形管道](graphics-pipeline.md)

 

 





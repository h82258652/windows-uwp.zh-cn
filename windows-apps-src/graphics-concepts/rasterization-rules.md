---
title: 光栅化规则
description: 光栅化规则定义矢量数据如何映射到光栅数据。
ms.assetid: B604725F-96A5-4DB6-B597-9EC57FBBC024
keywords:
- 光栅化规则
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 72be66c3bf7b0e58092ae3a7e1baf82c9e686f0c
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5825195"
---
# <a name="rasterization-rules"></a>光栅化规则


光栅化规则定义矢量数据如何映射到光栅数据。 光栅数据将贴靠到随后会被剔除和剪裁的整数位置（目的是绘制最低数量的像素），并且每像素属性在传递到像素着色器之前会被内插（从每顶点属性）。

存在几种类型的规则，这些规则依赖于正在映射的基元的类型以及数据是否使用多重采样来减少失真。 以下各个图示展示了如何处理极端情况。

## <a name="span-idtrianglespanspan-idtrianglespanspan-idtrianglespantriangle-rasterization-rules-without-multisampling"></a><span id="Triangle"></span><span id="triangle"></span><span id="TRIANGLE"></span>三角形光栅化规则（未使用多重采样）


落到三角形内的任何像素中心都会被绘制；如果像素通过了左上规则，则假定该像素位于内部。 左上规则是，如果某个像素中心位于三角形的上边缘或左边缘，则将其定义为位于三角形内。

其中：

-   上边缘是完全水平且高于其他边缘的边缘。
-   左边缘是不完全水平且位于三角形左侧的边缘。一个三角形可能有一个或两个左边缘。

左上规则将确保相邻的三角形绘制一次。

下图显示了由于位于三角形内或遵循左上规则而绘制的像素的示例。

![左上三角形光栅化的示例](images/d3d10-rasterrulestriangle.png)

像素的浅灰色和深色灰色覆盖将它们显示为成组的像素以指示它们位于哪个三角形内。

## <a name="span-idline1spanspan-idline1spanspan-idline1spanline-rasterization-rules-aliased-without-multisampling"></a><span id="Line_1"></span><span id="line_1"></span><span id="LINE_1"></span>线光栅化规则（已失真，未使用多重采样）


线光栅化规则使用菱形测试区确定线是否覆盖像素。 对于 x 主线（-1 &lt;= 斜率 &lt;= + 1 的线），菱形测试区包括（显示为实线）左下边缘、右下边缘和底部角；菱形不包括（显示为虚线）左上边缘、右上边缘、顶部角、左上角和右上角。 y 主线是非 x 主线的所有线；其菱形测试区与为 x 主线描述的相同，只不过还包括了右侧角。

给定菱形区域，如果某个像素沿着某条线的一头向另一头移动时，该线离开了该像素的菱形测试区，则该线将覆盖该像素。 线条带的行为方式相同，因为它作为一系列线绘制。

下图显示了一些示例。

![失真的线光栅化的示例](images/d3d10-rasterrulesline.png)

## <a name="span-idline2spanspan-idline2spanspan-idline2spanline-rasterization-rules-antialiased-without-multisampling"></a><span id="Line_2"></span><span id="line_2"></span><span id="LINE_2"></span>线光栅化规则（已抗锯齿，未使用多重采样）


抗锯齿线将被光栅化为像是矩形一样（宽度 = 1）。 该矩形将与生成每像素覆盖值的呈现目标相交，这些值将相乘得到像素着色器输出 alpha 分量。 当在已进行多重采样的呈现目标上绘制线时，不会执行抗锯齿。

人们认为执行抗锯齿线呈现没有“最佳”方法。 Direct3D 作为准则采用的方法如下图所示。 此方法凭经验产生，展示了大量被认为必需的可视属性。

硬件不需要与此算法完全匹配；针对此引用的测试应该有基于下面列出的某些原则的“合理的”公差，从而支持各种硬件实现和筛选器内核大小。 这种灵活性在硬件实现中完全不受支持，但是，可以通过 Direct3D 将其向上传递到应用程序，而不只是绘制线和观察/测量其外观。

![抗锯齿线光栅化的示例](images/d3d10-rasterruleslineaa.png)

此算法将生成相对平滑的线，该线具有统一的强度和最小的锯齿边缘或编结。 封闭线的莫尔条纹已被减至最少。 没有很好地覆盖首尾相连的线段之间的接合处。

筛选器内核是边缘模糊量与 gamma 矫正导致的强度变化之间的合理权衡。 覆盖值将根据以下公式与[输出合并 (OM) 阶段](output-merger-stage--om-.md)相乘并得到像素着色器 o0.a (srcAlpha)：

srcColor \* srcAlpha + destColor \* (1-srcAlpha)

## <a name="span-idpointspanspan-idpointspanspan-idpointspanpoint-rasterization-rules-without-multisampling"></a><span id="Point"></span><span id="point"></span><span id="POINT"></span>点光栅化规则（不使用多重采样）


点被解释为好像由 Z 模式（使用三角形光栅化规则）中的两个三角形组成一样。 坐标标识了一个像素宽的正方形的中心。 不需要对点进行任何剔除。

下图显示了一些示例。

![点光栅化的示例](images/d3d10-rasterrulespoint.png)

## <a name="span-idmultisamplespanspan-idmultisamplespanspan-idmultisamplespanmultisample-anti-aliasing-rasterization-rules"></a><span id="Multisample"></span><span id="multisample"></span><span id="MULTISAMPLE"></span>多重采样抗锯齿光栅化规则


多重采样抗锯齿 (MSAA) 通过在多个子样本位置使用像素覆盖和深度模具测试来减少几何失真。 为了提高性能，将通过跨覆盖的子像素共享着色器输出来为每个覆盖的像素执行一次每像素计算。 多重采样抗锯齿无法减少图面失真。 样本位置和重建功能依赖于硬件实现。

下图显示了一些示例。

![多重采样抗锯齿光栅化的示例](images/d3d10-rasterrulesmsaa.png)

样本位置的数量依赖于多重采样模式。 顶点属性将在像素中心内插，因为这是调用像素着色器的位置（如果未覆盖中心，这将变成外推）。 可在像素着色器中将属性标记为质心采样，这将导致未覆盖的像素在像素的区域与基元的相交处内插属性。

像素着色器将对每个 2x2 像素区域运行以支持派生计算（哪个像素使用 x 和 y 增量）。 这意味着调用着色器的次数将超过所显示的次数，以填充最少的 2x2 量程（不依赖于多重采样）。 着色器结果将为通过每样本深度模具测试的每个覆盖的样本写出。

一般而言，基元的光栅化规则不受多重采样抗锯齿的影响，除非：

-   对于三角形，将为每个样本位置执行覆盖测试（不适合像素中心）。 如果覆盖了多个样本位置，像素着色器将在属性内插于像素中心的情况下运行一次。 将为通过深度/模具测试的像素中的每个覆盖的样本位置存储（复制）结果。

    线被视为由两个三角形组成的矩形，线宽为 1.4。

-   对于点，将为每个样本位置执行覆盖测试（不适合像素中心）。

多重采样格式可在呈现目标中使用，该目标可通过[加载](https://msdn.microsoft.com/library/windows/desktop/bb509694)读回到着色器中，因为着色器访问的各个样本不需要解析。 多重采样资源不支持深度格式，因此，深度格式被限制为仅限呈现目标。

无类型格式支持多重采样以允许资源视图以不同的方式解释数据。 例如，你可以使用 R8G8B8A8\_TYPELESS 创建多重采样资源，通过将呈现目标视图资源与 R8G8B8A8\_UINT 格式结合使用来对资源进行呈现，然后使用 R8G8B8A8\_UNORM 数据格式将内容解析到另一个资源。

### <a name="span-idhardwaresupportspanspan-idhardwaresupportspanspan-idhardwaresupportspanhardware-support"></a><span id="Hardware_Support"></span><span id="hardware_support"></span><span id="HARDWARE_SUPPORT"></span>硬件支持

API 通过质量级别号报告对多重采样的硬件支持。 例如，质量级别 0 表示硬件不支持多重采样（在特定格式和质量级别）。 质量级别 3 表示硬件支持三种不同的样本布局和/或求解算法。 还可进行以下假设：

-   支持多重采样的任何格式都对该系列中的每个格式支持相同的质量级别号。
-   支持多重采样且具有 \_UNORM、\_SRGB、\_SNORM 或 \_FLOAT 格式的每个格式也支持求解。

### <a name="span-idcentroidsamplingspanspan-idcentroidsamplingspanspan-idcentroidsamplingspancentroid-sampling-of-attributes-when-multisample-antialiasing"></a><span id="Centroid_Sampling"></span><span id="centroid_sampling"></span><span id="CENTROID_SAMPLING"></span>使用多重采样抗锯齿时的属性的质心采样

默认情况下，顶点属性在多重采样抗锯齿期间将内插到像素中心；如果未覆盖像素中心，属性将外推到像素中心。 如果包含质心语义（假定像素未完全覆盖）的像素着色器输入将在像素的覆盖区域内的某个位置被采样（可能位于覆盖的样本位置之一）。 样本掩码（由光栅器状态指定）将在质心计算之前应用。 因此，被掩盖的样本不会用作质心位置。

参考光栅器将为类似于下面的质心采样选择样本位置：

-   样本掩码允许所有样本。 如果覆盖了像素或未覆盖任何样本，则使用像素中心。 否则，将选择第一个覆盖的样本 - 从像素中心开始并向外移动。
-   样本掩码将关闭除一个样本之外的所有样本（常见方案）。 应用程序可通过循环浏览一位样本掩码值并为使用质心采样的每个样本重新呈现场景来实现多通道超采样。 这要求应用程序调整派生对象以相应地选择更详细的纹理 mip，从而提高纹理采样密度。

### <a name="span-idderivativecalculationsspanspan-idderivativecalculationsspanspan-idderivativecalculationsspanderivative-calculations-when-multisampling"></a><span id="Derivative_Calculations"></span><span id="derivative_calculations"></span><span id="DERIVATIVE_CALCULATIONS"></span>在多重取样时派生计算

像素着色器始终使用最小为 2x2 的像素区域运行，以支持派生计算 - 从相邻像素中选取数据之间的增量（假设已在单位间距为水平或垂直的情况下对每个像素中的数据进行采样）。 这不受多重采样的影响。

如果在已经过质心采样的属性上请求了派生对象，则不会调整硬件计算，从而导致派生对象不准确。 着色器期望呈现目标空间中存在单位矢量，但可能获得与其他一些矢量空间相关的非单位矢量。 因此，应用程序有责任在从已经过质心采样的属性请求派生对象时发出警告。

事实上，不建议将派生对象与质心采样相结合。 质心采样在绝不能外推基元内插属性的情况下可能很有用，但同时应作出一些权衡，例如可能在以下位置跳动的属性：基元边缘跨过像素（而不是持续更改）或跨过无法由派生 LOD 的纹理采样运算使用的派生对象的位置。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[附录](appendix.md)

[光栅器 (RS) 阶段](rasterizer-stage--rs-.md)

 

 





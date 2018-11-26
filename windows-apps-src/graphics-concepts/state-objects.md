---
title: 状态对象
description: 设备状态被分为状态对象，这些状态对象可大大降低状态变化的成本。 状态对象有多个，每一个状态对象旨在对特定管道阶段的一组状态进行初始化。 状态对象因 Direct3D 的版本而异。
ms.assetid: D998745C-2B75-4E59-9923-AD1A17A92E05
keywords:
- 状态对象
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3437119979073a5cec27948fc90f954e06c2fc93
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7707118"
---
# <a name="state-objects"></a>状态对象


设备状态被分为状态对象，这些状态对象可大大降低状态变化的成本。 状态对象有多个，每一个状态对象旨在对特定管道阶段的一组状态进行初始化。 状态对象因 Direct3D 的版本而异。

## <a name="span-idinputlayoutspanspan-idinputlayoutspanspan-idinputlayoutspaninput-layout-state"></a><span id="Input_Layout"></span><span id="input_layout"></span><span id="INPUT_LAYOUT"></span>输入布局状态


此组状态规定[输入汇编程序 (IA) 阶段](input-assembler-stage--ia-.md)如何从输入缓冲区读取数据，并将数据汇编供顶点着色器使用。 其中包括输入缓冲区的元素数量以及输入数据的签名等状态。 输入汇编程序 (IA) 阶段通过流式传输将基元从内存传入管道。

## <a name="span-idrasterizerspanspan-idrasterizerspanspan-idrasterizerspanrasterizer-state"></a><span id="Rasterizer"></span><span id="rasterizer"></span><span id="RASTERIZER"></span>光栅器状态


此组状态能够对[光栅器 (RS) 阶段](rasterizer-stage--rs-.md)进行初始化。 此对象包括填充或精选模式等状态，让剪刀矩形能够剪切和设置多重采样参数。 此阶段可将基元光栅化为像素，执行相关操作，例如将基元剪切并映射至视区。

## <a name="span-iddepthstencilspanspan-iddepthstencilspanspan-iddepthstencilspandepth-stencil-state"></a><span id="DepthStencil"></span><span id="depthstencil"></span><span id="DEPTHSTENCIL"></span>深度模具状态


此组状态可对[输出合并器 (OM) 阶段](output-merger-stage--om-.md)的深度模具部分进行初始化。 具体而言，此对象可初始化深度和模具测试。

## <a name="span-idblendspanspan-idblendspanspan-idblendspanblend-state"></a><span id="Blend"></span><span id="blend"></span><span id="BLEND"></span>混合状态


此组状态可对[输出合并器 (OM) 阶段](output-merger-stage--om-.md)的混合部分进行初始化。

## <a name="span-idsamplerspanspan-idsamplerspanspan-idsamplerspansampler-state"></a><span id="Sampler"></span><span id="sampler"></span><span id="SAMPLER"></span>取样器状态


此组状态可初始化取样器对象。 着色器阶段使用取样器对象筛选内存中的纹理。

在 Direct3D 中，取样器对象不与特定纹理绑定，只描述在有任何附带资源的情况下如何进行筛选。

## <a name="span-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanperformance-considerations"></a><span id="Performance_Considerations"></span><span id="performance_considerations"></span><span id="PERFORMANCE_CONSIDERATIONS"></span>性能注意事项


设计 API 使用状态对象可带来多项性能优势。 其中包括验证对象创建时的状态、实现状态对象在硬件中的缓存以及显著减少在状态设置 API 调用（将图柄传递至状态对象，而不是状态）时通过的状态量。

为了实现这些性能改进，应用程序启动时，应在呈现循环之前创建状态对象。 状态对象不可变，也就是说在创建后，你无法对其进行更改。 必须将其销毁并重新创建。

你可使用各类取样器状态组合，创建多个取样器对象。 然后，调用将图柄传递至对象（不同于取样器状态）的相应“设置”API，实现对取样器状态的更改。 由于调用数量和数据量大大减少，因此，这能够显著降低每一个状态更改渲染帧期间的间接成本。

或者，你也可以选择使用效果系统，它能够自动管理应用程序状态对象的有效创建和销毁。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[图形管道](graphics-pipeline.md)

[视图](views.md)

 

 





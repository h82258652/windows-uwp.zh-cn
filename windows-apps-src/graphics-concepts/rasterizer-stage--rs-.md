---
title: "光栅器 (RS) 阶段"
description: "未在视图中的光栅器剪辑基元为像素着色器 (PS) 阶段准备基元并确定如何调用像素着色器。"
ms.assetid: 7E80724B-5696-4A99-91AF-49744B5CD3A9
keywords: "光栅器 (RS) 阶段"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 1226ad54c6af2f064badf2b1e00088e3b1c70a29
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="rasterizer-rs-stage"></a>光栅器 (RS) 阶段


未在视图中的光栅器剪辑基元为[像素着色器 (PS) 阶段](pixel-shader-stage--ps-.md)准备基元并确定如何调用像素着色器。 光栅化阶段将矢量信息（由形状或基元构成）转换为光栅图像（由像素构成）以便显示实时 3D 图形。

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>用途和用法


在光栅化期间，每个基元将转换为像素，同时跨每个基元内插每顶点值。 光栅化包括将剪裁顶点以呈现视锥、执行被 z 除运算以提供视角、将基元映射到 2D 视口以及决定如何调用像素着色器。 虽然使用像素着色器是可选操作，但光栅化阶段始终执行剪裁（一种将点转换为齐性空间的透视分隔）并将顶点映射到视口。

你可以通过告知管道没有像素着色器（通过将[像素着色器 (PS) 阶段](pixel-shader-stage--ps-.md)设置为 **NULL** 并禁用深度和模具测试）来禁用光栅化。 禁用后，与光栅化有关的管道计数器将无法更新。

在实现分层 Z 缓冲区优化的硬件上，你可以通过将像素着色器 (PS) 阶段设置为 **NULL** 并启用深度和模具测试来允许预加载 z 缓冲区。

请参阅[光栅化规则](rasterization-rules.md)。

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>输入


假定进入光栅化阶段的顶点 (x,y,z,w) 将处于齐性裁剪空间。 在此坐标空间里，X 轴指向右，Y 轴指向上，Z 轴指向与摄像机相反的方向。

固定函数光栅化 (RS) 阶段由流输出 (SO) 阶段和/或上一个管道阶段（例如，[几何着色器 (GS) 阶段](geometry-shader-stage--gs-.md)）提供源。 如果未使用 GS，则 RS 由[域着色器 (DS) 阶段](domain-shader-stage--ds-.md)提供源。 如果也未使用 DS，则 RS 由[顶点着色器 (VS) 阶段](vertex-shader-stage--vs-.md)提供源。

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>输出


使用像素着色器 (PS) 阶段是可选操作；光栅化阶段可改为直接输出到[输出合并 (OM) 阶段](output-merger-stage--om-.md)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[光栅化规则](rasterization-rules.md)

[图形管道](graphics-pipeline.md)

 

 





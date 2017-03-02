---
title: "光栅器排序视图 (ROV)"
description: "利用光栅器排序视图，可以解决一些深度缓冲区限制，特别是使多个包含透明度的纹理都应用于相同像素。"
ms.assetid: BCB1EE0D-4C1D-4E17-BDB7-173F448E0A7B
keywords:
- "光栅器排序视图 (ROV)"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ee1ac48111c94f88d87e595eaaf6f5442d137ce4
ms.lasthandoff: 02/07/2017

---

# <a name="rasterizer-ordered-view-rov"></a>光栅器排序视图 (ROV)


利用光栅器排序视图，可以解决一些深度缓冲区限制，特别是使多个包含透明度的纹理都应用于相同像素。

光栅器排序视图允许将“无规则透明度”(OIT) 算法应用于像素呈现。 深度缓冲区仅允许绘制或遮挡像素，没有部分遮挡透明度的概念。 OIT 算法按正确的顺序应用透明纹理，如果透明玻璃对象应在使用透明纹理的某种植物后面的玻璃窗后出现，则可以预测到将准确绘制最终结果。 在没有 ROV 和 OIT 算法的情况下，这些透明对象的绘制顺序是无法预测的，并且呈现的场景会使人混淆并出错。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[视图](views.md)

 

 






---
title: 光栅器排序视图 (ROV)
description: 利用光栅器排序视图，可以解决一些深度缓冲区限制，特别是使多个包含透明度的纹理都应用于相同像素。
ms.assetid: BCB1EE0D-4C1D-4E17-BDB7-173F448E0A7B
keywords:
- 光栅器排序视图 (ROV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ed49ba81c44a799e4d827d636f601c77d01333b3
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "7717304"
---
# <a name="rasterizer-ordered-view-rov"></a>光栅器排序视图 (ROV)


利用光栅器排序视图，可以解决一些深度缓冲区限制，特别是使多个包含透明度的纹理都应用于相同像素。

光栅器排序视图允许将“无规则透明度”(OIT) 算法应用于像素呈现。 深度缓冲区仅允许绘制或遮挡像素，没有部分遮挡透明度的概念。 OIT 算法按正确的顺序应用透明纹理，如果透明玻璃对象应在使用透明纹理的某种植物后面的玻璃窗后出现，则可以预测到将准确绘制最终结果。 在没有 ROV 和 OIT 算法的情况下，这些透明对象的绘制顺序是无法预测的，并且呈现的场景会使人混淆并出错。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[视图](views.md)

 

 





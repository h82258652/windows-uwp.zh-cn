---
title: 取样器
description: 采样是读取纹理中的输入值或读取其他资源的过程。 \ 0034;取样器\ 0034;是从资源中读取的任何对象。
ms.assetid: 7ECE13BB-9FC5-44A3-B1B2-2FE163F1D627
keywords:
- 取样器
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e80e160e1225e510ab95f52cbd9f21049c890457
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663652"
---
# <a name="sampler"></a>取样器


采样是读取纹理中的输入值或读取其他资源的过程。 “取样器”是从资源中读取的任何对象。

从纹理取样并在屏幕区域呈现存在许多问题和项目。 例如，如果用于呈现的区域像素为 50 × 50，纹理像素为 16 × 16 或 256 × 256，那么需考虑对纹理进行拉伸或缩小。 因为此类尺寸不匹配问题会导致项目难以被接受，因此需使用可减轻此类项目影响的筛选技术。 常见的筛选方法为将小纹理用于较大的呈现区域，进行“双线性”筛选。

使用区域进行呈现时会出现的另一个问题为视图倾斜角度非常大（例如，由于视图角度问题，256 × 256 的纹理呈现至像素为 100（宽）× 5（深）的区域）。 这种情况下，经常会用到“各向异性”筛选。 各向异性筛选会去除混叠效应，而不会过度模糊，所以其提供的图片质量优于双线性筛选，但在计算方面的费用更高。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[视图](views.md)

 

 





---
title: 常量缓冲区视图 (CBV)
description: 常量缓冲区包含着色器常量数据。 其优点在于数据持续存在，并且可由任意 GPU 着色器访问，直到需要更改数据。
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords:
- 常量缓冲区视图 (CBV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7179f8644970a24a9e7b9ce50a4bcb4d5d225d46
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370453"
---
# <a name="constant-buffer-view-cbv"></a>常量缓冲区视图 (CBV)


常量缓冲区包含着色器常量数据。 其优点在于数据持续存在，并且可由任意 GPU 着色器访问，直到需要更改数据。

常量缓冲区的典型数据是世界、投影和视图矩阵，它们在绘制一帧的整个过程中保持恒定。

常量缓冲区布局应匹配 HLSL 布局（请参考[常量变量的封装规则](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-packing-rules)）。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[视图](views.md)

 

 





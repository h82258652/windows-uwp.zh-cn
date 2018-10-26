---
title: 常量缓冲区视图 (CBV)
description: 常量缓冲区包含着色器常量数据。 其优点在于数据持续存在，并且可由任意 GPU 着色器访问，直到需要更改数据。
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords:
- 常量缓冲区视图 (CBV)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cca90c705c7bb4dd1c7e283a9c3ed267cd282b56
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5554449"
---
# <a name="constant-buffer-view-cbv"></a>常量缓冲区视图 (CBV)


常量缓冲区包含着色器常量数据。 其优点在于数据持续存在，并且可由任意 GPU 着色器访问，直到需要更改数据。

常量缓冲区的典型数据是世界、投影和视图矩阵，它们在绘制一帧的整个过程中保持恒定。

常量缓冲区布局应匹配 HLSL 布局（请参考[常量变量的封装规则](https://msdn.microsoft.com/library/windows/desktop/bb509632.aspx)）。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[视图](views.md)

 

 





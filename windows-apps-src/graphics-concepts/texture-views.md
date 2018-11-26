---
title: 纹理视图
description: 在 Direct3D 中，使用视图访问纹理资源，这是适用于内存中资源的硬件转译机制。
ms.assetid: 18DABFCE-8A36-4C4E-B08E-10428B05D701
keywords:
- 纹理视图
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e9167db4648dd193acaff0a224f3378486d171ad
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7692064"
---
# <a name="texture-views"></a>纹理视图


在 Direct3D 中，使用视图访问纹理资源，这是适用于内存中资源的硬件解释的机制。 视图允许特定管道阶段仅访问所需的[子资源](resource-types.md)（采用应用程序所需的表示形式）。

视图支持无类型资源的概念。 无类型资源是所创建的具有特定大小，但无特定数据类型的资源。 数据在绑定到管道时动态解译。

下图例举了通过创建相应的着色器资源视图，将含 6 个纹理的 2D 纹理数组绑定作为着色器资源。 然后，以纹理数组的形式实行资源寻址。 （请注意：子资源不能同时绑定为管道的输入和输出。）

![图示为含 6 个纹理的纹理数组](images/d3d10-cube-texture-faces.png)

将 2D 纹理数组作为呈现对象时，资源可以被视为带 mipmap 层级（本例中为三个层级）的 2D 纹理（本例中，含 6 个纹理）数组。

针对呈现对象创建视图对象，并命名为“CreateRenderTargetView”。 然后，调用 OMSetRenderTargets 以设置针对管道的呈现目标视图。 通过调用绘制并使用 RenderTargetArrayIndex 对数组中的相应纹理编索引的方式呈现目标。 你可以使用子资源（mipmap 层级，数组下标组合）绑定到任何子资源组。 你可以绑定到第二 mipmap 层级并且只更新所需的特定 mipmap 层级，如下图所示。

![图示为仅绑定到纹理数组的第二 mipmap 层级](images/d3d10-cube-texture-faces-subresource.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[资源](resources.md)

 

 





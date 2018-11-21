---
title: 压缩的纹理格式
description: 此部分包含有关压缩纹理格式的内部组织的信息。
ms.assetid: 24D17B9F-8CA7-4006-9E0F-178C6B3CAEC9
keywords:
- 压缩的纹理格式
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4dd0ff85198107263fba458bd97c0323a7049d20
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "7439149"
---
# <a name="compressed-texture-formats"></a>压缩的纹理格式


此部分包含有关压缩纹理格式的内部组织的信息。 使用压缩的纹理时无需这些详细信息，因为你可以使用 Direct3D 功能转换为压缩格式和从压缩格式转换。 但是，如果你想要直接处理压缩的曲面数据，则此信息很有用。

Direct3D 使用将纹理贴图分为 4 x 4 纹素块的压缩格式。 如果纹理无透明度（不透明）或透明度由 1 位 alpha 指定，则使用 8 字节块表示纹理贴图块。 如果纹理贴图包含透明纹素并使用 alpha 通道，则使用 16 位块表示它。

任何一个纹理必须指定其数据存储为 64 位还是 128 位（每个 16 纹素组）。 如果纹理使用 64 位块（即格式 BC1），则可以对同一纹理中的块混合使用透明度和 1 位 alpha 格式。 换言之，分别为每个 16 纹素块比较 color\_0 和 color\_1 的无符号整数度量值。

如果使用 128 位块，必须在显式（格式 BC2）或内插（格式 BC3）模式下为整个纹理指定 alpha 通道。 与颜色一样，选择内插模式时，块可以使用八个内插 alpha 模式或六个内插 alpha 模式。 同样，分别为每个块比较 alpha\_0 和 alpha\_1 的度量值。

采用字节（而非块）度量 BCn 格式的跨度。 例如，如果宽度为 16，则跨度为四个块（BC1 为 4\*8，BC2 或 BC3 为 4\*16）。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[压缩的纹理资源](compressed-texture-resources.md)

 

 





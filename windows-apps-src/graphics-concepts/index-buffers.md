---
title: "索引缓冲区"
description: "索引缓冲区是包含索引数据的内存缓冲区，索引数据是到顶点缓冲区的整数偏移量，用于渲染基元。"
ms.assetid: 14D3DEC5-CF74-488B-BE41-16BF5E3201BE
keywords:
- "索引缓冲区"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 6aa62a7506b37314b1952a6687920a2cdf3deca3
ms.lasthandoff: 02/07/2017

---

# <a name="index-buffers"></a>索引缓冲区


*索引缓冲区*是包含索引数据的内存缓冲区，索引数据是到顶点缓冲区的整数偏移量，用于渲染基元。

索引缓冲区是包含索引数据的内存缓冲区。 索引数据或索引是到顶点缓冲区的整数偏移量，用于渲染基元。

顶点缓冲区包含顶点；因此，你可以绘制包含或不包含索引基元的顶点缓冲区。 但是，由于索引缓冲区包含索引，因此不能使用没有对应顶点缓冲区的索引缓冲区。

## <a name="span-idindexbufferdescriptionspanspan-idindexbufferdescriptionspanspan-idindexbufferdescriptionspanindex-buffer-description"></a><span id="Index_Buffer_Description"></span><span id="index_buffer_description"></span><span id="INDEX_BUFFER_DESCRIPTION"></span>索引缓冲区描述


索引缓冲区在功能方面进行描述，例如其存在于内存中的位置、其是否支持读取和写入以及其能够包含的索引的类型和数量。

索引缓冲区描述用于告知应用程序现有的缓冲区是如何创建的。 你提供一个空描述结构，以便系统填充之前创建的索引缓冲区的能力。

## <a name="span-idindexprocessingrequirementsspanspan-idindexprocessingrequirementsspanspan-idindexprocessingrequirementsspanindex-processing-requirements"></a><span id="Index_Processing_Requirements"></span><span id="index_processing_requirements"></span><span id="INDEX_PROCESSING_REQUIREMENTS"></span>索引处理要求


索引处理操作的性能很大程度上取决于索引缓冲区存在于内存中的位置，以及所用的是何种类型的渲染设备。 应用程序在创建索引缓冲区时控制其内存分配。

应用程序可以直接将索引写入到在驱动程序优化存储器中分配的索引缓冲区。 这种技术可以避免后来进行冗余的复制操作。 如果你的应用程序从索引缓冲区读回数据，则此技术无法正常工作，因为宿主从驱动程序优化存储器执行读取操作可能会非常缓慢。 因此，如果你的应用程序需要在处理期间读取或将数据不规律地写入缓冲区，则系统内存索引缓冲区是更好的选择。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[顶点和索引缓冲区](vertex-and-index-buffers.md)

 

 






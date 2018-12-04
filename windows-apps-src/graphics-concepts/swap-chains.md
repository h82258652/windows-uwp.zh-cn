---
title: 交换链
description: 交换链是用于向用户显示帧的缓冲区集合。
ms.assetid: A38E8BB7-1E77-4D93-B321-D3572A80D5DD
keywords:
- 交换链
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 486eb4adc1151bac1bf6a04a8f54b67530b426a3
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8482678"
---
# <a name="swap-chains"></a>交换链


交换链是用于向用户显示帧的缓冲区集合。 应用程序每次提供要显示的新帧时，交换链中的第一个缓冲区将替代已显示的缓冲区。 此过程称为*交换*或*翻转*。

图形适配器有一个指向图面的指针，该图面表示监视器上正在显示的图像，称为前台缓冲区。 随着监视器不断刷新，图形卡会将前台缓冲区中的内容发送至监视器进行显示。 但是，这会导致渲染实时图像时出现“撕裂”问题。 该问题的核心是，监视器刷新频率与计算机其余部分相比非常缓慢。 通常刷新频率介于 60 Hz（每秒 60 次）到 100 Hz 之间。

如果你的应用程序在监视器刷新过程中更新前台缓冲区，显示的图像将被截成两半，上半部分包含旧图像，下半部分包含新图像。 此问题被称为*撕裂*。

## <a name="span-idavoidingtearingspanspan-idavoidingtearingspanspan-idavoidingtearingspanavoiding-tearing"></a><span id="Avoiding_tearing"></span><span id="avoiding_tearing"></span><span id="AVOIDING_TEARING"></span>避免撕裂


Direct3D 提供两种避免撕裂的选项：

-   一种选项只允许监视器在垂直回扫（或垂直同步）操作时更新。 监视器刷新图像的方式通常是水平移动光针，从监视器左上角开始，Z 字形移动到右下角。 光针达到底部后，监视器将重新校准，将其移回左上角，重新开始刷新过程。

    此重新校准过程称为垂直同步。垂直在同步期间，监视器不会绘制任何内容，因此在监视器再次开始绘制之前，不会看到对前台缓冲区的任何更新。 垂直同步相对较慢，但是，还不足以在等待时渲染复杂场景。 若要避免撕裂，同时能够渲染复杂场景，则需要进行后台缓冲。

-   一种选项使用后台缓冲技术。 后台缓冲是向屏幕外图面（称为后台缓冲区）绘制场景的过程。 前台缓冲区之外的任何图面均称为屏幕外图面，因为此类图面永远不会通过监视器直接显示。

    通过使用后台缓冲区，应用程序可以在系统空闲时（即没有正在等待处理的 Windows 消息时）自由渲染场景，而无需考虑监视器刷新频率。 后台缓冲带来了另一个问题：如何以及何时将后台缓冲区移至前台缓冲区。

## <a name="span-idsurfaceflippingspanspan-idsurfaceflippingspanspan-idsurfaceflippingspansurface-flipping"></a><span id="Surface_flipping"></span><span id="surface_flipping"></span><span id="SURFACE_FLIPPING"></span>图面翻转


后台缓冲区移至前台缓冲区的过程称为图面翻转。 由于图形卡只使用指向代表前台缓冲区的图面的指针，所以只需更改指针就可以将后台缓冲区移至前台缓冲区。 当应用程序要求 Direct3D 将后台缓冲区显示到前台缓冲区时，Direct3D 只需“翻转”两种图面指针。 结果是后台缓冲区变成新的前台缓冲区，而旧的前台缓冲区则变成了新的后台缓冲区。

每当应用程序要求 Direct3D 设备显示后台缓冲区时，就会调用图面翻转；但是，可以将 Direct3D 设置为将请求加入队列，直到发生垂直同步。 此选项称为 Direct3D 设备的显示间隔。 新的后台缓冲区中的数据可能无法重用，具体取决于应用程序如何指定 Direct3D 处理图面翻转的方式。

图面翻转在多媒体、动画和游戏软件中非常关键；其原理相当于用一沓纸来制作动画。 在每页纸上，艺术家对图形稍加改动，当你快速翻动纸张时，图画就会呈现出动画效果。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[设备](devices.md)

 

 





---
title: 设备类型
description: Direct3D 设备类型包括硬件抽象层 (hal) 设备和参考光栅器。
ms.assetid: 64084B23-10C0-4541-8E93-FB323385D2F0
keywords:
- 设备类型
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cbf7d984226984391da340c74791dad4a6c0d8fb
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5925907"
---
# <a name="device-types"></a>设备类型


Direct3D 设备类型包括硬件抽象层 (hal) 设备和参考光栅器。

## <a name="span-idhaldevicespanspan-idhaldevicespanspan-idhaldevicespanhal-device"></a><span id="HAL_Device"></span><span id="hal_device"></span><span id="HAL_DEVICE"></span>HAL 设备


主要设备类型是 hal 设备，它支持硬件加速光栅以及硬件和软件顶点处理。 如果运行应用程序的计算机配备支持 Direct3D 的显示适配器，则应用程序应该用它进行 Direct3D 操作。 Direct3D hal 设备实现全部或部分转换、光照和硬件中的光栅化模块。

应用程序不能直接访问图形适配器。 它们调用 Direct3D 功能和方法。 Direct3D 通过 hal 访问硬件。 如果运行应用程序的计算机支持 hal，它将使用 hal 设备获得最佳性能。

## <a name="span-idreferencedevicespanspan-idreferencedevicespanspan-idreferencedevicespanreference-device"></a><span id="Reference_Device"></span><span id="reference_device"></span><span id="REFERENCE_DEVICE"></span>参考设备


Direct3D 支持被称为参考设备或参考光栅器的其他设备类型。 与软件设备不同，参考光栅器支持每个 Direct3D 功能。 此设备旨在用于调试，因此仅在已安装了 DirectX SDK 的计算机上可用。 由于这些功能的实现在于精度而非速度，并且是在软件中实现，因此结果不是非常快。 参考光栅器确实会使用特殊的 CPU 说明（如果可以），但它不用于零售应用程序。 仅将参考光栅器仅用于功能测试或演示目的。

## <a name="span-idhalvsrefspanspan-idhalvsrefspanspan-idhalvsrefspanhal-vs-ref-devices"></a><span id="HAL_vs_REF"></span><span id="hal_vs_ref"></span><span id="HAL_VS_REF"></span>HAL 与 REF 设备


HAL（硬件抽象层）设备和 REF（参考光栅器）设备是 Direct3D 设备的两个主要类型；第一个基于硬件支持并且速度很快，但可能不支持所有功能；而第二个不使用硬件加速，因此速度很慢，但保证正确地支持一整套 Direct3D 功能。 一般情况下，你只需要使用 HAL 设备，但如果要使用图形卡不支持的某些高级功能，那么你可能需要回退到参考设备。

HAL 设备产生异常结果时，也就是说，当你确定代码正确但并未得到预期结果时，你可能也想要使用 REF 设备。 REF 设备保证能够正常运行，因此你可能希望在 REF 设备上测试应用程序并查看异常行为是否还在继续。 如果其无法正常运行，这意味着要么 (a) 应用程序认为图形卡支持其不支持的内容，要么 (b) 它是一个驱动程序错误。 如果应用程序仍无法在 REF 设备上运行，则是应用程序错误。

## <a name="span-idhardwarevssoftwarespanspan-idhardwarevssoftwarespanspan-idhardwarevssoftwarespanhardware-vs-software-vertex-processing"></a><span id="Hardware_vs_Software"></span><span id="hardware_vs_software"></span><span id="HARDWARE_VS_SOFTWARE"></span>硬件与软件顶点处理


硬件与软件顶点处理仅适用于 HAL 设备。 当推送顶点穿过管道时，需要（轮流通过世界、视图和投影矩阵）转换和（通过 D3D 的内置灯）照亮它们，此处理阶段被称为 T & L（转换和照明）。 硬件顶点处理意味着此操作是在硬件中完成的（如果硬件支持），而软件顶点处理是在软件中完成的。 常规的做法是先尝试创建硬件 T&L 设备，如果失败的话，再尝试创建混合设备；如果再次失败，最后尝试创建软件。 （如果软件失败，则放弃和退出（会弹出错误消息）。）

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[设备](devices.md)

 

 





---
title: 丢失的设备
description: Direct3D 设备可以处于运行状态或丢失状态。
ms.assetid: 1639CC02-8000-4208-AA95-91C1F0A3B08D
keywords:
- 丢失的设备
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2f0b42a10c2cdd61aef84e08d6bd4f6408a978c3
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "8326123"
---
# <a name="lost-devices"></a>丢失的设备


Direct3D 设备可以处于运行状态或丢失状态。 *运行*状态是设备的正常状态（设备正在运行），并能如预期般呈现所有内容。 当出现全屏应用程序失去键盘焦点而导致无法呈现内容等事件时，设备会转换到*丢失*状态。 丢失状态的特征在于所有呈现操作都没有任何提示就失败，这意味着即使呈现操作失败，呈现方法也可能返回成功代码。

根据设计，未指定可导致设备丢失的所有场景。 一些典型的示例包括丢失焦点，例如当用户按下 ALT+TAB 时或当系统对话框进行初始化时。 设备也可能由于电源管理事件或另一个应用程序采取全屏操作而丢失。 此外，任何重置设备故障都会使设备进入丢失状态。

从 [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) 派生的所有方法都可以保证在设备丢失后正常工作。 设备丢失后，每个函数一般有以下三个选项：

-   失败并返回“设备丢失”错误 - 这意味着应用程序需要认识到设备已丢失，以便应用程序知道某些事件没有按预期出现。
-   没有任何提示就失败、返回 S\_OK 或任何其他返回代码 - 如果函数没有任何提示就失败，应用程序通常无法区分“成功”和“没有任何提示就失败”。
-   返回一个返回代码。

## <a name="span-idrespondingtoalostdevicespanspan-idrespondingtoalostdevicespanspan-idrespondingtoalostdevicespanresponding-to-a-lost-device"></a><span id="Responding_to_a_Lost_Device"></span><span id="responding_to_a_lost_device"></span><span id="RESPONDING_TO_A_LOST_DEVICE"></span>响应丢失的设备


丢失的设备必须在重置后重新创建资源（包括视频存储器资源）。 如果设备丢失，应用程序会查询设备以了解其能否恢复到运行状态。 如果不能，应用程序将等待，直到设备可以恢复。

如果设备可以恢复，则应用程序通过销毁所有视频存储器资源和任何交换链来准备设备。 重置是在设备丢失时唯一有效的过程，并且是应用程序可以将设备从丢失状态改变为运行状态的唯一方式。 如果应用程序未释放所有已分配的资源，包括渲染目标和深度模板表面，重置将失败。

在大多数情况下，Direct3D 的高频调用不返回有关设备是否已丢失的任何信息。 应用程序可以继续调用渲染方法，但不接收丢失设备的通知。 内部会放弃这些操作，直到设备被重置为运行状态。

## <a name="span-idlockingoperationsspanspan-idlockingoperationsspanspan-idlockingoperationsspanlocking-operations"></a><span id="Locking_Operations"></span><span id="locking_operations"></span><span id="LOCKING_OPERATIONS"></span>锁定操作


在内部，Direct3D 会执行充足的工作，以确保锁定操作将在设备丢失后成功。 但是，不保证视频存储器资源的数据在锁定操作期间准确无误。 保证不返回任何错误代码。 这使得在锁定操作期间可以向应用程序执行写入操作，而不理会设备是否丢失。

## <a name="span-idresourcesspanspan-idresourcesspanspan-idresourcesspanresources"></a><span id="Resources"></span><span id="resources"></span><span id="RESOURCES"></span>资源


资源可能会使用视频存储器。 丢失的设备会与适配器拥有的视频存储器断开连接，因而在设备丢失时不能保证视频存储器的分配。 因此，所有资源创建方法都实现为成功，但实际上只分配了虚拟系统内存。 由于所有视频存储器资源都必须在设备调整大小之前销毁，所以不会出现过度分配视频存储器的问题。 这些虚拟表面允许锁定和复制操作看似正常工作，直到应用程序发现设备已丢失。

必须先释放所有视频存储器，然后设备才能从丢失状态重置为运行状态。 通过转换到运行状态，其他的状态数据会被自动销毁。

我们建议你开发通过单一代码路径响应设备丢失的应用程序。 此代码路径可能类似于（或等同于）在启动时初始化设备的代码路径。

## <a name="span-idretrieveddataspanspan-idretrieveddataspanspan-idretrieveddataspanretrieved-data"></a><span id="Retrieved_Data"></span><span id="retrieved_data"></span><span id="RETRIEVED_DATA"></span>检索的数据


Direct3D 允许应用程序依据硬件的单程渲染来验证纹理和渲染状态。

Direct3D 还允许应用程序将生成的或先前写入的图像从视频存储器资源复制到非易失性系统内存资源。 由于此类传输的源图像可能在任何时间丢失，Direct3D 允许此类复制操作在设备丢失时失败。

由于设备丢失时没有主表面，复制操作可能会失败。 由于在设备丢失时无法创建后台缓冲区，创建交换链也可能会失败。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[设备](devices.md)

 

 





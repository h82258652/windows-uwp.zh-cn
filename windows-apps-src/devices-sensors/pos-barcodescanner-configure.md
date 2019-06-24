---
title: 配置条形码扫描仪
description: 了解如何配置条码扫描器为目标应用程序。
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 8466c56ef73a1c38c67e28cf52de7f380e6c563a
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321583"
---
# <a name="configure-a-barcode-scanner"></a>配置条形码扫描仪

条形码扫描仪可以在几个不同模式下配置。  为目标应用程序正确配置条形码扫描仪非常重要。

许多条形码扫描仪可以在**键盘 wedge** 模式下配置，这让条形码扫描仪可以显示为 Windows 的键盘。  这允许你将条形码扫描到无条形码扫描仪感知的应用程序（如记事本）。  当你在此模式中扫描条形码时，来自条形码扫描仪的已解码数据将在插入点插入，就像你使用键盘键入数据。  如果你希望从 UWP 应用程序更多地控制条形码扫描仪，则需要在非键盘 wedge 模式下对其进行配置。

## <a name="usb-barcode-scanner"></a>USB 条形码扫描仪
USB 连接的条形码扫描仪必须在 **HID POS 扫描仪**模式下配置，以使用 Windows 附带的条形码扫描仪驱动程序。 此驱动程序是实现**HID 点的销售用法表**规范发布到[USB HID](https://www.usb.org/hid)。  请参考条形码扫描仪文档或联系条形码扫描仪制造商获取启用 **HID POS 扫描仪**模式的说明。  配置为 **HID POS 扫描仪**后，条形码扫描仪将在 **POS 条形码扫描仪**节点的“设备管理器”中显示为 **POS HID 条形码扫描仪**。

你的条形码扫描仪制造商可能还有供应商特定的驱动程序，可以使用除 **HID POS 扫描仪**以外的模式支持 UWP 条形码扫描仪 API。  如果你安装了制造商提供的驱动程序与条码扫描器的 UWP Api 兼容，可能会看到下面列出的特定于供应商的设备**POS 条码扫描器**设备管理器中。

## <a name="bluetooth-barcode-scanner"></a>蓝牙条形码扫描仪
蓝牙连接的扫描仪必须在**串行端口协议 - 简单串行接口 (SPP-SSI)** 模式下配置，以使用 UWP 条形码扫描仪 API。  请参考条形码扫描仪文档或联系条形码扫描仪制造商获取启用 **SPP-SSI 模式**的说明。

可以使用蓝牙条形码扫描程序之前必须将它使用配对**设置 > 设备 > 蓝牙 & 其他设备 > 添加 Bluetooth 打印机或其他设备**。

可以启动和控制配对的进程使用[Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration)命名空间。  请参阅[对设备](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices)有关详细信息。

[!INCLUDE [feedback](./includes/pos-feedback.md)]
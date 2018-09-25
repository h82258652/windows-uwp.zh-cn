---
author: TerryWarwick
title: 配置条形码扫描仪
description: 了解如何配置条形码扫描仪的目标应用程序。
ms.author: jken
ms.date: 08/29/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: b33c1d33fe88a09de36e8f80a3034b915d338861
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2018
ms.locfileid: "4178332"
---
# <a name="configure-a-barcode-scanner"></a>配置条形码扫描仪

条形码扫描仪可以在几个不同模式下配置。  为目标应用程序正确配置条形码扫描仪非常重要。

许多条形码扫描仪可以在**键盘 wedge** 模式下配置，这让条形码扫描仪可以显示为 Windows 的键盘。  这允许你将条形码扫描到无条形码扫描仪感知的应用程序（如记事本）。  当你在此模式中扫描条形码时，来自条形码扫描仪的已解码数据将在插入点插入，就像你使用键盘键入数据。  如果你希望从 UWP 应用程序更多地控制条形码扫描仪，则需要在非键盘 wedge 模式下对其进行配置。

## <a name="usb-barcode-scanner"></a>USB 条形码扫描仪
USB 连接的条形码扫描仪必须在 **HID POS 扫描仪**模式下配置，以使用 Windows 附带的条形码扫描仪驱动程序。 此驱动程序是发布到[USB HID](http://www.usb.org/developers/hidpage/) **HID 销售点使用表**规范的实现。  请参考条形码扫描仪文档或联系条形码扫描仪制造商获取启用 **HID POS 扫描仪**模式的说明。  配置为 **HID POS 扫描仪**后，条形码扫描仪将在 **POS 条形码扫描仪**节点的“设备管理器”中显示为 **POS HID 条形码扫描仪**。

你的条形码扫描仪制造商可能还有供应商特定的驱动程序，可以使用除 **HID POS 扫描仪**以外的模式支持 UWP 条形码扫描仪 API。  如果你已经安装了制造商提供的驱动程序与 UWP 条形码扫描仪 Api 兼容，你可能会看到下面**POS 条形码扫描仪**设备管理器中列出的供应商特定设备。

## <a name="bluetooth-barcode-scanner"></a>蓝牙条形码扫描仪
蓝牙连接的扫描仪必须在**串行端口协议 - 简单串行接口 (SPP-SSI)** 模式下配置，以使用 UWP 条形码扫描仪 API。  请参考条形码扫描仪文档或联系条形码扫描仪制造商获取启用 **SPP-SSI 模式**的说明。

你可以使用蓝牙条形码扫描仪之前必须先使用配对**设置 > 设备 > 蓝牙和其他设备 > 添加蓝牙或其他设备**。

你可以启动和控制配对过程使用[Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration)命名空间。  有关详细信息，请参阅[配对设备](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices)。

[!INCLUDE [feedback](./includes/pos-feedback.md)]
---
author: mukin
title: "POS 设备支持"
description: "本文包含有关每个 POS 设备系列的设备支持的信息"
ms.author: mukin
ms.date: 05/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 58018385082f7f7e49edba0351d919bc840ade05
ms.sourcegitcommit: 53930c9871461f6106f785ae4fabb2296eb359f1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/23/2017
---
# <a name="pos-device-support"></a>POS 设备支持

## <a name="barcode-scanner"></a>条形码扫描仪
| 连接性 | 支持 |
| -------------|-------------|
| USB          | <p>Windows 包含用于 USB 连接的条形码扫描仪的内置类驱动程序，该驱动程序基于 [USB.org](http://www.usb.org/developers/hidpage/) 定义的 HID POS 扫描仪使用表 (8c) 规范。 有关已知兼容设备的列表，请参阅下表。  请参阅条形码扫描仪手册或联系制造商，以确定是否可以采用 USB.HID.POS 扫描仪模式配置它。 </p><p>Windows 还支持供应商特定驱动程序的实现，以便为不支持 USB.HID.POS 扫描仪标准的其他条形码扫描仪提供支持。 请与条形码扫描仪制造商联系以核实供应商特定驱动程序可用性。</p>|
| 蓝牙    | <p>Windows 支持基于 SPP SSI 的蓝牙条形码扫描仪。 有关已知兼容设备的列表，请参阅下表。</p> |

### <a name="compatible-hardware"></a>兼容的硬件
| 类别 | 连接性 | 制造商/型号 |
|--------------|-----------|-----------|
| **1D 手持扫描仪** | **USB** |Honeywell Voyager 1200g<br/>Honeywell Voyager 1202g<br/>Honeywell Voyager 1202-bf<br/>Honeywell Voyager 145Xg（可升级）|
| **1D 手持扫描仪** | **蓝牙** |Socket Mobile CHS 7Ci<br/> Socket Mobile CHS 7Di<br/> Socket Mobile CHS 7Mi<br/> Socket Mobile CHS 7Pi<br/>Socket Mobile DuraScan D700<br/> Socket Mobile DuraScan D730<br/>Socket Mobile SocketScan S800（以前称为 CHS 8Ci） <br/>|
|**2D 手持扫描仪** | **USB** |Code Reader™ 950<br/>Code Reader™ 1021<br/>Code Reader™ 1421<br/> Honeywell Granit 198Xi<br/>Honeywell Granit 191Xi<br/>Honeywell Xenon 1900g<br/>Honeywell Xenon 1902g<br/>Honeywell Xenon 1902g-bf<br/>Honeywell Xenon 1900h<br/>Honeywell Xenon 1902h<br/>Honeywell Voyager 145Xg（可升级）<br/>Honeywell Voyager 1602g<br/>Intermec SG20|
|**2D 手持扫描仪** | **蓝牙** |Socket Mobile SocketScan S850（以前称为 CHS 8Qi）|
| **演示扫描仪** | **USB** |Code Reader™ 5000<br/>Honeywell Genesis 7580g<br/>Honeywell Orbit 7190g|
| **柜台扫描仪** | **USB** |Honeywell Stratos 2700|
| **扫描引擎** | **USB** | Honeywell N5680<br/>Honeywell N3680|
| **Windows Mobile 设备**| **内置** |Bluebird EF400<br/>Bluebird EF500<br/>Bluebird EF500R<br/>Honeywell CT50<br/>Honeywell D75e<br/>Janam XT2<br/>Panasonic FZ-E1<br/>Panasonic FZ-F1<br/>PointMobile PM80<br/>Zebra TC700j|
| **Windows Mobile 设备**| **自定义** | 具有条形码扫描仪护套的 HP Elite X3 |

## <a name="cash-drawer"></a>收银机
| 连接性 | 支持 |
| -------------|-------------|
| 网络/蓝牙 | <p> 可以通过网络或蓝牙直接连接到收银机，具体取决于收银机设备的功能。 </p>|
| DK 端口 | <p> 没有网络或蓝牙功能的收银机可以通过支持的 POS 打印机或 Star Micronics DK-AirCash 附件上的 DK 端口进行连接。 </p>
| OPOS    | <p> 支持任何支持 OPOS 驱动程序和/或提供 OPOS 服务对象的收银机设备。 按照特定设备制造商安装说明安装 OPOS 驱动程序。 这可启用 USB 和其他基于制造商规范的连接。 </p> |

**注意：**有关 DK-AirCash 的详细信息，请联系 Star Micronics。

### <a name="networkbluetooth"></a>网络/蓝牙
| 制造商 |    型号 |
|--------------|-----------|
| APG Cash Drawer |    NetPRO、BluePRO |

## <a name="line-display"></a>行显示
支持任何支持 OPOS 驱动程序和/或提供 OPOS 服务对象的行显示设备。 按照特定设备制造商安装说明安装 OPOS 驱动程序。

## <a name="magnetic-stripe-reader"></a>磁条阅读器

Windows 包含用于 USB 连接的磁条阅读器的内置类驱动程序，该驱动程序基于 [USB.org](http://www.usb.org/developers/hidpage/) 定义的 HID POS 扫描仪使用表 (8c) 规范。

### <a name="vendor-specific-support"></a>供应商特定支持
Windows 根据供应商 ID 和产品 ID (VID/PID) 为 Magtek 和 IDTech 的以下磁条阅读器提供支持。

| 制造商 |     型号 |    部件号 |
|--------------|-----------|--------------|
| IDTech | SecureMag (VID:0ACD PID:2010) | IDRE-3x5xxxx |
| |    MiniMag (VID:0ACD PID:0500) |    IDMB-3x5xxxx |
| Magtek | MagneSafe (VID:0801 PID:0011) |    210730xx |
| |    Dynamag (VID:0801 PID:0002) |    210401xx |

### <a name="custom-vendor-specific"></a>特定于自定义供应商
Windows 支持实现其他供应商特定驱动程序以支持其他磁条阅读器。 请与你的磁条阅读器制造商联系以核实可用性。

## <a name="pos-printer"></a>POS 打印机
Windows 支持使用 ESC/POS 打印机控制语言打印到连接了网络和蓝牙的收据打印机的功能。 有关 ESC/POS 的详细信息，请参阅 [Epson ESC/POS（可进行格式设置）](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/epson-esc-pos-with-formatting)。

虽然 API 中公开的类、枚举和接口支持收据打印机、单据打印机以及日记帐打印机，但驱动程序接口仅支持收据打印机。 如果此时尝试使用单据打印机或日记帐打印机，则将返回“未实现”状态。

| 连接性 | 支持 |
| -------------|-------------|
| 网络/蓝牙 | <p> 可以通过网络或蓝牙直接连接到 POS 打印机，具体取决于 POS 打印机设备的功能。 </p>|
| OPOS    | <p> 支持任何支持 OPOS 驱动程序和/或提供 OPOS 服务对象的 POS 打印机设备。 按照特定设备制造商安装说明安装 OPOS 驱动程序。 这可启用 USB 和其他基于制造商规范的连接。 </p> |

### <a name="stationary-pos-printers-networkbluetooth"></a>固定的 POS 打印机（网络/蓝牙）
| 制造商 |    型号 |
|--------------|-----------|
| Epson |    TM-T88V、TM-T70、TM-T20、TM-U220 |

### <a name="mobile-pos-printers-bluetooth"></a>移动 POS 打印机（蓝牙）
| 制造商 |    型号 |
|--------------|-----------|
| Epson |    Mobilink P20 (TM-P20)、Mobilink P60 (TM-P60)、Mobilink P80 (TM-P80) |
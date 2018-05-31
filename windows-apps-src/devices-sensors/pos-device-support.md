---
author: TerryWarwick
title: 服务点硬件支持
description: 本文包含有关每个服务点设备类的硬件支持的信息
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ecb2468497115c9595f6fd17ab61b30caed507ab
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832091"
---
# <a name="supported-point-of-service-peripherals"></a>支持的服务点外设

## <a name="barcode-scanner"></a>条形码扫描仪
| 连接性 | 支持 |
| -------------|-------------|
| USB          | <p>Windows 包含用于 USB 连接的条形码扫描仪的内置类驱动程序，该驱动程序基于 [USB.org](http://www.usb.org/developers/hidpage/) 定义的 HID POS 扫描仪使用表 (8c) 规范。有关已知兼容设备的列表，请参阅下表。  请参阅条形码扫描仪手册或联系制造商，以确定如何在 **USB.HID.POS 扫描仪**模式下配置扫描仪。 </p><p>Windows 还支持供应商特定驱动程序的实现，以便为不支持 USB.HID.POS 扫描仪标准的其他条形码扫描仪提供支持。 请与条形码扫描仪制造商联系以核实供应商特定驱动程序可用性。</p><p>有关创建自定义条形码扫描仪驱动程序的信息，请联系条形码扫描仪制造商，或者参阅[条形码扫描仪驱动程序设计指南](https://aka.ms/pointofservice-drv)</p> |
| 蓝牙    | <p>Windows 支持串行端口协议 - 基于简单串行接口 (SPP SSI) 的蓝牙条形码扫描仪。 有关已知兼容设备的列表，请参阅下表。 请参阅条形码扫描仪手册或联系制造商，以确定如何在 **SPP-SSI** 模式下配置扫描仪。</p> |
| 网络摄像头       | <p>从 Windows 10 版本 1803 开始，你可以通过来自通用 Windows 应用程序的标准相机镜头来读取条形码。 建议使用支持自动对焦且最低分辨率为 1920x1440 的相机。  如果条形码打印得足够大，一些分辨率较低的相机可以读取标准条形码。  宽度更窄的条形码可能需要使用分辨率更高的相机。</p>| 
|


### <a name="compatible-barcode-scanners"></a>兼容的条形码扫描仪
| 类别 | 连接性 | 制造商/型号 |
|--------------|-----------|-----------|
| **1D 手持扫描仪** | **USB** |Honeywell Voyager 1200g<br/>Honeywell Voyager 1202g<br/>Honeywell Voyager 1202-bf<br/>Honeywell Voyager 145Xg（可升级）|
| **1D 手持扫描仪** | **蓝牙** |Socket Mobile CHS 7Ci<br/> Socket Mobile CHS 7Di<br/> Socket Mobile CHS 7Mi<br/> Socket Mobile CHS 7Pi<br/>Socket Mobile DuraScan D700<br/> Socket Mobile DuraScan D730<br/>Socket Mobile SocketScan S800（以前称为 CHS 8Ci） <br/>|
|**2D 手持扫描仪** | **USB** |Code Reader™ 950<br/>Code Reader™ 1021<br/>Code Reader™ 1421<br/> Honeywell Granit 198Xi<br/>Honeywell Granit 191Xi<br/>Honeywell Xenon 1900g<br/>Honeywell Xenon 1902g<br/>Honeywell Xenon 1902g-bf<br/>Honeywell Xenon 1900h<br/>Honeywell Xenon 1902h<br/>Honeywell Voyager 145Xg（可升级）<br/>Honeywell Voyager 1602g<br/>Intermec SG20<br/>Zebra DS2278<br/>Zebra DS8108 ¹<hr><small>¹ 最低需要固件 016 (2018.01.18)。 可使用 [123Scan](http://www.zebra.com/123Scan) 升级</small>|
|**2D 手持扫描仪** | **蓝牙** |Socket Mobile SocketScan S850（以前称为 CHS 8Qi）|
| **演示扫描仪** | **USB** |Code Reader™ 5000<br/>Honeywell Genesis 7580g<br/>Honeywell Orbit 7190g|
| **柜台扫描仪** | **USB** |Honeywell Stratos 2700|
| **扫描引擎** | **USB** | Honeywell N5680<br/>Honeywell N3680|
| **Windows Mobile 设备**| **内置** |Bluebird EF400<br/>Bluebird EF500<br/>Bluebird EF500R<br/>Honeywell CT50<br/>Honeywell D75e<br/>Janam XT2<br/>Panasonic FZ-E1<br/>Panasonic FZ-F1<br/>PointMobile PM80<br/>Zebra TC700j|
| **Windows Mobile 设备**| **自定义** | 具有条形码扫描仪护套的 HP Elite X3 |


## <a name="cash-drawer"></a>收银机
| 连接性 | 支持 |
| -------------|-------------|
| 网络/蓝牙 | <p> 可以通过网络或蓝牙直接连接到收银机，具体取决于收银机设备的功能。 </p><p>APG 收银机：NetPRO、BluePRO</p> |
| DK 端口 | <p> 没有网络或蓝牙功能的收银机可以通过支持的收据打印机或 Star Micronics DK-AirCash 附件上的 DK 端口进行连接。 </p>
| OPOS    | <p> 通过制造商提供的 OPOS 服务对象支持任何 OPOS 兼容收银机。 按照设备制造商安装说明安装 OPOS 驱动程序。 </p> |


## <a name="customer-display-linedisplay"></a>客户显示 (LineDisplay)
通过制造商提供的 OPOS 服务对象支持任何 OPOS 兼容行显示设备。 按照设备制造商安装说明安装 OPOS 驱动程序。

## <a name="magnetic-stripe-reader"></a>磁条阅读器
Windows 根据供应商 ID 和产品 ID (VID/PID) 为 Magtek 和 IDTech 的以下磁条阅读器提供支持。

| 制造商 |    型号 |  部件号 |
|--------------|-----------|--------------|
| IDTech | SecureMag (VID:0ACD PID:2010) | IDRE-3x5xxxx |
| | MiniMag (VID:0ACD PID:0500) |   IDMB-3x5xxxx |
| Magtek | MagneSafe (VID:0801 PID:0011) |  210730xx |
| | Dynamag (VID:0801 PID:0002) |   210401xx |

 Windows 支持实现其他供应商特定驱动程序以支持其他磁条阅读器。 请与你的磁条阅读器制造商联系以核实可用性。 有关创建自定义磁条阅读器驱动程序的信息，请联系磁条阅读器制造商或参阅[磁条阅读器驱动程序设计指南](https://aka.ms/pointofservice-drv)。

## <a name="receipt-printer-posprinter"></a>收据打印机 (POSPrinter)
| 连接 | 支持 |
| -------------|-------------|
| 网络和蓝牙 | <p>Windows 使用 Epson ESC/POS 打印机控制语言支持连接了网络和蓝牙的收据打印机。  系统将使用 POSPrinter API 自动发现下面列出的打印机。 虽然也可以使用提供 ESC/POS 仿真的其他收据打印机，但是需要执行[带外配对](https://aka.ms/pointofservice-oobpairing)过程与其关联。</p><p>注意：此方法不支持单据站和日志站。</p> |
| OPOS    | <p> 通过 OPOS 服务对象支持任何 OPOS 兼容收据打印机。 按照设备制造商安装说明安装 OPOS 驱动程序。 </p> |

### <a name="stationary-receipt-printers-networkbluetooth"></a>固定收据打印机（网络/蓝牙）
| 制造商 |    型号 |
|--------------|-----------|
| Epson |   TM-T88V、TM-T70、TM-T20、TM-U220 |

### <a name="mobile-receipt-printers-bluetooth"></a>移动收据打印机（蓝牙）
| 制造商 |    型号 |
|--------------|-----------|
| Epson |   Mobilink P20 (TM-P20)、Mobilink P60 (TM-P60)、Mobilink P80 (TM-P80) |

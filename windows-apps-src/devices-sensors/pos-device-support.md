---
title: 服务点硬件支持
description: 本文包含有关每个服务点设备类的硬件支持的信息
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 11dadd91c3106f6881c357d5a13e09b451f2a1e8
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259659"
---
# <a name="supported-point-of-service-peripherals"></a>支持的服务点外设

## <a name="barcode-scanner"></a>条形码扫描仪
| 连接性 | 支持 |
| -------------|-------------|
| “USB”          | <p>Windows 包含用于 USB 连接的条形码扫描仪的内置类驱动程序，该驱动程序基于 [USB.org](https://www.usb.org/hid) 定义的 HID POS 扫描仪使用表 (8c) 规范。有关已知兼容设备的列表，请参阅下表。  请参阅条形码扫描仪手册或联系制造商，以确定如何在 **USB.HID.POS 扫描仪**模式下配置扫描仪。 </p><p>Windows 还支持供应商特定驱动程序的实现，以便为不支持 USB.HID.POS 扫描仪标准的其他条形码扫描仪提供支持。 请与条形码扫描仪制造商联系以核实供应商特定驱动程序可用性。</p><p>有关创建自定义条形码扫描仪驱动程序的信息，请联系条形码扫描仪制造商，或者参阅[条形码扫描仪驱动程序设计指南](https://docs.microsoft.com/windows-hardware/drivers/ddi/_pos/index)</p> |
| “蓝牙”    | <p>Windows 支持串行端口协议 - 基于简单串行接口 (SPP SSI) 的蓝牙条形码扫描仪。 有关已知兼容设备的列表，请参阅下表。 请参阅条形码扫描仪手册或联系制造商，以确定如何在 **SPP-SSI** 模式下配置扫描仪。</p> |
| 网络摄像头       | <p>从 Windows 10 版本 1803 开始，你可以通过来自通用 Windows 应用程序的标准相机镜头来读取条形码。 建议使用支持自动对焦且最低分辨率为 1920x1440 的相机。  如果条形码打印得足够大，一些分辨率较低的相机可以读取标准条形码。  宽度更窄的条形码可能需要使用分辨率更高的相机。</p>| 
|


| 制造商  | 型号                          | 功能 | 左侧的“连接”    | 在任务栏的搜索框中键入         | “模式”                      |
|---------------|--------------------------------|------------|--------------|--------------|---------------------------|
| 代码          | Reader™ 950                    | 2D         | “USB”          | 手持设备     | HID POS Scanner           |
| 代码          | Reader™ 1021                   | 2D         | “USB”          | 手持设备     | HID POS Scanner           |
| 代码          | Reader™ 1421                   | 2D         | “USB”          | 手持设备     | HID POS Scanner           |
| 代码          | Reader™ 5000                   | 2D         | “USB”          | 呈现 | HID POS Scanner           |
| Honeywell     | Genesis 7580g                  | 2D         | “USB”          | 呈现 | HID POS Scanner           |
| Honeywell     | Granit 198Xi                   | 2D         | “USB”          | 手持设备     | HID POS Scanner           |
| Honeywell     | Granit 191Xi                   | 2D         | “USB”          | 手持设备     | HID POS Scanner           |
| Honeywell     | N5680                          | 2D         | 内置     | Component    | HID POS Scanner           |
| Honeywell     | N3680                          | 2D         | 内置     | Component    | HID POS Scanner           |
| Honeywell     | Orbit 7190g                    | 2D         | “USB”          | 呈现 | HID POS Scanner           |
| Honeywell     | Stratos 2700                   | 2D         | “USB”          | In Counter   | HID POS Scanner           |
| Honeywell     | Voyager 1200g                  | 1D         | “USB”          | 手持设备     | HID POS Scanner           |
| Honeywell     | Voyager 1202g                  | 1D         | “USB”          | 手持设备     | HID POS Scanner           |
| Honeywell     | Voyager 1202-bf                | 1D         | “USB”          | 手持设备     | HID POS Scanner           |
| Honeywell     | Voyager 145Xg                  | 1D / 2D<sup>1</sup>   | “USB”          | 手持设备     | HID POS Scanner           |
| Honeywell     | Voyager 1602g                  | 2D         | “USB”          | 手持设备     | HID POS Scanner           |
| Honeywell     | Xenon 1900g                    | 2D         | “USB”          | 手持设备     | HID POS Scanner           |
| Honeywell     | Xenon 1902g                    | 2D         | “USB”          | 手持设备     | HID POS Scanner           |
| Honeywell     | Xenon 1902g-bf                 | 2D         | “USB”          | 手持设备     | HID POS Scanner           |
| Honeywell     | Xenon 1900h                    | 2D         | “USB”          | 手持设备     | HID POS Scanner           |
| Honeywell     | Xenon 1902h                    | 2D         | “USB”          | 手持设备     | HID POS Scanner           |
| HP            | Value Barcode Scanner (HR2150) | 2D         | “USB”          | 手持设备     | HID POS Scanner           |
| Intermec      | SG20                           | 2D         | “USB”          | 手持设备     | HID POS Scanner           |
| Socket Mobile | CHS 7Ci                        | 1D         | “蓝牙”    | 手持设备     | Serial Port Profile (SPP) |
| Socket Mobile | CHS 7Di                        | 1D         | “蓝牙”    | 手持设备     | Serial Port Profile (SPP) |
| Socket Mobile | CHS 7Mi                        | 1D         | “蓝牙”    | 手持设备     | Serial Port Profile (SPP) |
| Socket Mobile | CHS 7Pi                        | 1D         | “蓝牙”    | 手持设备     | Serial Port Profile (SPP) |
| Socket Mobile | CHS 8Ci                        | 1D         | “蓝牙”    | 手持设备     | Serial Port Profile (SPP) |
| Socket Mobile | DuraScan D700                  | 1D         | “蓝牙”    | 手持设备     | Serial Port Profile (SPP) |
| Socket Mobile | DuraScan D730                  | 1D         | “蓝牙”    | 手持设备     | Serial Port Profile (SPP) |
| Socket Mobile | DuraScan D740                  | 2D         | “蓝牙”    | 手持设备     | Serial Port Profile (SPP) |
| Socket Mobile | SocketScan S700                | 1D         | “蓝牙”    | 手持设备     | Serial Port Profile (SPP) |
| Socket Mobile | SocketScan S730                | 1D         | “蓝牙”    | 手持设备     | Serial Port Profile (SPP) |
| Socket Mobile | SocketScan S740                | 2D         | “蓝牙”    | 手持设备     | Serial Port Profile (SPP) |
| Socket Mobile | SocketScan S800                | 1D         | “蓝牙”    | 手持设备     | Serial Port Profile (SPP) |
| Socket Mobile | SocketScan S850                | 2D         | “蓝牙”    | 手持设备     | Serial Port Profile (SPP) |
| Zebra         | DS2208<sup>2</sup>                        | 2D         | “USB”          | 手持设备     | HID POS Scanner           |
| Zebra         | DS2278                         | 2D         | “USB”          | 手持设备     | HID POS Scanner           |
| Zebra         | DS8108<sup>3</sup>                        | 2D         | “USB”          | 手持设备     | HID POS Scanner           |
| Zebra         | DS8178<sup>4</sup>                         | 2D         | “USB”          | 手持设备     | HID POS Scanner           | 


<sup>1</sup> Upgradable to support 2D barcodes through Honeywell <br/>
<sup>2</sup> Minimum firmware 009 (2018.07.09) required. Upgradable using Zebra [123Scan](http://www.zebra.com/123scan).<br/>
<sup>3</sup> Minimum firmware 016 (2018.01.18) required. Upgradable using Zebra [123Scan](http://www.zebra.com/123scan).<br/> 
<sup>4</sup> Minimum firmware 023 (2019.03.11) required. Upgradable using Zebra [123Scan](http://www.zebra.com/123scan).<br/>

<hr>

### <a name="windows-devices-with-built-in-barcode-scanner"></a>Windows devices with built-in barcode scanner
| 制造商   | 型号 | 操作系统 |
|----------------|-------|------------------|
| Innowi         | ChecOut-M | Windows 10   |

### <a name="windows-mobile-devices-with-built-in-barcode-scanner"></a>Windows Mobile devices with built-in barcode scanner
| 制造商   | 型号 | 操作系统 |
|----------------|-------|------------------|
| Bluebird       | EF400 | Windows 移动版   |
| Bluebird       | EF500 | Windows 移动版   |
| Bluebird       | EF500R | Windows 移动版   |
| Honeywell      | CT50   | Windows 移动版   |
| Honeywell      | D75e | Windows 移动版   |
| Janam          | XT2      | Windows 移动版   |
| 松下      | FZ-E1 | Windows 移动版   |
| 松下      | FZ-F1 |Windows 移动版   |
| PointMobile    | PM80 | Windows 移动版   |
| Zebra          | TC700j | Windows 移动版   |
| HP             | Elite X3 Jacket | Windows 移动版   |




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

 Windows 支持实现其他供应商特定驱动程序以支持其他磁条阅读器。 请与你的磁条阅读器制造商联系以核实可用性。 有关创建自定义磁条阅读器驱动程序的信息，请联系磁条阅读器制造商或参阅[磁条阅读器驱动程序设计指南](https://docs.microsoft.com/windows-hardware/drivers/ddi/_pos/index)。

## <a name="receipt-printer-posprinter"></a>收据打印机 (POSPrinter)
| 连接性 | 支持 |
| -------------|-------------|
| 网络和蓝牙 | <p>Windows 使用 Epson ESC/POS 打印机控制语言支持连接了网络和蓝牙的收据打印机。  系统将使用 POSPrinter API 自动发现下面列出的打印机。 虽然也可以使用提供 ESC/POS 仿真的其他收据打印机，但是需要执行[带外配对](https://docs.microsoft.com/windows/uwp/devices-sensors/point-of-service#out-of-band-pairing)过程与其关联。</p><p>注意：此方法不支持单据站和日志站。</p> |
| OPOS    | <p> 通过 OPOS 服务对象支持任何 OPOS 兼容收据打印机。 按照设备制造商安装说明安装 OPOS 驱动程序。 </p> |

### <a name="stationary-receipt-printers-networkbluetooth"></a>固定收据打印机（网络/蓝牙）
| 制造商 |    型号 |
|--------------|-----------|
| Epson |   TM-T88V、TM-T70、TM-T20、TM-U220 |

### <a name="mobile-receipt-printers-bluetooth"></a>移动收据打印机（蓝牙）
| 制造商 |    型号 |
|--------------|-----------|
| Epson |   Mobilink P20 (TM-P20)、Mobilink P60 (TM-P60)、Mobilink P80 (TM-P80) |

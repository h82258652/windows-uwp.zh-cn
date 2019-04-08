---
title: 服务点硬件支持
description: 本文包含有关每个服务点设备类的硬件支持的信息
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5154593065ce40c5ac67a4873d58b2aac913d1f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663092"
---
# <a name="supported-point-of-service-peripherals"></a>支持的服务点外设

## <a name="barcode-scanner"></a>条形码扫描仪
| 连接性 | 支持 |
| -------------|-------------|
| USB          | <p>Windows 包含的内置类驱动程序连接的 USB 条码扫描器的基于定义的 HID POS 扫描程序使用表 (8c) 规范[USB.org](https://www.usb.org/developers/hidpage/)。有关已知兼容设备的列表，请参阅下表。  请参阅条形码扫描仪手册或联系制造商，以确定如何在 **USB.HID.POS 扫描仪**模式下配置扫描仪。 </p><p>Windows 还支持供应商特定驱动程序的实现，以便为不支持 USB.HID.POS 扫描仪标准的其他条形码扫描仪提供支持。 请与条形码扫描仪制造商联系以核实供应商特定驱动程序可用性。</p><p>有关创建自定义条形码扫描仪驱动程序的信息，请联系条形码扫描仪制造商，或者参阅[条形码扫描仪驱动程序设计指南](https://aka.ms/pointofservice-drv)</p> |
| 蓝牙    | <p>Windows 支持串行端口协议 - 基于简单串行接口 (SPP SSI) 的蓝牙条形码扫描仪。 有关已知兼容设备的列表，请参阅下表。 请参阅条形码扫描仪手册或联系制造商，以确定如何在 **SPP-SSI** 模式下配置扫描仪。</p> |
| Webcam       | <p>从 Windows 10 版本 1803 开始，你可以通过来自通用 Windows 应用程序的标准相机镜头来读取条形码。 建议使用支持自动对焦且最低分辨率为 1920x1440 的相机。  如果条形码打印得足够大，一些分辨率较低的相机可以读取标准条形码。  宽度更窄的条形码可能需要使用分辨率更高的相机。</p>| 
|


| 制造商  | 型号                          | 功能 | 连接    | 在任务栏的搜索框中键入         | 模式                      |
|---------------|--------------------------------|------------|--------------|--------------|---------------------------|
| 代码          | Reader™ 950                    | 2D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
| 代码          | Reader™ 1021                   | 2D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
| 代码          | Reader™ 1421                   | 2D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
| 代码          | Reader™ 5000                   | 2D         | USB          | 呈现 | HID 的 POS 扫描程序           |
| Honeywell     | 生成 7580 g                  | 2D         | USB          | 呈现 | HID 的 POS 扫描程序           |
| Honeywell     | Granit 198Xi                   | 2D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
| Honeywell     | Granit 191Xi                   | 2D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
| Honeywell     | N5680                          | 2D         | 内部     | Component    | HID 的 POS 扫描程序           |
| Honeywell     | N3680                          | 2D         | 内部     | Component    | HID 的 POS 扫描程序           |
| Honeywell     | 轨迹 7190 g                    | 2D         | USB          | 呈现 | HID 的 POS 扫描程序           |
| Honeywell     | Stratos 2700                   | 2D         | USB          | 在计数器中   | HID 的 POS 扫描程序           |
| Honeywell     | 姊妹 1200 g                  | 1D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
| Honeywell     | 姊妹 1202 g                  | 1D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
| Honeywell     | 姊妹 1202 bf                | 1D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
| Honeywell     | 姊妹 145Xg                  | 一维 2 D ¹   | USB          | 手持设备     | HID 的 POS 扫描程序           |
| Honeywell     | 姊妹 1602 g                  | 2D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
| Honeywell     | Xenon 1900 g                    | 2D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
| Honeywell     | Xenon 1902 g                    | 2D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
| Honeywell     | Xenon 1902 g bf                 | 2D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
| Honeywell     | Xenon 1900 h                    | 2D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
| Honeywell     | Xenon 1902 h                    | 2D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
| HP            | 值条形码扫描程序 (HR2150) | 2D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
| Intermec      | SG20                           | 2D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
| Socket Mobile | CHS 7Ci                        | 1D         | 蓝牙    | 手持设备     | 串行端口配置文件 (SPP) |
| Socket Mobile | CHS 7Di                        | 1D         | 蓝牙    | 手持设备     | 串行端口配置文件 (SPP) |
| Socket Mobile | CHS 7 Mi                        | 1D         | 蓝牙    | 手持设备     | 串行端口配置文件 (SPP) |
| Socket Mobile | CHS 7Pi                        | 1D         | 蓝牙    | 手持设备     | 串行端口配置文件 (SPP) |
| Socket Mobile | CHS 8Ci                        | 1D         | 蓝牙    | 手持设备     | 串行端口配置文件 (SPP) |
| Socket Mobile | DuraScan D700                  | 1D         | 蓝牙    | 手持设备     | 串行端口配置文件 (SPP) |
| Socket Mobile | DuraScan D730                  | 1D         | 蓝牙    | 手持设备     | 串行端口配置文件 (SPP) |
| Socket Mobile | DuraScan D740                  | 2D         | 蓝牙    | 手持设备     | 串行端口配置文件 (SPP) |
| Socket Mobile | SocketScan S700                | 1D         | 蓝牙    | 手持设备     | 串行端口配置文件 (SPP) |
| Socket Mobile | SocketScan S730                | 1D         | 蓝牙    | 手持设备     | 串行端口配置文件 (SPP) |
| Socket Mobile | SocketScan S740                | 2D         | 蓝牙    | 手持设备     | 串行端口配置文件 (SPP) |
| Socket Mobile | SocketScan S800                | 1D         | 蓝牙    | 手持设备     | 串行端口配置文件 (SPP) |
| Socket Mobile | SocketScan S850                | 2D         | 蓝牙    | 手持设备     | 串行端口配置文件 (SPP) |
| 斑马         | DS2208²                        | 2D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
| 斑马         | DS2278                         | 2D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
| 斑马         | DS8108³                        | 2D         | USB          | 手持设备     | HID 的 POS 扫描程序           |
|


¹ Upgradable 以支持通过 Honeywell 2D 条形码 <br/>
² 最小值固件 009 (2018.07.09) 所需。 可升级使用斑马[123Scan](http://www.zebra.com/123Scan)。<br/>
³ 最小值固件 016 (2018.01.18) 所需。 可升级使用斑马[123Scan](http://www.zebra.com/123Scan)。 


<hr>

### <a name="windows-devices-with-built-in-barcode-scanner"></a>使用内置的条形码扫描程序的 Windows 设备
| 制造商   | 型号 | 操作系统 |
|----------------|-------|------------------|
| Innowi         | ChecOut-M | Windows 10   |

### <a name="windows-mobile-devices-with-built-in-barcode-scanner"></a>使用内置的条形码扫描程序在 Windows 移动设备
| 制造商   | 型号 | 操作系统 |
|----------------|-------|------------------|
| 蓝       | EF400 | Windows Mobile   |
| 蓝       | EF500 | Windows Mobile   |
| 蓝       | EF500R | Windows Mobile   |
| Honeywell      | CT50   | Windows Mobile   |
| Honeywell      | D75e | Windows Mobile   |
| Janam          | XT2      | Windows Mobile   |
| 松下      | FZ 版 E1 | Windows Mobile   |
| 松下      | FZ F1 |Windows Mobile   |
| PointMobile    | PM80 | Windows Mobile   |
| 斑马          | TC700j | Windows Mobile   |
| HP             | 精英 X3 Jacket | Windows Mobile   |




## <a name="cash-drawer"></a>收银机
| 连接性 | 支持 |
| -------------|-------------|
| 网络/蓝牙 | <p> 可以通过网络或蓝牙直接连接到收银机，具体取决于收银机设备的功能。 </p><p>APG 银：NetPRO、BluePRO</p> |
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
| 连接性 | 支持 |
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

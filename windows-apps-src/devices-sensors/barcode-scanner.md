---
author: mukin
title: "条形码扫描仪"
description: "本文包含有关条形码扫描仪服务点设备系列的信息"
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 8d9ef9bc08fa666c2af1348450f7a5fb0a0c7b65
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="barcode-scanner"></a>条形码扫描仪
使应用程序开发人员能够访问[条形码扫描仪](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.barcodescanner)，以根据硬件支持情况从各种条形码标志（如 UPC 和二维码）中检索解码的数据。 有关受支持的标志的完整列表，请参阅 [BarcodeSymbologies](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.barcodesymbologies) 类。

## <a name="requirements"></a>要求
需要此命名空间的应用程序需要向应用包清单添加“pointOfService”[DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef)。

## <a name="device-support"></a>设备支持

### <a name="usb"></a>USB

#### <a name="hidscanner"></a>HID.Scanner
Windows 包含一个基于 USB.org 所定义的 HID.Scanner (8C) 使用页面的条形码扫描仪类驱动程序。 此类驱动程序将支持任何实施此标准的条形码扫描仪，如：制造商型号 Honeywell 1900GSR 2、1200g 2 Intermec SG20

请参阅条形码扫描仪手册或联系制造商，以确定是否可以将它配置为 USB.HID.Scanner。

#### <a name="hidvendor-specific"></a>特定于 HID.Vendor
Windows 支持实现特定于供应商的驱动程序以支持其他条形码扫描仪。 如果内置 USB.HID.Scanner 不支持该设备，请与你的条形码扫描仪制造商联系以核实可用性。

### <a name="bluetooth"></a>蓝牙
#### <a name="serial-port-protocol-spp--simple-serial-interface-ssi"></a>串行端口协议 (SPP) – 简单串行接口 (SSI)
Windows 支持基于 SPP SSI 的蓝牙条形码扫描仪。

| 制造商 |    型号 |
|--------------|-----------|
| Socket Mobile |    **CHS 7 系列：** <br/> 7 Ci 1D Imager 条形码扫描仪 <br/> 7Di 1 D 耐用型 Imager 条形码扫描仪 <br/> 7Mi 1D 激光条形码扫描仪 <br/> 7Pi 1D 耐用型激光条形码扫描仪 <br/> **DuraScan 700 系列：** <br/> D700 1D Imager 条形码扫描仪 <br/> D730 1D 激光条形码扫描仪 <br/> **SocketScan 800 系列** <br/> S800 1D Imager 条形码扫描仪（以前的 CHS 8Ci） <br/> S850 2D Imager 条形码扫描仪（以前的 CHS 8Qi）

## <a name="examples"></a>示例
有关示例实现，请参阅条形码扫描仪示例。
+    [条形码扫描仪示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)

---
author: mukin
title: "POS 打印机"
description: "本文包含有关服务点打印机设备系列的信息"
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: d8340af651157bb6fae82785812f259c16d0a6c0
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="pos-printer"></a>POS 打印机

使应用程序开发人员能够使用 Epson ESC/POS 打印机控制语言打印到连接了网络和蓝牙的[收据打印机](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.posprinter)。

## <a name="requirements"></a>要求
需要此命名空间的应用程序需要向应用包清单添加“pointOfService”[DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef)。

## <a name="device-support"></a>设备支持
Windows 支持使用 ESC/POS 打印机控制语言打印到连接了网络和蓝牙的收据打印机的功能。 有关 ESC/POS 的详细信息，请参阅 [Epson ESC/POS（可进行格式设置）](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/epson-esc-pos-with-formatting)。

虽然 API 中公开的类、枚举和接口支持收据打印机、单据打印机以及日记帐打印机，但驱动程序接口仅支持收据打印机。 如果此时尝试使用单据打印机或日记帐打印机，则将返回“未实现”状态。

目前仅支持下表中列出的网络和蓝牙设备型号。 目前不支持 USB 连接的打印机。 请重新查看将来要添加的更多支持。

### <a name="stationary-pos-printers-network-bluetooth"></a>固定的 POS 打印机（网络、蓝牙）
| 制造商 |    型号 |
|--------------|-----------|
| Epson |    TM-T88V、TM-T70、TM-T20、TM-U220 |

### <a name="mobile-pos-printers-bluetooth"></a>移动 POS 打印机（蓝牙）
| 制造商 |    型号 |
|--------------|-----------|
| Epson |    Mobilink P20 (TM-P20)、Mobilink P60 (TM-P60)、Mobilink P80 (TM-P80) |

## <a name="examples"></a>示例
有关示例实现，请参阅 POS 打印机示例。
+ [POS 打印机示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

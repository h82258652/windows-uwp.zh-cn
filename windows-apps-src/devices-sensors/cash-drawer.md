---
author: mukin
title: "收银机"
description: "本文包含有关收银机服务点设备系列的信息"
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 
ms.openlocfilehash: 376272356cf720ddd9519f0077e771a1016abb1e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="cash-drawer"></a>收银机

使应用程序开发人员能够与[收银机](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.cashdrawer) 交互。

## <a name="requirements"></a>要求
需要此命名空间的应用程序需要向应用包清单添加“pointOfService”[DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef)。

## <a name="device-support"></a>设备支持
可以通过网络或蓝牙直接连接到收银机，具体取决于收银机设备的功能。 此外，没有网络或蓝牙功能的收银机可以通过支持的 POS 打印机或 Star Micronics DK-AirCash 附件上的 DK 端口进行连接。 目前，不支持通过 USB 或串行端口连接收银机。

**注意：**有关 DK-AirCash 的详细信息，请联系 Star Micronics。

### <a name="networkbluetooth"></a>网络/蓝牙
| 制造商 |    型号 |
|--------------|-----------|
| APG Cash Drawer |    NetPRO、BluePRO |

## <a name="examples"></a>示例
有关示例实现，请参阅收银机示例。
+    [收银机示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)

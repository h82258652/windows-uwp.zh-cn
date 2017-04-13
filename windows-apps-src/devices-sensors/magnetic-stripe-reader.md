---
author: mukin
title: "磁条阅读器"
description: "本文包含有关磁条阅读器服务点设备系列的信息"
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: a11fe7a63c0444ac986e7bfe0d50472249e5196e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="magnetic-stripe-reader"></a>磁条阅读器

使应用程序开发人员能够访问[磁条阅读器](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.magneticstripereader)，以检索磁条卡（例如信用卡/借记卡、会员卡、门禁卡等）中的数据。

## <a name="requirements"></a>要求
需要此命名空间的应用程序需要向应用包清单添加“pointOfService”[DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef)。

## <a name="device-support"></a>设备支持
### <a name="usb"></a>USB
### <a name="supported-vendor-specific"></a>特定于支持的供应商
Windows 根据供应商 ID 和产品 ID (VID/PID) 为 Magtek 和 IDTech 的以下磁条阅读器提供支持。

| 制造商 |     型号 |    部件号 |
|--------------|-----------|--------------|
| IDTech | SecureMag (VID:0ACD PID:2010) | IDRE-3x5xxxx |
| |    MiniMag (VID:0ACD PID:0500) |    IDMB-3x5xxxx |
| Magtek | MagneSafe (VID:0801 PID:0011) |    210730xx |
| |    Dynamag (VID:0801 PID:0002) |    210401xx |

### <a name="custom-vendor-specific"></a>特定于自定义供应商
Windows 支持实现其他供应商特定驱动程序以支持其他磁条阅读器。 请与你的磁条阅读器制造商联系以核实可用性。

## <a name="examples"></a>示例
有关示例实现，请参阅磁条阅读器示例。
+    [磁条阅读器示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)

---
author: TerryWarwick
title: PointOfService 设备共享
description: 与他人共享 PointOfService 外设
ms.author: jken
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 591ba592d1c17b03ae29c6fb98ef546bc18b8bdc
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2018
ms.locfileid: "7445673"
---
# <a name="pointofservice-device-sharing"></a>PointOfService 设备共享

了解如何在多台电脑依赖于共享外围设备，而不是专用的外设连接到每台计算机的环境中的其他计算机共享网络或连接蓝牙外围设备。

## <a name="device-sharing"></a>设备共享

网络和蓝牙连接的 PointOfService 外设时通常使用环境 wheere 在多个客户端设备共享相同的外设全天。  忙碌的零售或食物服务环境中要将附加到外围设备的客户端设备的功能中的任何延迟关联可以关闭交易记录与客户和移到下的效率对没有影响。 在使用为厨房打印机客户的订单的详细信息，将传输到准备厨房收据打印机的其中一个快速服务餐馆方案将从客户获得订单的多个客户端设备。  顺序完成后每个客户端设备应该能够声明共享的打印机并立即打印厨房的顺序。

在这些环境中，务必为完全**释放**设备对象的应用程序，以便另一个可以声明相同的设备。

释放的末尾的使用阻止 PosPrinter

```Csharp 
using Windows.Devices.PointOfService;
using(PosPrinter printer = await PosPrinter.FromIdAsync("Device ID"))
{
    if (printer != null)
    {
        // Exercise the printer.
    }

    // When leaving this scope, printer.Dispose() is automatically invoked, 
    // releasing the session we have with the printer.
}
```


释放的 PosPrinter 通过显式调用 dispose （）

```Csharp 
using Windows.Devices.PointOfService;

PosPrinter printer = await PosPrinter.FromIdAsync("Device ID");
if (printer != null)
{
    // Exercise the printer, then dispose of the printer explicitly.
    printer.Dispose();
}
```

## <a name="api-methods-used"></a>使用 API 方法 

+ [BarcodeScanner.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [CashDrawer.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ [MagneticStripeReader.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [PosPrinter.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]

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
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5976812"
---
# <a name="pointofservice-device-sharing"></a>PointOfService 设备共享

了解如何在多台电脑依赖于共享的外围设备而不是专用的外围设备连接到每台计算机的环境中的其他计算机共享网络或连接蓝牙外围设备。

## <a name="device-sharing"></a>设备共享

网络和蓝牙连接的 PointOfService 外设时通常使用环境 wheere 在多个客户端设备共享相同的外设全天。  忙零售或食物服务环境中要将附加到外围设备的客户端设备的功能中的任何延迟影响对可以关闭交易记录与客户并转到下一个关联的效率。 在其中收据打印机用作厨房打印机的客户的订单详细信息，将传输到准备厨房快速服务餐馆方案将客户从获得订单的多个客户端设备。  完成订单后的每个客户端设备应该能够声明共享的打印机并立即打印厨房的顺序。

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


通过调用 dispose （） 显式释放的 PosPrinter

```Csharp 
using Windows.Devices.PointOfService;

PosPrinter printer = await PosPrinter.FromIdAsync("Device ID");
if (printer != null)
{
    // Exercise the printer, then dispose of the printer explicitly.
    printer.Dispose();
}
```

## <a name="api-methods-used"></a>API 方法使用 

+ [BarcodeScanner.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [CashDrawer.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ [MagneticStripeReader.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [PosPrinter.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]

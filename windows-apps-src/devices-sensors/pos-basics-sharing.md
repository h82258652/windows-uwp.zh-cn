---
title: PointOfService 设备共享
description: 与他人共享 PointOfService 外设
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 53dc22b2aa35b5e69854f6fb489ff6a454c73bf6
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8473300"
---
# <a name="pointofservice-device-sharing"></a>PointOfService 设备共享

了解如何在多台电脑依赖于共享的外围设备，而不是专用的外设连接到每台计算机的环境中的其他计算机共享网络或连接蓝牙外围设备。

## <a name="device-sharing"></a>设备共享

网络和蓝牙连接的 PointOfService 外设时通常使用环境 wheere 在多个客户端设备共享相同的外设全天。  忙碌的零售或食物服务环境中要将附加到外围设备的客户端设备的功能中的任何延迟影响对可以关闭与客户交易并转到下一个关联的效率。 在使用厨房打印机作为客户的订单的详细信息，将传输到准备厨房收据打印机的其中一个快速服务餐馆方案将从客户获得订单的多个客户端设备。  完成订单后的每个客户端设备应该能够声明共享的打印机并立即打印厨房的顺序。

在以下环境中，务必为完全**释放**设备对象的应用程序，以便另一个可以声明相同的设备。

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

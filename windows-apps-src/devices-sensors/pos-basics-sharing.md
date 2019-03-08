---
title: PointOfService 设备共享
description: 与其他人共享 PointOfService 外围设备
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 53dc22b2aa35b5e69854f6fb489ff6a454c73bf6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618942"
---
# <a name="pointofservice-device-sharing"></a>PointOfService 设备共享

了解如何与多台 Pc，依赖于共享外围设备而不是附加到每台计算机的专用外围设备的环境中的其他计算机共享网络或连接的蓝牙外围设备。

## <a name="device-sharing"></a>共享设备

网络和蓝牙连接的 PointOfService 外围设备通常使用在环境 wheere 中多个客户端设备正在共享一天中的相同外围设备。  在繁忙的零售或食物服务环境中客户端设备能够将附加到外围设备的功能中的任何延迟会影响可以关闭与客户的事务和移至下一个相关联的效率。 在快速服务餐馆方案中，为厨房打印机使用回执打印机传输到厨房中使用适用于准备客户的订单的详细信息将从客户获取订单的多个客户端设备。  订单处理完成后的每个客户端设备应该能够声明共享的打印机并立即打印厨房中使用的顺序。

在这些环境中，非常重要的应用程序完全**dispose**设备对象，以便另一个可以声明同一个设备。

在 using 块末尾 PosPrinter 释放

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


释放 PosPrinter 通过显式调用 dispose （）

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

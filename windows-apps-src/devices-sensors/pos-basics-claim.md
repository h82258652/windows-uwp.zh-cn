---
author: TerryWarwick
title: PointOfService 设备声明型号
description: 了解 PointOfService 声明型号
ms.author: jken
ms.date: 06/4/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 202234530945e55ef9c0d0fb68cf9ca83d2e15c3
ms.sourcegitcommit: ce45a2bc5ca6794e97d188166172f58590e2e434
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2018
ms.locfileid: "1983725"
---
# <a name="point-of-service-device-claim-model"></a>服务点设备声明型号

## <a name="claiming-a-device-for-exclusive-use"></a>声明独占使用设备

在成功创建 PointOfService 设备对象后，你必须使用适合的设备类型声明方法进行声明，然后才能使用该设备输入或输出。  声明授予应用程序对很多设备功能的独占访问权限，以确保一个应用程序不会干扰其他应用程序使用设备。  一次只有一个应用程序可以声明独占使用 PointOfService 设备。 

此示例显示如何在成功创建了条形码扫描仪对象后声明条形码扫描仪设备。

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}
```

> [!Warning]
> 声明可能在以下情况下丢失：
> 1. 另一个应用已请求了对同一设备的声明，并且你的应用没有为响应 **ReleaseDeviceRequested** 事件发出 **RetainDevice**。  （有关详细信息，请参阅下面的[声明协商](#Claim-negotiation)。）
> 2. 你的应用已暂停，这导致设备对象关闭，声明因此不再有效。 （有关详细信息，请参阅[设备对象生命周期](pos-basics-deviceobject.md#device-object-lifecycle)。）

### <a name="apis-used-for-claiming"></a>用于声明的 API

|设备|声明 |
|-|:-|
|BarcodeScanner | [ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | 
|CashDrawer | [ClaimDrawerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | 
|LineDisplay | [ClaimAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |
|MagneticStripeReader | [ClaimReaderAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) | 
|PosPrinter | [ClaimPrinterAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) | 
|

## <a name="claim-negotiation"></a>声明协商

由于 Windows 是一个多任务环境，有可能同一台计算机上有多个应用程序需要以协作方式访问外设。  PointOfService API 提供了允许多个应用程序共享连接到计算机的外设的协商模型。

当同一台计算机上的第二个应用程序请求声明已由其他应用程序声明的 PointOfService 外设时，将发布 **ReleaseDeviceRequested** 事件通知。 如果应用程序当前正在使用该设备来避免丢失声明，具有有效声明的应用程序必须通过调用 **RetainDevice** 响应事件通知。 

如果具有有效声明的应用程序未立即使用 **RetainDevice** 响应，将假设该应用程序已暂停或不需要设备，声明将被撤销并给予新应用程序。 

此示例演示在另一个应用请求发布设备后，如何保留已声明的条形码扫描仪。  

```Csharp
claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner myScanner)
{
    // Retain exclusive access to the device
    myScanner.RetainDevice();  
}
```
### <a name="apis-used-for-claim-negotiation"></a>用于声明协商的 API

|声明的设备|发布通知| 保留设备 |
|-|:-|:-|
|ClaimedBarcodeScanner | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.retaindevice)
|ClaimedCashDrawer | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.retaindevice)
|ClaimedLineDisplay | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedMagneticStripeReader | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedPosPrinter | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.retaindevice)
|

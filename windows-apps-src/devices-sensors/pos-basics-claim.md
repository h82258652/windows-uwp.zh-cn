---
author: TerryWarwick
title: PointOfService 设备声明和启用模型
description: 了解 PointOfService 声明和启用模型
ms.author: jken
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: df9c4764b8f7d752a132d6759054660f481cce55
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5701470"
---
# <a name="point-of-service-device-claim-and-enable-model"></a>服务点设备声明和启用模型

## <a name="claiming-for-exclusive-use"></a>声明独占使用

在成功创建 PointOfService 设备对象后，你必须使用适合的设备类型声明方法进行声明，然后才能使用该设备输入或输出。  声明授予应用程序对很多设备功能的独占访问权限，以确保一个应用程序不会干扰其他应用程序使用设备。  一次只有一个应用程序可以声明独占使用 PointOfService 设备。 

> [!Note]
> 声明操作建立于某个设备的独占锁，但不会将其放到运行状态。  有关详细信息，请参阅[启用设备进行 I/O 操作](#Enable-device-for-I/O-operations)。

### <a name="apis-used-to-claim--release"></a>Api 用于声明 / 发布

|设备|声明 | 版本 | 
|-|:-|:-|
|BarcodeScanner | [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | [ClaimedBarcodeScanner.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.close) |
|CashDrawer | [CashDrawer.ClaimDrawerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | [ClaimedCashDrawer.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.close) | 
|LineDisplay | [LineDisplay.ClaimAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |  [ClaimedineDisplay.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.close) | 
|MagneticStripeReader | [MagneticStripeReader.ClaimReaderAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) |  [ClaimedMagneticStripeReader.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.close) | 
|PosPrinter | [PosPrinter.ClaimPrinterAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) |  [ClaimedPosPrinter.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.close) | 
 | 

## <a name="enable-device-for-io-operations"></a>启用设备进行 I/O 操作

声明操作只是建立对所用设备的独占权限，但不会将其放到运行状态。  若要接收事件，或执行任何操作，输出必须启用使用**EnableAsync**的设备。  相反，你可以调用**DisableAsync**停止侦听设备或执行输出中的事件。  你还可以使用**IsEnabled**来确定你的设备的状态。

### <a name="apis-used-enable--disable"></a>使用 Api 启用/禁用

| 设备 | 启用 | 禁用 | IsEnabled？ |
|-|:-|:-|:-|
|ClaimedBarcodeScanner | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isenabled) | 
|ClaimedCashDrawer | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.isenabled) |
|ClaimedLineDisplay | 不 Applicable¹ | 不 Applicable¹ | 不 Applicable¹ | 
|ClaimedMagneticStripeReader | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.isenabled) |  
|ClaimedPosPrinter | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.disableasyc) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.isenabled) |
|

¹ 行显示不需要显式启用 I/O 操作的设备。  通过执行 I/O PointOfService LineDisplay Api 来自动执行启用。

## <a name="code-sample-claim-and-enable"></a>代码示例： 声明和启用

此示例显示如何在成功创建了条形码扫描仪对象后声明条形码扫描仪设备。

```Csharp

    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use 
        claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();

        if(claimedBarcodeScanner != null)
        {
            // after successful claim, enable scanner for data events to fire
            await claimedBarcodeScanner.EnableAsync();
        }
        else
        {
            Debug.WriteLine("Failure to claim barcodeScanner");
        }
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
    
```

> [!Warning]
> 声明可能在以下情况下丢失：
> 1. 另一个应用已请求了对同一设备的声明，并且你的应用没有为响应 **ReleaseDeviceRequested** 事件发出 **RetainDevice**。  （有关详细信息，请参阅下面的[声明协商](#Claim-negotiation)。）
> 2. 你的应用已暂停，这导致设备对象关闭，声明因此不再有效。 （有关详细信息，请参阅[设备对象生命周期](pos-basics-deviceobject.md#device-object-lifecycle)。）


## <a name="claim-negotiation"></a>声明协商

由于 Windows 是一个多任务环境，有可能同一台计算机上有多个应用程序需要以协作方式访问外设。  PointOfService API 提供了允许多个应用程序共享连接到计算机的外设的协商模型。

当同一台计算机上的第二个应用程序请求声明已由其他应用程序声明的 PointOfService 外设时，将发布 **ReleaseDeviceRequested** 事件通知。 如果应用程序当前正在使用该设备来避免丢失声明，具有有效声明的应用程序必须通过调用 **RetainDevice** 响应事件通知。 

如果具有有效声明的应用程序未立即使用 **RetainDevice** 响应，将假设该应用程序已暂停或不需要设备，声明将被撤销并给予新应用程序。 

第一步是创建响应**RetainDevice** **ReleaseDeviceRequested**事件的事件处理程序。  

```Csharp
    /// <summary>
    /// Event handler for the ReleaseDeviceRequested event which occurs when 
    /// the claimed barcode scanner receives a Claim request from another application
    /// </summary>
    void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner myScanner)
    {
        // Retain exclusive access to the device
        myScanner.RetainDevice();
    }
```

然后与你已声明的设备注册事件处理程序

```Csharp
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use 
        claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();

        if(claimedBarcodeScanner != null)
        {
            // register a release request handler to prevent loss of scanner during active use
            claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

            // after successful claim, enable scanner for data events to fire
            await claimedBarcodeScanner.EnableAsync();          
        }
        else
        {
            Debug.WriteLine("Failure to claim barcodeScanner");
        }
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
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

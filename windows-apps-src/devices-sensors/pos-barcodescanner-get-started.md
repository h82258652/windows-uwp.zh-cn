---
author: TerryWarwick
title: 条形码扫描仪入门
description: 了解如何从通用 Windows 应用程序与条形码扫描仪交互
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: ed0fa79f5bbdfdaf8ca1f3273fa8d741f17efe1d
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
ms.locfileid: "1833078"
---
# <a name="getting-started-with-barcode-scanners"></a>条形码扫描仪入门

了解如何从通用 Windows 应用程序与条形码扫描仪交互。  本主题提供有关条形码扫描仪特定功能的信息。

## <a name="configuring-your-barcode-scanner"></a>配置条形码扫描仪
条形码扫描仪可以在几个不同模式下配置。  为目标应用程序正确配置条形码扫描仪非常重要。  许多条形码扫描仪可以在**键盘 wedge** 模式下配置，这让条形码扫描仪可以显示为 Windows 的键盘。  这允许你将条形码扫描到无条形码扫描仪感知的应用程序（如记事本）。  当你在此模式中扫描条形码时，来自条形码扫描仪的已解码数据将在插入点插入，就像你使用键盘键入数据。  如果你希望从 UWP 应用程序更多地控制条形码扫描仪，则需要在非键盘 wedge 模式下对其进行配置。

### <a name="usb-barcode-scanner"></a>USB 条形码扫描仪
USB 连接的条形码扫描仪必须在 **HID POS 扫描仪**模式下配置，以使用 Windows 附带的条形码扫描仪驱动程序。 此驱动程序是发布到 [**USB-HID**](http://www.usb.org/developers/hidpage/) 的 **HID 销售点使用表**规范的实现。  请参考条形码扫描仪文档或联系条形码扫描仪制造商获取启用 **HID POS 扫描仪**模式的说明。  配置为 **HID POS 扫描仪**后，条形码扫描仪将在 **POS 条形码扫描仪**节点的“设备管理器”中显示为 **POS HID 条形码扫描仪**。
你的条形码扫描仪制造商可能还有供应商特定的驱动程序，可以使用除 **HID POS 扫描仪**以外的模式支持 UWP 条形码扫描仪 API。  如果你已安装了制造商提供的与 UWP 条形码扫描仪 API 兼容的驱动程序，则可能会在“设备管理器”中的 **POS 条形码扫描仪**下看到列出的供应商特定设备。

### <a name="bluetooth-barcode-scanner"></a>蓝牙条形码扫描仪
蓝牙连接的扫描仪必须在**串行端口协议 - 简单串行接口 (SPP-SSI)** 模式下配置，以使用 UWP 条形码扫描仪 API。  请参考条形码扫描仪文档或联系条形码扫描仪制造商获取启用 **SPP-SSI 模式**的说明。  
你必须先使用“设置”-“设备”-“蓝牙和其他设备”-“添加蓝牙或其他设备”将蓝牙条形码扫描仪配对后才能够使用它。  
你可以使用 **Windows.Devices.Enumeration** 命名空间来启动和控制配对规则。  有关详细信息，请参阅[**配对设备**](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices)。

## <a name="working-with-symbologies"></a>使用标志
[**条形码标志**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)是到特定条形码格式的数据映射。 一些常见的标志包括 UPC、Code 128、QR 码等。UWP 条形码扫描仪 API 允许应用程序控制在不手动配置扫描仪的情况下扫描仪如何处理这些标志。 

### <a name="determine-which-symbologies-are-supported"></a>确定哪些标志受支持 
由于你的应用程序可能要使用来自多个制造商的不同条形码扫描仪型号，你可能需要查询以确定扫描仪支持的标志。  如果你的应用程序需要并非所有扫描仪都支持的特定标志，或你需要启用扫描仪上已手动或以编程方式禁用的标志，这可能很有用。
在通过使用 [**BarcodeScanner.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync) 获得 [**BarcodeScanner**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) 对象后，调用 [**GetSupportedSymbologiesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) 以获取设备支持的标志列表。

### <a name="determine-if-a-specific-symbology-is-supported"></a>确定是否支持特定标志
若要确定扫描仪是否支持某个特定标志，你可以调用 [**IsSymbologySupportedAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)

### <a name="changing-which-symbologies-are-recognized"></a>更改识别哪些标志
在某些情况下，你可能需要使用条形码扫描仪支持的标志的子集。  若要阻止你不打算在应用程序中使用的标志，这尤为有用。 例如，为确保用户扫描正确的条形码，你可以在获取项目 SKU 时将扫描限制为 UPC 或 EAN，在获取序列号时将扫描限制为 Code 128。
在你知道了扫描仪支持的标志后，你可以设置希望扫描仪识别的标志。  这可以在使用 [**ClaimScannerAsyc**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) 建立 [**ClaimedBarcodeScanner**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) 对象后执行。 你可以调用 [**SetActiveSymbologiesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) 来启用一组特定的标志，而从列表中略去的标志将被禁用。

### <a name="restricting-scan-data-by-data-length"></a>按数据长度限制扫描数据
某些标志的长度是可变的，如 Code 39 或 Code 128。  此标志的条形码可以放到包含通常属于特定长度的不同数据的各标志附近。 设置所需的特定数据长度可以防止无效扫描。

| 方法    | 说明 |
| :-------- | :---------- |
| [**SetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetSymbologyAttributesAsync_System_UInt32_Windows_Devices_PointOfService_BarcodeSymbologyAttributes_) | 让你可以配置所需的解码数据长度范围，以及扫描仪如何处理检查数字。 |
| [**GetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_) | 允许你检索当前的长度和检查数字配置。 |

> [!Important] 
> 通过首先检查以下属性来确认你的条形码扫描仪支持使用标志属性：[**SetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetSymbologyAttributesAsync_System_UInt32_Windows_Devices_PointOfService_BarcodeSymbologyAttributes_) 或 [**GetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_) | 允许你检索当前的长度和检查数字配置。 :
> - [**IsDecodeLengthSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)
> - [**ICheckDigitTransmissionSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitTransmissionSupported)
> - [**IsCheckDigitValidationSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitValidationSupported)

## <a name="using-software-trigger-with-barcode-scanners"></a>将软件触发器与条形码扫描仪结合使用
### <a name="initiate-scan-from-software"></a>从软件启动扫描
如果你在演示模式中使用条形码扫描仪，或者如果扫描仪没有实体触发器（如基于相机的条形码扫描仪），它对控制从软件进行扫描的操作非常有用。 你可以通过调用 [**StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync) 来启动扫描过程。  
根据 [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 的值，扫描仪可能在扫描一个条形码后即停止，或持续扫描直到你调用 [**StopSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync)。

设置所需的 [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 值来在解码条形码时控制扫描仪行为。

| 值 | 说明 |
| ----- | ----------- |
| True   | 在扫描一个条形码后停止 |
| False  | 持续扫描条形码而不停止 |


> [!Important]
> 通过首先检查属性 [**IsSoftwareTriggerSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported) 确认你的条形码扫描仪支持使用软件触发器。

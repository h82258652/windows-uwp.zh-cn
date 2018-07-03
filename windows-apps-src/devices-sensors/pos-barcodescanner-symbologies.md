---
author: TerryWarwick
title: 使用条形码扫描仪标志
description: 本文包含有关条形码扫描仪识读码制的信息。
ms.author: jken
ms.date: 05/3/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: f6e03d62316a1b842330f39ac958e4471a895815
ms.sourcegitcommit: dc3389ef2e2c94b324872a086877314d6f963358
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/11/2018
ms.locfileid: "1874519"
---
# <a name="working-with-symbologies"></a>使用标志
[条形码标志](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)是到特定条形码格式的数据映射。 一些常见的标志包括 UPC、Code 128、QR 码等。UWP 条形码扫描仪 API 允许应用程序控制在不手动配置扫描仪的情况下扫描仪如何处理这些标志。 

## <a name="determine-which-symbologies-are-supported"></a>确定哪些标志受支持 
由于你的应用程序可能要使用来自多个制造商的不同条形码扫描仪型号，你可能需要查询以确定扫描仪支持的标志。  如果你的应用程序需要并非所有扫描仪都支持的特定标志，或你需要启用扫描仪上已手动或以编程方式禁用的标志，这可能很有用。
在通过使用 [BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync) 获得 [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) 对象后，调用 [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) 获取设备支持的标志的列表。

## <a name="determine-if-a-specific-symbology-is-supported"></a>确定是否支持特定标志
若要确定扫描仪是否支持某个特定标志，可以调用 [IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)

## <a name="changing-which-symbologies-are-recognized"></a>更改识别哪些标志
在某些情况下，你可能需要使用条形码扫描仪支持的标志的子集。  若要阻止你不打算在应用程序中使用的标志，这尤为有用。 例如，为确保用户扫描正确的条形码，你可以在获取项目 SKU 时将扫描限制为 UPC 或 EAN，在获取序列号时将扫描限制为 Code 128。
在你知道了扫描仪支持的标志后，你可以设置希望扫描仪识别的标志。  这可以在使用 [ClaimScannerAsyc](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) 建立 [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) 对象后执行。 可以调用 [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) 来启用一组特定的标志，列表中略去的标志将禁用。

## <a name="restricting-scan-data-by-data-length"></a>按数据长度限制扫描数据
某些标志的长度是可变的，如 Code 39 或 Code 128。  此标志的条形码可以放到包含通常属于特定长度的不同数据的各标志附近。 设置所需的特定数据长度可以防止无效扫描。

| 方法    | 说明 |
| :-------- | :---------- |
| [SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetSymbologyAttributesAsync_System_UInt32_Windows_Devices_PointOfService_BarcodeSymbologyAttributes_) | 让你可以配置所需的解码数据长度范围，以及扫描仪如何处理检查数字。 |
| [GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_) | 允许你检索当前的长度和检查数字配置。 |

> [!Important] 
> 通过首先检查以下属性确认条形码扫描仪是否支持使用标志属性。 
> - [IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)
> - [ICheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitTransmissionSupported)
> - [IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitValidationSupported)

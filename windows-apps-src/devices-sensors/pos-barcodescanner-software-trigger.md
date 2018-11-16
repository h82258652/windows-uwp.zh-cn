---
author: eliotcowley
title: 使用软件触发器
description: 了解如何控制从软件进行扫描的操作。
ms.author: elcowle
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 03fbed5a0145a093b1e7a2535012077644aaf2e2
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2018
ms.locfileid: "6836769"
---
# <a name="use-a-software-trigger"></a>使用软件触发器

如果你在演示模式中使用条形码扫描仪，或者如果扫描仪没有实体触发器（如基于相机的条形码扫描仪），它对控制从软件进行扫描的操作非常有用。 你可以通过调用 [StartSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync) 来启动扫描过程。

根据 [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 的值，扫描仪可能在扫描一个条形码后即停止，或持续扫描直到你调用 [StopSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync)。

设置所需的 [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 值来在解码条形码时控制扫描仪行为。

| 值 | 说明 |
| ----- | ----------- |
| True   | 在扫描一个条形码后停止 |
| False  | 持续扫描条形码而不停止 |


> [!Important]
> 通过首先检查属性 [IsSoftwareTriggerSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported) 确认你的条形码扫描仪支持使用软件触发器。

下面的示例演示了如何启动使用软件触发器，这将阻止扫描后它将扫描一个条形码扫描：

```cs
private void SoftwareTrigger(BarcodeScanner barcodeScanner, ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    if (barcodeScanner.Capabilities.IsSoftwareTriggerSupported)
    {
        claimedBarcodeScanner.IsDisabledOnDataReceived = true;
        await claimedBarcodeScanner.StartSoftwareTriggerAsync();
    }
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]
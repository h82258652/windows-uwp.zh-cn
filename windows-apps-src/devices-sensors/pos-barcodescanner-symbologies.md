---
author: TerryWarwick
title: 使用条形码扫描仪标志
description: 本文包含有关条形码扫描仪识读码制的信息。
ms.author: jken
ms.date: 08/29/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 8bd1dffe4da7b3725ef7716fe9cf28bdf8eaf34f
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2018
ms.locfileid: "3663497"
---
# <a name="working-with-symbologies"></a>使用标志
[条形码标志](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)是到特定条形码格式的数据映射。 一些常见的标志包括 UPC、 Code 128、 QR 码等。  通用 Windows 平台条形码扫描仪 Api 允许应用程序控制扫描仪如何处理这些标志而无需手动配置扫描仪。 

## <a name="determine-which-symbologies-are-supported"></a>确定哪些标志受支持 
由于你的应用程序可能要使用来自多个制造商的不同条形码扫描仪型号，你可能需要查询以确定扫描仪支持的标志。  如果你的应用程序需要并非所有扫描仪都支持的特定标志，或你需要启用扫描仪上已手动或以编程方式禁用的标志，这可能很有用。

在通过使用 [BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync) 获得 [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) 对象后，调用 [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) 获取设备支持的标志的列表。

下面的示例获取的条形码扫描仪，受支持的标志列表，并将它们显示在文本块：

```cs
private void DisplaySupportedSymbologies(BarcodeScanner barcodeScanner, TextBlock textBlock) 
{
    var supportedSymbologies = await barcodeScanner.GetSupportedSymbologiesAsync();

    foreach (uint item in supportedSymbologies)
    {
        string symbology = BarcodeSymbologies.GetName(item);
        textBlock.Text += (symbology + "\n");
    }
}
```

## <a name="determine-if-a-specific-symbology-is-supported"></a>确定是否支持特定标志
若要确定扫描仪是否支持某个特定标志可以调用[IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)。

下面的示例检查条形码扫描仪是否支持**Code32**标志：

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>更改识别哪些标志
在某些情况下，你可能需要使用条形码扫描仪支持的标志的子集。  若要阻止你不打算在应用程序中使用的标志，这尤为有用。 例如，为确保用户扫描正确的条形码，你可以在获取项目 SKU 时将扫描限制为 UPC 或 EAN，在获取序列号时将扫描限制为 Code 128。

在你知道了扫描仪支持的标志后，你可以设置希望扫描仪识别的标志。  这可以完成后使用[ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)对象。 可以调用 [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) 来启用一组特定的标志，列表中略去的标志将禁用。

下面的示例将活动的已声明的条形码扫描仪标志设置为[Code39](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39)和[Code39Ex](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex):

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>条形码标志属性
不同条形码标志可以有不同的属性，例如支持多个解码一部分的原始数据传输到主机检查数字的长度和检查数字验证。 与[BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)类中，你可以获取和设置给定[ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)和条形码标志这些属性。

你可以获取与[GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_)给定标志的属性。 下面的代码段获取**ClaimedBarcodeScanner**Upca 标志的属性。

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

当你完成修改的属性，并准备好进行设置时，你可以调用[SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync)。 此方法返回一个**布尔值**，如果已成功设置的属性，这为**true** 。

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>限制扫描数据按数据长度
某些标志的长度是可变的，如 Code 39 或 Code 128。  这些标志的条形码可以找到彼此靠近包含通常属于特定长度的不同数据。 设置所需的特定数据长度可以防止无效扫描。

在设置之前解码长度，检查条形码标志是否支持多个与[IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)长度。 一旦你知道它受支持，你可以设置[DecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind)，该常量的类型[BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)。 此属性可以是以下值之一：

* **AnyLength**： 解码的任意数量的长度。
* **离散**： 解码的[DecodeLength1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1)或[DecodeLength2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2)单字节字符长度。
* **范围**： 解码**DecodeLength1**和**DecodeLength2**单字节字符之间的长度。 **DecodeLength1**和**DecodeLength2**执行几 （可以是更高版本或低于其他） 的顺序。

最后，你可以设置**DecodeLength1**和**DecodeLength2**来控制所需的数据长度的值。

以下代码片段演示了设置解码长度：

```cs
private async Task<bool> SetDecodeLength(
    ClaimedBarcodeScanner scanner,
    uint symbology, 
    BarcodeSymbologyDecodeLengthKind kind, 
    uint decodeLength1, 
    uint decodeLength2)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsDecodeLengthSupported)
    {
        attributes.DecodeLengthKind = kind;
        attributes.DecodeLength1 = decodeLength1;
        attributes.DecodeLength2 = decodeLength2;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-transmission"></a>检查数字传输

你可以设置某个标志的另一个属性是是否检查数字将传输到主机的原始数据的一部分。 将此设置之前，请确保标志支持检查[IsCheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported)数字传输。 然后，设置是否使用[IsCheckDigitTransmissionEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled)启用检查数字传输。

以下代码片段演示了设置检查数字传输：

```cs
private async Task<bool> SetCheckDigitTransmission(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitTransmissionSupported)
    {
        attributes.IsCheckDigitTransmissionEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-validation"></a>检查数字验证

你还可以设置是否将验证条形码检查数字。 将此设置之前，请确保标志支持检查数字验证通过[IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported)。 然后，设置是否使用[IsCheckDigitValidationEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled)启用检查数字验证。

以下代码片段演示了设置检查数字验证：

```cs
private async Task<bool> SetCheckDigitValidation(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitValidationSupported)
    {
        attributes.IsCheckDigitValidationEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>另请参阅

* [条形码扫描仪](pos-barcodescanner.md)
* [BarcodeSymbologies 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
* [BarcodeScanner 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [ClaimedBarcodeScanner 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)
* [BarcodeSymbologyAttributes 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)
* [BarcodeSymbologyDecodeLengthKind 枚举](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)
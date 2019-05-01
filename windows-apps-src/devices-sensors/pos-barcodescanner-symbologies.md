---
title: 使用条形码扫描仪标志
description: 本文包含有关条形码扫描仪识读码制的信息。
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: ee78ffbc49fdcb7f8e87844dea1e2ce29297e9f3
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63816686"
---
# <a name="working-with-symbologies"></a>使用标志
[条形码标志](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)是到特定条形码格式的数据映射。 一些常见符号包括 UPC、 代码 128、 QR 代码等。  通用 Windows 平台条形码扫描程序 Api 允许应用程序可以控制如何扫描程序无需手动配置扫描程序处理这些符号。 

## <a name="determine-which-symbologies-are-supported"></a>确定哪些标志受支持 
由于你的应用程序可能要使用来自多个制造商的不同条形码扫描仪型号，你可能需要查询以确定扫描仪支持的标志。  如果你的应用程序需要并非所有扫描仪都支持的特定标志，或你需要启用扫描仪上已手动或以编程方式禁用的标志，这可能很有用。

在通过使用 [BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync) 获得 [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) 对象后，调用 [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) 获取设备支持的标志的列表。

下面的示例获取条形码扫描仪支持符号的列表，并将其显示在文本块：

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
若要确定扫描程序是否支持可以调用特定符号[IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)。

以下示例将检查是否支持条码扫描器**Code32**符号：

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>更改识别哪些符号
在某些情况下，你可能需要使用条形码扫描仪支持的标志的子集。  若要阻止你不打算在应用程序中使用的标志，这尤为有用。 例如，为确保用户扫描正确的条形码，你可以在获取项目 SKU 时将扫描限制为 UPC 或 EAN，在获取序列号时将扫描限制为 Code 128。

在你知道了扫描仪支持的标志后，你可以设置希望扫描仪识别的标志。  这可以完成后已建立[ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)对象使用[ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync)。 可以调用 [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) 来启用一组特定的标志，列表中略去的标志将禁用。

下面的示例设置向已声明的条码扫描器 active 符号[Code39](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39)并[Code39Ex](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex):

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>条码符号属性
不同的条形码符号可以具有不同的属性，例如支持多个解码长度，作为原始数据的一部分传输到主机校验位，并检查数字验证。 与[BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)类，可以获取和设置这些属性的给定[ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)和条码符号。

你可以获取与给定符号的属性[GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_)。 以下代码段将获取有关 Upca 符号的属性**ClaimedBarcodeScanner**。

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

完成修改属性和已准备好将其设置后，可以调用[SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync)。 此方法返回**bool**，即**true**如果属性已成功设置。

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>限制扫描数据的数据长度
某些标志的长度是可变的，如 Code 39 或 Code 128。  可以包含不同的数据通常特定长度的相邻位于这些符号的条形码。 设置所需的特定数据长度可以防止无效扫描。

在解码长度设置之前, 检查条码符号是否支持使用多个长度[IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)。 一旦您知道支持此功能，就可以设置[DecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind)，它属于类型[BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)。 此属性可以是以下值之一：

* **AnyLength**:解码的任意数量的长度。
* **离散**:解码的长度[DecodeLength1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1)或[DecodeLength2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2)单字节字符。
* **范围**:解码之间的长度**DecodeLength1**并**DecodeLength2**单字节字符。 顺序**DecodeLength1**并**DecodeLength2**做的事项 （可以是高于还是低于其他）。

最后，您可以设置的值**DecodeLength1**并**DecodeLength2**来控制所需的数据的长度。

下面的代码段演示如何设置解码长度：

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

您可以设置符号的另一个属性是是否检查数字不会传输到主机作为原始数据的一部分。 在此项设置之前, 请确保符号支持检查与数字传输[IsCheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported)。 然后，设置是否检查数字传输启用了[IsCheckDigitTransmissionEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled)。

下面的代码段演示了如何设置检查数字传输：

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

此外可以设置是否将验证条形码校验位。 在此项设置之前, 请确保符号支持检查使用的数字验证[IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported)。 然后，设置是否检查数字验证启用了[IsCheckDigitValidationEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled)。

下面的代码段演示如何设置检查数字验证：

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

## <a name="see-also"></a>请参阅

* [条形码扫描程序](pos-barcodescanner.md)
* [BarcodeSymbologies 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
* [BarcodeScanner 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [ClaimedBarcodeScanner 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)
* [BarcodeSymbologyAttributes 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)
* [BarcodeSymbologyDecodeLengthKind 枚举](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)
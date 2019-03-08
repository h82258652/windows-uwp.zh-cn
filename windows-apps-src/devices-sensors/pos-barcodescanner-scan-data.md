---
title: 获取并了解条形码数据
description: 了解如何获取和解释，扫描条码数据。
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ece246ffd369ee21c089598f07b2566424757f55
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605962"
---
# <a name="obtain-and-understand-barcode-data"></a>获取并了解条形码数据

一旦设置了条形码扫描程序，您当然需要一种了解您扫描的数据。 扫描条形码，当[DataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived)引发事件。 [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)应订阅此事件。 **DataReceived**事件传递[BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)对象，可用于访问条码数据。

## <a name="subscribe-to-the-datareceived-event"></a>订阅 DataReceived 事件

一旦您有**ClaimedBarcodeScanner**，将其订阅**DataReceived**事件：

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

将传递的事件处理程序**ClaimedBarcodeScanner**和一个**BarcodeScannerDataReceivedEventArgs**对象。 可以通过此对象的访问条码数据[报表](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report)属性，其类型[BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)。

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>获取数据

一旦您有**BarcodeScannerReport**，可以访问和分析条码数据。 **BarcodeScannerReport**具有三个属性：

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata):完整的原始条码数据。
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel):已解码的条形码标签，这不包括标头、 校验和和其他杂项信息。
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype):已解码的条形码标签类型。 可能的值中定义[BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)类。

如果你想要两个位置访问**ScanDataLabel**或**ScanDataType**，则必须首先设置[IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled)到**true**。

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>获取扫描数据类型

获取已解码的条形码标签类型是相当普通&mdash;我们只需调用[GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)上**ScanDataType**。

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>获取扫描数据标签

若要获取的已解码的条形码标签，有几件事需要注意。 只有特定数据类型包含编码的文本，因此首先应检查是否符号可以转换为一个字符串，然后将转换从我们获取的缓冲区**ScanDataLabel**到已编码的 utf-8 字符串。

```cs
private string GetDataLabel(BarcodeScannerDataReceivedEventArgs args)
{
    uint scanDataType = args.Report.ScanDataType;

    // Only certain data types contain encoded text.
    // To keep this simple, we'll just decode a few of them.
    if (args.Report.ScanDataLabel == null)
    {
        return "No data";
    }

    // This is not an exhaustive list of symbologies that can be converted to a string.
    else if (scanDataType == BarcodeSymbologies.Upca ||
        scanDataType == BarcodeSymbologies.UpcaAdd2 ||
        scanDataType == BarcodeSymbologies.UpcaAdd5 ||
        scanDataType == BarcodeSymbologies.Upce ||
        scanDataType == BarcodeSymbologies.UpceAdd2 ||
        scanDataType == BarcodeSymbologies.UpceAdd5 ||
        scanDataType == BarcodeSymbologies.Ean8 ||
        scanDataType == BarcodeSymbologies.TfStd)
    {
        // The UPC, EAN8, and 2 of 5 families encode the digits 0..9
        // which are then sent to the app in a UTF8 string (like "01234").
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanDataLabel);
    }

    // Some other symbologies (typically 2-D symbologies) contain binary data that
    // should not be converted to text.
    else
    {
        return "Decoded data unavailable.";
    }
}
```

### <a name="get-the-raw-scan-data"></a>获取原始扫描数据

若要从条码中获取完整的原始数据，我们只需转换从我们获取的缓冲区**ScanData**转换为字符串。

```cs
private string GetRawData(BarcodeScannerDataReceivedEventArgs args)
{
    // Get the full, raw barcode data.
    if (args.Report.ScanData == null)
    {
        return "No data";
    }

    // Just to show that we have the raw data, we'll print the value of the bytes.
    else
    {
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanData);
    }
}
```

这些数据位于，一般情况下，扫描程序从提供的格式。 消息标头和尾部信息会删除，但是，因为它们不包含应用程序的有用信息，可能需要特定于扫描程序的。

常见标头信息是前缀字符 （如 STX 字符）。 如果扫描程序将生成一个，常见的尾部信息会终止符字符 （如 ETX 或 CR 字符） 和块检查字符。

此属性应包含一个符号字符，如果其中一个返回的扫描程序 (例如， **A**的 UPC 一个)。 它还应包括检查位数字，如果它们存在的标签中，并返回的扫描程序。 （请注意，符号字符和校验位可能或可能不会显示，取决于扫描程序配置。 如果扫描程序将返回其存在，但将不生成，或者如果它们没有显示计算它们。)

可能会将一些商品标有补充条形码。 此条形码通常位于右侧的主要条形码，并包含的信息的其他两个或五个字符。 如果扫描程序读取商品，其中包含 main 和补充条形码，补充字符追加到 main 的字符，并将结果传递给应用程序作为一个标签。 （请注意，扫描程序可能支持的配置，启用或禁用的补充代码读取）。

可能使用多个标签，有时称为标记一些商品*multisymbol 标签*或*分层标签*。 这些条形码通常垂直排列和可能的相同或不同符号。 如果扫描程序读取包含多个标签的商品，每个条形码传送到应用程序作为一个单独的标签。 这是由于当前标准化这些条形码类型缺少必需的。 其中一个不能确定所有变体取决于各个条码数据。 因此，应用程序将需要确定当多个标签条形码读取基于返回的数据。 （请注意，扫描程序可能会或可能不支持读取多个标签。）

此值设置为之前**DataReceived**到应用程序引发事件。

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>另请参阅
* [条形码扫描程序](pos-barcodescanner.md)
* [ClaimedBarcodeScanner 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [BarcodeScannerDataReceivedEventArgs 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [BarcodeScannerReport 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [BarcodeSymbologies 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)

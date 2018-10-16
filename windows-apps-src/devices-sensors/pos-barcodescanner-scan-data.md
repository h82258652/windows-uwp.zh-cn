---
author: eliotcowley
title: 获取并了解条形码数据
description: 了解如何获取和解释你扫描条形码数据。
ms.author: elcowle
ms.date: 08/29/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 0992ea54092063ba53f23871599905e58f1b456e
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2018
ms.locfileid: "4679596"
---
# <a name="obtain-and-understand-barcode-data"></a>获取并了解条形码数据

一旦你已设置你的条形码扫描仪，你当然需要一种了解你扫描的数据。 当扫描条形码时，会引发的[DataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived)事件。 [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)应该订阅此事件。 **DataReceived**事件传递一个[BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)对象，它可用于访问的条形码数据。

## <a name="subscribe-to-the-datareceived-event"></a>订阅 DataReceived 事件

一旦**ClaimedBarcodeScanner**，使其订阅**DataReceived**事件：

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

**ClaimedBarcodeScanner**和**BarcodeScannerDataReceivedEventArgs**对象将传递的事件处理程序。 你可以通过此对象的[报告](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report)，则该属性的类型[BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)访问的条形码数据。

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>获取数据

**BarcodeScannerReport**后，你可以访问和分析条形码数据。 **BarcodeScannerReport**具有三个属性：

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata)： 完整、 原始条形码数据。
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel)： 解码的条形码标签，其中不包括在标头、 校验和、 和其他信息。
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype)： 解码的条形码标签类型。 [BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)类中定义的可能值。

如果你想要访问**ScanDataLabel**或**ScanDataType**，则必须首先设置为**true**的[IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) 。

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>获取扫描数据类型

获取已解码的条形码标签类型都相当简单&mdash;我们只需调用[GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) **ScanDataType**。

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>获取扫描数据标签

若要获取已解码的条形码标签，有你需要注意的一些事项。 只有某些数据类型包含编码的文本，因此你应该首先检查是否标志可以转换为字符串，，然后将转换我们从**ScanDataLabel**获取为 utf-8 编码字符串的缓冲区。

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

若要获取完整，原始数据从条形码，我们只需将转换我们从**ScanData**获取转换为字符串的缓冲区。

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

这些数据是，一般情况下，格式为从扫描仪传递。 邮件标头和预告片信息删除，但是，因为它们不包含应用程序的有用信息，并且可能是特定于扫描仪的。

常见的标头信息是前缀字符 （如 STX 字符）。 预告片的常见信息是终止符字符 （如 ETX 或回车字符） 和一个块检查字符，如果扫描仪将生成一个。

此属性应包含符号字符，如果扫描仪将返回 (例如，**一个**用于 UPC A)。 它还应包括检查数字，如果它们存在标签中，并返回的扫描仪。 （请注意，符号字符和检查数字可能会也可能不存在，取决于扫描仪配置。 扫描仪将其返回，如果存在，但不是会生成或计算它们，如果它们不存在。)

某些商品可能带有补充条形码。 此条形码通常位于右侧的主条形码，并由其他两个或五个字符的信息。 如果扫描仪读取商品，其中包含主要和补充条形码、 补充的字符将附加到的主角，和结果传递给该应用程序作为一个标签。 （请注意，扫描仪可能支持的配置，启用或禁用的补充代码的读数。）

可能会使用多个标签，有时称为*multisymbol 标签*或*分层的标签*标记一些商品。 这些条形码通常垂直排列，并且可能会引起的相同或不同的标志。 如果扫描仪读取包含多个标签的商品，每个条形码会传递给该应用程序作为单独的标签。 这是标准化的由于这些条形码类型当前缺少必需的。 一个不能确定取决于单个条形码数据的所有变体。 因此，应用程序将需要确定当多个标签条形码已阅读根据返回的数据。 （请注意，扫描仪可能会也可能不支持多个标签读取）。

此值将在对应用程序在引发**DataReceived**事件之前。

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>另请参阅
* [条形码扫描仪](pos-barcodescanner.md)
* [ClaimedBarcodeScanner 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [BarcodeScannerDataReceivedEventArgs 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [BarcodeScannerReport 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [BarcodeSymbologies 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
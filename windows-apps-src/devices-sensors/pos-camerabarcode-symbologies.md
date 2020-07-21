---
title: 相机条形码扫描仪标志
description: 相机条形码扫描仪支持的标志
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 481d10f2fea076f45124a3c75819dfe6494300bf
ms.sourcegitcommit: 48e047a581fcfcc9a4084d65a78b89f2c01cf4f3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2020
ms.locfileid: "85448397"
---
# <a name="symbologies"></a>标志

本主题为 Windows 10 附带的软件条形码解码器支持的每个标志提供示例条形码，包括：UPC/EAN、Code 39、Code 128、Interleaved 2 of 5、Databar Omnidirectional、Databar Stacked、QR 码和 GS1DWCode。

Windows 10 使用与软件解码器结合使用的标准镜头照相机来生成条形码扫描器。 本文是指软件解码器支持的符号。 具有内置硬件解码器的专用条形码扫描器设备可能支持其他符号，请联系条形码扫描器制造商获取详细信息。 除非另行指定，否则在 Windows 10 build 17134 或更高版本的所有版本中都支持所列的符号。

使用[GetSupportedSymbologiesAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync)确定条形码扫描器支持的特定符号。

> [!NOTE]
> Windows 10 中内置的软件解码器由[*Digimarc Corporation*](https://www.digimarc.com/)提供。

## <a name="1d-symbologies"></a>1D 标志

### <a name="code-39"></a>Code 39
![示例条形码 - Code 39](images/pos/sample-barcode-code39.png)

### <a name="code-128"></a>Code 128
![示例条形码 - Code 128](images/pos/sample-barcode-code128.png)

### <a name="databar-omnidirectional"></a>Databar Omnidirectional
![示例条形码 - Databar Omnidirectional](images/pos/sample-barcode-databar-omnidirectional.png) 
### <a name="databar-stacked"></a>Databar Stacked
![示例条形码 - Databar Stacked](images/pos/sample-barcode-databar-stacked.png)

### <a name="ean-8"></a>EAN-8
![示例条形码 - EAN-8](images/pos/sample-barcode-ean8.png)

### <a name="ean-13"></a>EAN-13
![示例条形码 - EAN-13](images/pos/sample-barcode-ean13.png)

### <a name="interleaved-2-of-5"></a>Interleaved 2 of 5
![示例条形码 - Interleaved 2 of 5](images/pos/sample-barcode-interleaved-2-of-5.png)

### <a name="upc-a"></a>UPC-A
![示例条形码 - UPC A](images/pos/sample-barcode-upca.png)

### <a name="upc-e"></a>UPC-E
![示例条形码 - UPC E](images/pos/sample-barcode-upce.png)

## <a name="2d-symbologies"></a>2D 标志
### <a name="qr-code"></a>QR 码
![示例条形码 - QR 码](images/pos/sample-barcode-qrcode.png)

## <a name="digital-watermark"></a>数字水印
### <a name="gs1-dwcode"></a>GS1-DWCode

请使用你的相机条形码扫描仪应用程序扫描下方的包装图像查看实际的 GS1DWCode。  此图像使用 UPCA 856107006854 编码。  请访问 http://www.digimarc.com 了解有关 GS1DWCode 功能的详细信息。

![示例条形码 - GS1DWCode](images/pos/Rice-Box-V7.jpg)

## <a name="see-also"></a>请参阅

### <a name="samples"></a>示例

- [条形码扫描仪示例](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)

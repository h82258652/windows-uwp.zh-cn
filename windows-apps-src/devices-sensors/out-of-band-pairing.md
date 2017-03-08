---
author: IvorB
ms.assetid: E9ADC88F-BD4F-4721-8893-0E19EA94C8BA
title: "带外配对"
description: "带外配对使应用无需发现即可连接到服务点外围设备。"
ms.author: ivorb
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: dc0bad7d8625817cfffcc84f89afeed96a07a21f
ms.lasthandoff: 02/07/2017

---
# <a name="out-of-band-pairing"></a>带外配对

带外配对使应用无需发现即可连接到服务点外围设备。 应用必须使用 [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/windows.devices.pointofservice.aspx) 命名空间并将特定格式的字符串（带外 blob）传递给外设所需的相应的 **FromIdAsync** 方法。 当执行 **FromIdAsync** 时，在该操作返回到调用方之前，主机设备要进行配对并连接到外设。

## <a name="out-of-band-blob-format"></a>带外 blob 格式

```json
    "connectionKind":"Network",
    "physicalAddress":"AA:BB:CC:DD:EE:FF",
    "connectionString":"192.168.1.1:9001",
    "peripheralKinds":"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}",
    "providerId":"{02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2}",
    "providerName":"PrinterProtocolProvider.dll"
```

**connectionKind** - 连接类型。 有效值为“网络”和“蓝牙”。

**physicalAddress** - 外设的 MAC 地址。 例如，如果是网络打印机，则打印机测试表提供的 MAC 地址将采用 AA:BB:CC:DD:EE:FF 格式。

**connectionString** - 外设的连接字符串。 例如，如果是网络打印机，则打印机测试表提供的 IP 地址将采用 192.168.1.1:9001 格式。 所有蓝牙外设会省略此字段。

**peripheralKinds** - 设备类型的 GUID。 有效值为：

| 设备类型 | GUID |
| ---- | ---- |
| *POS 打印机* | C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33 |
| *条形码扫描仪* | C243FFBD-3AFC-45E9-B3D3-2BA18BC7EBC5 |
| *收银机* | 772E18F2-8925-4229-A5AC-6453CB482FDA |


**providerId** -协议提供程序类的 GUID。 有效值为：

| 协议提供程序类 | GUID |
| ---- | ---- |
| *通用 ESC/POS 网络打印机* | 02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2 |
| *通用 ESC/POS BT 打印机* | CCD5B810-95B9-4320-BA7E-78C223CAF418 |
| *Epson BT 打印机* | 94917594-544F-4AF8-B53B-EC6D9F8A4464 |
| *Epson 网络打印机* | 9F0F8BE3-4E59-4520-BFBA-AF77614A31CE |
| *星形网络打印机* | 1E3A32C2-F411-4B8C-AC91-CC2C5FD21996 |
| *套接字 BT 扫描仪* | 6E7C8178-A006-405E-85C3-084244885AD2 |
| *APG 网络收银机* | E619E2FE-9489-4C74-BF57-70AED670B9B0 |
| *APG BT 收银机* | 332E6550-2E01-42EB-9401-C6A112D80185 |


**providerName** - 提供程序 DLL 的名称。 默认提供程序包括：

| 提供程序 | DLL 名称 |
| ---- | ---- |
| 打印机 | PrinterProtocolProvider.dll |
| 收银机 | CashDrawerProtocolProvider.dll |
| 扫描仪 | BarcodeScannerProtocolProvider.dll |

## <a name="usage-example-network-printer"></a>使用示例：网络打印机

```csharp
String oobBlobNetworkPrinter =
  "{\"connectionKind\":\"Network\"," +
  "\"physicalAddress\":\"AA:BB:CC:DD:EE:FF\"," +
  "\"connectionString\":\"192.168.1.1:9001\"," +
  "\"peripheralKinds\":\"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}\"," +
  "\"providerId\":\"{02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2}\"," +
  "\"providerName\":\"PrinterProtocolProvider.dll\"}";

printer = await PosPrinter.FromIdAsync(oobBlobNetworkPrinter);
```

## <a name="usage-example-bluetooth-printer"></a>使用示例：蓝牙打印机

```csharp
string oobBlobBTPrinter =
    "{\"connectionKind\":\"Bluetooth\"," +
    "\"physicalAddress\":\"AA:BB:CC:DD:EE:FF\"," +
    "\"peripheralKinds\":\"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}\"," +
    "\"providerId\":\"{CCD5B810-95B9-4320-BA7E-78C223CAF418}\"," +
    "\"providerName\":\"PrinterProtocolProvider.dll\"}";

printer = await PosPrinter.FromIdAsync(oobBlobBTPrinter);

```


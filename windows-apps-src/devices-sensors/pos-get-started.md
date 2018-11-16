---
author: TerryWarwick
title: 服务点入门
description: 本文包含有关服务点 UWP API 入门的信息。
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: f3959254787ce22bea27495520805485e0ea179b
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "6995121"
---
# <a name="getting-started-with-point-of-service"></a>服务点入门

服务点、销售点或服务点设备是用于帮助零售交易的计算机外设。 服务点设备的示例包括电子收银机、条形码扫描仪、磁条阅读器和收据打印机。

下面将介绍使用通用 Windows 平台 (UWP) PointOfService API 连接服务点设备的基础知识。 我们将介绍设备枚举、检查设备功能、声明设备和设备共享。 我们以条形码扫描仪设备为例，但这里的几乎所有指南都适用于任何兼容 UWP 的服务点设备。 （有关受支持的设备的列表，请参阅[服务点设备支持](pos-device-support.md)）。

## <a name="finding-and-connecting-to-point-of-service-peripherals"></a>查找和连接到服务点外设

应用可以使用服务点设备之前，必须与运行应用的电脑配对。 可通过多种方法连接到服务点设备，比如通过编程方式或通过“设置”应用。

### <a name="connecting-to-devices-by-using-the-settings-app"></a>通过使用“设置”应用连接到设备
将服务点设备（如条形码扫描仪）插入电脑后，它将像任何其他设备一样显示。 你可以在“设置”应用的**设备 > 蓝牙和其他设备**部分找到。 在此位置，你可以通过选择**添加蓝牙或其他设备**与服务点设备配对。

有些服务点设备可能不会在“设置”应用中显示，直到使用服务点 API 以编程方式对其进行枚举。

### <a name="getting-a-single-point-of-service-device-with-getdefaultasync"></a>GetDefaultAsync 获取单个服务点设备
在一个简单的用例中，你可能仅将一个服务点外设连接到运行应用的电脑上，并且希望尽快对其进行设置。 若要执行该操作，请使用此处所示的 **GetDefaultAsync** 方法检索“默认”设备。

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

如果找到默认设备，就可以准备声明检索到的设备对象。 “声明”设备向应用程序提供它的独占访问权，防止来自多个进程的命令冲突。

> [!NOTE] 
> 如果多个服务点设备连接到电脑，**GetDefaultAsync** 将返回它发现的第一个设备。 出于此原因，请使用 **FindAllAsync**，除非你确定只有一个服务点设备对应用程序可见。

### <a name="enumerating-a-collection-of-devices-with-findallasync"></a>使用 FindAllAsync 枚举设备的集合

当连接到多台设备时，你必须枚举 **PointOfService** 设备对象的集合才能找到你想要声明的设备对象。 例如，以下代码创建当前连接的所有条形码扫描仪的集合，然后搜索集合中具有特定名称的扫描仪。

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;

string selector = BarcodeScanner.GetDeviceSelector();       
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
    if (devInfo.Name.Contains("1200G"))
    {
        Debug.WriteLine(" Found one");
    }
}
```

### <a name="scoping-the-device-selection"></a>限定设备选择的范围
连接到设备时，你可能希望将搜索限定在你的应用有权访问的一组服务点外设。 使用 **GetDeviceSelector** 方法可以限定选择的范围，以通过特定的方法（蓝牙、USB 等）仅检索连接的设备。 你可以创建通过**蓝牙**、**IP**、**本地**或**所有连接类型**搜索设备的选择器。 这可能很有用，因为无线设备发现需要的时间比本地（有线）发现要长。 你可以通过将 **FindAllAsync** 限制为**本地**连接类型来确保本地设备连接的确定性等待时间。 例如，此代码检索所有可通过本地连接访问的条形码扫描仪。 

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

### <a name="reacting-to-device-connection-changes-with-devicewatcher"></a>对设备连接的反应根据 DeviceWatcher 进行改变

当你的应用运行时，有时设备会断开连接或更新，或需要添加新设备。 你可以使用 **DeviceWatcher** 类访问设备相关的事件，使你的应用可以作出相应的响应。 下面是如何使用 **DeviceWatcher** 的示例，如果添加、删除或更新设备，将调用方法存根。

```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Removed += DeviceWatcher_Removed;
deviceWatcher.Updated += DeviceWatcher_Updated;

void DeviceWatcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    // TODO: Add the DeviceInformation object to your collection
}

void DeviceWatcher_Removed(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Remove the item in your collection associated with DeviceInformationUpdate
}

void DeviceWatcher_Updated(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Update your collection with information from DeviceInformationUpdate
}
```

## <a name="checking-the-capabilities-of-a-point-of-service-device"></a>检查服务点设备的功能
即使在设备类范围内，如条形码扫描仪，每台设备的属性可能会根据型号的不同而有很大的差异。 如果你的应用需要特定的设备属性，你可能需要检查每个连接的设备对象，以确定是否支持属性。 例如，你的企业可能要求使用特定的条形码打印模式创建标签。 下面介绍如何检查以查看已连接的条形码扫描仪是否支持某个符号。 

> [!NOTE]
> 符号是条形码用于编码消息的语言映射。

```Csharp
try
{
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(deviceId);
    if (await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32))
    {
        Debug.WriteLine("Has symbology");
    }
}
catch (Exception ex)
{
    Debug.WriteLine("FromIdAsync() - " + ex.Message);
}
```

### <a name="using-the-devicecapabilities-class"></a>使用 Device.Capabilities 类
**Device.Capabilities** 类是所有服务点设备类的属性，用于获取有关每个设备的一般信息。 例如，此示例中确定设备是否支持统计报告，且如果支持，则检索受支持的任何类型的统计信息。

```Csharp
try
{
    if (barcodeScanner.Capabilities.IsStatisticsReportingSupported)
    {
        Debug.WriteLine("Statistics reporting is supported");

        string[] statTypes = new string[] {""};
        IBuffer ibuffer = await barcodeScanner.RetrieveStatisticsAsync(statTypes);
    }
}
catch (Exception ex)
{
    Debug.WriteLine("EX: RetrieveStatisticsAsync() - " + ex.Message);
}
```

## <a name="claiming-a-point-of-service-device"></a>声明服务点设备
在你可以使用服务点设备进行活动输入或输出前，你必须声明它，授予应用程序对其许多功能的独占访问权。 此代码演示在你使用前文所述的其中一种方法找到设备后，如何声明条形码扫描仪设备。

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}
```

### <a name="retaining-the-device"></a>保留设备
当通过网络或蓝牙连接使用服务点设备时，你可能希望与网络上的其他应用共享该设备。 （有关详细信息，请参阅[共享设备](#sharing-a-device-between-apps)。）在其他情况下，你可能要保留此设备以便长期使用。 此示例演示在另一个应用请求发布设备后，如何保留已声明的条形码扫描仪。

```Csharp
claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner e)
{
    e.RetainDevice();  // Retain exclusive access to the device
}
```

## <a name="input-and-output"></a>输入和输出

声明设备后，你几乎可以准备使用它了。 若要接受来自该设备的输入，必须设置和启用一个用来接收数据的代理。 在下面的示例中，我们声明了一个条形码扫描仪设备，并设置其解码属性，然后调用 **EnableAsync** 启用对来自设备的输入解码。 此过程因设备类而异，因此若要获取有关如何设置用于非条形码设备的代理的指南，请参阅相关的 [UWP 应用示例](https://github.com/Microsoft/Windows-universal-samples#devices-and-sensors)。

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
    if (claimedBarcodeScanner != null)
    {
        claimedBarcodeScanner.DataReceived += claimedBarcodeScanner_DataReceived;
        claimedBarcodeScanner.IsDecodeDataEnabled = true;
        await claimedBarcodeScanner.EnableAsync();
    }
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}


void claimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    string symbologyName = BarcodeSymbologies.GetName(args.Report.ScanDataType);
    var scanDataLabelReader = DataReader.FromBuffer(args.Report.ScanDataLabel);
    string barcode = scanDataLabelReader.ReadString(args.Report.ScanDataLabel.Length);
}
```

## <a name="sharing-a-device-between-apps"></a>在不同应用间共享设备

服务点设备通常用于在短时间内有多个应用需要访问它们的情况。  通过蓝牙或 IP 网络本地（USB 或其他有线连接）连接到多个应用的设备可以共享。 根据每个应用的需要，一个进程可能需要释放它在设备上的声明。 此代码释放我们已声明的条形码扫描仪设备，允许其他应用声明并使用它。

```Csharp
if (claimedBarcodeScanner != null)
{
    claimedBarcodeScanner.Dispose();
    claimedBarcodeScanner = null;
}
```

> [!NOTE]
> 已声明和未声明的服务点设备类会实现 [IClosable 接口](https://docs.microsoft.com/uwp/api/windows.foundation.iclosable)。 如果设备已通过网络或蓝牙连接到应用，在连接另一个应用前必须丢弃已声明和未声明的对象。

## <a name="see-also"></a>另请参阅
+ [条形码扫描仪示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [收银机示例]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [行显示示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [磁条阅读器示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [POSPrinter 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)


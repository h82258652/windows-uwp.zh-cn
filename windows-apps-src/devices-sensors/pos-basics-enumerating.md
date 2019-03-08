---
title: 枚举 PointOfService 设备
description: 了解如何枚举 PointOfService 设备
ms.date: 10/08/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 27d25864941b9d73c9b12e6329eab79fac1b15bf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620902"
---
# <a name="enumerating-point-of-service-devices"></a>枚举服务点设备
在本部分中，您将学习如何[定义设备选择器](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector)，用来查询设备，系统和使用此选择器来枚举点的服务设备使用以下方法之一：

**方法 1:**[使用设备选取器](#method-1-use-a-device-picker)
<br/>
显示设备选取器 UI，让用户选择已连接的设备。 此方法处理更新的列表，当设备连接，并将其删除，并且是更简单且比其他方法更安全。

**方法 2:**[获取第一个可用的设备](#method-2-get-first-available-device)<br />使用[GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync)访问特定的服务点设备类中第一个可用的设备。

**方法 3:**[设备的快照](#method-3-snapshot-of-devices)<br />枚举的时间在出现在给定时点系统上的点的服务设备的快照。 当你想要生成自己的 UI 或需要枚举设备而无需向用户显示 UI 时这会很有用。 [FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)会阻止结果之前完成整个枚举。

**方法 4:**[枚举并观看](#method-4-enumerate-and-watch)<br />[DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)是更为强大和灵活枚举模型，可用于枚举当前存在的设备，并且还会添加或从系统中删除设备时收到通知。  当你想要在后台维护当前的设备列表以显示在 UI 中而不是等待快照发生时，这非常有用。

## <a name="define-a-device-selector"></a>定义设备选择器
设备选择器将使你可以在枚举设备时限制要搜索的设备。  这将允许您以仅获取相关结果，并减少枚举所需的设备所需的时间。

可以使用**GetDeviceSelector**的所需的设备选择器获取该类型的设备类型的方法。 例如，使用[PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector)将为您提供一个选择器来枚举所有[PosPrinters](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)附加到系统，包括 USB、 网络和蓝牙 POS 打印机。

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

**GetDeviceSelector**不同设备类型的方法是：

* [BarcodeScanner.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

使用**GetDeviceSelector**方法采用[PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)值作为参数，您可以限制您的选择器来枚举本地、 网络、 或蓝牙附加 POS 设备减少要完成的查询所需的时间。  下面的示例显示了此方法以定义一个选择器，只是在本地支持使用附加 POS 打印机。

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> 请参阅[生成设备选择器](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector)用于生成更多高级选择器字符串。

## <a name="method-1-use-a-device-picker"></a>方法 1：使用设备选取器

[DevicePicker](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker)类可用于显示包含的用户可供选择的设备列表选取器浮出控件。 可以使用[筛选器](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.filter)属性选择的设备以显示选择器中的类型。 此属性属于类型[DevicePickerFilter](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter)。 可以将设备类型添加到筛选器使用[SupportedDeviceClasses](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses)或[SupportedDeviceSelectors](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors)属性。

若要显示设备选取器准备就绪后，可以调用[PickSingleDeviceAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync)方法，它将显示选取器 UI 并返回所选的设备。 将需要指定[Rect](https://docs.microsoft.com/uwp/api/windows.foundation.rect)这将确定在浮出控件的显示位置。 此方法将返回[DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)对象，因此可以使用它与点的服务 Api，你将需要使用**FromIdAsync**所需的特定设备类的方法。 您传递[DeviceInformation.Id](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id)属性设置为该方法的*deviceId*参数，并获取返回值作为设备类的实例。

下面的代码段将创建**DevicePicker**，向其添加条形码扫描程序筛选器和用户选择一台设备，然后创建**BarcodeScanner**对象基于设备 ID:

```cs
private async Task<BarcodeScanner> GetBarcodeScanner()
{
    DevicePicker devicePicker = new DevicePicker();
    devicePicker.Filter.SupportedDeviceSelectors.Add(BarcodeScanner.GetDeviceSelector());
    Rect rect = new Rect();
    DeviceInformation deviceInformation = await devicePicker.PickSingleDeviceAsync(rect);
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(deviceInformation.Id);
    return barcodeScanner;
}
```

## <a name="method-2-get-first-available-device"></a>方法 2：获取第一个可用的设备

若要获取的点的服务设备的最简单方法是使用**GetDefaultAsync**获取 Point of Service 设备类中的第一个可用设备。 

下面的示例演示如何使用[GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync)有关[BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)。 编码模式是类似的所有点的服务的设备类别。

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> **GetDefaultAsync**因为它可能从一个会话到下一步返回不同的设备必须谨慎使用。 许多事件可能影响此枚举，从而导致出现不同的第一个可用设备，包括： 
> - 连接到计算机的摄像头更改 
> - 连接到您的计算机的服务设备的入点更改
> - 更改在网络上可用的网络连接点的服务设备
> - 在您的计算机范围内的蓝牙 Point of Service 设备中更改 
> - 对服务点配置的更改 
> - 安装驱动程序或 OPOS 服务对象
> - 服务点扩展的安装
> - Windows 操作系统更新

## <a name="method-3-snapshot-of-devices"></a>方法 3：设备的快照

在有些情况下，你可能想要生成自己的 UI 或需要枚举设备而无需向用户显示 UI。  在这些情况下，您无法枚举当前连接或与系统使用配对的设备的快照[DeviceInformation.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)。  此方法将保留任何结果直到整个枚举完成。

> [!TIP]
> 建议使用**GetDeviceSelector**方法替换**PosConnectionTypes**时使用的参数**FindAllAsync**以与连接类型将查询限制所需。  网络和蓝牙连接可能会延迟结果之前必须完成其枚举**FindAllAsync**返回结果。

> [!CAUTION] 
> **FindAllAsync**返回设备数组。  此数组的顺序可能随会话改变，因此，不建议通过使用数组的硬编码索引来依赖特定顺序。  使用[DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)属性，以筛选结果或提供一个可供选择的用户的 UI。

此示例使用上面定义来拍摄快照使用的设备的选择器**FindAllAsync**然后枚举每个项返回的集合，并将设备名称和 ID 写入到调试输出。 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> 使用时[Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) Api，你将经常需要使用[DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)对象以获取有关特定设备的信息。 例如， [DeviceInformation.ID](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id)属性可以用于恢复和重复使用同一个设备，如果在将来的会话中可用并[DeviceInformation.Name](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name)属性可用于显示应用程序中的目的。  请参阅[DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)引用页，了解有关可用的其他属性的信息。

## <a name="method-4-enumerate-and-watch"></a>方法 4：枚举并观看

一种更强大更灵活的枚举设备的方法就是创建 [DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)。  设备观察程序动态枚举设备，以便应用程序在完成初始枚举后在添加、删除或者更改设备的情况下收到通知。  一个**DeviceWatcher**将允许你检测网络连接时，设备处于联机状态、 蓝牙设备处于范围内，如如果本地连接的设备被拔出，以便您可以采取相应的操作中也在应用程序。

此示例使用上面定义来创建的选择器**DeviceWatcher**也作为定义的事件处理程序[Added](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added)，[已删除](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed)，和[已更新](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated)通知。 你需要填写希望对每个通知采取的措施的详细信息。

```Csharp
using Windows.Devices.Enumeration;

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

> [!TIP]
> 请参阅[Enumerate 和监视设备]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices)有关详细信息的使用**DeviceWatcher**。

## <a name="see-also"></a>另请参阅
* [如何开始使用服务点](pos-basics.md)
* [DeviceInformation 类](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)
* [PosPrinter 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)
* [PosConnectionTypes 枚举](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [BarcodeScanner 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [DeviceWatcher 类](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]
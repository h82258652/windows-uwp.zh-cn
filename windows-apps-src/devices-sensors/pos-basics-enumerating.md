---
title: 枚举 PointOfService 设备
description: 了解如何枚举 PointOfService 设备
ms.date: 10/08/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 7759186d45d3488336a1b793d173d6d1f21aa601
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7853585"
---
# <a name="enumerating-point-of-service-devices"></a>枚举服务点设备
在此部分，你将了解如何[定义设备选择器](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector)（用于向系统提供查询设备），以及如何通过以下方法之一使用此选择器枚举服务点设备：

**方法 1:**[使用设备选取器](#method-1:-use-a-device-picker)
<br/>
显示设备选取器 UI，并让用户选择是连接的设备。 此方法处理更新列表，当设备连接，并将其删除，并简单且更安全比其他方法。

**方法 2:**[获取第一个可用设备](#Method-1:-get-first-available-device)<br />使用[GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync)访问特定服务点设备类中的第一个可用设备。

**方法 3:**[设备的快照](#Method-2:-Snapshot-of-devices)<br />枚举服务点设备上存在的给定时刻的系统时间的快照。 当你想要生成自己的 UI 或需要枚举设备而无需向用户显示 UI 时这会很有用。 [FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)将保留结果直到整个枚举完成。

**方法 4:**[枚举并监视](#Method-3:-Enumerate-and-watch)<br />[DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)是更强大更灵活的枚举模型，让你能够枚举当前存在的设备和还可添加或从系统中删除设备时收到通知。  当你想要在后台维护当前的设备列表以显示在 UI 中而不是等待快照发生时，这非常有用。

## <a name="define-a-device-selector"></a>定义设备选择器
设备选择器将使你可以在枚举设备时限制要搜索的设备。  这将允许你可以只获取相关结果，并减少枚举所需的设备所花费的时间。

你可以为你正在寻找来获取设备选择器，该类型的设备类型的使用**GetDeviceSelector**方法。 例如，使用[PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector)将提供你提供枚举所有[Posprinter](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)的选择器连接到系统，其中包括 USB、 网络和蓝牙 POS 打印机。

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

为不同设备类型**GetDeviceSelector**方法如下：

* [BarcodeScanner.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

可以通过使用**GetDeviceSelector**方法，它接受一个[PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)值作为一个参数，限制你的选择器来枚举本地、 网络、 或蓝牙连接 POS 设备，从而减少完成查询所花费的时间。  下面的示例演示如何使用此方法来定义选择器支持仅本地连接的 POS 打印机

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> 请参阅[生成设备选择器](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector)了解如何生成更高级的筛选器字符串。

## <a name="method-1-use-a-device-picker"></a>方法 1： 使用设备选取器

[DevicePicker](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker)类允许你显示选取器浮出控件，其中包含可供选择的用户的设备的列表。 你可以使用[筛选器](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.filter)属性选择哪些类型的设备选取器中显示。 此属性属于类型[DevicePickerFilter](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter)。 你可以将设备类型添加到使用[SupportedDeviceClasses](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses)或[SupportedDeviceSelectors](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors)属性的筛选器。

当你准备好显示设备选取器时，你可以调用[PickSingleDeviceAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync)方法，它将显示选取器 UI 并返回所选的设备。 你需要指定[Rect](https://docs.microsoft.com/uwp/api/windows.foundation.rect)将确定浮出控件显示的位置。 此方法将返回一个[DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)对象，因此，若要使用它使用服务点 Api，你将需要使用所需的特定设备类**FromIdAsync**的方法。 作为该方法的*deviceId*参数传递[DeviceInformation.Id](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id)属性并获取返回的值为设备类的实例。

以下代码段创建**DevicePicker**，添加条形码扫描仪筛选器，具有用户选择设备，然后创建基于设备 ID 的**BarcodeScanner**对象：

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

## <a name="method-2-get-first-available-device"></a>方法 2： 获取第一个可用设备

若要获取服务点设备的最简单方法是使用**GetDefaultAsync**获取服务点设备类内的第一个可用设备。 

下面的示例说明了使用[GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync)来[获取 BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)。 类似的所有服务点设备类的编码模式。

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> **GetDefaultAsync**必须小心使用，因为它可能会返回另一台设备从一个会话的下一步。 许多事件可能影响此枚举，从而导致出现不同的第一个可用设备，包括： 
> - 连接到计算机的摄像头更改 
> - 更改入设备连接到计算机的服务点
> - 在网络上可用的网络连接服务点设备更改
> - 计算机的范围内的蓝牙服务点设备更改 
> - 服务点配置更改 
> - 安装驱动程序或 OPOS 服务对象
> - 安装服务点扩展
> - Windows 操作系统更新

## <a name="method-3-snapshot-of-devices"></a>方法 3： 设备的快照

在有些情况下，你可能想要生成自己的 UI 或需要枚举设备而无需向用户显示 UI。  在这些情况下，你可以枚举当前使用 [DeviceInformation.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 连接或与系统配对的设备的快照。  此方法将保留任何结果直到整个枚举完成。

> [!TIP]
> 建议使用**PosConnectionTypes**参数的**GetDeviceSelector**方法，当使用**FindAllAsync**来限制你所需的连接类型的查询。  网络和蓝牙连接可能延迟结果，因为其枚举必须在返回**FindAllAsync**结果前完成。

> [!CAUTION] 
> **FindAllAsync**返回设备数组。  此数组的顺序可能随会话改变，因此，不建议通过使用数组的硬编码索引来依赖特定顺序。  使用[DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)属性筛选结果或提供用户可供选择的 UI。

此示例使用拍摄快照的设备使用**FindAllAsync**上面定义的选择器，然后通过集合返回的项目的每个枚举和设备名称和 ID 写入调试输出。 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> 在使用 [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) API 时，你经常需要使用 [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 对象来获取有关特定设备的信息。 例如， [DeviceInformation.ID](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id)属性可用于恢复和重复使用相同的设备，如果它也可在将来的会话[DeviceInformation.Name](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name)属性可用于在应用中显示。  请参阅 [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 参考页面了解有关其他可用属性的信息。

## <a name="method-4-enumerate-and-watch"></a>方法 4： 枚举并监视

一种更强大更灵活的枚举设备的方法就是创建 [DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)。  设备观察程序动态枚举设备，以便应用程序在完成初始枚举后在添加、删除或者更改设备的情况下收到通知。  **DeviceWatcher**允许你检测网络连接设备何时在线，蓝牙设备何时处于范围内，以及本地连接的设备是否已拔出，以便你可以在你的应用程序内采取相应操作。

此示例使用上方定义来创建**DeviceWatcher**的选择器，以及定义的[添加](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added)、[已删除](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed)，和[更新](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated)通知的事件处理程序。 你需要填写希望对每个通知采取的措施的详细信息。

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
> 在使用**DeviceWatcher**的更多详细信息，请参阅[枚举并监视设备]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices)。

## <a name="see-also"></a>另请参阅
* [服务点入门](pos-basics.md)
* [DeviceInformation 类](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)
* [PosPrinter 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)
* [PosConnectionTypes 枚举](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [BarcodeScanner 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [DeviceWatcher 类](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]
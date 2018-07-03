---
author: TerryWarwick
title: 枚举 PointOfService 设备
description: 了解如何枚举 PointOfService 设备
ms.author: jken
ms.date: 06/8/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: be1fdc42295fc03ff86a69e287a4089abe547689
ms.sourcegitcommit: ee77826642fe8fd9cfd9858d61bc05a96ff1bad7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/11/2018
ms.locfileid: "2018435"
---
# <a name="enumerating-point-of-service-devices"></a>枚举服务点设备
在此部分，你将了解如何[**定义设备选择器**](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector)（用于向系统提供查询设备），以及如何通过以下方法之一使用此选择器枚举服务点设备：

**方法 1：**[**获取第一个可用设备**](#Method-1:-get-first-available-device)<br />在此部分，你将学习如何使用 **GetDefaultAsync** 访问特定 PointOfService 设备类中的第一个可用设备。

**方法 2：**[**设备的快照**](#Method-2:-Snapshot-of-devices)<br />在此部分，你将学会如何枚举在给定的时间点在系统上出现的 PointOfService 设备的快照。 当你想要生成自己的 UI 或需要枚举设备而无需向用户显示 UI 时这会很有用。 FindAllAsync 将保留结果直到整个枚举完成。

**方法 3：**[**枚举并监视**](#Method-3:-Enumerate-and-watch)<br />在此部分，你将了解更强大、更灵活的枚举模型，让你能够枚举当前存在的设备，还可在向系统添加或删除设备时收到通知。  当你想要在后台维护当前的设备列表以显示在 UI 中而不是等待快照发生时，这非常有用。
 

---
## <a name="define-a-device-selector"></a>定义设备选择器
设备选择器将使你可以在枚举设备时限制要搜索的设备。  这让你可以只获取相关结果，并减少枚举所需设备使用的时间。  

使用 [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) 将向你提供枚举连接到系统的所有 POSPrinter 的选择器，包括 USB、网络和蓝牙 POSPrinter。

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();   

```

使用将 [**PosConnectionTypes**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes) 作为参数的 [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector_Windows_Devices_PointOfService_PosConnectionTypes_) 方法，你可以限制选择器来枚举本地、网络或连接蓝牙的 POSPrinter，从而减少完成查询所需的时间。  下面的示例显示如何使用此方法来定义只支持本地连接的 POSPrinter 的选择器。

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);   

```
> [!TIP]
> 请参阅[**生成设备选择器**](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector)了解如何生成更高级的筛选器字符串。

---

## <a name="method-1-get-first-available-device"></a>方法 1：获取第一个可用设备

获取 PointOfService 设备的最简单方法是使用 **GetDefaultAsync** 来获取 PointOfService 设备类内的第一个可用设备。 

下面的示例显示如何使用 [**GetDefaultAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) 来获取 BarcodeScanner。 所有 PointOfService 设备类的编码模式都是相似的。

```Csharp

using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();

```

> [!CAUTION]
> GetDefaultAsync 必须小心使用，因为它可能会将一个会话的不同设备返回到下一个会话。 许多事件可能影响此枚举，从而导致出现不同的第一个可用设备，包括： 
> - 连接到计算机的摄像头更改 
> - 连接到计算机的 PointOfService 设备更改
> - 网络中可用的网络连接 PointOfService 设备更改
> - 计算机范围内的蓝牙 PointOfService 设备更改 
> - PointOfService 配置更改 
> - 安装驱动程序或 OPOS 服务对象
> - 安装 PointOfService 扩展
> - Windows 操作系统更新

---

## <a name="method-2-snapshot-of-devices"></a>方法 2：设备的快照

在有些情况下，你可能想要生成自己的 UI 或需要枚举设备而无需向用户显示 UI。  在这些情况下，你可以枚举当前使用 [**DeviceInformation.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 连接或与系统配对的设备的快照。  此方法将保留任何结果直到整个枚举完成。

> [!TIP]
> 在使用 FindAllAsync 将查询限制为所需的连接类型时，建议使用具有 PosConnectionTypes 参数的 GetDeviceSelector 方法。  网络和蓝牙连接可能延迟结果，因为它们的枚举必须在返回 FindAllAsync 结果前完成。

>[!CAUTION] 
>FindAllAsync 返回设备数组。  此数组的顺序可能随会话改变，因此，不建议通过使用数组的硬编码索引来依赖特定顺序。  使用 DeviceInformation 属性筛选结果或为用户提供用于选择的 UI。

此示例使用上方定义的选择器来使用 FindAllAsync 获取设备快照，然后通过集合返回的每个项目枚举，并将设备名称和 ID 写入调试输出。 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> 在使用 [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) API 时，你经常需要使用 [**DeviceInformation**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 对象来获取有关特定设备的信息。 例如，如果在将来的会话中提供，[**DeviceInformation.ID**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 属性可用于恢复和重复使用同一设备，[**DeviceInformation.Name**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) 属性可在你的应用中用来显示。  请参阅 [**DeviceInformation**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 参考页面了解有关其他可用属性的信息。

---

## <a name="method-3-enumerate-and-watch"></a>方法 3：枚举并监视

一种更强大更灵活的枚举设备的方法就是创建 [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)。  设备观察程序动态枚举设备，以便应用程序在完成初始枚举后在添加、删除或者更改设备的情况下收到通知。  DeviceWatcher 允许你检测网络连接设备何时在线，蓝牙设备何时处于范围内，以及本地连接设备是否已拔出，以便你可以在应用程序内采取相应的措施。

此示例使用上方定义的选择器来创建 DeviceWatcher，并为“已添加”、“已删除”和“已更新”通知定义事件处理程序。 你需要填写希望对每个通知采取的措施的详细信息。

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
> 请参阅[**枚举并监视设备**]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices)了解使用 DeviceWatcher 的更多详细信息。

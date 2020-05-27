---
title: 蓝牙 GATT 客户端
description: 本文提供用于通用 Windows 平台 (UWP) 应用的蓝牙通用属性配置文件 (GATT) 客户端概述，以及用于常见用例的示例代码。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e894b750ba2119e2cca6f316f9671c51386d800c
ms.sourcegitcommit: e51f9489d8c977c3498afb1a75c91f96ac3a642b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/26/2020
ms.locfileid: "83854673"
---
# <a name="bluetooth-gatt-client"></a>蓝牙 GATT 客户端

本文演示用于通用 Windows 平台 (UWP) 应用的蓝牙通用属性 (GATT) 客户端 API 的用法，以及用于常见 GATT 客户端任务的示例代码：

- 查询附近设备
- 连接到设备
- 枚举设备的受支持服务和特征
- 读取和写入特征
- 订阅特征值更改时的通知

> **重要的 API**
>
> - [**Windows. 蓝牙**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth)
> - [**Bluetooth.genericattributeprofile 替换。**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)

## <a name="overview"></a>概述

开发人员可使用 [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile) 命名空间中的 API 访问蓝牙 LE 设备。 蓝牙 LE 设备通过以下内容的集合公开其功能：

- 服务
- 特征
- 描述符

服务定义 LE 设备的功能合约，并且包含定义该服务的特征的集合。 这些特征反过来包括描述特征的描述符。 这 3 个术语通常称为设备的属性。

蓝牙 LE GATT API 公开对象和函数，而不是对原始传输的访问权限。 GATT API 还使开发人员可以在能够执行以下任务的情况下使用蓝牙 LE 设备：

- 执行属性发现
- 读取和写入属性值
- 为 Characteristic ValueChanged 事件注册回调

要创建有用的实现，开发人员必须具有应用程序要使用的 GATT 服务和特征的前期知识，并且必须处理特征的特征值，以便由 API 提供的二进制数据在向用户呈现之前转换为有用的数据。 蓝牙 GATT API 仅公开与蓝牙 LE 设备进行通信所需的基本基元。 要解释数据，应用程序配置文件必须经蓝牙 SIG 标准配置文件定义，或经由设备供应商实现的自定义配置文件定义。 配置文件创建应用程序和设备之间的绑定合约，此合约关于交换的数据所代表的内容以及如何解释它。

为方便起见，蓝牙 SIG 将持续提供[公共配置文件的列表](https://www.bluetooth.com/specifications/adopted-specifications#gattspec)。

## <a name="query-for-nearby-devices"></a>查询附近设备

可通过两个主要方法查询附近设备：

- Windows.Devices.Enumeration 中的 DeviceWatcher
- Windows.Devices.Bluetooth.Advertisement 中的 AdvertisementWatcher

第二个方法在[播发](ble-beacon.md)文档中进行了详细讨论，因此在此处不会进行深入讨论，但基本思路是查找满足特定[播发筛选器](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter)的附近设备的蓝牙地址。 拥有地址之后，便可以调用 [BluetoothLEDevice.FromBluetoothAddressAsync](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.bluetoothledevice.frombluetoothaddressasync) 以获取对设备的引用。

现在返回到 DeviceWatcher 方法。 蓝牙 LE 设备就像 Windows 中的任何其他设备一样，可以使用[枚举 API](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) 进行查询。 使用 [DeviceWatcher](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) 类，并传入指定要查找的设备的查询字符串：

```csharp
// Query for extra properties you want returned
string[] requestedProperties = { "System.Devices.Aep.DeviceAddress", "System.Devices.Aep.IsConnected" };

DeviceWatcher deviceWatcher =
            DeviceInformation.CreateWatcher(
                    BluetoothLEDevice.GetDeviceSelectorFromPairingState(false),
                    requestedProperties,
                    DeviceInformationKind.AssociationEndpoint);

// Register event handlers before starting the watcher.
// Added, Updated and Removed are required to get all nearby devices
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Updated += DeviceWatcher_Updated;
deviceWatcher.Removed += DeviceWatcher_Removed;

// EnumerationCompleted and Stopped are optional to implement.
deviceWatcher.EnumerationCompleted += DeviceWatcher_EnumerationCompleted;
deviceWatcher.Stopped += DeviceWatcher_Stopped;

// Start the watcher.
deviceWatcher.Start();
```

启动了 DeviceWatcher 之后，对于满足相关设备的 [Added](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added) 事件处理程序中的查询的每个设备，你都会收到 [DeviceInformation](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)。 有关 DeviceWatcher 的更详细介绍，请参阅 [Github 上](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing)的完整示例。

## <a name="connecting-to-the-device"></a>连接到设备

发现所需设备之后，使用 [DeviceInformation.Id](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 获取相关设备的蓝牙 LE 设备对象：

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```

另一方面，对设备的 BluetoothLEDevice 对象的所有引用的释放（以及如果系统上没有其他应用拥有对设备的引用）都会在一小段超时时间之后触发自动断开连接。

```csharp
bluetoothLeDevice.Dispose();
```

如果应用需要再次访问设备，则仅仅重新创建设备对象并访问特征（在下一部分中讨论）会触发操作系统在需要时重新连接。 如果设备处于附近，则可以访问设备，否则它会返回并具有 DeviceUnreachable 错误。  

## <a name="enumerating-supported-services-and-characteristics"></a>枚举受支持的服务和特征

现在你拥有 BluetoothLEDevice 对象，下一步是发现设备公开的数据。 执行此操作的第一步是查询服务：

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();

if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```

确定了感兴趣的服务之后，下一步是查询特征。

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();

if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```

操作系统返回你随后可以对其执行操作的 GattCharacteristic 对象的 ReadOnly 列表。

## <a name="perform-readwrite-operations-on-a-characteristic"></a>对特征执行读取/写入操作

特征是基于 GATT 的通信的基本单元。 它包含表示设备上一段不同数据的值。 例如，电池电量特征具有表示设备电池电量的值。

读取特征属性以确定支持的操作：

```csharp
GattCharacteristicProperties properties = characteristic.CharacteristicProperties

if(properties.HasFlag(GattCharacteristicProperties.Read))
{
    // This characteristic supports reading from it.
}
if(properties.HasFlag(GattCharacteristicProperties.Write))
{
    // This characteristic supports writing to it.
}
if(properties.HasFlag(GattCharacteristicProperties.Notify))
{
    // This characteristic supports subscribing to notifications.
}
```

如果支持读取，则可以读取值：

```csharp
GattReadResult result = await selectedCharacteristic.ReadValueAsync();
if (result.Status == GattCommunicationStatus.Success)
{
    var reader = DataReader.FromBuffer(result.Value);
    byte[] input = new byte[reader.UnconsumedBufferLength];
    reader.ReadBytes(input);
    // Utilize the data as needed
}
```

写入特征遵循类似模式：

```csharp
var writer = new DataWriter();
// WriteByte used for simplicity. Other common functions - WriteInt16 and WriteSingle
writer.WriteByte(0x01);

GattCommunicationStatus result = await selectedCharacteristic.WriteValueAsync(writer.DetachBuffer());
if (result == GattCommunicationStatus.Success)
{
    // Successfully wrote to device
}
```

> **提示**：当使用从许多蓝牙 api 获取的原始缓冲区时， [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader)和[DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter)是或缺的。

## <a name="subscribing-for-notifications"></a>订阅通知

确保特征支持“指示”或“通知”（检查特征属性以进行确保）。

> **旁白**：“指示”被视为更可靠，因为每个值更改事件都与来自客户端设备的确认相结合。 “通知”更为普遍，因为大多数 GATT 事务宁愿节省电量，而不是非常可靠。 在任何情况下，它们全部在控制器层进行处理，因此不会涉及到应用。 但现在你知道，我们将它们简单地统称为“通知”。

获取通知之前要考虑两个事项：

- 写入客户端特征配置描述符 (CCCD)
- 处理 Characteristic.ValueChanged 事件

写入 CCCD 可向服务器设备告知此客户端要在该特定特征值每次更改时收到通知。 若要实现此目的，请执行以下操作：

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```

现在，每当在远程设备上更改值时，便会调用 GattCharacteristic 的 ValueChanged 事件。 只需实现处理程序即可：

```csharp
characteristic.ValueChanged += Characteristic_ValueChanged;

...

void Characteristic_ValueChanged(GattCharacteristic sender,
                                    GattValueChangedEventArgs args)
{
    // An Indicate or Notify reported that the value has changed.
    var reader = DataReader.FromBuffer(args.CharacteristicValue)
    // Parse the data however required.
}
```

---
title: 蓝牙 GATT 客户端
description: 本文概述了适用于通用 Windows 平台 (UWP) 应用，以及有关常见的用例的示例代码的蓝牙通用属性配置文件 (GATT) 客户端。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3ae656b473a4dd5999588057b0ec970645703eec
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "7842422"
---
# <a name="bluetooth-gatt-client"></a>蓝牙 GATT 客户端


**重要的 API**

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

本文介绍了通用 Windows 平台 (UWP) 应用，以及有关常见的 GATT 客户端任务的示例代码的蓝牙通用属性 (GATT) 客户端 Api 的用法：
- 查询附近设备
- 连接到设备
- 枚举的受支持的服务和设备的特性
- 读取和写入特性
- 订阅的通知时特性值发生更改

## <a name="overview"></a>概述
开发人员可以使用[**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)命名空间中的 Api 访问蓝牙 LE 设备。 蓝牙 LE 设备通过以下内容的集合公开其功能：

-   服务
-   特性
-   描述符

服务定义 LE 设备的功能合约，并且包含定义该服务的特性的集合。 这些特性反过来包括描述特性的描述符。 这些 3 个术语通常称为设备的属性。

蓝牙 LE GATT Api 公开对象和函数，而不是对原始传输的访问。 GATT Api 还使开发人员可以使用蓝牙 LE 设备能够执行以下任务：

-   探索属性
-   读取和写入属性值
-   为 Characteristic ValueChanged 事件注册回调

若要创建有用的实现，开发人员必须具有特定的特性值，以便由 API 提供的二进制数据转换为的前期知识的 GATT 服务和应用程序应使用的特性并且过程之前显示给用户的有用的数据。 蓝牙 GATT API 仅公开与蓝牙 LE 设备进行通信所需的基本基元。 要解释数据，应用程序配置文件必须经蓝牙 SIG 标准配置文件定义，或经由设备供应商实现的自定义配置文件定义。 配置文件创建应用程序和设备之间的绑定合约，此合约关于交换的数据所代表的内容以及如何解释它。

为方便起见，蓝牙 SIG 将持续提供[公共配置文件](https://www.bluetooth.com/specifications/adopted-specifications#gattspec)的列表。

## <a name="query-for-nearby-devices"></a>查询附近设备
有两种主要方法来查询附近设备：
- 在 Windows.Devices.Enumeration DeviceWatcher
- 在 Windows.Devices.Bluetooth.Advertisement AdvertisementWatcher

第二个方法进行了详细讨论在 length[广告](ble-beacon.md)文档中以便它不会讨论更此处但基本的想法将找到附近满足特定[广告筛选器](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter.aspx)的设备的蓝牙地址。 地址后，你可以调用[BluetoothLEDevice.FromBluetoothAddressAsync](https://msdn.microsoft.com/en-us/library/windows/apps/mt608819.aspx)来获取对设备的引用。 

现在，返回到 DeviceWatcher 方法。 蓝牙 LE 设备在 Windows 中的任何其他设备一样，且可使用[枚举 Api](https://msdn.microsoft.com/library/windows/apps/BR225459)查询。 使用[DeviceWatcher](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher)类，并传递查询字符串指定要查找的设备： 

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
一旦你开始 DeviceWatcher，你将收到[DeviceInformation](https://msdn.microsoft.com/library/windows/apps/br225393)为满足问题的设备[添加](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher.added)事件处理程序中的查询每台设备。 有关 DeviceWatcher 的更多详细信息，请参阅完整的示例[在 Github 上](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing)。 

## <a name="connecting-to-the-device"></a>连接到设备
一旦发现所需的设备，则使用[DeviceInformation.Id](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.id)获取设备的蓝牙 LE 设备对象问题： 

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```
另一方面，释放对 BluetoothLEDevice 的所有引用设备的对象 （以及系统上的任何其他应用是否具有对该设备的引用） 将触发自动在较小的超时时间后断开连接。 

```csharp
bluetoothLeDevice.Dispose();
```
如果应用需要再次访问该设备，只需重新创建设备对象和访问特性 （在下一节中讨论） 将触发操作系统在必要时重新连接。 如果设备在附近，你将有权访问设备否则为它将返回带有 DeviceUnreachable 错误。  

## <a name="enumerating-supported-services-and-characteristics"></a>枚举受支持的服务和特性
既然你已拥有一个 BluetoothLEDevice 对象下, 一步是发现设备公开的数据。 若要执行此操作的第一步是查询服务： 

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```
一旦确定感兴趣的服务下, 一步是对查询的特征。 

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```  
操作系统会返回 GattCharacteristic 的只读列表对象，然后可以对执行操作。

## <a name="perform-readwrite-operations-on-a-characteristic"></a>执行了特性的读/写操作

特点是 GATT 的基本单元基于通信。 它包含一个值，表示一个不同的设备上的数据。 例如，电池级别特征都有一个值，表示该设备的电池剩余电量。

读取特性的属性，以确定哪些操作也受支持：
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

读取受支持，你可以读取的值： 
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
写入特性遵循类似的模式： 
```csharp
var writer = new DataWriter();
// WriteByte used for simplicity. Other commmon functions - WriteInt16 and WriteSingle
writer.WriteByte(0x01);

GattReadResult result = await selectedCharacteristic.WriteValueAsync(writer.DetachBuffer());
if (result.Status == GattCommunicationStatus.Success)
{
    // Successfully wrote to device
}
```
> **提示**： 获取熟悉使用[DataReader](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datareader.aspx)和[DataWriter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datawriter.aspx)。 使用你获得许多蓝牙 Api 的原始缓冲区时，其功能将不可或缺。 
## <a name="subscribing-for-notifications"></a>订阅通知

请确保在特性支持标识或通知 （检查特性的属性，以确保）。 

> **预留**： 指示被视为更可靠，因为每个值更改事件结合确认从客户端设备。 通知更普遍大多数 GATT 交易将而节省电源而是非常可靠。 在任何情况下，所有这些处理在控制器层因此应用不会不获取会涉及到。 我们将汇集起来称为它们只是"notifications"，但现在你已知道。 

有要负责获取通知前两个事项：
- 写入到客户端特性配置描述符 (CCCD)
- 处理 Characteristic.ValueChanged 事件

写入到 CCCD 告知此客户端想知道该特定特征值更改的每次的服务器设备。 若要实现此目的，请执行以下操作： 

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```
现在，GattCharacteristic ValueChanged 事件将获取每次调用在远程设备上更改获取的值。 只是实现的处理程序： 

```csharp
characteristic.ValueChanged += Characteristic_ValueChanged;
// ... 

void Characteristic_ValueChanged(GattCharacteristic sender, 
                                    GattValueChangedEventArgs args)
{
    // An Indicate or Notify reported that the value has changed.
    var reader = DataReader.FromBuffer(args.CharacteristicValue)
    // Parse the data however required.
}
```



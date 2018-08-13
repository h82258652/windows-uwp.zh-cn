---
author: msatranjr
title: 蓝牙 GATT 客户端
description: 本文概述了对于通用 Windows 平台 (UWP) 应用程序，以及用于常见的使用案例示例代码的蓝牙泛型属性配置文件 (GATT) 客户端。
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 555fec6d534c07898acd911f9cd84a11ac66dcd8
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "304553"
---
# <a name="bluetooth-gatt-client"></a>蓝牙 GATT 客户端


**重要的 API**

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

本文说明对于通用 Windows 平台 (UWP) 应用程序，以及 GATT 客户端的常见任务的示例代码蓝牙泛型属性 (GATT) 客户端 Api 的用法：
- 查询附近的设备
- 连接到设备
- 枚举的支持的服务和设备的特性
- 读取和写入特征
- 订阅通知时特征值更改为

## <a name="overview"></a>概述
开发人员可以使用[**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)命名空间中 Api 访问蓝牙 LE 设备。 蓝牙 LE 设备通过以下内容的集合公开其功能：

-   服务
-   特性
-   描述符

服务定义 LE 设备功能合同，并且包含定义服务的特征的集合。 这些特性反过来包括描述特性的描述符。 这些 3 术语一般被称为设备的属性。

蓝牙 LE GATT Api 公开对象和函数，而不是到原始传输访问。 GATT Api 还启用开发人员可以使用蓝牙 LE 设备能够执行以下任务：

-   执行属性发现
-   读取和写入属性值
-   注册特征 ValueChanged 事件的回调

若要创建的有用实现开发人员必须具有特定的特征值以便所提供的 API 的二进制数据转换为的预备知识 GATT 服务和应用程序要使用的特征和过程有用的数据之前显示给用户。 蓝牙 GATT API 仅公开与蓝牙 LE 设备进行通信所需的基本基元。 要解释数据，应用程序配置文件必须经蓝牙 SIG 标准配置文件定义，或经由设备供应商实现的自定义配置文件定义。 配置文件创建应用程序和设备之间的绑定合约，此合约关于交换的数据所代表的内容以及如何解释它。

为方便起见，蓝牙 SIG 将持续提供[公共配置文件](https://www.bluetooth.com/specifications/adopted-specifications#gattspec)的列表。

## <a name="query-for-nearby-devices"></a>查询附近的设备
有两种主要方法，用于查询附近的设备：
- 在 Windows.Devices.Enumeration DeviceWatcher
- 在 Windows.Devices.Bluetooth.Advertisement AdvertisementWatcher

讨论第方法在 length[广告](ble-beacon.md)文档中以便它将不讨论多此处但的基本思想是查找附近满足特定的[广告筛选器](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter.aspx)的设备的蓝牙地址。 地址后，您可以调用[BluetoothLEDevice.FromBluetoothAddressAsync](https://msdn.microsoft.com/en-us/library/windows/apps/mt608819.aspx)以获取对设备的引用。 

现在，回 DeviceWatcher 方法。 蓝牙 LE 设备就像在 Windows 中的任何其他设备，可以使用[枚举 Api](https://msdn.microsoft.com/library/windows/apps/BR225459)查询。 使用[DeviceWatcher](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher)类，并指定要查找的设备的查询字符串传递： 

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
一旦您已启动 DeviceWatcher，您将收到[DeviceInformation](https://msdn.microsoft.com/library/windows/apps/br225393)为满足事件处理程序[添加了](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher.added)问题的设备中的查询的每个设备。 更多详细看 DeviceWatcher 请参阅完整的示例[在 Github](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing)。 

## <a name="connecting-to-the-device"></a>连接到设备
一旦发现所需的设备，则使用[DeviceInformation.Id](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.id)蓝牙 LE 设备对象获取设备问题： 

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```
另一方面，释放所有引用 BluetoothLEDevice 的设备对象 （和系统上的任何其他应用程序是否具有对设备） 将触发自动小超时后断开。 

```csharp
bluetoothLeDevice.Dispose();
```
如果在应用程序需要再次访问该设备，只需重新创建设备对象和访问特征 （在下一节讨论） 将触发 OS，以在必要时重新连接。 如果附近设备，您将获取访问设备否则为它将返回具有一个 DeviceUnreachable 错误。  

## <a name="enumerating-supported-services-and-characteristics"></a>支持的服务和特征枚举
既然您已经 BluetoothLEDevice 对象下, 一步是发现设备公开哪些数据。 若要执行此操作的第一步是为查询服务： 

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```
一旦被标识感兴趣的服务下, 一步是查询特征。 

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```  
操作系统返回 GattCharacteristic ReadOnly 列表对象，然后可以在执行操作。

## <a name="perform-readwrite-operations-on-a-characteristic"></a>执行上特征的读/写操作

特征是 GATT 的基本单位基于通信。 它包含一个值，表示一个不同的设备上的数据。 例如，电池级别特征有一个值，该值代表设备的电池级别。

读取特征属性可确定支持哪些操作：
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

如果读取受支持，您可以读取的值： 
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
写入特征如下所示类似的模式： 
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
> **提示**： 获取习惯使用[DataReader](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datareader.aspx)和[DataWriter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datawriter.aspx)。 使用很多蓝牙 Api 获取原始缓冲区时，其功能将不可或缺。 
## <a name="subscribing-for-notifications"></a>订阅通知

请确保特征支持指明或 Notify （检查特征的属性，以确保）。 

> **留出**： 指示被视为更可靠，因为每个值已更改事件耦合与来自客户端设备的确认信息。 通知是因为大多数 GATT 事务将而节省电源，而不是非常可靠普及。 在任何情况下，所有的是在处理控制器层以便不会不获取涉及应用程序。 我们将代称其为只"通知"，但现在已知道。 

有两项操作来获取通知之前的处理：
- 写入客户端特征配置描述符 (CCCD)
- 用来处理 Characteristic.ValueChanged 事件

写入 CCCD 告知服务器设备此客户端希望了解该特定的特征值更改的每次。 若要实现此目的，请执行以下操作： 

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```
现在，GattCharacteristic ValueChanged 事件将会调用每次远程设备获取更改的值。 剩下的就是实现处理程序： 

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



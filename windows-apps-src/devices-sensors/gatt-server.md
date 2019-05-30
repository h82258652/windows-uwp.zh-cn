---
title: 蓝牙 GATT 服务器
description: 本文提供用于通用 Windows 平台 (UWP) 应用的蓝牙通用属性配置文件 (GATT) 服务器概述，以及用于常见用例的示例代码。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f59ae45486ee72f9d901898f6b03674e6b3e299c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370093"
---
# <a name="bluetooth-gatt-server"></a>蓝牙 GATT 服务器


**重要的 Api**
- [**Windows.Devices.Bluetooth**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)


本文演示用于通用 Windows 平台 (UWP) 应用的蓝牙通用属性 (GATT) 服务器 API 的用法，以及用于常见 GATT 服务器任务的示例代码： 
- 定义支持的服务
- 发布服务器，以便远程客户端可以它
- 播发服务支持
- 响应读取和写入请求
- 向订阅的客户端发送通知

## <a name="overview"></a>概述
Windows 通常采用客户端角色进行操作。 不过在许多方案中，也会要求 Windows 充当蓝牙 LE GATT 服务器。 针对 IoT 设备的几乎所有方案以及大多数跨平台 BLE 通信都要求 Windows 作为 GATT 服务器。 此外，向附近可穿戴设备发送通知也已成功需要此技术的常见方案。  
> 在继续之前确保清楚了解 [GATT 客户端文档](gatt-client.md)中的所有概念。  

服务器操作会围绕服务提供程序和 GattLocalCharacteristic 进行。 这两个类将提供如何声明、 实现和公开到远程设备的数据的层次结构所需的功能。

## <a name="define-the-supported-services"></a>定义支持的服务
应用可以声明将由 Windows 发布的一个或多个服务。 每个服务都通过 UUID 唯一地进行标识。 

### <a name="attributes-and-uuids"></a>属性和 UUID
每个服务、特征和描述符都通过自己的唯一 128 位 UUID 进行定义。
> 所有 Windows API 都使用术语 GUID，但是蓝牙标准将这些定义为 UUID。 对于我们的用途，这两个术语可以互换，因此我们会继续使用术语 UUID。 

如果属性是标准的并按照蓝牙 SIG 的定义进行定义，则它还会具有对应的 16 位短 ID（例如电池电量 UUID 是 0000**2A19**-0000-1000-8000-00805F9B34FB，而短 ID 是 0x2A19）。 这些标准 UUID 可以在 [GattServiceUuids](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids) 和 [GattCharacteristicUuids](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids) 中进行查看。

如果应用在实现自己的自定义服务，则必须生成自定义 UUID。 这可以在 Visual Studio 中通过“工具”->“创建 Guid”轻松实现（使用选项 5 可采用“xxxxxxxx-xxxx-...xxxx”格式获取它）。 此 uuid 现在可以用于声明新的本地服务、特征或描述符。

#### <a name="restricted-services"></a>受限服务
以下服务由系统保留，此时无法发布：
1. 设备信息服务 (DIS)
2. 通用属性配置文件服务 (GATT)
3. 通用访问配置文件服务 (GAP)
4. 人机接口设备服务 (HOGP)
5. 扫描参数服务 (SCP)

> 创建受阻服务的尝试会导致从 CreateAsync 调用返回 BluetoothError.DisabledByPolicy。

#### <a name="generated-attributes"></a>生成的属性
以下描述符由系统基于在特征创建过程中提供的 GattLocalCharacteristicParameters 自动生成：
1. 客户端特征配置（如果特征标记为可指示或不可指示）。
2. 特征用户说明（如果设置了 UserDescription 属性）。 有关详细信息，请参阅 GattLocalCharacteristicParameters.UserDescription 属性。
3. 特征格式（指定的每个演示格式一个描述符）。  有关详细信息，请参阅 GattLocalCharacteristicParameters.PresentationFormats 属性。
4. 特征聚合格式（如果指定多个演示格式）。  有关详细信息，请参阅 GattLocalCharacteristicParameters.PresentationFormats 属性。
5. 特征扩展属性（如果使用扩展属性位标记了特征）。

> 扩展属性描述符的值通过 ReliableWrites 和 WritableAuxiliaries 特征属性进行确定。

> 尝试创建保留描述符会导致异常。

> 请注意，当前不支持广播。  指定广播 GattCharacteristicProperty 会导致异常。

### <a name="build-up-the-hierarchy-of-services-and-characteristics"></a>层次结构中的服务和特征生成
GattServiceProvider 用于创建并播发根主要服务定义。  每个服务都需要自己的在 GUID 中采用的 ServiceProvider 对象： 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> 主要服务是 GATT 树的顶层。 主要服务包含特征以及其他服务（称为“包含”或辅助服务）。 

现在，使用所需特征和描述符填充服务：

```csharp
GattLocalCharacteristicResult characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid1, ReadParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_readCharacteristic = characteristicResult.Characteristic;
_readCharacteristic.ReadRequested += ReadCharacteristic_ReadRequested;

characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid2, WriteParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_writeCharacteristic = characteristicResult.Characteristic;
_writeCharacteristic.WriteRequested += WriteCharacteristic_WriteRequested;

characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid3, NotifyParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_notifyCharacteristic = characteristicResult.Characteristic;
_notifyCharacteristic.SubscribedClientsChanged += SubscribedClientsChanged;
```
如上所述，这也是为每个特征支持的操作声明事件处理程序的好位置。  若要正确响应请求，应用必须为属性支持的每种请求类型定义并设置事件处理程序。  无法注册处理程序会导致系统使用 *UnlikelyError* 立即完成请求。

### <a name="constant-characteristics"></a>固定特征
有时，一些特征值在应用生存期过程中不会发生更改。 在此情况下，建议声明固定特征以防止不必要的应用激活： 

```csharp
byte[] value = new byte[] {0x21};
var constantParameters = new GattLocalCharacteristicParameters
{
    CharacteristicProperties = (GattCharacteristicProperties.Read),
    StaticValue = value.AsBuffer(),
    ReadProtectionLevel = GattProtectionLevel.Plain,
};

var characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid4, constantParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
```
## <a name="publish-the-service"></a>发布服务
完整定义服务之后，下一步是发布服务支持。 这会通知操作系统应在远程设备执行服务发现时返回服务。  必须设置两个属性，即 IsDiscoverable 和 IsConnectable：  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**:播发的播发，使设备可发现中的远程设备的友好名称。
- **IsConnectable**:播发以便在外围角色中使用的可连接播发。

> 当服务 Discoverable 且 Connectable 时，系统会将服务 Uuid 添加到播发数据包。  播发数据包中只有 31 个字节，128 位 UUID 会占用其中的 16 个字节！

> 请注意，如果在前台发布服务，则应用程序必须在应用程序挂起时调用 StopAdvertising。

## <a name="respond-to-read-and-write-requests"></a>响应读取和写入请求
正如我们上面在声明必需特征时所看到的那样，GattLocalCharacteristics 具有 3 种类型的事件，即 ReadRequested、WriteRequested 和 SubscribedClientsChanged。

### <a name="read"></a>Read
当远程设备尝试从特征读取值（并且它不是常数值）时，会调用 ReadRequested 事件。 对其调用读取的特征以及参数（包含有关远程设备的信息）会传递到委托： 

```csharp
characteristic.ReadRequested += Characteristic_ReadRequested;
// ... 

async void ReadCharacteristic_ReadRequested(GattLocalCharacteristic sender, GattReadRequestedEventArgs args)
{
    var deferral = args.GetDeferral();
    
    // Our familiar friend - DataWriter.
    var writer = new DataWriter();
    // populate writer w/ some data. 
    // ... 

    var request = await args.GetRequestAsync();
    request.RespondWithValue(writer.DetachBuffer());
    
    deferral.Complete();
}
``` 

### <a name="write"></a>写入
当远程设备尝试向特征写入值时，会使用有关远程设备的详细信息、要向其写入的特征和值本身来调用 WriteRequested 事件： 

```csharp
characteristic.ReadRequested += Characteristic_ReadRequested;
// ...

async void WriteCharacteristic_WriteRequested(GattLocalCharacteristic sender, GattWriteRequestedEventArgs args)
{
    var deferral = args.GetDeferral();
    
    var request = await args.GetRequestAsync();
    var reader = DataReader.FromBuffer(request.Value);
    // Parse data as necessary. 

    if (request.Option == GattWriteOption.WriteWithResponse)
    {
        request.Respond();
    }
    
    deferral.Complete();
}
```
有 2 种类型的写入，即具有和没有响应。 使用 GattWriteOption（GattWriteRequest 对象上的属性）查明远程设备所执行的写入类型。 

## <a name="send-notifications-to-subscribed-clients"></a>向订阅的客户端发送通知
通知是最常见的 GATT 服务器操作，可执行将数据推送到远程设备的关键功能。 有时，你要通知所有订阅的客户端，但是其他时候可能要选取要将新值发送到的设备： 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

当新设备订阅通知时，会调用 SubscribedClientsChanged 事件： 

```csharp
characteristic.SubscribedClientsChanged += SubscribedClientsChanged;
// ...

void _notifyCharacteristic_SubscribedClientsChanged(GattLocalCharacteristic sender, object args)
{
    List<GattSubscribedClient> clients = sender.SubscribedClients;
    // Diff the new list of clients from a previously saved one 
    // to get which device has subscribed for notifications. 

    // You can also just validate that the list of clients is expected for this app.  
}

```
> 请注意，应用程序可以使用 MaxNotificationSize 属性获取特定客户端的最大通知大小。  系统会截断任何大于最大大小的数据。

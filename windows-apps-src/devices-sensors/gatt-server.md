---
author: msatranjr
title: 蓝牙 GATT 服务器
description: 本文概述了对于通用 Windows 平台 (UWP) 应用程序，以及用于常见的使用案例示例代码的蓝牙泛型属性配置文件 (GATT) 服务器。
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 27154fbb535b76995fba97702e65a9c0b2a8291c
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "610762"
---
# <a name="bluetooth-gatt-server"></a>蓝牙 GATT 服务器


**重要的 API**
- [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)


本文介绍对于通用 Windows 平台 (UWP) 应用程序，以及 GATT 服务器的常见任务的示例代码的蓝牙泛型属性 (GATT) 服务器 Api: 
- 定义的受支持的服务
- 发布服务器，使其可以通过远程客户端发现
- 公布支持服务
- 读取和写入请求的响应
- 将通知发送到已订阅的客户端

## <a name="overview"></a>概述
在客户端角色通常运行 Windows。 不过，很多情况下出现要求 Windows 充当蓝牙 LE GATT 服务器以及。 对于 IoT 设备，大多数跨平台的通信以及几乎所有方案将都需要 Windows 为 GATT 服务器。 此外，将通知发送给附近 wearable 设备已成为需要此技术以及流行方案。  
> 确保在继续之前清除[GATT 客户端文档](gatt-client.md)中所有的概念。  

服务器操作都将围绕服务提供商和 GattLocalCharacteristic。 这两个类将提供所需声明、 实现和公开的数据的远程设备层次结构的功能。

## <a name="define-the-supported-services"></a>定义的受支持的服务
您的应用程序可能声明将发布的 Windows 的一个或多个服务。 由 UUID 唯一标识每个服务。 

### <a name="attributes-and-uuids"></a>属性和 Uuid
定义每个服务、 特征和描述符入时很自己的唯一 128 位 UUID。
> 所有 Windows Api 使用的术语的 GUID，但蓝牙标准定义这些作为 Uuid。 我们为了，以下两个字词可互换，以便我们将继续使用这个词 UUID。 

如果该属性是标准和中定义的蓝牙签名定义，还会有一个相应的 16 位短 ID (例如电池级别 UUID 是 0000**2A19**-0000-1000年-8000-00805F9B34FB 和短 ID 是 0x2A19)。 可以看到这些标准 Uuid [GattServiceUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids.aspx)和[GattCharacteristicUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids.aspx)中。

如果实现您的应用程序有自己的自定义服务，必须自定义 UUID 生成。 这是轻松地完成在 Visual Studio 中通过工具-> CreateGuid （使用选项 5 以获取中的"xxxxxxxx-格式-...格式"格式）。 现在可以使用此 uuid 声明新的本地服务、 特征或描述符。

#### <a name="restricted-services"></a>受限的服务
以下服务保留由系统，此时无法发布：
1. 设备信息服务 （疏散）
2. 泛型属性配置文件服务 (GATT)
3. 一般访问配置文件服务 （间隙）
4. 人体学接口设备服务 (HOGP)
5. 扫描参数服务 (SCP)

> 试图创建阻止的服务将导致 BluetoothError.DisabledByPolicy CreateAsync 调用从其返回。

#### <a name="generated-attributes"></a>生成的属性
以下描述符是由系统，基于 GattLocalCharacteristicParameters 特征的创建过程中提供的自动生成：
1. 客户端特征，其配置 （如果特征标记为 indicatable 或 notifiable）。
2. 典型用户说明 （如果 UserDescription 属性设置）。 请参阅 GattLocalCharacteristicParameters.UserDescription 属性的详细信息。
3. 典型的格式 （一个描述符为每个指定的演示文稿格式）。  请参阅 GattLocalCharacteristicParameters.PresentationFormats 属性的详细信息。
4. Characteristic 聚合格式 （如果指定多个演示文稿格式）。  GattLocalCharacteristicParameters.See PresentationFormats 属性的详细信息。
5. Characteristic 扩展属性 （如果用扩展的属性位标记特征）。

> 通过 ReliableWrites 和 WritableAuxiliaries 特征属性确定扩展属性描述符的值。

> 试图创建保留的描述符将导致引发异常。

> 请注意，广播不支持这一次。  指定广播 GattCharacteristicProperty 将导致引发异常。

### <a name="build-up-the-heirarchy-of-services-and-characteristics"></a>构建服务和特征的层次结构
GattServiceProvider 用于创建并公布根主要服务定义。  每个服务需要它是自己 GUID 中所需的服务提供商处对象： 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> 主要服务是 GATT 树的最高级别。 主要服务包含特征以及其他服务 （称为包括或辅助服务）。 

现在，填充它具有所需的特征和描述符服务：

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
如上所示，这也是声明事件处理程序的每个特征支持的操作的好地方。  要正确响应请求，应用程序必须定义并为特性支持每个请求类型的事件处理程序。  未能注册处理程序将导致请求被立即完成与*UnlikelyError*系统。

### <a name="constant-characteristics"></a>常量的特征
有时，会将不会更改过程中的应用程序生命周期的典型值。 在这种情况下，最好声明常量的特征，以防止不必要的应用程序激活： 

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
## <a name="publish-the-service"></a>发布该服务
一旦服务完全定义下, 一步是将发布支持服务。 这将通知操作系统远程设备执行发现服务时，应返回该服务。  必须设置两个属性-IsDiscoverable 和 IsConnectable:  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**： 公布到广告，使设备可供搜索中的远程设备的友好名称。
- **IsConnectable**： 公布为在外围角色中使用的可连接公告。

> 可发现和 Connectable 服务时，系统将添加到广告数据包的服务 Uuid。  广告数据包中有仅 31 个字节和 128 位 UUID 所占用的 16 ！

> 请注意，在前台发布服务时，应用程序必须 StopAdvertising 时调用应用程序挂起。

## <a name="respond-to-read-and-write-requests"></a>读取和写入请求的响应
正如我们看到上方而声明的必需的特征，GattLocalCharacteristics 具有 3 种类型的事件-ReadRequested、 WriteRequested 和 SubscribedClientsChanged。

### <a name="read"></a>已阅读
当远程设备尝试从特征读取值 （并不是以常量值） 时，调用 ReadRequested 事件。 读取调用 args （包含有关远程设备） 以及特征传递给委托： 

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
当远程设备尝试将值写入特征时，使用设备的详细信息远程，无法写入到的特征和值本身调用 WriteRequested 事件： 

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
有两种类型的写入-具有和没有响应。 使用 GattWriteOption （GattWriteRequest 对象的属性） 明白哪种类型的写执行远程设备。 

## <a name="send-notifications-to-subscribed-clients"></a>将通知发送到已订阅的客户端
最常见的 GATT 服务器操作，通知执行将数据推送到远程设备的关键功能。 有时，您需要通知所有已订阅的客户端但 othertimes 要选择哪些设备可以发送的新值： 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

当新设备订阅通知时，获取调用 SubscribedClientsChanged 事件： 

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
> 请注意，应用程序可以获得最大通知大小特定客户端使用 MaxNotificationSize 属性。  任何大于最大大小的数据将被截断系统。

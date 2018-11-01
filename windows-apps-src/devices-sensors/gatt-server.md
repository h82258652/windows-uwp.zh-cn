---
author: msatranjr
title: 蓝牙 GATT 服务器
description: 本文概述了适用于通用 Windows 平台 (UWP) 应用，以及有关常见的用例的示例代码的蓝牙通用属性配置文件 (GATT) 服务器。
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b8a941b7b80bd5d34e88798ec586d9c1d52e2887
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5875495"
---
# <a name="bluetooth-gatt-server"></a>蓝牙 GATT 服务器


**重要的 API**
- [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)


本文介绍了适用于通用 Windows 平台 (UWP) 应用，以及有关常见的 GATT 服务器任务的示例代码的蓝牙通用属性 (GATT) 服务器 Api: 
- 定义的受支持的服务
- 发布服务器，以便它可以发现的远程客户端
- 播发服务支持
- 若要读取和写入请求进行响应
- 将通知发送到订阅的客户端

## <a name="overview"></a>概述
在客户端角色通常运行 Windows。 不过，许多方案会出现这需要 Windows 将充当蓝牙 LE GATT 服务器以及。 适用于 IoT 设备，以及大多数跨平台的通信的几乎所有方案都需要 Windows GATT 服务器。 此外，将通知发送到附近可穿戴设备已成为一个受欢迎的方案，该要求以及此技术。  
> 请确保[GATT 客户端文档](gatt-client.md)中的所有概念之前清除。  

服务器操作将围绕服务提供商和 GattLocalCharacteristic。 这两个类将提供声明、 实现和公开到远程设备的数据结构所需的功能。

## <a name="define-the-supported-services"></a>定义的受支持的服务
你的应用可能会声明将发布的 Windows 的一个或多个服务。 UUID 唯一地标识每个服务。 

### <a name="attributes-and-uuids"></a>属性和 Uuid
定义每个服务、 特征和描述符的是自己的唯一 128 位 UUID。
> 所有的 Windows Api 使用的术语 GUID，但蓝牙标准定义这些后台任务视为 Uuid。 对我们来说，这些两个术语彼此互换，因此我们将继续使用术语 UUID。 

如果该属性是标准和定义蓝牙 SIG 定义，还会有一个相应的 16 位短 ID (例如电池级别 UUID 是 0000**2A19**-0000-1000年-8000-00805F9B34FB 和简短的 ID 是 0x2A19)。 这些标准 Uuid 可以[GattServiceUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids.aspx)和[GattCharacteristicUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids.aspx)中所示。

如果你的应用实现它自己的自定义服务，自定义 UUID 需要生成。 这是轻松地完成在 Visual Studio 中通过工具-> CreateGuid （使用选项 5 进入"xxxxxxxx-xxxx-...xxxx"格式）。 现在可以使用此 uuid 来声明新的本地服务、 特征或描述符。

#### <a name="restricted-services"></a>受限的服务
以下服务由系统保留，并且不能这一次发布：
1. 设备信息服务 （疏散）
2. 通用属性配置文件服务 (GATT)
3. 通用访问个人资料服务 （间隙）
4. 人体学接口设备服务 (HOGP)
5. 扫描参数服务 (SCP)

> 尝试创建阻止的服务将导致 BluetoothError.DisabledByPolicy 从 CreateAsync 调用返回。

#### <a name="generated-attributes"></a>生成的属性
以下描述符是自动生成系统，具体取决于 GattLocalCharacteristicParameters 提供期间创建的特征：
1. 客户端特征配置 （如果特征标记为 indicatable 或 notifiable）。
2. 特征用户说明 （如果 UserDescription 设置）。 请参阅 GattLocalCharacteristicParameters.UserDescription 属性的详细信息。
3. 特征格式 （对于指定每个演示文稿格式的一个描述符）。  请参阅 GattLocalCharacteristicParameters.PresentationFormats 属性的详细信息。
4. 特征聚合格式 （如果未指定多个演示文稿格式）。  有关详细信息的 GattLocalCharacteristicParameters.See PresentationFormats 属性。
5. 特征扩展属性 （如果特征带有扩展的属性位）。

> 通过 ReliableWrites 和 WritableAuxiliaries 特征属性确定的扩展属性描述符的值。

> 尝试创建保留的描述符将导致出现异常。

> 请注意，广播不支持这一次。  指定广播 GattCharacteristicProperty 将导致出现异常。

### <a name="build-up-the-heirarchy-of-services-and-characteristics"></a>建立服务和特性的层次结构
GattServiceProvider 用于创建和播发根主要服务定义。  每个服务需要它是自己的服务提供商对象，在 GUID 中： 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> 主要服务是 GATT 树的最高级别。 主要服务包含特征以及其他服务 （称为包含或辅助服务）。 

现在，填充使用所需的特征和描述符的服务：

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
如上所示，这也是不错的声明每个特性支持的操作的事件处理程序。  若要正确响应的请求，应用必须定义，并为该属性支持每个请求类型设置事件处理程序。  无法注册一个处理程序会被立即完成与*UnlikelyError*由系统在请求中。

### <a name="constant-characteristics"></a>常量特性
有时，存在不会更改应用的生命周期期间的特性值。 在此情况下，最好声明常量的特征，以防止不必要的应用激活： 

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
一旦该服务已完全定义下, 一步是发布服务支持。 这将通知操作系统在远程设备执行服务发现时，应返回该服务。  你将需要设置两个属性的 IsDiscoverable 和 IsConnectable:  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**： 公布在广告，使设备发现远程设备的友好名称。
- **IsConnectable**： 公布用于外设角色可连接的广告。

> 可发现和 Connectable 服务时，系统会将服务 Uuid 添加到广告数据包。  广告数据包中只有 31 字节，并且其中 16 所需的 128 位 UUID ！

> 请注意，在前台发布服务时，应用程序时必须调用 StopAdvertising 应用程序挂起。

## <a name="respond-to-read-and-write-requests"></a>若要读取和写入请求进行响应
正如我们之前所见而声明所需的特征，GattLocalCharacteristics 将产生 3 种类型的事件-ReadRequested、 WriteRequested 和 SubscribedClientsChanged。

### <a name="read"></a>已阅读
在远程设备尝试读取特性值 （并且不常数值） 时，调用 ReadRequested 事件。 读取调用以及参数 （包含在远程设备有关的信息） 的特点传递给委托： 

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
当在远程设备尝试值写入特性时，有关详细信息在远程设备，以写入的特性和值本身调用 WriteRequested 事件： 

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
有两种类型的写入-带有和不响应。 使用 GattWriteOption （GattWriteRequest 对象的属性） 来查明在远程设备正在执行哪种类型的写入。 

## <a name="send-notifications-to-subscribed-clients"></a>将通知发送到订阅的客户端
最常见的 GATT 服务器操作，通知执行将数据推送到远程设备的关键功能。 有时，你会想要通知所有订阅的客户端，但 othertimes 你可能想要选择哪个设备发送给新值： 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

当通知订阅的新设备时，获取调用 SubscribedClientsChanged 事件： 

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
> 请注意，应用程序可以获得最大通知大小与 MaxNotificationSize 属性的特定客户端。  任何大于最大大小的数据将被截断由系统。

---
ms.assetid: 5B3A6326-15EE-4618-AA8C-F1C7FB5232FB
title: Bluetooth RFCOMM
description: 本文提供通用 Windows 平台 (UWP) 应用中的蓝牙 RFCOMM 的概述，以及如何发送或接收文件的示例代码。
---
# Bluetooth RFCOMM

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** 重要的 API **

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.Rfcomm**](https://msdn.microsoft.com/library/windows/apps/Dn263529)

本文提供通用 Windows 平台 (UWP) 应用中的蓝牙 RFCOMM 的概述，以及如何发送或接收文件的示例代码。

## 概述

[
            **Windows.Devices.Bluetooth.Rfcomm**](https://msdn.microsoft.com/library/windows/apps/Dn263529) 命名空间中的 API 以面向 Windows.Devices 的现有模式为基础而构建，包括 [**enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) 和 [**instantiation**](https://msdn.microsoft.com/library/windows/apps/BR225654)。 数据读取和编写旨在充分利用 [**established data stream patterns**](https://msdn.microsoft.com/library/windows/apps/BR208119) 和 [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/BR241791) 中的对象。 会话描述协议 (SDP) 属性有一个值和一个预期类型。 但是，一些常用的设备具有错误的 SDP 属性实现，其中的值不属于预期类型。 此外，RFCOMM 的许多用法完全不需要其他 SDP 属性。 因此，此 API 提供对未解析 SDP 数据的访问权限，开发人员可从这里获取所需的信息。

RFCOMM API 使用服务标识符的概念。 尽管服务标识符只是一个 128 位 GUID，它通常还被指定为 16 或 32 位的整数。 RFCOMM API 为服务标识符提供包装器，允许将它们作为 128 位 GUID 以及 32 位整数来指定和使用，但是不提供 16 位整数。 这对 API 来说不是问题，因为语言可以自动扩大到 32 位整数，并且仍然可以正确生成标识符。

应用可在后台任务中执行多步设备操作，即使系统将应用移到后台并暂停，操作也能继续运行至完成。 这可实现可靠的设备服务（例如对永久性设置或固件的更改）和内容同步，并且无需用户坐在旁边盯着进度栏。 将 [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn297315) 用于设备服务，将 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn297337) 用于内容同步。 请注意，这些后台任务对应用可在后台运行的时间长度有所限制，并不允许无限期操作或无限同步。

## 作为客户端发送文件

发送文件时，最基本的方案是基于所需服务连接到配对设备。 这包括以下步骤：

-   使用 **RfcommDeviceService.GetDeviceSelector\*** 函数帮助生成 AQS 查询，该查询可用于枚举所需服务的配对设备实例。
-   选取一个枚举的设备，创建 [**RfcommDeviceService**](https://msdn.microsoft.com/library/windows/apps/Dn263463)，并按需读取 SDP 属性（使用 [**established data helpers**](https://msdn.microsoft.com/library/windows/apps/BR208119) 解析该属性的数据）。
-   创建一个套接字并使用 [**StreamSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/Hh701504) 的 [**RfcommDeviceService.ConnectionHostName**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.rfcomm.rfcommdeviceservice.connectionhostname.aspx) 和 [**RfcommDeviceService.ConnectionServiceName**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.rfcomm.rfcommdeviceservice.connectionservicename.aspx) 属性，通过 StreamSocket.ConnectAsync 操作连接到使用适当参数的远程设备服务。
-   按照现成的数据流模式从文件读取数据区块，并在该套接字的 [**StreamSocket.OutputStream**](https://msdn.microsoft.com/library/windows/apps/BR226920) 上将其发送到设备。

```csharp
Windows.Devices.Bluetooth.RfcommDeviceService _service;
Windows.Networking.Sockets.StreamSocket _socket;

async void Initialize()
{
    // Enumerate devices with the object push service
    auto services =
        await Windows.Devices.Enumeration.DeviceInformation.FindAllAsync(
            RfcommDeviceService.GetDeviceSelector(
                RfcommServiceId.ObexObjectPush));

    if (services.Count > 0) 
    {
        // Initialize the target Bluetooth BR device
        auto service = await RfcommDeviceService.FromIdAsync(services[0].Id);

        // Check that the service meets this App’s minimum requirement
        if (SupportsProtection(service) &amp;&amp; IsCompatibleVersion(service))
        {
            _service = service;

            // Create a socket and connect to the target
            _socket = new StreamSocket();
            await _socket.ConnectAsync(
                _service.ConnectionHostName,
                _service.ConnectionServiceName,
                SocketProtectionLevel
                    .BluetoothEncryptionAllowNullAuthentication);

            // The socket is connected. At this point the App can wait for
            // the user to take some action, e.g. click a button to send a
            // file to the device, which could invoke the Picker and then
            // send the picked file. The transfer itself would use the
            // Sockets API and not the Rfcomm API, and so is omitted here for
            // brevity.
        }
    }
}

// This App requires a connection that is encrypted but does not care about
// whether its authenticated.
bool SupportsProtection(RfcommDeviceService service)
{
    switch (service.ProtectionLevel)
    {
    case SocketProtectionLevel.PlainSocket:
        if ((service.MaximumProtectionLevel == SocketProtectionLevel
                .BluetoothEncryptionWithAuthentication)
            || (service.MaximumProtectionLevel == SocketProtectionLevel
                .BluetoothEncryptionAllowNullAuthentication)
        {
            // The connection can be upgraded when opening the socket so the
            // App may offer UI here to notify the user that Windows may
            // prompt for a PIN exchange.
            return true;
        }
        else
        {
            // The connection cannot be upgraded so an App may offer UI here
            // to explain why a connection won’t be made.
            return false;
        }
    case SocketProtectionLevel.BluetoothEncryptionWithAuthentication:
        return true;
    case SocketProtectionLevel.BluetoothEncryptionAllowNullAuthentication:
        return true;
    }
    return false;
}

// This App relies on CRC32 checking available in version 2.0 of the service.
const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint MINIMUM_SERVICE_VERSION = 200;
bool IsCompatibleVersion(RfcommDeviceService service)
{
    auto attributes = await service.GetSdpRawAttributesAsync(
        BluetothCacheMode.Uncached);
    auto attribute = attributes[SERVICE_VERSION_ATTRIBUTE_ID];
    auto reader = DataReader.FromBuffer(attribute);

    // The first byte contains the attribute' s type
    byte attributeType = reader.ReadByte();
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint version = reader.Uint32();
        return version >= MINIMUM_SERVICE_VERSION;
    }
}
```

```cpp
Windows::Devices::Bluetooth::RfcommDeviceService^ _service;
Windows::Networking::Sockets::StreamSocket^ _socket;

void Initialize()
{
    // Enumerate devices with the object push service
    create_task(
        Windows::Devices::Enumeration::DeviceInformation::FindAllAsync(
            RfcommDeviceService::GetDeviceSelector(
                RfcommServiceId::ObexObjectPush)))
    .then([](DeviceInformationCollection^ services)
    {
        if (services->Size > 0) 
        {
            // Initialize the target Bluetooth BR device
            create_task(RfcommDeviceService::FromIdAsync(services[0]->Id))
            .then([](RfcommDeviceService^ service)
            {
                // Check that the service meets this App’s minimum
                // requirement
                if (SupportsProtection(service)
                    &amp;&amp; IsCompatibleVersion(service))
                {
                    _service = service;

                    // Create a socket and connect to the target
                    _socket = ref new StreamSocket();
                    create_task(_socket->ConnectAsync(
                        _service->ConnectionHostName,
                        _service->ConnectionServiceName,
                        SocketProtectionLevel
                            ::BluetoothEncryptionAllowNullAuthentication)
                    .then([](void)
                    {
                        // The socket is connected. At this point the App can
                        // wait for the user to take some action, e.g. click
                        // a button to send a file to the device, which could
                        // invoke the Picker and then send the picked file.
                        // The transfer itself would use the Sockets API and
                        // not the Rfcomm API, and so is omitted here for
                        //brevity.
                    });
                }
            });
        }
    });
}

// This App requires a connection that is encrypted but does not care about
// whether its authenticated.
bool SupportsProtection(RfcommDeviceService^ service)
{
    switch (service->ProtectionLevel)
    {
    case SocketProtectionLevel->PlainSocket:
        if ((service->MaximumProtectionLevel == SocketProtectionLevel
                ::BluetoothEncryptionWithAuthentication)
            || (service->MaximumProtectionLevel == SocketProtectionLevel
                ::BluetoothEncryptionAllowNullAuthentication)
        {
            // The connection can be upgraded when opening the socket so the
            // App may offer UI here to notify the user that Windows may
            // prompt for a PIN exchange.
            return true;
        }
        else
        {
            // The connection cannot be upgraded so an App may offer UI here
            // to explain why a connection won’t be made.
            return false;
        }
    case SocketProtectionLevel::BluetoothEncryptionWithAuthentication:
        return true;
    case SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication:
        return true;
    }
    return false;
}

// This App relies on CRC32 checking available in version 2.0 of the service.
const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint MINIMUM_SERVICE_VERSION = 200;
bool IsCompatibleVersion(RfcommDeviceService^ service)
{
    auto attributes = await service->GetSdpRawAttributesAsync(
        BluetoothCacheMode::Uncached);
    auto attribute = attributes[SERVICE_VERSION_ATTRIBUTE_ID];
    auto reader = DataReader.FromBuffer(attribute);

    // The first byte contains the attribute' s type
    byte attributeType = reader->ReadByte();
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint version = reader->Uint32();
        return version >= MINIMUM_SERVICE_VERSION;
    }
}
```

## 作为服务器接收文件

另一个常用的 RFCOMM 应用方案是，在电脑上托管服务并将其公开以用于其他设备。

-   创建一个 [**RfcommServiceProvider**](https://msdn.microsoft.com/library/windows/apps/Dn263511) 以播发所需的服务。
-   根据需要设置 SDP 属性（使用 [**established data helpers**](https://msdn.microsoft.com/library/windows/apps/BR208119) 生成该属性的数据）并开始播发 SDP 记录供其他设备检索。
-   若要连接到客户端设备，请创建套接字侦听器以开始侦听传入的连接请求。
-   当收到连接时，请存储连接的套接字以便以后进行处理。
-   按照现成的数据流模式从套接字的 InputStream 读取数据区块，并将其保存到文件。

```csharp
Windows.Devices.Bluetooth.RfcommServiceProvider _provider;

async void Initialize()
{
    // Initialize the provider for the hosted RFCOMM service
    _provider = await Windows.Devices.Bluetooth.
        RfcommServiceProvider.CreateAsync(RfcommServiceId.ObexObjectPush);

    // Create a listener for this service and start listening
    StreamSocketListener listener = new StreamSocketListener();
    listener.ConnectionReceived += OnConnectionReceived;
    await listener.BindServiceNameAsync(
        _provider.ServiceId.AsString(),
        SocketProtectionLevel
            .BluetoothEncryptionAllowNullAuthentication);

    // Set the SDP attributes and start advertising
    InitializeServiceSdpAttributes(_provider);
    _provider.StartAdvertising();
}

const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint SERVICE_VERSION = 200;
void InitializeServiceSdpAttributes(RfcommServiceProvider provider)
{
    auto writer = new Windows.Storage.Streams.DataWriter();

    // First write the attribute type
    writer.WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE)
    // Then write the data
    writer.WriteUint32(SERVICE_VERSION);
    
    auto data = writer.DetachBuffer();
    provider.SdpRawAttributes.Add(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceived(
    StreamSocketListener listener,
    StreamSocketListenerConnectionReceivedEventArgs args)
{
    // Stop advertising/listening so that we’re only serving one client
    _provider.StopAdvertising();
    await listener.Close();
    _socket = args.Socket;

    // The client socket is connected. At this point the App can wait for
    // the user to take some action, e.g. click a button to receive a file
    // from the device, which could invoke the Picker and then save the
    // received file to the picked location. The transfer itself would use
    // the Sockets API and not the Rfcomm API, and so is omitted here for
    // brevity.
}
```

```cpp
Windows::Devices::Bluetooth::RfcommServiceProvider^ _provider;

void Initialize()
{
    // Initialize the provider for the hosted RFCOMM service
    create_task(Windows::Devices::Bluetooth.
        RfcommServiceProvider::CreateAsync(
            RfcommServiceId::ObexObjectPush))
    .then([](RfcommServiceProvider^ provider) -> task<void> {
        _provider = provider;

        // Create a listener for this service and start listening
        auto listener = ref new StreamSocketListener();
        listener->ConnectionReceived += ref new TypedEventHandler<
                StreamSocketListener^,
                StreamSocketListenerConnectionReceivedEventArgs^>
           (&amp;OnConnectionReceived);
        return create_task(listener->BindServiceNameAsync(
            _provider->ServiceId->AsString(),
            SocketProtectionLevel
                ::BluetoothEncryptionAllowNullAuthentication));
    }).then([listener](void) {
        // Set the SDP attributes and start advertising
        InitializeServiceSdpAttributes(_provider);
        _provider->StartAdvertising();
    });
}

const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint SERVICE_VERSION = 200;
void InitializeServiceSdpAttributes(RfcommServiceProvider^ provider)
{
    auto writer = ref new Windows::Storage::Streams::DataWriter();

    // First write the attribute type
    writer->WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE)
    // Then write the data
    writer->WriteUint32(SERVICE_VERSION);
    
    auto data = writer->DetachBuffer();
    provider->SdpRawAttributes->Add(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceived(
    StreamSocketListener^ listener,
    StreamSocketListenerConnectionReceivedEventArgs^ args)
{
    // Stop advertising/listening so that we’re only serving one client
    _provider->StopAdvertising();
    create_task(listener->Close())
    .then([args](void) {
        _socket = args->Socket;

        // The client socket is connected. At this point the App can wait for
        // the user to take some action, e.g. click a button to receive a
        // file from the device, which could invoke the Picker and then save
        // the received file to the picked location. The transfer itself
        // would use the Sockets API and not the Rfcomm API, and so is
        // omitted here for brevity.
    });
}
```



<!--HONumber=Mar16_HO1-->



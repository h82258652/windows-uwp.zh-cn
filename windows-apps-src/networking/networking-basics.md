---
author: stevewhims
description: 针对任何支持网络的应用的必做事项。
title: 网络基础知识
ms.assetid: 1F47D33B-6F00-4F74-A52D-538851FD38BE
ms.author: stwhi
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 50ac9fcf984fa6c4ebad7e480ebfc2d002256e26
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2018
ms.locfileid: "5995838"
---
# <a name="networking-basics"></a>网络基础知识
针对任何支持网络的应用的必做事项。

## <a name="capabilities"></a>功能
要使用网络功能，你必须将相应的功能元素添加到你的应用清单中。 如果你的应用清单中未指定任何网络功能，则你的应用将不具备任何网络功能，从而连接到网络的任何尝试都将失败。

以下是最常用的网络功能。

| 功能 | 描述 |
|------------|-------------|
| **internetClient** | 提供对 Internet 及公共场所（如机场和咖啡厅）网络的出站访问。 多数需要进行 Internet 访问的应用都应使用此功能。 |
| **internetClientServer** | 为应用提供来自 Internet 及公共场所（如机场和咖啡厅）网络的入站和出站访问。 |
| **privateNetworkClientServer** | 为应用提供用户信任场所（如家里和办公室）中的入站和出站网络访问。 |

在某些情况下，你的应用可能还需要具备其他功能。

| 功能 | 描述 |
|------------|-------------|
| **enterpriseAuthentication** | 允许应用连接至要求提供域凭据的网络资源。 如要启用此功能，域管理员必须为所有应用均启用此功能。 示例将是从专用 Intranet 上的 Sharepoint 服务器中检索数据的应用。 <br/> 通过此功能可以使用你的凭据来访问要求提供凭据的网络中的网络资源。 具有此功能的应用可在网络上模拟其用户。 <br/> 无需此功能，应用即可通过身份验证的代理访问 Internet。 |
| **proximity** | 与邻近计算机的设备进行近距离感应通信所必需的功能。 近距离感应可用于向附近设备上的应用程序发送邀请或与其进行连接。 <br/> 此功能让应用可以访问邻近设备网络，并在用户同意发送或接受邀请的情况下与这些设备进行连接。 |
| **sharedUserCertificates** | 此功能让应用可以访问软件和硬件证书，如智能卡证书。 在运行过程中调用此功能时，用户必须采取插入卡或选择证书等操作。 <br/> 使用此功能时，应用会将你的软件和硬件证书或智能卡用于识别。 你的雇主、银行或政府服务机构可以使用此功能来进行识别。 |

## <a name="communicating-when-your-app-is-not-in-the-foreground"></a>当你的应用不在前台时通信
[使用后台任务支持应用](https://msdn.microsoft.com/library/windows/apps/mt299103)包含了有关应用不在前台时使用后台任务进行工作的常规信息。 更具体地说，当它不是当前的前台应用，但数据仍通过网络发送给它时，你的代码必须采取特殊的步骤以接收通知。 控制通道触发器用于 Windows8，在此目的，并且它们在 windows 10 中仍受支持。 [**here**](https://msdn.microsoft.com/library/windows/apps/hh701032) 提供了有关使用控制通道触发器的完整信息。 Windows 10 中的新技术提供更好的功能降低开销，对于某些方案，例如已启用推送的流套接字： 套接字代理和套接字活动触发器。

如果你的应用使用了 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)、[**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 或 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906)，则你的应用可以将开放套接字的所有权转移给系统提供的套接字代理，然后退出前台甚至终止。 在已传输的套接字上建立连接或流量送达该套接字后，你的应用或其指定的后台任务将被激活。 如果你的应用未运行，它将启动。 然后，套接字代理将使用 [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) 通知你的应用收到新流量。 你的应用将从套接字代理回收套接字并处理该套接字上的流量。 这意味着，当你的应用未处理网络流量时，将消耗非常少的系统资源。

套接字代理旨在替代控制通道触发器（如果适用），因为前者能够提供相同的功能，但限制更少且内存占用更小。 套接字代理可由非锁屏应用使用，并且其在手机上的使用方式与其他设备上的使用方式相同。 当流量送达以便由套接字代理激活应用时，这些应用无需处于运行状态。 并且套接字代理支持在 TCP 套接字上侦听，而控制通道触发器不支持此操作。

### <a name="choosing-a-network-trigger"></a>选择网络触发器
在一些应用场景中，这两种触发器均适宜。 当你选择要在应用中使用的触发器类型时，请考虑以下建议。

-   如果你使用的是 [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151)、[**System.Net.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 或 [System.Net.Http.HttpClientHandler](http://go.microsoft.com/fwlink/p/?linkid=241638)，则必须使用 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)。
-   如果使用的是已启用推送的 **StreamSockets**，则可以使用控制通道触发器，不过应该首选 [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009)。 当目前未使用连接时，选择后者允许系统释放内存并会降低电源要求。
-   如果你想要在当前没有服务网络请求时最大程度减少你的应用的内存占用，则首选 [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009)（如果可能）。
-   如果你希望你的应用能够在系统处于连接待机模式下时接收数据，应使用 [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009)。

有关如何使用套接字代理的详细信息和示例，请参阅[后台网络通信](network-communications-in-the-background.md)。

## <a name="secured-connections"></a>安全连接
安全套接字层 (SSL) 和最新的传输层安全 (TLS) 都是旨在为网络通信提供身份验证和加密功能的加密协议。 这些协议专门用于在发送和接收网络数据时防止发生窃听和篡改。 这些协议使用一种客户端-服务器模型进行协议交换。 这些协议还会使用数字证书和证书颁发机构来验证服务器是否是其声明的服务器。

### <a name="creating-secure-socket-connections"></a>创建安全套接字连接
[**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 对象可以配置为在客户端和服务器之间使用 SSL/TLS 进行通信。 对 SSL/TLS 的这一支持仅限于在 SSL/TLS 协商中将 **StreamSocket** 对象用作客户端。 当收到传入通信时，你无法将 SSL/TLS 与 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 所创建的 **StreamSocket** 一起使用，因为 **StreamSocket** 类没有实现作为服务器的 SSL/TLS 协商。

有以下两种方法可以借助 SSL/TLS 确保 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 连接的安全：

-   [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) - 建立到网络服务的初始连接并立即协商对所有通信使用 SSL/TLS。
-   [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) - 先不加密连接到网络服务。 应用可以发送或接收数据。 然后升级连接，对此后所有通信使用 SSL/TLS。

SocketProtectionLevel 指定应用建立或升级连接所需的套接字保护级别。 但是，对于已建立的连接，其最终保护级别取决于连接的两个终结点之间的协商进程。 如果另一个终结点需要更低的保护级别，则最终的保护级别可能要比你指定的级别更低。 

 异步操作成功完成后，你可以通过 [**StreamSocketinformation.ProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/hh967868) 属性检索 [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) 或 [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) 调用中使用的要求保护级别。 但是，这不会反映连接使用的实际保护级别。

> [!NOTE]
> 你的代码不应隐式依赖于使用特定的保护级别或默认使用给定安全级别的假设。 安全状况会不断变化，为了避免使用带有已知缺陷的协议，协议和默认保护级别将随着时间的推移而发生变化。 默认值可能会因单个计算机配置或已安装的软件和已应用的修补程序而异。 如果应用依赖于使用特定的安全级别，则必须明确指定该级别，然后进行检查以确保在已建立的连接上实际使用了该安全级别。

### <a name="use-connectasync"></a>使用 ConnectAsync
[**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) 可用于建立与网络服务的初始连接，并随后立即协商对所有通信使用 SSL/TLS。 有两种 **ConnectAsync** 方法支持传递 *protectionLevel* 参数：

-   [**ConnectAsync(EndpointPair, SocketProtectionLevel)**](https://msdn.microsoft.com/library/windows/apps/hh701511) - 在 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 对象上启动异步操作以连接到指定为 [**EndpointPair**](https://msdn.microsoft.com/library/windows/apps/hh700953) 对象和 [**SocketProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/br226880) 的远程网络目标。
-   [**ConnectAsync(HostName, String, SocketProtectionLevel)**](https://msdn.microsoft.com/library/windows/apps/br226916) - 在 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 对象上启动异步操作以连接到由远程主机名、远程服务名和 [**SocketProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/br226880) 所指定的远程目标。

如果 *protectionLevel* 参数设置为 **Windows.Networking.Sockets.SocketProtectionLevel.Ssl**，当调用上述任一 [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) 方法时，必须建立 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 才能使用 SSL/TLS 进行加密。 此值需要加密而且绝不允许使用 NULL 密码。

一般来说，使用这些 [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) 方法的顺序都是相同的。

-   创建一个 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)。
-   如果需要在套接字上使用高级选项，请使用 [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226917) 属性获取与 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 对象相关联的 [**StreamSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226893) 实例。 针对 **StreamSocketControl** 设置一个属性。
-   调用上述 [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) 方法之一以启动连接到远程目标的操作，并立即协商使用 SSL/TLS。
-   实际上使用 [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) 协商得到的 SSL 强度可在异步操作成功完成后通过获取 [**StreamSocketinformation.ProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/hh967868) 属性来确定。

以下示例将会创建 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)，尝试建立与网络服务的连接并立即协商使用 SSL/TLS。 如果协商成功，则在客户端和网络服务器之间使用 **StreamSocket** 的所有网络通信都将加密。

```csharp
using Windows.Networking;
using Windows.Networking.Sockets;

    // Define some variables and set values
    StreamSocket clientSocket = new StreamSocket();
     
    HostName serverHost = new HostName("www.contoso.com");
    string serverServiceName = "https";
    
    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 
    
    // Try to connect to contoso using HTTPS (port 443)
    try {

        // Call ConnectAsync method with SSL
        await clientSocket.ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel.Ssl);

        NotifyUser("Connected");
    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }
        
        NotifyUser("Connect failed with error: " + exception.Message);
        // Could retry the connection, but for this simple example
        // just close the socket.
        
        clientSocket.Dispose();
        clientSocket = null; 
    }
           
    // Add code to send and receive data using the clientSocket
    // and then close the clientSocket
```

```cppwinrt
#include <winrt/Windows.Networking.Sockets.h>

using namespace winrt;
...
    // Define some variables, and set values.
    Windows::Networking::Sockets::StreamSocket clientSocket;

    Windows::Networking::HostName serverHost{ L"www.contoso.com" };
    winrt::hstring serverServiceName{ L"https" };

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages.

    // Try to connect to the server using HTTPS and SSL (port 443).
    try
    {
        co_await clientSocket.ConnectAsync(serverHost, serverServiceName, Windows::Networking::Sockets::SocketProtectionLevel::Tls12);
        NotifyUser(L"Connected");
    }
    catch (winrt::hresult_error const& exception)
    {
        NotifyUser(L"Connect failed with error: " + exception.message());
        clientSocket = nullptr;
    }
    // Add code to send and receive data using the clientSocket,
    // then set the clientSocket to nullptr when done to close it.
```

```cpp
using Windows::Networking;
using Windows::Networking::Sockets;

    // Define some variables and set values
    StreamSocket^ clientSocket = new ref StreamSocket();
 
    HostName^ serverHost = new ref HostName("www.contoso.com");
    String serverServiceName = "https";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to the server using HTTPS and SSL (port 443)
    task<void>(clientSocket->ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel::SSL)).then([this] (task<void> previousTask) {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();
            NotifyUser("Connected");
        }
        catch (Exception^ exception)
        {
            NotifyUser("Connect failed with error: " + exception->Message);
            
            clientSocket.Close();
            clientSocket = null;
        }
    });
    // Add code to send and receive data using the clientSocket
    // Then close the clientSocket when done
```

### <a name="use-upgradetosslasync"></a>使用 UpgradeToSslAsync
当你的代码使用 [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) 时，它将首先在不加密的情况下建立与网络服务的连接。 应用可以发送或接收某些数据，然后升级连接，以对此后所有通信使用 SSL/TLS。

[**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) 方法有两个参数。 *protectionLevel* 参数表示所需的保护级别。 *validationHostName* 参数是在升级到 SSL 时用于进行验证的远程网络目标的主机名。 通常情况下，*validationHostName* 将是应用最初建立连接时所使用的相同主机名。 如果 *protectionLevel* 参数设置为 **Windows.System.Socket.SocketProtectionLevel.Ssl**，当调用 **UpgradeToSslAsync** 时，[**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 必须通过套接字在此后的通信上使用 SSL/TLS 进行加密。 此值需要加密而且绝不允许使用 NULL 密码。

一般来说，使用 [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) 方法的顺序都是：

-   创建一个 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)。
-   如果需要在套接字上使用高级选项，请使用 [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226917) 属性获取与 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 对象相关联的 [**StreamSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226893) 实例。 针对 **StreamSocketControl** 设置一个属性。
-   如果任何数据需要以不加密的形式进行发送和接收，则立即发送。
-   调用 [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) 方法以启动将连接升级为使用 SSL/TLS 的操作。
-   实际上使用 [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) 协商得到的 SSL 强度可在异步操作成功完成后通过获取 [**StreamSocketinformation.ProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/hh967868) 属性来确定。

以下示例将会创建 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)、尝试建立与网络服务的连接、发送一些初始数据，然后协商使用 SSL/TLS。 如果协商成功，则在客户端和网络服务器之间使用 **StreamSocket** 的所有网络通信都会加密。

```csharp
using Windows.Networking;
using Windows.Networking.Sockets;
using Windows.Storage.Streams;

    // Define some variables and set values
    StreamSocket clientSocket = new StreamSocket();
 
    HostName serverHost = new HostName("www.contoso.com");
    string serverServiceName = "http";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to contoso using HTTP (port 80)
    try {
        // Call ConnectAsync method with a plain socket
        await clientSocket.ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel.PlainSocket);

        NotifyUser("Connected");

    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }

        NotifyUser("Connect failed with error: " + exception.Message, NotifyType.ErrorMessage);
        // Could retry the connection, but for this simple example
        // just close the socket.

        clientSocket.Dispose();
        clientSocket = null; 
        return;
    }

    // Now try to send some data
    DataWriter writer = new DataWriter(clientSocket.OutputStream);
    string hello = "Hello, World! ☺ ";
    Int32 len = (int) writer.MeasureString(hello); // Gets the UTF-8 string length.
    writer.WriteInt32(len);
    writer.WriteString(hello);
    NotifyUser("Client: sending hello");

    try {
        // Call StoreAsync method to store the hello message
        await writer.StoreAsync();

        NotifyUser("Client: sent data");

        writer.DetachStream(); // Detach stream, if not, DataWriter destructor will close it.
    }
    catch (Exception exception) {
        NotifyUser("Store failed with error: " + exception.Message);
        // Could retry the store, but for this simple example
            // just close the socket.

            clientSocket.Dispose();
            clientSocket = null; 
            return;
    }

    // Now upgrade the client to use SSL
    try {
        // Try to upgrade to SSL
        await clientSocket.UpgradeToSslAsync(SocketProtectionLevel.Ssl, serverHost);

        NotifyUser("Client: upgrade to SSL completed");
           
        // Add code to send and receive data 
        // The close clientSocket when done
    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }

        NotifyUser("Upgrade to SSL failed with error: " + exception.Message);

        clientSocket.Dispose();
        clientSocket = null; 
        return;
    }
```

```cppwinrt
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>

using namespace winrt;
using namespace Windows::Storage::Streams;
...
    // Define some variables, and set values.
    Windows::Networking::Sockets::StreamSocket clientSocket;

    Windows::Networking::HostName serverHost{ L"www.contoso.com" };
    winrt::hstring serverServiceName{ L"https" };

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages. 

    // Try to connect to the server using HTTP (port 80).
    try
    {
        co_await clientSocket.ConnectAsync(serverHost, serverServiceName, Windows::Networking::Sockets::SocketProtectionLevel::PlainSocket);
        NotifyUser(L"Connected");
    }
    catch (winrt::hresult_error const& exception)
    {
        NotifyUser(L"Connect failed with error: " + exception.message());
        clientSocket = nullptr;
    }

    // Now, try to send some data.
    DataWriter writer{ clientSocket.OutputStream() };
    winrt::hstring hello{ L"Hello, World! ☺ " };
    uint32_t len{ writer.MeasureString(hello) }; // Gets the size of the string, in bytes.
    writer.WriteInt32(len);
    writer.WriteString(hello);
    NotifyUser(L"Client: sending hello");

    try
    {
        co_await writer.StoreAsync();
        NotifyUser(L"Client: sent hello");

        writer.DetachStream(); // Detach the stream when you want to continue using it; otherwise, the DataWriter destructor closes it.
    }
    catch (winrt::hresult_error const& exception)
    {
        NotifyUser(L"Store failed with error: " + exception.message());
        // We could retry the store operation. But, for this simple example, just close the socket by setting it to nullptr.
        clientSocket = nullptr;
        co_return;
    }

    // Now, upgrade the client to use SSL.
    try
    {
        co_await clientSocket.UpgradeToSslAsync(Windows::Networking::Sockets::SocketProtectionLevel::Tls12, serverHost);
        NotifyUser(L"Client: upgrade to SSL completed");

        // Add code to send and receive data using the clientSocket,
        // then set the clientSocket to nullptr when done to close it.
    }
    catch (winrt::hresult_error const& exception)
    {
        // If this is an unknown status, then the error is fatal and retry will likely fail.
        Windows::Networking::Sockets::SocketErrorStatus socketErrorStatus{ Windows::Networking::Sockets::SocketError::GetStatus(exception.to_abi()) };
        if (socketErrorStatus == Windows::Networking::Sockets::SocketErrorStatus::Unknown)
        {
            throw;
        }

        NotifyUser(L"Upgrade to SSL failed with error: " + exception.message());
        // We could retry the store operation. But for this simple example, just close the socket by setting it to nullptr.
        clientSocket = nullptr;
        co_return;
    }
```

```cpp
using Windows::Networking;
using Windows::Networking::Sockets;
using Windows::Storage::Streams;

    // Define some variables and set values
    StreamSocket^ clientSocket = new ref StreamSocket();
 
    Hostname^ serverHost = new ref HostName("www.contoso.com");
    String serverServiceName = "http";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to contoso using HTTP (port 80)
    task<void>(clientSocket->ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel::PlainSocket)).then([this] (task<void> previousTask) {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();
            NotifyUser("Connected");
        }
        catch (Exception^ exception)
        {
            NotifyUser("Connect failed with error: " + exception->Message);
 
            clientSocket->Close();
            clientSocket = null;
        }
    });
       
    // Now try to send some data
    DataWriter^ writer = new ref DataWriter(clientSocket.OutputStream);
    String hello = "Hello, World! ☺ ";
    Int32 len = (int) writer->MeasureString(hello); // Gets the UTF-8 string length.
    writer->writeInt32(len);
    writer->writeString(hello);
    NotifyUser("Client: sending hello");

    task<void>(writer->StoreAsync()).then([this] (task<void> previousTask) {
        try {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();

            NotifyUser("Client: sent hello");

            writer->DetachStream(); // Detach stream, if not, DataWriter destructor will close it.
       }
       catch (Exception^ exception) {
               NotifyUser("Store failed with error: " + exception->Message);
               // Could retry the store, but for this simple example
               // just close the socket.
 
               clientSocket->Close();
               clientSocket = null;
               return
       }
    });

    // Now upgrade the client to use SSL
    task<void>(clientSocket->UpgradeToSslAsync(clientSocket.SocketProtectionLevel.Ssl, serverHost)).then([this] (task<void> previousTask) {
        try {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();

           NotifyUser("Client: upgrade to SSL completed");
           
           // Add code to send and receive data 
           // Then close clientSocket when done
        }
        catch (Exception^ exception) {
            // If this is an unknown status it means that the error is fatal and retry will likely fail.
            if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
                throw;
            }

            NotifyUser("Upgrade to SSL failed with error: " + exception.Message);

            clientSocket->Close();
            clientSocket = null; 
            return;
        }
    });
```

### <a name="creating-secure-websocket-connections"></a>创建安全 WebSocket 连接
与传统套接字连接相同，为 UWP 应用使用 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 和 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 功能时，使用传输层安全性 (TLS)/安全套接字层 (SSL) 也可以加密 WebSocket 连接。 一般来说，你希望使用安全的 WebSocket 连接。 这会提高连接成功的可能性，因为许多代理会拒绝未加密的 WebSocket 连接。

有关如何创建或升级到网络服务的安全套接字连接的示例，请参阅[如何借助 TLS/SSL 确保 WebSocket 连接的安全](https://msdn.microsoft.com/library/windows/apps/xaml/hh994399)。

除了 TLS/SSL 加密之外，服务器可能需要 **Sec-WebSocket-Protocol** 标头值才能完成初始握手。 该值由 [**StreamWebSocketInformation.Protocol**](https://msdn.microsoft.com/library/windows/apps/hh701514) 和 [**MessageWebSocketInformation.Protocol**](https://msdn.microsoft.com/library/windows/apps/hh701358) 属性表示，用于指示连接的协议版本，并让服务器可以正确解释开始握手和随后交换的数据。 如果服务器在任何时刻无法以可关闭连接的安全方式解释传入数据，则使用此协议信息。

如果客户端发出的初始请求不包含此值，或提供了与服务器的预期不相符的值，则预期值会在发生 WebSocket 握手错误时 从服务器发送到客户端。

## <a name="authentication"></a>身份验证
如何在通过网络进行连接时提供身份验证凭据。

### <a name="providing-a-client-certificate-with-the-streamsocket-class"></a>通过 StreamSocket 类提供客户端证书
[**Windows.Networking.StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 类支持使用 SSL/TLS 应用来验证应用正在与其交互的服务器。 在某些情况下，应用还需要使用 TLS 客户端证书对服务器进行自身验证。 在 windows 10，你可以在[**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226893)设置的对象 （这必须在启动 TLS 握手之前） 上提供客户端证书。 如果服务器请求客户端证书，Windows 将通过提供的证书做出响应。

下面是演示如何实现此目的的代码段：

```csharp
var socket = new StreamSocket();
Windows.Security.Cryptography.Certificates.Certificate certificate = await GetClientCert();
socket.Control.ClientCertificate = certificate;
await socket.ConnectAsync(destination, SocketProtectionLevel.Tls12);
```

### <a name="providing-authentication-credentials-to-a-web-service"></a>向 Web 服务提供身份验证凭据
使应用可与安全 Web 服务交互的每个网络 API 均提供其自有方法来初始化客户端，或者使用服务器和代理身份验证凭据来设置请求头。 每个方法均使用 [**PasswordCredential**](https://msdn.microsoft.com/library/windows/apps/br227061) 对象进行设置，该对象指示用户名、密码和将这些凭据用于的资源。 下表提供这些 API 的映射：

| **WebSockets** | [**MessageWebSocketControl.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br226848) |
|-------------------------|----------------------------------------------------------------------------------------------------------|
|  | [**MessageWebSocketControl.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br226847) |
|  | [**StreamWebSocketControl.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br226928) |
|  | [**StreamWebSocketControl.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br226927) |
| **Background Transfer** | [**BackgroundDownloader.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/hh701076) |
|  | [**BackgroundDownloader.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/hh701068) |
|  | [**BackgroundUploader.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/hh701184) |
|  | [**BackgroundUploader.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/hh701178) |
| **Syndication** | [**SyndicationClient(PasswordCredential)**](https://msdn.microsoft.com/library/windows/apps/hh702355) |
|  | [**SyndicationClient.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br243461) |
|  | [**SyndicationClient.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br243459) |
| **AtomPub** | [**AtomPubClient(PasswordCredential)**](https://msdn.microsoft.com/library/windows/apps/hh702262) |
|  | [**AtomPubClient.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br243428) |
|  | [**AtomPubClient.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br243423) |

## <a name="handling-network-exceptions"></a>处理网络异常
在大多数编程领域中，异常均表示由程序中的某些缺陷而导致的重大问题或失败。 在网络编程中，还存在额外的来源会导致异常，即网络本身，这是网络通信的特性。 网络通信本身是不可靠的，且易出现出乎意料的失败。 对于你的应用使用网络的每一种方式，你都必须保留一些状态信息，而且你的应用代码必须通过更新该状态信息和启动适合应用的逻辑来处理网络异常，从而重新建立或重试失败的通信。

当通用 Windows 应用引发异常时，异常处理程序可以检索关于异常原因的更详细的信息，以更好地了解此次失败，并作出正确的决策。

每个语言投影都支持一种访问该详细信息的方法。 异常在通用 Windows 应用中投影为 **HRESULT** 值。 *Winerror.h* include 文件包含一个可能的 **HRESULT** 值的大型列表，其中包含网络错误。

网络 API 支持不同方法来检索关于异常原因的详细信息。

-   一些 API 提供了一种帮助程序方法，可将异常中的 **HRESULT** 值转换为枚举值。
-   而另一些 API 提供一种方法来检索实际的 **HRESULT** 值。

## <a name="related-topics"></a>相关主题
* [Windows 10 中的网络 API 改进](http://blogs.windows.com/buildingapps/2015/07/02/networking-api-improvements-in-windows-10/)

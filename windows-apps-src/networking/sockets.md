---
author: DelfCo
description: "作为一名通用 Windows 平台 (UWP) 应用开发人员，你可以将 Windows.Networking.Sockets 与 Winsock 结合使用，以与其他设备通信。"
title: "套接字"
ms.assetid: 23B10A3C-E33F-4CD6-92CB-0FFB491472D6
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.openlocfilehash: 8933fb5c970203746fe1a00c71c0630fa264ebf6
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="sockets"></a>套接字

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**重要的 API**

-   [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)
-   [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673)

作为一名通用 Windows 平台 (UWP) 应用开发人员，你可以将 [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 与 [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms737523) 结合使用，以与其他设备通信。 关于使用 **Windows.Networking.Sockets** 命名空间执行网络操作的详细指南，请参阅本主题。

>**注意** 作为[网络隔离](https://msdn.microsoft.com/library/windows/apps/hh770532.aspx)的一部分，系统禁止在通过本地环回地址 (127.0.0.0) 或明确指定本地 IP 地址运行于同一台计算机的两个 UWP 应用之间建立套接字连接（Sockets 或 WinSock）。 这意味着你无法使用套接字在两个 UWP 应用之间进行通信。 UWP 提供了其他可用于应用间通信的机制。 请参阅[应用到应用的通信](https://msdn.microsoft.com/windows/uwp/app-to-app/index)以了解详细信息。

## <a name="basic-tcp-socket-operations"></a>基本的 TCP 套接字操作

TCP 套接字对于长时间的连接提供双向低级网络数据传输。 TCP 套接字是由大部分网络协议在 Internet 上使用的基础功能。 本部分显示了如何借助将 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 和 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 类用作 [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 命名空间一部分的 TCP 流套接字使 UWP 应用可以发送和接收数据。 在本部分中，我们将创建一个非常简单的应用，该应用将用作回显服务器和客户端来演示基本的 TCP 操作。

**创建 TCP 回显服务器**

下面的代码示例演示了如何创建 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 对象并开始侦听传入的 TCP 连接。

```csharp
try
{
    //Create a StreamSocketListener to start listening for TCP connections.
    Windows.Networking.Sockets.StreamSocketListener socketListener = new Windows.Networking.Sockets.StreamSocketListener();

    //Hook up an event handler to call when connections are received.
    socketListener.ConnectionReceived += SocketListener_ConnectionReceived;

    //Start listening for incoming TCP connections on the specified port. You can specify any port that' s not currently in use.
    await socketListener.BindServiceNameAsync("1337");
}
catch (Exception e)
{
    //Handle exception.
}
```

下面的代码示例实现了在上面的示例中附加到 [**StreamSocketListener.ConnectionReceived**](https://msdn.microsoft.com/library/windows/apps/hh701494) 事件的 SocketListener\_ConnectionReceived 事件处理程序。 每当远程客户端与回显服务器建立连接时，[**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 类都会调用此事件处理程序。

```csharp
private async void SocketListener_ConnectionReceived(Windows.Networking.Sockets.StreamSocketListener sender, 
    Windows.Networking.Sockets.StreamSocketListenerConnectionReceivedEventArgs args)
{
    //Read line from the remote client.
    Stream inStream = args.Socket.InputStream.AsStreamForRead();
    StreamReader reader = new StreamReader(inStream);
    string request = await reader.ReadLineAsync();
    
    //Send the line back to the remote client.
    Stream outStream = args.Socket.OutputStream.AsStreamForWrite();
    StreamWriter writer = new StreamWriter(outStream);
    await writer.WriteLineAsync(request);
    await writer.FlushAsync();
}
```

**创建 TCP 回显客户端**

下面的代码示例演示了如何创建 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 对象、与远程服务器建立连接、发送请求以及接收响应。

```csharp
try
{
    //Create the StreamSocket and establish a connection to the echo server.
    Windows.Networking.Sockets.StreamSocket socket = new Windows.Networking.Sockets.StreamSocket();
    
    //The server hostname that we will be establishing a connection to. We will be running the server and client locally,
    //so we will use localhost as the hostname.
    Windows.Networking.HostName serverHost = new Windows.Networking.HostName("localhost");
    
    //Every protocol typically has a standard port number. For example HTTP is typically 80, FTP is 20 and 21, etc.
    //For the echo server/client application we will use a random port 1337.
    string serverPort = "1337";
    await socket.ConnectAsync(serverHost, serverPort);

    //Write data to the echo server.
    Stream streamOut = socket.OutputStream.AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    string request = "test";
    await writer.WriteLineAsync(request);
    await writer.FlushAsync();

    //Read data from the echo server.
    Stream streamIn = socket.InputStream.AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string response = await reader.ReadLineAsync();
}
catch (Exception e)
{
    //Handle exception here.            
}
```

## <a name="basic-udp-socket-operations"></a>基本的 UDP 套接字操作

UDP 套接字为不需要已建立连接的网络通信提供了双向低级网络数据传输。 因为 UDP 套接字不会在两个端点上保持连接，所以它们会为远程计算机之间的网络提供快速而简单的解决方案。 然而，UDP 套接字根本不能确保网络数据包的完整性，也不能确保它们是否能使其实现远程目标。 使用 UDP 的应用程序的一些示例包括本地网络发现和本地聊天客户端。 本部分演示了通过创建简单的回显服务器和客户端将 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 类用于发送和接收 UDP 消息。

**创建 UDP 回显服务器**

下面的代码示例演示了如何创建 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 对象并将其绑定到特定端口，以便你可以帧听传入的 UDP 消息。

```csharp
Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();

socket.MessageReceived += Socket_MessageReceived;

//You can use any port that is not currently in use already on the machine.
string serverPort = "1337";

//Bind the socket to the serverPort so that we can start listening for UDP messages from the UDP echo client.
await socket.BindServiceNameAsync(serverPort);
```

下面的代码示例实现了 **Socket\_MessageReceived** 事件处理程序，以读取从客户端接收的消息并将同样的消息发送回去。

```csharp
private async void Socket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    //Read the message that was received from the UDP echo client.
    Stream streamIn = args.GetDataStream().AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string message = await reader.ReadLineAsync();

    //Create a new socket to send the same message back to the UDP echo client.
    Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();
    
    //Use a separate port number for the UDP echo client because both will be unning on the same machine.
    string clientPort = "1338"
    Stream streamOut = (await socket.GetOutputStreamAsync(args.RemoteAddress, clientPort)).AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    await writer.WriteLineAsync(message);
    await writer.FlushAsync();
}
```

**创建 UDP 回显客户端**

下面的代码示例演示了如何创建 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 对象并将其绑定到特定端口，以便你可以侦听传入的 UDP 消息并向 UDP 回显服务器发送 UDP 消息。

```csharp
private async void testUdpSocketServer()
{
    Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();
    
    socket.MessageReceived += Socket_MessageReceived;
    
    //You can use any port that is not currently in use already on the machine. We will be using two separate and random 
    //ports for the client and server because both the will be running on the same machine.
    string serverPort = "1337";
    string clientPort = "1338";
    
    //Because we will be running the client and server on the same machine, we will use localhost as the hostname.
    Windows.Networking.HostName serverHost = new Windows.Networking.HostName("localhost");
    
    //Bind the socket to the clientPort so that we can start listening for UDP messages from the UDP echo server.
    await socket.BindServiceNameAsync(clientPort);
                
    //Write a message to the UDP echo server.
    Stream streamOut = (await socket.GetOutputStreamAsync(serverHost, serverPort)).AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    string message = "Hello, world!";
    await writer.WriteLineAsync(message);
    await writer.FlushAsync();
}
```

下面的代码示例实现了 **Socket\_MessageReceived** 事件处理程序以读取从 UDP 回显服务器接收的消息。

```csharp
private async void Socket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, 
    Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    //Read the message that was received from the UDP echo server.
    Stream streamIn = args.GetDataStream().AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string message = await reader.ReadLineAsync();
}
```

## <a name="background-operations-and-the-socket-broker"></a>后台操作和套接字代理

如果你的应用根据套接字来接收连接或数据，则你必须为应用不在前台运行时正确执行这些操作做好准备。 为此，请使用套接字代理。 有关如何使用套接字代理的详细信息，请参阅[后台网络通信](network-communications-in-the-background.md)。

## <a name="batched-sends"></a>批量发送

从 Windows 10 开始，Windows.Networking.Sockets 支持批量发送，这为你一次性发送多个数据缓冲区提供了一个途径，此时你所使用的上下文切换开销要比你分开发送每个缓冲区低很多。 如果你的应用正在执行 VoIP、VPN 或涉及需最大效率地移动大量数据的其他任务，此做法将特别有用。

对套接字上的 WriteAsync 的每次调用都会引发一次内核转换，以到达网络堆栈。 当应用一次写入多个缓冲区时，每次写入都会引发单独的内核转换，从而导致大量的开销。 新的批量发送模式优化了内核转换的频率。 此功能当前仅适用于 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 和已连接的 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 实例。

下面是应用如何以非最佳方式发送大量缓冲区的一个示例。

```csharp
// Send a set of packets inefficiently, one packet at a time.
// This is not recommended if you have many packets to send
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = stream.OutputStream;

foreach (IBuffer packet in packetsToSend)
{
    // Incurs kernel transition overhead for each packet
    await outputStream.WriteAsync(packet);
}
```

本示例演示了发送大量缓冲区的一种更有效的方式。 由于此技术使用了 C# 语言特有的功能，因此该技术仅面向 C# 程序员。 本示例中通过一次发送多个数据包，从而支持系统分批发送以优化内核转换，进而可提高性能。

```csharp
// More efficient way to send packets.
// This way enables the system to do batched sends
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = stream.OutputStream;

int i = 0;
Task[] pendingTasks = new Tast[packetsToSend.Count];
foreach (IBuffer packet in packetsToSend)
{
    // track all pending writes as tasks, but do not wait on one before peforming the next
    pendingTasks[i++] = outputStream.WriteAsync(packet).AsTask();
    // Do not modify any buffer' s contents until the pending writes are complete.
}
// Now, wait for all of the pending writes to complete
await Task.WaitAll(pendingTasks);
```

本示例演示了采用与批量发送兼容的方式发送大量缓冲区的另一种方法。 此外，由于它未使用任何特定于 C# 的功能，使得它适用于所有语言（尽管此处使用 C# 进行演示）。 它应改用 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 和 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 类（这是 Windows 10 中的新增功能）的 **OutputStream** 成员中已更改的行为。

```csharp
// More efficient way to send packets in Windows 10, using the new behavior of OutputStream.FlushAsync().
int i = 0;
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = socket.OutputStream;

var pendingWrites = new IAsyncOperationWithProgress<uint,uint> [packetsToSend.Count];

foreach (IBuffer packet in packetsToSend)
{
    pendingWrites[i++] = outputStream.WriteAsync(packet);
    // Do not modify any buffer' s contents until the pending writes are complete.
}

// Wait for all pending writes to complete. This step enables batched sends on the output stream.
await outputStream.FlushAsync();
```

在早期版本的 Windows 中，**FlushAsync** 会立即返回，并且不保证流上的所有操作均已完成。 在 Window10 中，此行为已发生更改。 **FlushAsync** 现保证先完成输出流上的所有操作，然后再返回。

在代码中使用批量写入存在一些很大的局限性。

-   在异步写入尚未完成之前，你将无法修改当前写入的 **IBuffer** 实例的内容。
-   **FlushAsync** 模式仅适用于 **StreamSocket.OutputStream** 和 **DatagramSocket.OutputStream**。
-   **FlushAsync** 模式适仅用于 Windows 10 以及更高版本。
-   在其他情况下，用 **Task.WaitAll** 来代替 **FlushAsync** 模式。

## <a name="port-sharing-for-datagramsocket"></a>DatagramSocket 的端口共享

Windows 10 引入了一个新 [**DatagramSocketControl**](https://msdn.microsoft.com/library/windows/apps/hh701190) 属性 [**MulticastOnly**](https://msdn.microsoft.com/library/windows/apps/dn895368)，以便你可以指定相关 **DatagramSocket** 能与绑定到同一地址/端口的其他 Win32 或 WinRT 多播套接字共存。

## <a name="providing-a-client-certificate-with-the-streamsocket-class"></a>通过 StreamSocket 类提供客户端证书

[**Windows.Networking.StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 类支持使用 SSL/TLS 应用来验证应用正在与其交互的服务器。 在某些情况下，应用还需要使用 TLS 客户端证书对服务器进行自身验证。 在 Windows 10 中，你可以在 [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226893) 对象上提供客户端证书（这必须在启动 TLS 握手之前进行设置）。 如果服务器请求客户端证书，Windows 将通过提供的证书做出响应。

下面是演示如何实现此目的的代码段：

```csharp
var socket = new StreamSocket();
Windows.Security.Cryptography.Certificates.Certificate certificate = await GetClientCert();
socket.Control.ClientCertificate = certificate;
await socket.ConnectAsync(destination, SocketProtectionLevel.Tls12);
```

## <a name="exceptions-in-windowsnetworkingsockets"></a>Windows.Networking.Sockets 中的异常

如果传递的字符串不是有效的主机名（包含主机名中不允许的字符），则与套接字一起使用的 [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113) 类的构造函数会引发异常。 如果应用获取 **HostName** 用户输入，则构造函数应位于 try/catch 块中。 如果引发了异常，该应用可以通知用户并请求新的主机名。

[**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 命名空间具有方便的帮助程序方法和枚举，以便在使用套接字和 WebSocket 时处理错误。 这有助于在应用中分别处理特定网络异常。

在进行 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)、[**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 或 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 操作时发生的错误将以 **HRESULT** 值的形式返回。 [**SocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701462) 方法用于将来自套接字操作的网络错误转化为 [**SocketErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh701457) 枚举值。 大部分 **SocketErrorStatus** 枚举值对应由本机 Windows 套接字操作返回的错误。 应用可以筛选特定 **SocketErrorStatus** 枚举值来基于异常原因修改应用行为。

在进行 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 或 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 操作时发生的错误将以 **HRESULT** 值的形式返回。 [**WebSocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701529) 方法用于将来自 WebSocket 操作的网络错误转化为 [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818) 枚举值。 大部分 **WebErrorStatus** 枚举值对应由本机 HTTP 客户端操作返回的错误。 应用可以筛选特定 **WebErrorStatus** 枚举值来基于异常原因修改应用行为。

对于参数验证错误，应用还可以使用来自异常的 **HRESULT** 来了解关于导致该异常的错误的详细信息。 可能的 **HRESULT** 值将在 *Winerror.h* 头文件中列出。 对于大部分参数验证错误，返回的 **HRESULT** 为 **E\_INVALIDARG**。

## <a name="the-winsock-api"></a>Winsock API

也可以在 UWP 应用中使用 [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673)。 支持的 Winsock API 基于 Windows Phone 8.1 的成员 Microsoft Silverlight 8.1，并继续支持大部分的类型、属性和方法（一些被视为过时的 API 已被删除）。 [此处](https://msdn.microsoft.com/library/windows/desktop/ms740673)提供了有关 Winsock 编程的详细信息。



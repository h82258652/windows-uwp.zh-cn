---
author: DelfCo
description: WebSocket 提供了一种机制，用于使用 HTTP 通过 Web 在客户端与服务器之间进行既快捷又安全的双向通信。
title: WebSockets
ms.assetid: EAA9CB3E-6A3A-4C13-9636-CCD3DE46E7E2
---

# WebSockets

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**重要的 API**

-   [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)
-   [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)

WebSocket 提供了一种机制，用于使用 HTTP 通过 Web 在客户端与服务器之间进行既快捷又安全的双向通信。

在 [WebSocket Protocol](http://tools.ietf.org/html/rfc6455) 下，数据通过全双工单套接字连接立即传输，从而允许从两个终结点实时发送和接收消息。 WebSocket 非常适合在实时游戏中使用，由于即时社交网络通知和显示的最新信息（游戏统计信息）都需要确保安全，并且需使用快速的数据传输。 通用 Windows 平台 (UWP) 开发人员可以使用 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 和 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 类，将支持 Websocket 协议的服务器连接在一起。

| MessageWebSocket                                                         | StreamWebSocket                                                                               |
|--------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| 适用于消息不是非常大的典型方案。   | 适用于将传输大文件（如照片或视频）的方案。 |
| 启用已接收整个 WebSocket 消息的通知。 | 允许由每个读取操作读取消息的各部分。                             |
| 支持 UTF-8 和二进制消息。                                 | 仅支持二进制消息。                                                                |
| 类似于 UDP 或数据报套接字。                                     | 类似于 TCP 或流套接字。                                                            |

在大多数情况下，你会希望使用安全的 WebSocket 连接，来加密已发送和接收的数据。 这还将提高连接成功的几率，因为许多代理会拒绝未加密的 WebSocket 连接。 WebSocket 协议定义了以下两种 URI 方案。

-   ws: - 用于未加密的连接。
-   wss: - 用于应该加密的安全连接。

若要加密 WebSocket 连接，请使用 wss: URI 方案，例如 `wss://www.contoso.com/mywebservice`。

## 使用 MessageWebSocket

[
            **MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 允许由每个读取操作读取消息的各部分。 **MessageWebSocket** 通常在消息不是非常大的应用场景中使用。 UTF-8 和二进制文件均受支持。

此部分中的代码将创建一个新 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)，连接到 WebSocket 服务器，然后将数据发送到服务器。 在成功建立连接后，应用将等待触发 [**MessageWebSocket.MessageReceived**](https://msdn.microsoft.com/library/windows/apps/br241358) 事件，从而指示已接收数据。

本示例使用了 WebSocket.org 回显服务器，该服务器仅将服务回显到向其发送任一字符串的发送方。 本示例借助“wss:”协议说明符，使用安全连接来发送和接收消息。

> [!div class="tabbedCodeSnippets"]
> ```cpp
> void Game::InitWebSockets()
> {
>     // Create a new web socket
>     m_messageWebSocket = ref new MessageWebSocket();
> 
>     // Set the message type to UTF-8
>     m_messageWebSocket->Control->MessageType = Windows::Networking::Sockets::SocketMessageType::Utf8;
> 
>     // Register callbacks for notifications of interest
>     m_messageWebSocket->MessageReceived += 
>        ref new TypedEventHandler<MessageWebSocket^, MessageWebSocketMessageReceivedEventArgs^>(this, &Game::WebSocketMessageReceived);
>     m_messageWebSocket->Closed += ref new TypedEventHandler<IWebSocket^, WebSocketClosedEventArgs^>(this, &Game::WebSocketClosed);
> 
>     // This test code uses the websocket.org echo service to illustrate sending a string and receiving the echoed string back
>     // Note that wss: makes this an encrypted connection.
>     m_serverUri = ref new Uri("wss://echo.websocket.org");
> 
>     // Establish the connection, and set m_socketConnected on success
>     create_task(m_messageWebSocket->ConnectAsync(m_serverUri)).then([this] (task<void> previousTask)
>     {
>         try
>         {
>             // Try getting all exceptions from the continuation chain above this point.
>             previousTask.get();
> 
>             // websocket connected. update state variable
>             m_socketConnected = true;
>             OutputDebugString(L"Successfully initialized websockets\n");
>         }
>         catch (Platform::COMException^ exception)
>         {
>             // Add code here to handle any exceptions
>             // HandleException(exception);
> 
>         }
>     });
> }
> ```
> ```cs
> MessageWebSocket webSock = new MessageWebSocket();
> 
> //In this case we will be sending/receiving a string so we need to set the MessageType to Utf8.
> webSock.Control.MessageType = SocketMessageType.Utf8;
> 
> //Add the MessageReceived event handler.
> webSock.MessageReceived += WebSock_MessageReceived;
> 
> //Add the Closed event handler.
> webSock.Closed += WebSock_Closed;
> 
> Uri serverUri = new Uri("wss://echo.websocket.org");
> 
> try
> {
>     //Connect to the server.
>     await webSock.ConnectAsync(serverUri);
> 
>     //Send a message to the server.
>     await WebSock_SendMessage(webSock, "Hello, world!");
> }
> catch (Exception ex)
> {
>     //Add code here to handle any exceptions
> }
> ```

在初始化 WebSocket 连接后，你的代码必须执行以下活动才能正常发送和接收数据。

### 为 MessageWebSocket.MessageReceived 事件实现回调

在使用 WebSocket 建立连接和发送数据前，你的应用需要先注册事件回调，然后才能在接收数据后收到通知。 当发生 [**MessageWebSocket.MessageReceived**](https://msdn.microsoft.com/library/windows/apps/br241358) 事件时， 会调用所注册的回调并接收来自 [**MessageWebSocketMessageReceivedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br226852) 的数据。 此示例编写时假定当前发送的消息采用的是 UTF-8 格式。

以下示例函数将从已连接的 WebSocket 服务器接收字符串，并将该字符串打印到调试程序输出窗口。

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::WebSocketMessageReceived(MessageWebSocket^ sender, MessageWebSocketMessageReceivedEventArgs^ args)
>{
>    DataReader^ messageReader = args->GetDataReader();
>    messageReader->UnicodeEncoding = Windows::Storage::Streams::UnicodeEncoding::Utf8;
>
>    String^ readString = messageReader->ReadString(messageReader->UnconsumedBufferLength);
>    // Data has been read and is now available from the readString variable.
>    swprintf(m_debugBuffer, 511, L"WebSocket Message received: %s\n", readString->Data());
>    OutputDebugString(m_debugBuffer);
>}
>```
>```csharp
>//The MessageReceived event handler.
>private void WebSock_MessageReceived(MessageWebSocket sender, MessageWebSocketMessageReceivedEventArgs args)
>{
>    DataReader messageReader = args.GetDataReader();
>    messageReader.UnicodeEncoding = UnicodeEncoding.Utf8;
>    string messageString = messageReader.ReadString(messageReader.UnconsumedBufferLength);
>
>    //Add code here to do something with the string that is received.
>}
>```

###  为 MessageWebSocket.Closed 事件实现回调

在使用 WebSocket 建立连接和发送数据前，你的应用需要先注册事件回调，然后才能在 WebSocket 被 WebSocket 服务器关闭时收到 通知。 当发生 [**MessageWebSocket.Closed**](https://msdn.microsoft.com/library/windows/apps/hh701364) 事件时，将调用已注册的回调，以指示连接已被 WebSocket 服务器关闭。

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::WebSocketClosed(IWebSocket^ sender, WebSocketClosedEventArgs^ args)
>{
>    // The method may be triggered remotely by the server sending unsolicited close frame or locally by Close()/delete operator.
>    // This method assumes we saved the connected WebSocket to a variable called m_messageWebSocket
>    if (m_messageWebSocket != nullptr)
>    {
>        delete m_messageWebSocket;
>        m_messageWebSocket = nullptr;
>        OutputDebugString(L"Socket was closed\n");
>    }
>    m_socketConnected = false;
> }
>```
>```csharp
>//The Closed event handler
>private void WebSock_Closed(IWebSocket sender, WebSocketClosedEventArgs args)
>{
>    //Add code here to do something when the connection is closed locally or by the server
>}
>```

###  通过 WebSocket 发送消息

在建立连接后，WebSocket 客户端可以将数据发送到该服务器。 [
            **DataWriter.StoreAsync**](https://msdn.microsoft.com/library/windows/apps/br208171) 方法将返回 一个映射到无符号整数的参数。 与建立连接的任务相比，这将更改我们定义发送消息的任务的方式。

**注意** 当你使用 MessageWebSocket 的 OutputStream 创建新的 DataWriter 对象时，DataWriter 将获取 OutputStream 的所有权，并且将在 DataWriter 超出范围时收回 Outputstream。 这会导致使用 OutputStream 的任何后续尝试失败，并显示 HRESULT 值 0x80000013。 若要避免收回 OutputStream，则可通过此代码调用 DataWriter 的 DetachStream 方法， 从而返回 WebSocket 对象的流的所有权。

以下函数将给定的字符串发送到连接的 WebSocket，并在调试程序输出窗口中打印验证消息。

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::SendWebSocketMessage(Windows::Networking::Sockets::MessageWebSocket^ sendingSocket, Platform::String^ message)
>{
>    if (m_socketConnected)
>    {
>        // WebSocket is connected, so send a message
>        m_messageWriter = ref new DataWriter(sendingSocket->OutputStream);
>
>        m_messageWriter->WriteString(message);
>
>        // Send the data as one complete message
>        create_task(m_messageWriter->StoreAsync()).then([this] (unsigned int)
>        {
>            // Send Completed
>            m_messageWriter->DetachStream();    // give the stream back to m_messageWebSocket
>            OutputDebugString(L"Sent websocket message\n");
>        })
>            .then([this] (task<void>> previousTask)
>        {
>            try
>            {
>                // Try getting all exceptions from the continuation chain above this point.
>                previousTask.get();
>            }
>            catch (Platform::COMException ^ex)
>            {
>                // Add code to handle the exception
>                // HandleException(exception);
>            }
>        });
>    }
>}
>```
>```csharp
>//Send a message to the server.
>private async Task WebSock_SendMessage(MessageWebSocket webSock, string message)
>{
>    DataWriter messageWriter = new DataWriter(webSock.OutputStream);
>    messageWriter.WriteString(message);
>    await messageWriter.StoreAsync();
>}
>```

## 将高级控件与 WebSocket 结合使用

[
            **MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 和 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 类在使用高级控件时均遵循相同的模型。 与上述主类对应的是访问高级控件的相关类。

[
            **MessageWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226843) 提供 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 对象上的 套接字控件数据。
[
            **StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924) 提供 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 对象上的 套接字控件数据。
对于这两类 WebSocket 而言，使用的高级控件的基本模式均相同。 下面的讨论将使用 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 作为示例， 不过相同的过程也适用于 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)。

1.  创建 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 对象。
2.  使用 [**StreamWebSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226934) 属性检索与 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 对象相关联的 [**StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924) 实例。
3.  通过获取或设置 [**StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924) 实例的属性，来获取或设置特定的高级控件。

为了能设置高级控件，[**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 和 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 将对此实施一些要求。

-   对于 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 上的所有高级控件，应用都必须始终在发出连接操作之前设置属性。 基于此要求，最佳做法是在创建 **StreamWebSocket** 对象后立即设置所有控件属性。 请勿在 [**StreamWebSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/br226933) 方法调用后尝试设置控件属性。
-   对于 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 上的所有高级控件（消息类型除外），必须在发出连接操作之前设置属性。 最佳做法是在创建 **MessageWebSocket** 对象后立即设置所有控件属性。 请勿在调用 [**MessageWebSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/br226859) 后尝试更改控件属性，消息类型除外。

## WebSocket 信息类

[
            **MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 和 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 都具有相应的类，用于提供有关 WebSocket 实例的其他信息。

[
            **MessageWebSocketInformation**](https://msdn.microsoft.com/library/windows/apps/br226849) 提供有关 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 的信息，以便于使用 [**MessageWebSocket.Information**](https://msdn.microsoft.com/library/windows/apps/br226861) 属性检索该信息类的实例。

[
            **StreamWebSocketInformation**](https://msdn.microsoft.com/library/windows/apps/br226929) 提供有关 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 的信息，以便于使用 [**StreamWebSocket.Information**](https://msdn.microsoft.com/library/windows/apps/br226935) 属性检索该信息类的实例。

请注意，这两个信息类上的所有属性均为只读形式，并且你能够在 Web 套接字对象的生存期内随时检索当前信息。

## 处理网络异常

在进行 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 或 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 操作时发生的错误将以 **HRESULT** 值的形式返回。 [
            **WebSocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701529) 方法用于将来自 WebSocket 操作的网络错误转化为 [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818) 枚举值。 大部分 **WebErrorStatus** 枚举值对应由本机 HTTP 客户端操作返回的错误。 应用可以筛选特定 **WebErrorStatus** 枚举值来基于异常原因修改应用行为。

对于参数验证错误，应用还可以使用来自异常的 **HRESULT** 来了解关于导致该异常的错误详细信息。 可能的 **HRESULT** 值将在 *Winerror.h* 头文件中列出。 对于大多数参数验证错误，返回的 **HRESULT** 为 **E\_INVALIDARG**。

## 对 WebSocket 操作设置超时

MessageWebSocket 和 StreamWebSocket 类使用内部系统服务，发送 WebSocket 客户端请求并从服务器接收响应。 WebSocket 连接操作使用的 默认超时值为 60 秒。 如果支持 WebSocket 的 HTTP 服务器临时关闭 或因网络中断而被阻止，且服务器不对或无法对 WebSocket 连接请求做出响应，则内部系统 服务将先等待默认 60 秒，然后返回一个错误，这将导致在 WebSocket ConnectAsync 方法上引发异常。 如果对 URI 中 HTTP 服务器名称进行名称查询时返回该名称的多个 IP 地址，则 内部系统将为该站点尝试最多 5 个 IP 地址，并且在每次尝试失败前都默认超时 60 秒。 在返回 错误和引发异常之前，发出 WebSock 连接请求的应用可能为尝试连接到多个 IP 地址而等待几分钟。 此行为可能会使用户以为应用已停止工作。 建立 WebSocket 连接后，用于发送和接收操作的 默认超时为 30 秒。

若要让你的应用更具响应性以及尽可能规避这些问题，可在 连接请求上设置较短的超时，这样该操作便会因超时早于默认设置中的相关值而较早出现失败。

为 StreamWebSocket 和 MessageWebSocket 设置超时的方式类似。 以下示例演示了如何在 StreamWebSocket 上设置超时，不过过程与 MessageWebSocket 类似。

1.  使用计时器创建一个需在指定延迟过后完成的任务。
2.  使用 cancellation\_token\_source 为 WebSocket 操作创建一项任务，以支持取消操作。
3.  如果带指定超时延迟的任务先于 WebSocket 连接操作完成，将取消 WebSocket 操作的任务。

下面的示例先创建了一个需在指定延迟过后完成的任务，然后创建了需在指定延迟过后取消的另一任务。 当尝试建立连接以设置指定超时，可将这些类与 StreamWebSocket 和 MessageWebSocket 结合使用。 用法示例将使用支持取消操作的 cancellation\_token\_source 在任务中调用 StreamWebSocket.ConnectAsync 方法。 如果超时首次完成，则 cancellation\_token\_source 将用于取消 WebSocket 连接操作的相关任务。

```cpp
    #include <agents.h>
    #include <ppl.h>
    #include <ppltasks.h>

    using namespace concurrency;
    using namespace std;

    // Creates a task that completes after the specified delay.
    task<void> complete_after(unsigned int timeout)
    {
        // A task completion event that is set when a timer fires.
        task_completion_event<void> tce;

        // Create a non-repeating timer.
        shared_ptr<timer<int>> fire_once(new timer<int>(timeout, 0, nullptr, false));
        
        // Create a call object that sets the completion event after the timer fires.
        shared_ptr<call<int>> callback(new call<int>([tce](int)
        {
            tce.set();
        }));

        // Connect the timer to the callback and start the timer.
        fire_once->link_target(callback.get());
        fire_once->start();

        // Create a task that completes after the completion event is set.
        task<void> event_set(tce);

        // Create a continuation task that cleans up resources and
        // and return that continuation task.
        return event_set.then([callback, fire_once]()
        {
        });
    }

    // Cancels the provided task after the specifed delay, if the task
    // did not complete.
    template<typename T>
    task<T> cancel_after_timeout(task<T> t, cancellation_token_source cts, unsigned int timeout)
    {
        // Create a task that returns true after the specified task completes.
        task<bool> success_task = t.then([](T)
        {
            return true;
        });
        // Create a task that returns false after the specified timeout.
        task<bool> failure_task = complete_after(timeout).then([]
        {
            return false;
        });

        // Create a continuation task that cancels the overall task  
        // if the timeout task finishes first. 
        return (failure_task || success_task).then([t, cts](bool success)
        {
            if (!success)
            {
                // Set the cancellation token. The task that is passed as the 
                // t parameter should respond to the cancellation and stop 
                // as soon as it can.
                cts.cancel();
            }
 
            // Return the original task.
            return t;
        });
    }
```



<!--HONumber=May16_HO2-->



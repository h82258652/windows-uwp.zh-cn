---
description: 要在网络通信不在后台运行的情况下继续网络通信，应用可以使用后台任务以及套接字代理或控制通道触发器。
title: 后台网络通信
ms.assetid: 537F8E16-9972-435D-85A5-56D5764D3AC2
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b0246e338c13027f8afc8da4aa919faa0911b39c
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "66371625"
---
# <a name="network-communications-in-the-background"></a>后台网络通信
不是在前台操作的情况下，若要继续进行网络通信，应用可以使用后台任务和以下两个选项之一。
- 套接字代理。 如果应用使用套接字进行长期连接，则当它退出前台时，可将套接字的所有权委托给系统套接字代理。 在流量到达套接字上后，该代理会激活应用并将所有权传输回应用；然后应用会处理到达的流量。
- 控制通道触发器。 

## <a name="performing-network-operations-in-background-tasks"></a>在后台任务中执行网络操作
- 在检索到程序包并且需要执行生存期较短的任务时，使用 [SocketActivityTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.socketactivitytrigger) 激活后台任务。 执行任务后，后台任务将会终止，以节省能源。
- 在检索到程序包并且需要执行生存期较长的任务时，使用 [ControlChannelTrigger](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 激活后台任务。

**与网络相关的条件和标志**

- 将 **InternetAvailable** 条件添加到你的后台任务 [BackgroundTaskBuilder.AddCondition](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)，以将后台任务触发时间延迟到网络堆栈运行后。 此条件可以省电，因为必须有网络连接才会执行后台任务。 此条件不提供实时激活。

无论使用哪种触发器，请设置后台任务上的 [IsNetworkRequested](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder)，以确保在后台任务运行时网络保持连接。 这将告知后台任务基础结构在执行任务时保持网络运行，即使设备已进入连接待机模式也是如此。 如果后台任务未使用 **IsNetworkRequested**，则当处于连接待机模式时（例如，当手机屏幕关闭时），后台任务将无法访问网络。

## <a name="socket-broker-and-the-socketactivitytrigger"></a>套接字代理和 SocketActivityTrigger
如果应用使用 [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket)、[**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) 或 [**StreamSocketListener**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocketListener) 连接，尽管它不在前台，你应该使用 [**SocketActivityTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger) 和套接字代理收到流量到达你的应用的通知。

为了使应用能够在处于非活动状态时接收并处理在套接字上接收的数据，你的应用必须在启动时执行某种一次性设置，然后在应用过渡为非活动状态时，将套接字所有权传输给套接字代理。

一次性设置步骤用于创建触发器、为触发器注册后台任务，以及为套接字代理启用套接字：
  - 通过将 TaskEntryPoint 参数设置为用于处理接收的数据包的代码，创建一个 **SocketActivityTrigger** 并为该触发器注册后台任务。
```csharp
            var socketTaskBuilder = new BackgroundTaskBuilder();
            socketTaskBuilder.Name = _backgroundTaskName;
            socketTaskBuilder.TaskEntryPoint = _backgroundTaskEntryPoint;
            var trigger = new SocketActivityTrigger();
            socketTaskBuilder.SetTrigger(trigger);
            _task = socketTaskBuilder.Register();
```
  - 在绑定套接字之前，调用该套接字上的 **EnableTransferOwnership**。
```csharp
           _tcpListener = new StreamSocketListener();

           // Note that EnableTransferOwnership() should be called before bind,
           // so that tcpip keeps required state for the socket to enable connected
           // standby action. Background task Id is taken as a parameter to tie wake pattern
           // to a specific background task.  
           _tcpListener. EnableTransferOwnership(_task.TaskId,SocketActivityConnectedStandbyAction.Wake);
           _tcpListener.ConnectionReceived += OnConnectionReceived;
           await _tcpListener.BindServiceNameAsync("my-service-name");
```

在正确设置套接字后，如果应用即将暂停，则调用该套接字上的 **TransferOwnership** 以将其传输给套接字代理。 代理会监视该套接字并在收到数据时激活你的后台任务。 以下示例包含了一个实用工具 **TransferOwnership** 函数来执行 **StreamSocketListener** 套接字的传输。 （注意，每一个不同类型的套接字都有其自己的 **TransferOwnership** 方法，因此你必须调用适用于正在传输其所有权的套接字的方法。 对于你使用的每个套接字类型，你的代码可能会包含带有一个实现的过载 **TransferOwnership** 帮助程序，以便 **OnSuspending** 代码可轻松读取。）

通过使用以下一种适当方法，应用会将套接字的所有权传输给套接字代理并传递后台任务的 ID：
-   [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket) 上的 [**TransferOwnership**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.datagramsocket.transferownership) 方法之一。
-   [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) 上的 [**TransferOwnership**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.transferownership) 方法之一。
-   [**StreamSocketListener**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocketListener) 上的 [**TransferOwnership**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocketlistener.transferownership) 方法之一。

```csharp

// declare int _transferOwnershipCount as a field.

private void TransferOwnership(StreamSocketListener tcpListener)
{
    await tcpListener.CancelIOAsync();

    var dataWriter = new DataWriter();
    ++_transferOwnershipCount;
    dataWriter.WriteInt32(transferOwnershipCount);
    var context = new SocketActivityContext(dataWriter.DetachBuffer());
    tcpListener.TransferOwnership(_socketId, context);
}

private void OnSuspending(object sender, SuspendingEventArgs e)
{
    var deferral = e.SuspendingOperation.GetDeferral();

    TransferOwnership(_tcpListener);
    deferral.Complete();
}
```
在后台任务的事件处理程序中：
   -  首先，延迟后台任务，以便你可以使用异步方法来处理事件。
```csharp
var deferral = taskInstance.GetDeferral();
```
   -  然后，从事件参数中提取 SocketActivityTriggerDetails 并查找引发该事件的原因：
```csharp
var details = taskInstance.TriggerDetails as SocketActivityTriggerDetails;
    var socketInformation = details.SocketInformation;
    switch (details.Reason)
```
   -   如果由于套接字活动而引发了该事件，请在该套接字上创建 DataReader、以异步方式加载阅读器，然后根据应用的设计使用数据。 请注意，必须将套接字的所有权返回到套接字代理，才能再次收到后续套接字活动的通知。

   在以下示例中，在套接字上接收的文本显示在 Toast 中。

```csharp
case SocketActivityTriggerReason.SocketActivity:
            var socket = socketInformation.StreamSocket;
            DataReader reader = new DataReader(socket.InputStream);
            reader.InputStreamOptions = InputStreamOptions.Partial;
            await reader.LoadAsync(250);
            var dataString = reader.ReadString(reader.UnconsumedBufferLength);
            ShowToast(dataString);
            socket.TransferOwnership(socketInformation.Id); /* Important! */
            break;
```

   -   如果由于活动计时器到期而引发了该事件，则你的代码应该通过套接字发送某些数据，以便使套接字保持活动并重新启动活动计时器。 另请注意，将套接字的所有权返回到套接字代理，以便收到后续事件通知，这一点很重要：

```csharp
case SocketActivityTriggerReason.KeepAliveTimerExpired:
            socket = socketInformation.StreamSocket;
            DataWriter writer = new DataWriter(socket.OutputStream);
            writer.WriteBytes(Encoding.UTF8.GetBytes("Keep alive"));
            await writer.StoreAsync();
            writer.DetachStream();
            writer.Dispose();
            socket.TransferOwnership(socketInformation.Id); /* Important! */
            break;
```

   -   如果由于关闭了套接字而引发了该事件，请重新建立该套接字，从而确保在你创建新的套接字后，将其所有权传输给套接字代理。 在此示例中，主机名和端口存储在本地设置中，以便它们可以用于建立新的套接字连接：

```csharp
case SocketActivityTriggerReason.SocketClosed:
            socket = new StreamSocket();
            socket.EnableTransferOwnership(taskInstance.Task.TaskId, SocketActivityConnectedStandbyAction.Wake);
            if (ApplicationData.Current.LocalSettings.Values["hostname"] == null)
            {
                break;
            }
            var hostname = (String)ApplicationData.Current.LocalSettings.Values["hostname"];
            var port = (String)ApplicationData.Current.LocalSettings.Values["port"];
            await socket.ConnectAsync(new HostName(hostname), port);
            socket.TransferOwnership(socketId);
            break;
```

   -   在完成处理事件通知后，不要忘记结束你的延迟：

```csharp
  deferral.Complete();
```

有关演示使用 [**SocketActivityTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger) 和套接字代理的完整示例，请参阅 [SocketActivityStreamSocket 示例](https://go.microsoft.com/fwlink/p/?LinkId=620606)。 在 Scenario1\_Connect.xaml.cs 中执行套接字的初始化，在 SocketActivityTask.cs 中执行后台任务实现。

你可能会注意到示例在创建新的套接字或获取现有套接字后立刻调用 **TransferOwnership**，而不是如本主题所述使用 **OnSuspending** 事件处理程序执行此操作。 这是因为此示例主要用于演示 [**SocketActivityTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger)，并且不使用该套接字来获取其他任何活动（尽管它在运行中）。 你的应用可能变得更加复杂，并且应该使用 **OnSuspending** 来确定调用 **TransferOwnership** 的时间。

## <a name="control-channel-triggers"></a>控制通道触发器
首先，请确保你使用的是正确的控制通道触发器 (CCT)。 如果使用的是 [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket)、[**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) 或 [**StreamSocketListener**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocketListener) 连接，我们建议使用 [**SocketActivityTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger)。 你可以将 CCT 用于 **StreamSocket**，不过它们会使用更多资源，并且在连接待机模式下可能不起作用。

如果使用的是 WebSockets、[**IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2)、[**System.Net.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 或 [**Windows.Web.Http.HttpClient**](/uwp/api/windows.web.http.httpclient)，则必须使用 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)。

## <a name="controlchanneltrigger-with-websockets"></a>ControlChannelTrigger 与 WebSockets
将 [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 或 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 与 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 结合使用时，需要应用某些特殊注意事项。 将 **MessageWebSocket** 或 **StreamWebSocket** 与 **ControlChannelTrigger** 结合使用时，应遵循某些特定于传输的使用模式和最佳做法。 此外，这些注意事项影响在 **StreamWebSocket** 上接收数据包的请求的处理方式。 不影响在 **MessageWebSocket** 上接收数据包的请求。

将 [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 或 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 与 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 结合使用时，应遵循以下使用模式和最佳做法：

-   必须始终保持发布一个未完成的套接字接收。 这是允许发生推送通知任务所必需的。
-   WebSocket 协议为保持连接消息定义一个标准模型。 [**WebSocketKeepAlive**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.WebSocketKeepAlive) 类可以将客户端发起的 WebSocket 协议保持连接消息发送给服务器。 应用应该将 **WebSocketKeepAlive** 类注册为 KeepAliveTrigger 的 TaskEntryPoint。

某些特殊的注意事项影响在 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 上接收数据包的请求的处理方式。 尤其在将 **StreamWebSocket** 与 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 结合使用时，应用必须使用用于处理读取操作的原始异步模式，而不是 C# 和 VB.NET 中的 **await** 模型或 C++ 中的 Task。 本部分后面的代码示例中说明了原始异步模式。

使用原始异步模式，Windows 可以将 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 的后台任务上的 [**IBackgroundTask.Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.) 方法与接收完成回调的返回同步。 在完成回调返回后，将调用 **Run** 方法。 这将确保应用已在调用 **Run** 方法前收到数据/错误。

请务必注意，应用必须在从完成回调返回控件前发布另一个读取操作。 另请务必注意，[**DataReader**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.DataReader) 无法直接与 [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 或 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 传输一起使用，因为这将中断上面所述的同步。 不支持在传输顶部直接使用 [**DataReader.LoadAsync**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.loadasync) 方法。 相反，[**StreamWebSocket.InputStream**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamwebsocket.inputstream) 属性上的 [**IBuffer**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IBuffer)（由 [**IInputStream.ReadAsync**](https://docs.microsoft.com/uwp/api/windows.storage.streams.iinputstream.readasync) 方法返回）可以在稍后传递给 [**DataReader.FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.frombuffer) 方法以进行进一步处理。

以下示例展示了如何在 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 上使用用于处理读取操作的原始异步模式。

```csharp
void PostSocketRead(int length)
{
    try
    {
        var readBuf = new Windows.Storage.Streams.Buffer((uint)length);
        var readOp = socket.InputStream.ReadAsync(readBuf, (uint)length, InputStreamOptions.Partial);
        readOp.Completed = (IAsyncOperationWithProgress<IBuffer, uint>
            asyncAction, AsyncStatus asyncStatus) =>
        {
            switch (asyncStatus)
            {
                case AsyncStatus.Completed:
                case AsyncStatus.Error:
                    try
                    {
                        // GetResults in AsyncStatus::Error is called as it throws a user friendly error string.
                        IBuffer localBuf = asyncAction.GetResults();
                        uint bytesRead = localBuf.Length;
                        readPacket = DataReader.FromBuffer(localBuf);
                        OnDataReadCompletion(bytesRead, readPacket);
                    }
                    catch (Exception exp)
                    {
                        Diag.DebugPrint("Read operation failed:  " + exp.Message);
                    }
                    break;
                case AsyncStatus.Canceled:

                    // Read is not cancelled in this sample.
                    break;
           }
       };
   }
   catch (Exception exp)
   {
       Diag.DebugPrint("failed to post a read failed with error:  " + exp.Message);
   }
}
```

在调用 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 的后台任务上的 [**IBackgroundTask.Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.) 方法前，将保证引发读取完成处理程序。 Windows 已进行内部同步以等待应用从读取完成回调中返回。 应用通常会在读取完成回调中快速处理 [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 或 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 中的数据或错误。 消息本身会在 **IBackgroundTask.Run** 方法的上下文内部进行处理。 在下面示例中，将通过使用读取完成处理程序将消息插入到其中和后台任务稍后处理的消息队列解释这一点。

以下示例展示读取完成处理程序在 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 上使用用于处理读取操作的原始同步模式。

```csharp
public void OnDataReadCompletion(uint bytesRead, DataReader readPacket)
{
    if (readPacket == null)
    {
        Diag.DebugPrint("DataReader is null");

        // Ideally when read completion returns error,
        // apps should be resilient and try to
        // recover if there is an error by posting another recv
        // after creating a new transport, if required.
        return;
    }
    uint buffLen = readPacket.UnconsumedBufferLength;
    Diag.DebugPrint("bytesRead: " + bytesRead + ", unconsumedbufflength: " + buffLen);

    // check if buffLen is 0 and treat that as fatal error.
    if (buffLen == 0)
    {
        Diag.DebugPrint("Received zero bytes from the socket. Server must have closed the connection.");
        Diag.DebugPrint("Try disconnecting and reconnecting to the server");
        return;
    }

    // Perform minimal processing in the completion
    string message = readPacket.ReadString(buffLen);
    Diag.DebugPrint("Received Buffer : " + message);

    // Enqueue the message received to a queue that the push notify
    // task will pick up.
    AppContext.messageQueue.Enqueue(message);

    // Post another receive to ensure future push notifications.
    PostSocketRead(MAX_BUFFER_LENGTH);
}
```

Websocket 的附加详细信息为保持连接处理程序。 WebSocket 协议为保持连接消息定义一个标准模型。

使用 [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 或 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 时，请将 [**WebSocketKeepAlive**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.WebSocketKeepAlive) 类实例注册为 KeepAliveTrigger 的 [**TaskEntryPoint**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.taskentrypoint)，以便不会暂停应用并允许其向服务器（远程终结点）定期发送保持连接消息。 此操作应作为后台注册应用代码的一部分，并在 程序包清单中完成。

[**Windows.Sockets.WebSocketKeepAlive**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.WebSocketKeepAlive) 的此任务入口点需要在两个位置中指定：

-   在源代码中创建 KeepAliveTrigger 触发器时（参见下例）。
-   在保持连接后台任务声明的应用包清单中。

以下示例将网络触发器通知和保持连接触发器添加到应用部件清单的 &lt;Application&gt; 元素下。

```xml
  <Extensions>
    <Extension Category="windows.backgroundTasks"
         Executable="$targetnametoken$.exe"
         EntryPoint="Background.PushNotifyTask">
      <BackgroundTasks>
        <Task Type="controlChannel" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks"
         Executable="$targetnametoken$.exe"
         EntryPoint="Windows.Networking.Sockets.WebSocketKeepAlive">
      <BackgroundTasks>
        <Task Type="controlChannel" />
      </BackgroundTasks>
    </Extension>
  </Extensions>
```

在 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 及 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket)、[**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 或 [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) 上的异步操作上下文中使用 **await** 语句时，应用必须非常小心。 可以使用 **Task&lt;bool&gt;** 对象为 **StreamWebSocket** 上的推动通知和 WebSocket 保持连接注册 **ControlChannelTrigger**，然后连接传输。 作为注册的一部分，将 **StreamWebSocket** 传输设置为 **ControlChannelTrigger** 的传输，并发布读取。 **Task.Result** 将阻止当前的线程，直到任务中的所有步骤执行并在邮件正文中返回语句。 在方法返回 true 或 false 之前，不会解决该任务。 这样可以保证已执行整个方法。 **Task** 可以包含多个受 **Task** 保护的 **await** 语句。 将 **StreamWebSocket** 或 **MessageWebSocket** 用作传输时，应将此模式与 **ControlChannelTrigger** 对象结合使用。 对于那些需要较长时间才能完成的操作（例如通常的异步读取操作），应用应使用前面讨论的原始异步模式。

以下示例在 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 上为推送通知和 WebSocket 保持连接注册 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)。

```csharp
private bool RegisterWithControlChannelTrigger(string serverUri)
{
    // Make sure the objects are created in a system thread
    // Demonstrate the core registration path
    // Wait for the entire operation to complete before returning from this method.
    // The transport setup routine can be triggered by user control, by network state change
    // or by keepalive task
    Task<bool> registerTask = RegisterWithCCTHelper(serverUri);
    return registerTask.Result;
}

async Task<bool> RegisterWithCCTHelper(string serverUri)
{
    bool result = false;
    socket = new StreamWebSocket();

    // Specify the keepalive interval expected by the server for this app
    // in order of minutes.
    const int serverKeepAliveInterval = 30;

    // Specify the channelId string to differentiate this
    // channel instance from any other channel instance.
    // When background task fires, the channel object is provided
    // as context and the channel id can be used to adapt the behavior
    // of the app as required.
    const string channelId = "channelOne";

    // For websockets, the system does the keepalive on behalf of the app
    // But the app still needs to specify this well known keepalive task.
    // This should be done here in the background registration as well
    // as in the package manifest.
    const string WebSocketKeepAliveTask = "Windows.Networking.Sockets.WebSocketKeepAlive";

    // Try creating the controlchanneltrigger if this has not been already
    // created and stored in the property bag.
    ControlChannelTriggerStatus status;

    // Create the ControlChannelTrigger object and request a hardware slot for this app.
    // If the app is not on LockScreen, then the ControlChannelTrigger constructor will
    // fail right away.
    try
    {
        channel = new ControlChannelTrigger(channelId, serverKeepAliveInterval,
                                   ControlChannelTriggerResourceType.RequestHardwareSlot);
    }
    catch (UnauthorizedAccessException exp)
    {
        Diag.DebugPrint("Is the app on lockscreen? " + exp.Message);
        return result;
    }

    Uri serverUriInstance;
    try
    {
        serverUriInstance = new Uri(serverUri);
    }
    catch (Exception exp)
    {
        Diag.DebugPrint("Error creating URI: " + exp.Message);
        return result;
    }

    // Register the apps background task with the trigger for keepalive.
    var keepAliveBuilder = new BackgroundTaskBuilder();
    keepAliveBuilder.Name = "KeepaliveTaskForChannelOne";
    keepAliveBuilder.TaskEntryPoint = WebSocketKeepAliveTask;
    keepAliveBuilder.SetTrigger(channel.KeepAliveTrigger);
    keepAliveBuilder.Register();

    // Register the apps background task with the trigger for push notification task.
    var pushNotifyBuilder = new BackgroundTaskBuilder();
    pushNotifyBuilder.Name = "PushNotificationTaskForChannelOne";
    pushNotifyBuilder.TaskEntryPoint = "Background.PushNotifyTask";
    pushNotifyBuilder.SetTrigger(channel.PushNotificationTrigger);
    pushNotifyBuilder.Register();

    // Tie the transport method to the ControlChannelTrigger object to push enable it.
    // Note that if the transport' s TCP connection is broken at a later point of time,
    // the ControlChannelTrigger object can be reused to plug in a new transport by
    // calling UsingTransport API again.
    try
    {
        channel.UsingTransport(socket);

        // Connect the socket
        //
        // If connect fails or times out it will throw exception.
        // ConnectAsync can also fail if hardware slot was requested
        // but none are available
        await socket.ConnectAsync(serverUriInstance);

        // Call WaitForPushEnabled API to make sure the TCP connection has
        // been established, which will mean that the OS will have allocated
        // any hardware slot for this TCP connection.
        //
        // In this sample, the ControlChannelTrigger object was created by
        // explicitly requesting a hardware slot.
        //
        // On systems that without connected standby, if app requests hardware slot as above,
        // the system will fallback to a software slot automatically.
        //
        // On systems that support connected standby,, if no hardware slot is available, then app
        // can request a software slot by re-creating the ControlChannelTrigger object.
        status = channel.WaitForPushEnabled();
        if (status != ControlChannelTriggerStatus.HardwareSlotAllocated
            && status != ControlChannelTriggerStatus.SoftwareSlotAllocated)
        {
            throw new Exception(string.Format("Neither hardware nor software slot could be allocated. ChannelStatus is {0}", status.ToString()));
        }

        // Store the objects created in the property bag for later use.
        CoreApplication.Properties.Remove(channel.ControlChannelTriggerId);

        var appContext = new AppContext(this, socket, channel, channel.ControlChannelTriggerId);
        ((IDictionary<string, object>)CoreApplication.Properties).Add(channel.ControlChannelTriggerId, appContext);
        result = true;

        // Almost done. Post a read since we are using streamwebsocket
        // to allow push notifications to be received.
        PostSocketRead(MAX_BUFFER_LENGTH);
    }
    catch (Exception exp)
    {
         Diag.DebugPrint("RegisterWithCCTHelper Task failed with: " + exp.Message);

         // Exceptions may be thrown for example if the application has not
         // registered the background task class id for using real time communications
         // broker in the package manifest.
    }
    return result
}
```

有关将 [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 或 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 与 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 结合使用的详细信息，请参阅 [ControlChannelTrigger StreamWebSocket 示例](https://go.microsoft.com/fwlink/p/?linkid=251232)。

## <a name="controlchanneltrigger-with-httpclient"></a>ControlChannelTrigger 与 HttpClient
将 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 与 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 结合使用时，需要应用某些特殊注意事项。 将 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 与 **ControlChannelTrigger** 结合使用时，应遵循某些特定于传输的使用模式和最佳做法。 此外，这些注意事项影响在 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 上接收数据包的请求的处理方式。

**注意** 目前，在使用网络触发器功能和 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 时，不支持使用 SSL 的   [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637)。
 
将 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 与 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 结合使用时，应遵循以下使用模式和最佳做法：

-   在将请求发送到特定 URI 之前，应用需要在 [System.Net.Http](https://go.microsoft.com/fwlink/p/?linkid=227894) 命名空间中的 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 或 [HttpClientHandler](https://go.microsoft.com/fwlink/p/?linkid=241638) 对象上设置各种属性和标题。
-   应用可能需要进行初始请求以正确测试并设置传输，然后再创建要用于 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 的 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 传输。 在应用确定可以正确设置传输后，可以将 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 对象配置为用于 **ControlChannelTrigger** 对象的传输对象。 设计此过程是为了防止在某些情况下断开通过传输建立的连接。 使用 SSL 与证书，应用可能需要显示一个对话框以输入 PIN 或者用于存在多个证书可供选择时。 可能需要进行代理身份验证和服务器身份验证。 如果代理或服务器身份验证已过期，则可能会关闭连接。 应用可以处理这些身份验证过期问题的一个方法是设置计时器。 当需要 HTTP 重定向时，不能保证能够可靠建立第二次连接。 初始测试请求可以确保在将 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 对象用作 **ControlChannelTrigger** 对象的传输之前，应用使用最新的重定向 URL。

与其他网络传输不同，[HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 对象无法直接传递到 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 对象的 [**UsingTransport**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.controlchanneltrigger.usingtransport) 方法中。 相反，必须专门构建 [HttpRequestMessage](https://go.microsoft.com/fwlink/p/?linkid=259153) 对象以用于 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 对象和 **ControlChannelTrigger**。 [HttpRequestMessage](https://go.microsoft.com/fwlink/p/?linkid=259153) 对象使用 [RtcRequestFactory.Create](https://go.microsoft.com/fwlink/p/?linkid=259154) 方法创建。 创建的 [HttpRequestMessage](https://go.microsoft.com/fwlink/p/?linkid=259153) 对象随后将传递给 **UsingTransport** 方法。

以下示例展示了如何构建 [HttpRequestMessage](https://go.microsoft.com/fwlink/p/?linkid=259153) 对象以用于 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 对象和 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)。

```csharp
using System;
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
using Windows.Networking.Sockets;

public HttpRequestMessage httpRequest;
public HttpClient httpClient;
public HttpRequestMessage httpRequest;
public ControlChannelTrigger channel;
public Uri serverUri;

private void SetupHttpRequestAndSendToHttpServer()
{
    try
    {
        // For HTTP based transports that use the RTC broker, whenever we send next request, we will abort the earlier
        // outstanding http request and start new one.
        // For example in case when http server is taking longer to reply, and keep alive trigger is fired in-between
        // then keep alive task will abort outstanding http request and start a new request which should be finished
        // before next keep alive task is triggered.
        if (httpRequest != null)
        {
            httpRequest.Dispose();
        }

        httpRequest = RtcRequestFactory.Create(HttpMethod.Get, serverUri);

        SendHttpRequest();
    }
        catch (Exception e)
    {
        Diag.DebugPrint("Connect failed with: " + e.ToString());
        throw;
    }
}
```

某些特殊的注意事项影响在 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 上发送 HTTP 请求以启动接收响应的请求的处理方式。 尤其在将 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 与 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 结合使用时，你的应用必须使用处理发送操作的 Task 而不是 **await** 模型。

使用 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637)，不会将 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 的后台任务上的 [**IBackgroundTask.Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.) 方法与接收完成回调的返回同步。 出于此原因，应用仅可以在 **Run** 方法中使用阻止 HttpResponseMessage 技术并等待，直至收到整个响应。

结合使用 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 和 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 明显不同于 [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket)、[**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 或 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 传输。 由于 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 代码，通过 Task 将 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 接收回调提供给应用。 这意味着，**ControlChannelTrigger** 推送通知任务会在将数据或错误调度到应用后立即引发。 在下面的示例中，代码将 [HttpClient.SendAsync](https://go.microsoft.com/fwlink/p/?linkid=241637) 方法返回的 responseTask 存储在推送通知任务将选取和内联处理的全局存储中。

以下示例展示了如何在与 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 一起使用时在 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 上处理发送请求。

```csharp
using System;
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
using Windows.Networking.Sockets;

private void SendHttpRequest()
{
    if (httpRequest == null)
    {
        throw new Exception("HttpRequest object is null");
    }

    // Tie the transport method to the controlchanneltrigger object to push enable it.
    // Note that if the transport' s TCP connection is broken at a later point of time,
    // the controlchanneltrigger object can be reused to plugin a new transport by
    // calling UsingTransport API again.
    channel.UsingTransport(httpRequest);

    // Call the SendAsync function to kick start the TCP connection establishment
    // process for this http request.
    Task<HttpResponseMessage> httpResponseTask = httpClient.SendAsync(httpRequest);

    // Call WaitForPushEnabled API to make sure the TCP connection has been established,
    // which will mean that the OS will have allocated any hardware slot for this TCP connection.
    ControlChannelTriggerStatus status = channel.WaitForPushEnabled();
    Diag.DebugPrint("WaitForPushEnabled() completed with status: " + status);
    if (status != ControlChannelTriggerStatus.HardwareSlotAllocated
        && status != ControlChannelTriggerStatus.SoftwareSlotAllocated)
    {
        throw new Exception("Hardware/Software slot not allocated");
    }

    // The HttpClient receive callback is delivered via a Task to the app.
    // The notification task will fire as soon as the data or error is dispatched
    // Enqueue the responseTask returned by httpClient.sendAsync
    // into a queue that the push notify task will pick up and process inline.
    AppContext.messageQueue.Enqueue(httpResponseTask);
}
```

以下示例展示了如何在与 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 一起使用时在 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 上读取收到的响应。

```csharp
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;

public string ReadResponse(Task<HttpResponseMessage> httpResponseTask)
{
    string message = null;
    try
    {
        if (httpResponseTask.IsCanceled || httpResponseTask.IsFaulted)
        {
            Diag.DebugPrint("Task is cancelled or has failed");
            return message;
        }
        // We' ll wait until we got the whole response.
        // This is the only supported scenario for HttpClient for ControlChannelTrigger.
        HttpResponseMessage httpResponse = httpResponseTask.Result;
        if (httpResponse == null || httpResponse.Content == null)
        {
            Diag.DebugPrint("Cannot read from httpresponse, as either httpResponse or its content is null. try to reset connection.");
        }
        else
        {
            // This is likely being processed in the context of a background task and so
            // synchronously read the Content' s results inline so that the Toast can be shown.
            // before we exit the Run method.
            message = httpResponse.Content.ReadAsStringAsync().Result;
        }
    }
    catch (Exception exp)
    {
        Diag.DebugPrint("Failed to read from httpresponse with error:  " + exp.ToString());
    }
    return message;
}
```

有关结合使用 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 和 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 的详细信息，请参阅 [ControlChannelTrigger HttpClient 示例](https://go.microsoft.com/fwlink/p/?linkid=258323)。

## <a name="controlchanneltrigger-with-ixmlhttprequest2"></a>ControlChannelTrigger 与 IXMLHttpRequest2
将 [**IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2) 与 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 结合使用时，需要应用某些特殊注意事项。 将 **IXMLHTTPRequest2** 与 **ControlChannelTrigger** 结合使用时，应遵循某些特定于传输的使用模式和最佳做法。 使用 **ControlChannelTrigger** 不影响在 **IXMLHTTPRequest2** 上发送或接收 HTTP 请求的请求的处理方式。

结合使用 [**IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2) 和 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 时的使用模式和最佳做法

-   [**IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2) 对象在用作传输时，其生存时间仅有一个请求/响应。 在与 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 对象结合使用时，可以方便地创建并设置 **ControlChannelTrigger** 对象一次，然后重复调用 [**UsingTransport**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.controlchanneltrigger.usingtransport) 方法，每次关联一个新的 **IXMLHTTPRequest2** 对象。 应用应首先删除上一个 **IXMLHTTPRequest2** 对象，然后再提供新 **IXMLHTTPRequest2** 对象，以确保应用不超出分配的资源限制。
-   在调用 [**Send**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nf-msxml6-ixmlhttprequest2-send) 方法前，应用可能需要调用 [**SetProperty**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nf-msxml6-ixmlhttprequest2-setproperty) 和 [**SetRequestHeader**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nf-msxml6-ixmlhttprequest2-setrequestheader) 方法来设置 HTTP 传输。
-   应用可能需要进行初始 [**Send**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nf-msxml6-ixmlhttprequest2-send) 请求以正确测试并设置传输，然后再创建要用于 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 的传输。 在应用确定正确设置传输后，可以将 [**IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2) 对象配置为与 **ControlChannelTrigger** 一起使用的传输对象。 设计此过程是为了防止在某些情况下断开通过传输建立的连接。 使用 SSL 与证书，应用可能需要显示一个对话框以输入 PIN 或者用于存在多个证书可供选择时。 可能需要进行代理身份验证和服务器身份验证。 如果代理或服务器身份验证已过期，则可能会关闭连接。 应用可以处理这些身份验证过期问题的一个方法是设置计时器。 当需要 HTTP 重定向时，不能保证能够可靠建立第二次连接。 初始测试请求可以确保在将 **IXMLHTTPRequest2** 对象用作 **ControlChannelTrigger** 对象的传输之前，应用使用最新的重定向 URL。

有关结合使用 [**IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2) 和 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 的详细信息，请参阅 [ControlChannelTrigger 和 IXMLHTTPRequest2 示例](https://go.microsoft.com/fwlink/p/?linkid=258538)。

## <a name="important-apis"></a>重要的 API
* [SocketActivityTrigger](/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger)
* [ControlChannelTrigger](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)

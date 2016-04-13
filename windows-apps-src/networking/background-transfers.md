---
description: 使用后台传输 API 以通过网络可靠地复制文件。
title: 后台传输
ms.assetid: 1207B089-BC16-4BF0-BBD4-FD99950C764B
---

# 后台传输

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242)
-   [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998)
-   [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)

使用后台传输 API 以通过网络可靠地复制文件。 后台传输 API 提供应用暂停期间在后台运行的高级上载和下载功能，并持续至应用终止。 API 监视网络状态，并在连接丢失时自动暂停和恢复传输，并且传输还具有流量感知和电量感知功能，这意味着可以根据当前连接和设备电池状态调整下载活动。 该 API 适用于使用 HTTP 上载和下载较大文件。 还支持 FTP，但只能用于下载。

后台传输独立于调用应用单独运行，主要是针对资源（如视频、音乐和大型图像）的长期传输操作设计的。 对于这些应用场景，使用后台传输非常必要，因为即使应用已暂停，下载仍会继续进行。

如果你下载的是可能快速完成的小型资源，应该使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) API 而不是后台传输。

## 使用 Windows.Networking.BackgroundTransfer


### 后台传输功能如何工作？

当应用使用后台传输来启动传输时，该请求将使用 [**BackgroundDownloader**](https://msdn.microsoft.com/library/windows/apps/br207126) 或 [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) 类对象进行配置和初始化。 每个传输操作都由系统单独处理，并与正在调用的应用分开。 如果你希望在应用 UI 中向用户提供状态，系统提供了进程信息；在发生传输时，你的应用可以暂停、恢复和取消传输，甚至可以读取数据。 这种由系统处理的传输方式可以促进智能化的电源使用，并防止应用在联网状态下因遇到类似如下事件而可能带来的问题：应用挂起、终止或网络状态突然更改。

### 使用后台传输执行经过身份验证的文件请求

后台传输可提供支持基本的服务器和代理凭据、cookie 的方法，并且还支持每个传输操作使用自定义的 HTTP 头（通过 [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146)）。

### 此功能如何适应网络状态更改或意外关机？

当网络状态发生更改时，后台传输功能可保持每个传输操作的一致性体验，从而智能地利用[连接](https://msdn.microsoft.com/library/windows/apps/hh452990)功能提供的连接和运营商流量套餐状态信息。 为了定义不同网络方案的行为，应用使用 [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) 定义的值为每个操作设置一个成本策略。

例如，为某个操作定义的成本策略可以指示该操作应在设备使用按流量计费的网络时自动暂停。 然后，当建立到“无限制”网络的连接时，自动恢复（或自动启动）传输。 有关如何按成本定义网络的更多信息，请参阅 [**NetworkCostType**](https://msdn.microsoft.com/library/windows/apps/br207292)。

虽然后台传输功能具备其自己的处理网络状态更改的机制，但对于使用网络连接功能的应用还有其他需要考虑的常规连接因素。 有关其他信息，请阅读[利用可用的网络连接信息](https://msdn.microsoft.com/library/windows/apps/hh452983)。

> **注意** 对于在移动设备上运行的应用，存在一些允许用户监控和限制根据连接类型、漫游状态和用户的流量套餐传输的数据量的功能。 因此，即使 [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) 表示传输应该继续，仍可在电话上暂停后台传输。

下表指示在电话的当前给定状态下，对于每个 [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) 值，允许在电话上进行后台传输的时间。 你可以使用 [**ConnectionCost**](https://msdn.microsoft.com/library/windows/apps/br207244) 类确定电话的当前状态。

| 设备状态                                                                                                                      | UnrestrictedOnly | 默认值 | 始终 |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------|---------|--------|
| 已连接到 WiFi                                                                                                                 | 允许            | 允许   | 允许  |
| 按流量计费的连接、未漫游、受数据限制、计划不超出限制                                                   | 拒绝             | 允许   | 允许  |
| 按流量计费的连接、未漫游、受数据限制、计划超出限制                                                       | 拒绝             | 拒绝    | 允许  |
| 按流量计费的连接、漫游、受数据限制                                                                                     | 拒绝             | 拒绝    | 允许  |
| 按流量计费的连接、不受数据限制。 仅当用户在“流量管理”UI 中启用“限制后台数据”时，才会发生该状态。 | 拒绝             | 拒绝    | 拒绝   |

 

## 上载文件


如果使用后台传输，上传作为 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 存在，该对象具有一系列用于重新启动或取消操作的控制方法。 系统根据 **UploadOperation** 自动处理应用事件（如暂停或终止）和连接更改；在应用暂停期间或暂停时，上传会继续运行，并且在应用终止后，仍然保持运行。 此外，正确设置 [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018) 可指示在对 Internet 连接使用按流量计费的网络时，应用是否将开始上传。

以下示例将指导你完成基本上传的创建和初始化，以及如何枚举和重新引入以前应用会话中保持的操作。

### 上载单个文件

创建上传从 [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) 开始。 该类用于提供使应用能够在创建结果 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 之前配置上传的方法。 以下示例说明如何使用所需的 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 和 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 对象执行该操作。

**标识文件和目标以供上传**

我们首先需要识别要上传的目标位置的 URI 和要上传的文件，然后才能开始创建 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224)。 在以下示例中，*uriString* 值使用 UI 输入的字符串进行填充，*file* 值使用 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) 操作返回的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 对象进行填充。

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_B "Identify the file and destination for the upload")]

**创建和初始化上传操作**

在上一步中，*uriString* 和 *file* 值会传递给下一个示例 UploadOp 的实例，在该示例中，这些值用于配置和启动新的上传操作。 首先，解析 *uriString* 来创建所需的 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 对象。

接下来，[**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) 使用提供的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) (*file*) 的属性来填充请求头并使用 **StorageFile** 对象设置 *SourceFile* 属性。 然后，调用 [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146) 方法以插入文件名（以字符串形式提供）和 [**StorageFile.Name**](https://msdn.microsoft.com/library/windows/apps/br227220) 属性。

最后，[**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) 会创建 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) (*upload*)。

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_A "Create and initialize the upload operation")]

请注意使用 JavaScript Promise 定义的异步方法调用。 查看上一示例中的一行：

```javascript
promise = upload.startAsync().then(complete, error, progress);
```

    The async method call is followed by a then statement which indicates methods, defined by the app, that are called when a result from the async method call is returned. For more information on this programming pattern, see [Asynchronous programming in JavaScript using promises](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx).

### 上传多个文件

**标识文件和目标以供上传**

    In a scenario involving multiple files transferred with a single [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), the process begins as it usually does by first providing the required destination URI and local file information. Similar to the example in the previous section, the URI is provided as a string by the end-user and [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) can be used to provide the ability to indicate files through the user interface as well. However, in this scenario the app should instead call the [**PickMultipleFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br207851) method to enable the selection of multiple files through the UI.

```javascript
function uploadFiles() {
       var filePicker = new Windows.Storage.Pickers.FileOpenPicker();
       filePicker.fileTypeFilter.replaceAll(["*"]);

       filePicker.pickMultipleFilesAsync().then(function (files) {
          if (files === 0) {
             printLog("No file selected");
                return;
          }

          var upload = new UploadOperation();
          var uriString = document.getElementById("serverAddressField").value;
          upload.startMultipart(uriString, files);

          // Persist the upload operation in the global array.
          uploadOperations.push(upload);
       });
    }
```

**创建给定参数的对象**

    The next two examples use code contained in a single example method, **startMultipart**, which was called at the end of the last step. For the purpose of instruction the code in the method that creates an array of [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) objects has been split from the code that creates the resultant [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224).

    First, the URI string provided by the user is initialized as a [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998). Next, the array of [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) objects (**files**) passed to this method is iterated through, each object is used to create a new [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) object which is then placed in the **contentParts** array.

```javascript
upload.startMultipart = function (uriString, files) {
        try {
            var uri = new Windows.Foundation.Uri(uriString);
            var uploader = new Windows.Networking.BackgroundTransfer.BackgroundUploader();

            var contentParts = [];
            files.forEach(function (file, index) {
                var part = new Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart("File" + index, file.name);
                part.setFile(file);
                contentParts.push(part);
            });
```

**创建和初始化分段上传操作**

    With our contentParts array populated with all of the [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) objects representing each [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) for upload, we are ready to call [**CreateUploadAsync**](https://msdn.microsoft.com/library/windows/apps/hh923973) using the [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) to indicate where the request will be sent.

```javascript
        // Create a new upload operation.
            uploader.createUploadAsync(uri, contentParts).then(function (uploadOperation) {

               // Start the upload and persist the promise to be able to cancel the upload.
               upload = uploadOperation;
               promise = uploadOperation.startAsync().then(complete, error, progress);
            });

         } catch (err) {
             displayError(err);
         }
     };
```

### 重新启动中断的上传操作

在完成或取消 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 时，将释放所有关联的系统资源。 然而，如果在发生这些操作之前终止应用，将中止任何活动操作，并仍将占有与每个操作相关的资源。 如果未枚举这些操作并且未重新引入下一个应用会话，那么它们将不会完成并将继续占有设备资源。

1.  在定义枚举保持的操作的函数之前，我们需要创建包含将返回的 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 对象的数组：

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_C "Restart interrupted upload operation")]

2.  接下来，我们定义函可枚举保持的操作并将其存储到数组中的函数。 请注意，为将回调重新分配到 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 而调用的 **load** 方法（如果它在应用终止后仍然保持）在本部分的后面部分中定义的 UploadOp 类中。

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_D "Enumerate persisted operations")]

## 下载文件

如果使用后台传输，每个下载作为 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 存在，该对象公开一系列用于暂停、恢复、重新启动和取消操作的控制方法。 系统根据 **DownloadOperation** 自动处理应用事件（如暂停或终止）和连接更改；在应用挂起期间或暂停时，下载会继续运行，并且在应用终止后，仍然保持运行。 对于移动网络情况，设置 [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018) 属性可指示在对 Internet 连接使用按流量计费的网络时，应用是否将开始或继续下载。

如果你下载的是可能快速完成的小型资源，应该使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) API 而不是后台传输。

以下示例将指导你完成基本下载的创建和初始化，以及如何枚举和重新引入以前应用会话中保持的操作。

### 配置和启动后台传输文件下载

以下示例演示如何使用表示 URI 和文件名的字符串来创建 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 对象和包含所请求文件的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)。 在本示例中，新文件会自动放置在预定义的位置中。 此外，可使用 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) 来允许用户指示要在设备上保存文件的位置。 请注意，为将回调重新分配到 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 而调用的 **load** 方法（如果它在应用终止后仍然保持）在本部分后面部分中定义的 DownloadOp 类中。

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_A)]

请注意使用 JavaScript Promise 定义的异步方法调用。 查看上一代码示例中的第 17 行：

```javascript
promise = download.startAsync().then(complete, error, progress);
```

异步方法调用后跟一个 then 语句，它指示从异步方法调用返回结果时调用的方法（由应用定义）。 有关此编程模式的详细信息，请参阅[在 JavaScript 中使用 Promise 进行异步编程](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx)。

### 添加其他操作控制方法

控制级别可通过实现附加的 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 方法来进行提升。 例如，将以下代码添加到上述示例将引入取消下载的功能。

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_B)]

### 在启动时枚举保持的操作

在完成或取消 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 时，将释放任何关联的系统资源。 不过，如果你的应用在任一事件发生之前停止，下载将暂停并在后台保持。 下例演示了如何将保持的下载重新引入新的应用会话。

1.  在定义枚举保持的操作的函数之前，我们需要创建包含将返回的 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 对象的数组：

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_D)]

2.  接下来，我们定义函可枚举保持的操作并将其存储到数组中的函数。 请注意，为将回调重新分配到 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 而调用的 **load** 方法在本部分后面部分中定义的 DownloadOp 示例中。

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_E)]

3.  现在可以使用填充的列表重新启动挂起的操作。

## 后处理

Windows 10 中的新功能可以在完成后台传输时运行应用程序代码，即使该应用未在运行。 例如，你的应用可能希望在电影结束下载后更新电影的可用列表，而不是在每次启动应用时让应用扫描新电影。 或者，你的应用可能希望通过再次尝试使用不同的服务器或端口来处理已失败的文件传输。 针对成功的传输和失败的传输，均会调用后处理，以便你可以使用它来实现自定义错误处理和重试逻辑。

后处理使用现有的后台任务基础结构。 创建后台任务，并在开始传输前将后台任务与传输关联。 然后传输在后台执行，并在它们完成时，调用后台任务以执行后处理。

后处理使用新的类 [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209)。 此类类似于现有的 [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030)，它允许你将后台传输一起分组，但 **BackgroundTransferCompletionGroup** 新增了一个功能，该功能可以指定要在传输完成时运行的后台任务。

使用后处理启动后台传输，如下所示。

1.  创建 [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209) 对象。 然后，创建 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 对象。 将生成器对象的 **Trigger** 属性设置为完成组对象，并将生成器的 **TaskEngtyPoint** 属性设置为应该在传输完成时执行的后台任务的入口点。 最后，调用 [**BackgroundTaskBuilder.Register**](https://msdn.microsoft.com/library/windows/apps/br224772) 方法以注册后台任务。 请注意，许多完成组可以共享一个后台任务入口点，但是每个后台任务注册只能有一个完成组。

   ```csharp
    var completionGroup = new BackgroundTransferCompletionGroup();
    BackgroundTaskBuilder builder = new BackgroundTaskBuilder();

    builder.Name = "MyDownloadProcessingTask";
    builder.SetTrigger(completionGroup.Trigger);
    builder.TaskEntryPoint = "Tasks.BackgroundDownloadProcessingTask";

    BackgroundTaskRegistration downloadProcessingTask = builder.Register();
    ```

2.  Next you associate background transfers with the completion group. Once all transfers are created, enable the completion group.

   ```csharp
    BackgroundDownloader downloader = new BackgroundDownloader(completionGroup);
    DownloadOperation download = downloader.CreateDownload(uri, file);
    Task<DownloadOperation> startTask = download.StartAsync().AsTask();

    // App still sees the normal completion path
    startTask.ContinueWith(ForegroundCompletionHandler);

    // Do not enable the CompletionGroup until after all downloads are created.
    downloader.CompletinGroup.Enable();
    ```

3.  The code in the background task extracts the list of operations from the trigger details, and your code can then inspect the details for each operation and perform appropriate post-processing for each operation.

   ```csharp
    public class BackgroundDownloadProcessingTask : IBackgroundTask
    {
      public async void Run(IBackgroundTaskInstance taskInstance)
      {
        var details = (BackgroundTransferCompletionGroupTriggerDetails)taskInstance.TriggerDetails;
        IReadOnlyList<DownloadOperation> downloads = details.Downloads;

        // Do post-processing on each finished operation in the list of downloads
      }
    }
    ```

The post-processing task is a regular background task. It is part of the pool of all background tasks, and it is subject to the same resource management policy as all background tasks.

Also, note that post-processing does not replace foreground completion handlers. If your app defines a foreground completion handler, and your app is running when the file transfer completes, then both your foreground completion handler and your background completion handler will be called. The order in which foreground and background tasks are called is not guaranteed. If you define both, you should ensure that the two tasks will work properly and not interfere with each other if they are running concurrently.

## Request timeouts

There are two primary connection timeout scenarios to take into consideration:

-   When establishing a new connection for a transfer, the connection request is aborted if it is not established within five minutes.

-   After a connection has been established, an HTTP request message that has not received a response within two minutes is aborted.

> **Note**  In either scenario, assuming there is Internet connectivity, Background Transfer will retry a request up to three times automatically. In the event Internet connectivity is not detected, additional requests will wait until it is.

## Debugging guidance

Stopping a debugging session in Microsoft Visual Studio is comparable to closing your app; PUT uploads are paused and POST uploads are terminated. Even while debugging, your app should enumerate and then restart or cancel any persisted uploads. For example, you can have your app cancel enumerated persisted upload operations at app startup if there is no interest in previous operations for that debug session.

While enumerating downloads/uploads on app startup during a debug session, you can have your app cancel them if there is no interest in previous operations for that debug session. Note that if there are Visual Studio project updates, like changes to the app manifest, and the app is uninstalled and re-deployed, [**GetCurrentUploadsAsync**](https://msdn.microsoft.com/library/windows/apps/hh701149) cannot enumerate operations created using the previous app deployment.

When using Background Transfer during development, you may get into a situation where the internal caches of active and completed transfer operations can get out of sync. This may result in the inability to start new transfer operations or interact with existing operations and [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030) objects. In some cases, attempting to interact with existing operations may trigger a crash. This result can occur if the [**TransferBehavior**](https://msdn.microsoft.com/library/windows/apps/dn279033) property is set to **Parallel**. This issue occurs only in certain scenarios during development and is not applicable to end users of your app.

Four scenarios using Visual Studio can cause this issue.

-   You create a new project with the same app name as an existing project, but a different language (from C++ to C#, for example).
-   You change the target architecture (from x86 to x64, for example) in an existing project.
-   You change the culture (from neutral to en-US, for example) in an existing project.
-   You add or remove a capability in the package manifest (adding **Enterprise Authentication**, for example) in an existing project.

Regular app servicing, including manifest updates which add or remove capabilities, do not trigger this issue on end user deployments of your app.
To work around this issue, completely uninstall all versions of the app and re-deploy with the new language, architecture, culture, or capability. This can be done via the **Start** screen or using PowerShell and the **Remove-AppxPackage** cmdlet.

## Exceptions in Windows.Networking.BackgroundTransfer

An exception is thrown when an invalid string for a the Uniform Resource Identifier (URI) is passed to the constructor for the [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) object.

**.NET:  **The [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) type appears as [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) in C# and VB.

In C# and Visual Basic, this error can be avoided by using the [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) class in the .NET 4.5 and one of the [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) methods to test the string received from the app user before the URI is constructed.

In C++, there is no method to try and parse a string to a URI. If an app gets input from the user for the [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998), the constructor should be in a try/catch block. If an exception is thrown, the app can notify the user and request a new hostname.

The [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) namespace has convenient helper methods and uses enumerations in the [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) namespace for handling errors. This can be useful for handling specific network exceptions differently in your app.

An error encountered on an asynchronous method in the [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) namespace is returned as an **HRESULT** value. The [**BackgroundTransferError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701093) method is used to convert a network error from a background transfer operation to a [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818) enumeration value. Most of the **WebErrorStatus** enumeration values correspond to an error returned by the native HTTP or FTP client operation. An app can filter on specific **WebErrorStatus** enumeration values to modify app behavior depending on the cause of the exception.

For parameter validation errors, an app can also use the **HRESULT** from the exception to learn more detailed information on the error that caused the exception. Possible **HRESULT** values are listed in the *Winerror.h* header file. For most parameter validation errors, the **HRESULT** returned is **E\_INVALIDARG**.



<!--HONumber=Mar16_HO1-->



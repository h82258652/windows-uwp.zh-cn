---
author: stevewhims
description: 使用后台传输 API 以通过网络可靠地复制文件。
title: 后台传输
ms.assetid: 1207B089-BC16-4BF0-BBD4-FD99950C764B
ms.author: stwhi
ms.date: 03/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fb273b6a37cb2f6322b0c9e3842b69676f82c616
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2018
ms.locfileid: "5401023"
---
# <a name="background-transfers"></a>后台传输
使用后台传输 API 以便在网络上可靠地复制文件。 后台传输 API 提供应用暂停期间在后台运行的高级上载和下载功能，并持续至应用终止。 API 监视网络状态，并在连接丢失时自动暂停和恢复传输，并且传输还具有流量感知和电量感知功能，这意味着可以根据当前连接和设备电池状态调整下载活动。 该 API 适用于使用 HTTP 上载和下载较大文件。 还支持 FTP，但只能用于下载。

后台传输独立于调用应用单独运行，主要是针对资源（如视频、音乐和大型图像）的长期传输操作设计的。 对于这些应用场景，使用后台传输非常必要，因为即使应用已暂停，下载仍会继续进行。

如果你下载的是可能快速完成的小型资源，应该使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) API 而不是后台传输。

## <a name="using-windowsnetworkingbackgroundtransfer"></a>使用 Windows.Networking.BackgroundTransfer

### <a name="how-does-the-background-transfer-feature-work"></a>后台传输功能如何工作？
当应用使用后台传输来启动传输时，该请求将使用 [**BackgroundDownloader**](https://msdn.microsoft.com/library/windows/apps/br207126) 或 [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) 类对象进行配置和初始化。 每个传输操作都由系统单独处理，并与正在调用的应用分开。 如果你希望在应用 UI 中向用户提供状态，系统提供了进程信息；在发生传输时，你的应用可以暂停、恢复和取消传输，甚至可以读取数据。 这种由系统处理的传输方式可以促进智能化的电源使用，并防止应用在联网状态下因遇到类似如下事件而可能带来的问题：应用挂起、终止或网络状态突然更改。

> [!NOTE]
> 由于每个应用资源所限，应用在任何时刻的传输数均不应多于 200 (DownloadOperations + UploadOperations)。 超出该限制可能会将应用的传输队列置于无法恢复的状态。

应用程序启动时，它必须在所有现有[**DownloadOperation**](/uwp/api/windows.networking.backgroundtransfer.downloadoperation?branch=live)和[**UploadOperation**](/uwp/api/windows.networking.backgroundtransfer.uploadperation?branch=live)对象上调用[**AttachAsync**](/uwp/api/windows.networking.backgroundtransfer.downloadoperation.AttachAsync) 。 不执行此操作将导致的已完成传输泄漏，并且最终将使你使用后台传输功能毫无用处。

### <a name="performing-authenticated-file-requests-with-background-transfer"></a>使用后台传输执行经过身份验证的文件请求
后台传输可提供支持基本的服务器和代理凭据、cookie 的方法，并且还支持每个传输操作使用自定义的 HTTP 头（通过 [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146)）。

### <a name="how-does-this-feature-adapt-to-network-status-changes-or-unexpected-shutdowns"></a>此功能如何适应网络状态更改或意外关机？
当网络状态发生更改时，后台传输功能可保持每个传输操作的一致性体验，从而智能地利用[连接](https://msdn.microsoft.com/library/windows/apps/hh452990)功能提供的连接和运营商流量套餐状态信息。 为了定义不同网络方案的行为，应用使用 [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) 定义的值为每个操作设置一个成本策略。

例如，为某个操作定义的成本策略可以指示该操作应在设备使用按流量计费的网络时自动暂停。 然后，当建立到“无限制”网络的连接时，自动恢复（或自动启动）传输。 有关如何按成本定义网络的更多信息，请参阅 [**NetworkCostType**](https://msdn.microsoft.com/library/windows/apps/br207292)。

虽然后台传输功能具备其自己的处理网络状态更改的机制，但对于使用网络连接功能的应用还有其他需要考虑的常规连接因素。 有关其他信息，请阅读[利用可用的网络连接信息](https://msdn.microsoft.com/library/windows/apps/hh452983)。

> **注意**  对于在移动设备上运行的应用，存在一些允许用户监控和限制根据连接类型、漫游状态和用户的流量套餐传输的数据量的功能。 因此，即使 [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) 表示传输应该继续，仍可在电话上暂停后台传输。

下表指示在电话的当前给定状态下，对于每个 [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) 值，允许在电话上进行后台传输的时间。 你可以使用 [**ConnectionCost**](https://msdn.microsoft.com/library/windows/apps/br207244) 类确定电话的当前状态。

| 设备状态                                                                                                                      | UnrestrictedOnly | 默认值 | 始终 |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------|---------|--------|
| 已连接到 WiFi                                                                                                                 | 允许            | 允许   | 允许  |
| 按流量计费的连接、未漫游、受数据限制、计划不超出限制                                                   | 拒绝             | 允许   | 允许  |
| 按流量计费的连接、未漫游、受数据限制、计划超出限制                                                       | 拒绝             | 拒绝    | 允许  |
| 按流量计费的连接、漫游、受数据限制                                                                                     | 拒绝             | 拒绝    | 允许  |
| 按流量计费的连接、不受数据限制。 仅当用户在“流量管理”UI 中启用“限制后台数据”时，才会发生该状态。 | 拒绝             | 拒绝    | 拒绝   |

## <a name="uploading-files"></a>上载文件
如果使用后台传输，上传作为 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 存在，该对象具有一系列用于重新启动或取消操作的控制方法。 系统根据 **UploadOperation** 自动处理应用事件（如暂停或终止）和连接更改；在应用暂停期间或暂停时，上传会继续运行，并且在应用终止后，仍然保持运行。 此外，正确设置 [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018) 可指示在对 Internet 连接使用按流量计费的网络时，应用是否将开始上传。

以下示例将指导你完成基本上传的创建和初始化，以及如何枚举和重新引入以前应用会话中保持的操作。

### <a name="uploading-a-single-file"></a>上载单个文件
创建上传从 [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) 开始。 该类用于提供使应用能够在创建结果 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 之前配置上传的方法。 以下示例说明如何使用所需的 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 和 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 对象执行该操作。

**标识文件和目标以供上传**

我们首先需要识别要上传的目标位置的 URI 和要上传的文件，然后才能开始创建 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224)。 在以下示例中，*uriString* 值使用 UI 输入的字符串进行填充，*file* 值使用 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) 操作返回的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 对象进行填充。

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_B "Identify the file and destination for the upload")]

**创建和初始化上传操作**

在上一步中，*uriString* 和 *file* 值会传递给下一个示例 UploadOp 的实例，在该示例中，这些值用于配置和启动新的上传操作。 首先，解析 *uriString* 来创建所需的 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 对象。

接下来，[**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) 使用提供的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) (*file*) 的属性来填充请求头并使用 **StorageFile** 对象设置 *SourceFile* 属性。 然后，调用 [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146) 方法以插入文件名（以字符串形式提供）和 [**StorageFile.Name**](https://msdn.microsoft.com/library/windows/apps/br227220) 属性。

最后，[**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) 创建 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) (*upload*)。

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_A "Create and initialize the upload operation")]

请注意使用 JavaScript Promise 定义的异步方法调用。 查看上个示例中的一行：

```javascript
promise = upload.startAsync().then(complete, error, progress);
```

异步方法调用后跟一个 then 语句，它指示从异步方法调用返回结果时调用的方法（由应用定义）。 有关此编程模式的详细信息，请参阅[在 JavaScript 中使用 Promise 进行异步编程](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx)。

### <a name="uploading-multiple-files"></a>上载多个文件
**标识文件和目标以用于上载**

在涉及到使用单个 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 传输多个文件的方案中，这一过程将像通常一样开始，首先提供必需的目标 URI 和本地文件信息。 与上一部分中的示例类似，由最终用户以字符串形式提供 URI，并且也可以使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 提供通过用户界面指示文件的功能。 但在此方案中，应用应改为调用 [**PickMultipleFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br207851) 方法，以便能够通过 UI 选择多个文件。

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

**为提供的参数创建对象**

下面的两个示例使用单示例方法 **startMultipart**（在上一步结束时调用）中包含的代码。 为了进行说明，创建 [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) 对象数组的方法中的代码已从创建结果 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 的代码中分离出来。

首先，用户提供的 URI 字符串初始化为 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998)。 接下来，循环访问传递到此方法的 [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) 对象 (**files**) 数组、使用每个对象创建一个新的 [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) 对象，然后将其置于 **contentParts** 数组中。

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

在使用所有 [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) 对象（表示每个要上载的 [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102)）填充 contentParts 数组后，我们将准备使用 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 调用 [**CreateUploadAsync**](https://msdn.microsoft.com/library/windows/apps/hh923973) 以指示将请求发送到哪里。

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

### <a name="restarting-interrupted-upload-operations"></a>重新启动中断的上载操作
在完成或取消 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 时，将释放所有关联的系统资源。 然而，如果在发生这些操作之前终止应用，将中止任何活动操作，并仍将占有与每个操作相关的资源。 如果未枚举这些操作并且未重新引入下一个应用会话，那么它们将不会完成并将继续占有设备资源。

1.  在定义枚举持续操作的函数之前，我们需要创建包含将返回的 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 对象的数组：

    [!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_C "Restart interrupted upload operation")]

1.  接下来，我们定义枚举持续操作并将其存储到数组中的函数。 请注意，为将回调重新分配到 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 而调用的 **load** 方法（如果它在应用终止后仍然保持）在本部分后面定义的 UploadOp 类中。

    [!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_D "Enumerate persisted operations")]

## <a name="downloading-files"></a>下载文件
如果使用后台传输，每个下载作为 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 存在，该对象公开一系列用于暂停、恢复、重新启动和取消操作的控制方法。 系统根据 **DownloadOperation** 自动处理应用事件（如暂停或终止）和连接更改；在应用挂起期间或暂停时，下载会继续运行，并且在应用终止后，仍然保持运行。 对于移动网络情况，设置 [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018) 属性可指示在对 Internet 连接使用按流量计费的网络时，应用是否将开始或继续下载。

如果你下载的是可能快速完成的小型资源，应该使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) API 而不是后台传输。

以下示例将指导你完成基本下载的创建和初始化，以及如何枚举和重新引入以前应用会话中保持的操作。

### <a name="configure-and-start-a-background-transfer-file-download"></a>配置和启动后台传输文件下载
以下示例演示如何使用表示 URI 和文件名的字符串来创建 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 对象和包含所请求文件的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)。 在本示例中，新文件会自动放置在预定义的位置中。 此外，可使用 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) 来允许用户指示要在设备上保存文件的位置。 请注意，为将回调重新分配到 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 而调用的 **load** 方法（如果它在应用终止后仍然保持）在本部分后面定义的 DownloadOp 类中。

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_A)]

请注意使用 JavaScript Promise 定义的异步方法调用。 查看上一代码示例中的第 17 行：

```javascript
promise = download.startAsync().then(complete, error, progress);
```

异步方法调用后跟一个 then 语句，它指示从异步方法调用返回结果时调用的方法（由应用定义）。 有关此编程模式的详细信息，请参阅[在 JavaScript 中使用 Promise 进行异步编程](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx)。

### <a name="adding-additional-operation-control-methods"></a>添加其他操作控制方法
控制级别可通过实现附加的 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 方法来进行提升。 例如，将以下代码添加到上述示例将引入取消下载的功能。

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_B)]

### <a name="enumerating-persisted-operations-at-start-up"></a>在启动时枚举保持的操作
在完成或取消 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 时，将释放任何关联的系统资源。 不过，如果你的应用在任一事件发生之前停止，下载将暂停并在后台保持。 下例演示了如何将保持的下载重新引入新的应用会话。

1.  在定义枚举保持的操作的函数之前，我们需要创建包含将返回的 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 对象的数组：

    [!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_D)]

1.  接下来，我们定义枚举持续操作并将其存储到数组中的函数。 请注意，为将回调重新分配到 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 而调用的 **load** 方法在本部分后面定义的 DownloadOp 示例中。

    [!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_E)]

1.  现在可以使用填充的列表重新启动挂起的操作。

## <a name="post-processing"></a>后处理
Windows 10 中的新功能可以在完成后台传输时运行应用程序代码，即使该应用未在运行。 例如，你的应用可能希望在电影结束下载后更新电影的可用列表，而不是在每次启动应用时让应用扫描新电影。 或者，你的应用可能希望通过再次尝试使用不同的服务器或端口来处理已失败的文件传输。 针对成功的传输和失败的传输，均会调用后处理，以便你可以使用它来实现自定义错误处理和重试逻辑。

后处理使用现有的后台任务基础结构。 创建后台任务，并在开始传输前将后台任务与传输关联。 然后传输在后台执行，并在它们完成时，调用后台任务以执行后处理。

后处理使用新的类 [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209)。 此类类似于现有的 [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030)，它允许你将后台传输一起分组，但 **BackgroundTransferCompletionGroup** 新增了一个功能，该功能可以指定要在传输完成时运行的后台任务。

使用后处理启动后台传输，如下所示。

1.  创建 [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209) 对象。 然后，创建 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 对象。 将生成器对象的 **Trigger** 属性设置为完成组对象，并将生成器的 **TaskEntryPoint** 属性设置为应该在传输完成时执行的后台任务的入口点。 最后，调用 [**BackgroundTaskBuilder.Register**](https://msdn.microsoft.com/library/windows/apps/br224772) 方法以注册后台任务。 请注意，许多完成组可以共享一个后台任务入口点，但是每个后台任务注册只能有一个完成组。

```csharp
var completionGroup = new BackgroundTransferCompletionGroup();
BackgroundTaskBuilder builder = new BackgroundTaskBuilder();

builder.Name = "MyDownloadProcessingTask";
builder.SetTrigger(completionGroup.Trigger);
builder.TaskEntryPoint = "Tasks.BackgroundDownloadProcessingTask";

BackgroundTaskRegistration downloadProcessingTask = builder.Register();
```

2.  接下来，将后台传输与完成组关联。 创建所有传输后，启用完成组。

```csharp
BackgroundDownloader downloader = new BackgroundDownloader(completionGroup);
DownloadOperation download = downloader.CreateDownload(uri, file);
Task<DownloadOperation> startTask = download.StartAsync().AsTask();

// App still sees the normal completion path
startTask.ContinueWith(ForegroundCompletionHandler);

// Do not enable the CompletionGroup until after all downloads are created.
downloader.CompletinGroup.Enable();
```

3.  后台任务中的代码从触发器详细信息中提取操作列表，然后代码检查每个操作的详细信息，并且针对每个操作执行相应的后处理。

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

后处理任务是常规后台任务。 它是所有后台任务池的一部分，并且它受制于与所有后台任务相同的资源管理策略。

此外请注意，后处理不会代替前台完成处理程序。 如果你的应用定义了前台完成处理程序，并且该应用在文件传输完成时运行，将会调用前台完成处理程序和后台完成处理程序。 前台任务和后台任务的调用顺序无法确定。 如果定义了前台任务和后台任务，你应确保两个任务将正常运行，并且在它们并发运行时不会相互干扰。

## <a name="request-timeouts"></a>请求超时
需要考虑两个主要的连接超时方案：

-   为传输建立新连接时，如果未在 5 分钟内建立连接请求，则会中止连接请求。

-   建立连接之后，将中止在两分钟内未收到响应的 HTTP 请求消息。

> **注意**  在任何一种方案中，假定存在 Internet 连接，后台传输将最多自动重试一个请求三次。 如果未检测到 Internet 连接，则其他请求将一直等待，直到检测到 Internet 连接。

## <a name="debugging-guidance"></a>调试指南
在 Microsoft Visual Studio 中停止调试会话与关闭你的应用相似；PUT 上载将暂停，POST 上载将终止。 即使在调试时，你的应用也应该枚举，然后重新启动或取消任何保持的上载。 例如，如果对该调试会话之前的操作没有兴趣，你可以使应用在应用启动时取消已枚举的持续上载操作。

在调试会话期间，当系统随着应用启动开始枚举下载/上载时，如果对该调试会话之前的操作没有兴趣，你可以让应用取消它们。 请注意，如果存在 Visual Studio 项目更新（例如，对应用清单的更改），并且应用被卸载并重新部署，[**GetCurrentUploadsAsync**](https://msdn.microsoft.com/library/windows/apps/hh701149) 不能枚举使用之前的应用部署创建的操作。

如果在开发过程中使用后台传输功能，则你可能会遇到活动和已完成传输操作的内部缓存不同步的情况。这可能会导致无法启动新的传输操作或者无法与现有操作和 [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030) 对象交互。 在某些情况下，尝试与现有操作交互可能引发崩溃。 如果 [**TransferBehavior**](https://msdn.microsoft.com/library/windows/apps/dn279033) 属性设置为 **Parallel**，则可能产生该结果。 该问题仅在开发期间的某些应用场景中发生，该情况不适用于应用的最终用户。

使用 Visual Studio 的四种应用场景可能会导致该问题。

-   你用于创建新项目的应用名称与现有项目相同，但语言不同（例如从 C++ 改为 C#）。
-   你更改了现有项目中的目标体系结构（例如，从 x86 改为 x64）。
-   你更改了现有项目中的区域性（例如，从中性改为 en-US）。
-   你在现有项目的程序包清单中添加或删除了一项功能（例如，添加**企业身份验证**）。

常规应用服务（包含可添加或删除功能的清单更新）不会在应用的最终用户部署时导致该问题。
若要解决该问题，请彻底卸载应用的所有版本，并采用新的语言、体系结构、区域性或功能重新部署。 可通过**开始**屏幕或使用 PowerShell 和 **Remove-AppxPackage** cmdlet 完成此操作。

## <a name="exceptions-in-windowsnetworkingbackgroundtransfer"></a>Windows.Networking.BackgroundTransfer 中的异常
将统一资源标识符 (URI) 的无效字符串传递给 [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 对象的构造函数时，将引发异常。

**.NET：**[**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 类型在 C# 和 VB 中显示为 [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx)。

在 C# 和 Visual Basic 中，通过使用 .NET 4.5 中的 [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) 类和 [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) 方法之一在构造 URI 之前测试从应用用户收到的字符串，可以避免该错误。

在 C++ 中，没有可用于试用字符串和将其解析到 URI 的方法。 如果应用获取 [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 用户输入，则构造函数应位于 try/catch 块中。 如果引发了异常，该应用可以通知用户并请求新的主机名。

[**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) 命名空间具有方便的帮助程序方法，并使用 [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 命名空间中用于处理错误的枚举。 这有助于在应用中分别处理特定网络异常。

在 [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) 命名空间中的异步方法上发生的错误返回为 **HRESULT** 值。 [**BackgroundTransferError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701093) 方法用于将来自后台传送操作的网络错误转化为 [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818) 枚举值。 大部分 **WebErrorStatus** 枚举值对应由本机 HTTP 或 FTP 客户端操作返回的错误。 应用可以筛选特定 **WebErrorStatus** 枚举值来基于异常原因修改应用行为。

对于参数验证错误，应用还可以使用来自异常的 **HRESULT** 来了解关于导致该异常的错误的详细信息。 可能的 **HRESULT** 值将在 *Winerror.h* 头文件中列出。 对于大多数参数验证错误，返回的 **HRESULT** 为 **E\_INVALIDARG**。

## <a name="important-apis"></a>重要的 API
* [**Windows.Networking.BackgroundTransfer**](/uwp/api/windows.networking.backgroundtransfer)
* [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri)
* [**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets)

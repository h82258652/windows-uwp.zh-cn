---
title: 向文件进行写入的最佳做法
description: 了解有关使用各种文件编写 FileIO 和 PathIO 类方法的最佳做法。
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a6a1d93b1deaad084ff25db946199b678b35703c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369514"
---
# <a name="best-practices-for-writing-to-files"></a>向文件进行写入的最佳做法

**重要的 Api**

* [**FileIO 类**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**PathIO 类**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

有时，在使用时遇到的常见的问题的一组开发人员**编写**方法的[ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)并[ **PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)类来执行文件系统 I/O 操作。 例如，常见的问题包括：

* 部分写入文件。
* 调用的方法之一时，应用将收到异常。
* 操作留下。TMP 文件的文件名称类似于目标文件名称。

**编写**方法的[ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)并[ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio)类包括以下：

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 本文提供了有关这些方法，开发人员的工作原理的详细信息更好地了解何时以及如何使用它们。 本文提供了指导原则并不会尝试为所有可能的文件 I/O 问题提供解决方案。 

> [!NOTE]
> 这篇文章重点**FileIO**中示例和讨论的方法。 但是， **PathIO**方法遵循类似的模式和大部分这篇文章中的指南也适用于这些方法。 

## <a name="convenience-vs-control"></a>与控件的便利

一个[ **StorageFile** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)对象不是像本机 Win32 编程模型的文件句柄。 相反， [ **StorageFile** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)是具有方法来操作其内容的文件的表示形式。

了解这一概念时，可以执行与 I/O **StorageFile**。 例如，[对文件进行写入](quickstart-reading-and-writing-files.md#writing-to-a-file)部分提供了三种方法可以向文件写入：

* 使用[ **FileIO.WriteTextAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync)方法。
* 通过创建一个缓冲区，然后再调用[ **FileIO.WriteBufferAsync** ](https://docs.microsoft.com/en-us/uwp/api/windows.storage.fileio.writebufferasync)方法。
* 使用流的四步模型：
  1. [打开](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync)文件来获取的流。
  2. [获取](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat)输出流。
  3. 创建[ **DataWriter** ](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter)对象并调用相应**编写**方法。
  4. [提交](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync)中的数据编写器的数据并[刷新](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync)输出流。

前两种方案是最常用的应用程序。 写入到单个操作中的文件易于编码和维护，并且它也会从许多复杂操作的文件 I/O 处理的应用的责任。 但是，这种便利有一定代价： 控制整个操作并捕获错误的特定点上的功能丢失。

## <a name="the-transactional-model"></a>事务模型

**编写**方法的[ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)并[ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio)类包装在第三个写入的步骤添加了一层上方所述的模型。 此层封装在存储事务。

若要在写入数据时出现问题的情况下保护原始文件的完整性**编写**方法使用通过打开文件使用的事务模型[ **OpenTransactedWriteAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync). 此过程将创建[ **StorageStreamTransaction** ](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction)对象。 创建此事务对象后，Api 编写遵循相似的格式的数据[文件访问](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess)示例或中的代码示例[ **StorageStreamTransaction** ](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction)一文。

下图说明了执行的基础任务**WriteTextAsync**中成功写入操作的方法。 此图提供了该操作的简化的视图。 例如，它会跳过步骤，例如文本编码和异步完成在不同线程上。

![UWP API 调用的顺序关系图写入文件](images/file-write-call-sequence.svg)

使用的优点**编写**方法的[ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)并[ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio)改为类使用流的更复杂的四步模型是：

* 一次 API 调用来处理所有中间步骤，包括错误。
* 如果出现问题，保留原始文件。
* 系统状态将尝试保留尽可能干净。

但是，与众多可能中间的故障点，没有失败的可能性。 发生错误时可能会难以理解其中的过程失败。 以下各节介绍一些使用时可能遇到的故障**编写**方法并提供可能的解决方案。

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>FileIO 和 PathIO 类的 Write 方法的常见错误代码

此表列出了应用程序开发人员使用时遇到的常见错误代码**编写**方法。 表中的步骤对应于在上图中的步骤。

|  错误名称 （值）  |  步骤  |  原因  |  解决方案  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0X80070005)  |  5  |  原始文件可能会标记为删除，可能是从以前的操作。  |  重试操作。</br>确保同步对文件的访问。  |
|  ERROR_SHARING_VIOLATION (0x80070020)  |  5  |  原始文件正由另一个排他写入。   |  重试操作。</br>确保同步对文件的访问。  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0x80070497)  |  19-20  |  不会替换原始文件 (file.txt)，因为它正在使用中。 另一个进程或操作获得了对文件的访问，则可能被替换为之前。  |  重试操作。</br>确保同步对文件的访问。  |
|  ERROR_DISK_FULL (0x80070070)  |  7, 14, 16, 20  |  事务处理的模型创建额外的文件，而这会消耗额外的存储。  |    |
|  ERROR_OUTOFMEMORY (已用完 0X8007000E)  |  14, 16  |  这可能是由于多个未完成 I/O 操作或大型文件的大小。  |  通过控制流的更精细方法可能会解决该错误。  |
|  E_FAIL (0x80004005) |  Any  |  其他  |  重试操作。 如果仍然失败，这可能是平台错误，应用程序应终止，因为它处于不一致的状态。 |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>可能会导致错误的文件状态的其他注意事项

除了返回的错误**编写**方法，下面是在对文件进行写入时，应用可以期望的指导。

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>当且仅当操作完成，数据已写入到文件

您的应用程序不应在文件中造成关于数据的任何假设，在进行写入操作时。 尝试访问该文件操作完成之前可能会导致不一致的数据。 您的应用程序应负责跟踪的未完成 I/o。

### <a name="readers"></a>Readers

如果该文件，写入到也是正由正常读取器 (即，使用打开[ **FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode)，后续读取将失败并返回错误 ERROR_OPLOCK_HANDLE_CLOSED (0x80070323)。 有时应用程序重试打开文件进行读取再次同时**编写**操作正在进行中。 这可能会导致在其上的争用条件**编写**最终失败时尝试覆盖原始文件，因为它不能取代。

### <a name="files-from-knownfolders"></a>从 KnownFolders 文件

您的应用程序可能不是唯一的应用尝试访问文件所在的任何[ **KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)。 则，如果操作成功，应用程序写入该文件的内容将保持不变不能保证下一次它会尝试读取该文件。 此外，共享或访问被拒绝在此种情况下更为常见的错误。

### <a name="conflicting-io"></a>冲突的 I/O

如果我们的应用程序使用可以降低并发错误的可能性**编写**其本地数据，但某些警告中的文件的方法是仍然需要。 如果多个**编写**操作同时向发送该文件，有关哪些数据最终在文件中不能保证。 若要缓解此问题，我们建议您的应用程序序列化**编写**文件的操作。

### <a name="tmp-files"></a>~ TMP 文件

有时，则会强制取消操作 （例如，如果应用已挂起或终止由操作系统），如果事务没有提交或适当地关闭。 这可以在使用的文件后保留 (。 ~ TMP) 扩展。 请考虑删除这些临时文件 （如果它们存在于应用的本地数据） 处理应用程序激活时。

## <a name="considerations-based-on-file-types"></a>基于文件类型的注意事项

某些错误可能会变得更加普遍，具体取决于文件、 在其正在访问它们的频率和文件大小的类型。 通常情况下，有三种类别的应用程序可以访问的文件：

* 文件中创建和编辑用户在应用的本地数据文件夹中。 这些是创建的仅在使用您的应用程序时编辑并且仅在应用程序中存在。
* 应用元数据。 您的应用程序使用这些文件来跟踪其自己的状态。
* 您的应用程序有声明功能来访问的位置的文件系统位置中的其他文件。 这些通常位于之一[ **KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)。

您的应用程序具有完全控制权限的文件前, 两个类别，因为它们是你的应用包文件的一部分，并以独占方式访问你的应用。 对于最后一个类别中的文件，必须注意，其他应用程序和操作系统服务可能正在访问这些文件同时应用。

具体取决于应用程序中，对文件的访问而异的频率：

* 非常低。 这些通常是后时的应用启动并保存挂起应用程序时打开的文件。
* 低。 这些是用户 （如保存或加载） 对其专门执行一个操作的文件。
* 中等或高。 这些是应用程序必须不断地更新数据 （例如，自动保存功能或跟踪的常量元数据） 的文件。

文件大小，请考虑以下图表中的性能数据**WriteBytesAsync**方法。 该图表比较了完成的操作与文件大小，通过在受控环境中的文件大小每 10000 次操作的平均性能的时间。

![WriteBytesAsync 性能](images/writebytesasync-performance.png)

在 y 轴上的时间值中忽略了有意此图表因为不同的硬件和配置会产生不同的绝对时间值。 但是，我们已一致地观察到这些趋势在我们的测试：

* 对于非常小的文件 (< = 1 MB):快速一致地完成操作的时间。
* 对于较大的文件 (> 1 MB):完成操作的时间开始呈指数级增长。

## <a name="io-during-app-suspension"></a>在应用程序暂挂期间的 I/O

您的应用程序必须能够处理挂起，如果想要保留状态信息或元数据以便在后续会话中使用。 有关应用程序暂挂背景信息，请参阅[应用程序生命周期](../launch-resume/app-lifecycle.md)并[这篇博客文章](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97)。

除非在 OS 授予向应用程序，扩展的执行挂起您的应用程序时它具有 5 秒钟，以释放其资源并保存其数据。 最佳的可靠性和用户体验，始终假设你需要处理挂起任务的时间是有限。 请记住以下准则在 5 秒的挂起任务的处理时间段：

* 尝试将 I/O 控制在最小以避免由刷新和发布操作所导致的争用情况。
* 避免编写需要数百毫秒或更多要写入的文件。
* 如果您的应用程序使用**编写**方法，请记住这些方法需要的所有中间步骤。

如果您的应用程序在挂起期间对少量的状态数据，在大多数情况下可以使用**编写**方法将数据刷新。 但是，如果您的应用程序使用大量的状态数据，请考虑使用流来直接存储数据。 这可以帮助减少延迟的事务模型通过引入**编写**方法。 

有关示例，请参阅[BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension)示例。

## <a name="other-examples-and-resources"></a>其他示例和资源

下面是几个示例和用于特定方案的其他资源。

### <a name="code-example-for-retrying-file-io-example"></a>重试文件 I/O 示例的代码示例

以下是有关如何重试写入一个伪代码示例 (C#)，假设写是用户选取来保存文件后必须执行：

```csharp
Windows.Storage.Pickers.FileSavePicker savePicker = new Windows.Storage.Pickers.FileSavePicker();
savePicker.FileTypeChoices.Add("Plain Text", new List<string>() { ".txt" });
Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();

Int32 retryAttempts = 5;

const Int32 ERROR_ACCESS_DENIED = unchecked((Int32)0x80070005);
const Int32 ERROR_SHARING_VIOLATION = unchecked((Int32)0x80070020);

if (file != null)
{
    // Application now has read/write access to the picked file.
    while (retryAttempts > 0)
    {
        try
        {
            retryAttempts--;
            await Windows.Storage.FileIO.WriteTextAsync(file, "Text to write to file");
            break;
        }
        catch (Exception ex) when ((ex.HResult == ERROR_ACCESS_DENIED) ||
                                   (ex.HResult == ERROR_SHARING_VIOLATION))
        {
            // This might be recovered by retrying, otherwise let the exception be raised.
            // The app can decide to wait before retrying.
        }
    }
}
else
{
    // The operation was cancelled in the picker dialog.
}
```

### <a name="synchronize-access-to-the-file"></a>同步对文件的访问

[使用.NET 博客进行并行编程](https://devblogs.microsoft.com/pfxteam/)是一个不错的资源的并行编程有关的指南。 具体而言，[发表 AsyncReaderWriterLock](https://devblogs.microsoft.com/pfxteam/building-async-coordination-primitives-part-7-asyncreaderwriterlock/)描述如何维护独占访问的文件的写入，同时允许并发读取访问权限。 请注意，序列的 I/O 会影响性能。

## <a name="see-also"></a>请参阅

* [创建、写入和读取文件](quickstart-reading-and-writing-files.md)

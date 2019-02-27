---
title: 用于向文件写入的最佳做法
description: 了解有关使用各种文件写入 FileIO 和 PathIO 类的方法的最佳实践。
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f8bed97e060015f92ff95c9f7d797bbcb83db431
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2019
ms.locfileid: "9115702"
---
# <a name="best-practices-for-writing-to-files"></a>用于向文件写入的最佳做法

**重要的 API**

* [**FileIO 类**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**PathIO 类**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

使用**编写**的[**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)和[**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)类的方法来执行文件系统 I/O 操作时，开发人员有时运行到一组的常见的问题。 例如，常见问题包括：

• 文件部分编写 • 应用时调用的方法之一收到的异常。 • 操作遗留。TMP 文件的文件名称类似于目标文件名称。

**编写**的[**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)和[**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)类的方法包括：

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 本文提供有关这些方法是开发人员的工作原理的详细信息更好地了解何时以及如何使用它们。 本文提供了指南并不会尝试提供所有可能的文件 I/O 问题的解决方案。 

> [!NOTE]
> 本文重点介绍示例和讨论的**FileIO**方法。 但是， **PathIO**方法遵循类似的模式，而且大部分本文章中的指南也适用于这些方法。 

## <a name="conveience-vs-control"></a>与控制 Conveience

[**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)对象不是像本机 Win32 编程模型文件句柄。 相反， [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)是具有方法来操作其内容的文件表示。

当执行与**StorageFile**I/O，了解这一概念非常有用。 例如，[写入到文件](quickstart-reading-and-writing-files.md#writing-to-a-file)部分将显示三种方法可写入文件：

* 使用[**FileIO.WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync)方法。
* 通过创建一个缓冲区并调用[**FileIO.WriteBufferAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.storage.fileio.writebufferasync)方法。
* 使用流的四个步骤模型：
  1. [打开](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync)该文件以获取的流。
  2. [获取](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat)输出流。
  3. 创建的[**DataWriter**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter)对象并调用相应的**写入**方法。
  4. [提交](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync)数据编写器和[刷新](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync)输出流中的数据。

前两个方案是最常使用的应用。 写入到单个操作中的文件易于代码和维护，并且它还将删除的应用的责任处理文件 I/O 的复杂性。 但是，此便利，成本： 控制整个操作，并且能够捕获错误的特定点处中断。

## <a name="the-transactional-model"></a>事务性模型

[**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)和[**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)类的**写入**方法换行上添加层使用上述方法的第三个写入模型的步骤。 此层封装在存储交易。

若要保护原始文件的完整性，以防写入数据时出现问题，**写入**方法时，可使用事务性模型通过打开使用[**OpenTransactedWriteAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync)的文件。 此过程创建[**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction)对象。 创建此交易对象后，Api 编写数据[文件访问](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess)示例或[**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction)文章中的代码示例遵循类似的方式。

下图说明了执行的基础任务成功写入操作中的**WriteTextAsync**方法。 此图提供了简化的操作的视图。 例如，在不同的线程上跳如文本编码和异步完成的步骤。

![UWP API 调用序列图写入文件](images/file-write-call-sequence.svg)

而不使用流的更复杂的四个步骤模型中使用的[**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)和[**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)类的**写入**方法的优点为：

* 一个 API 调用来处理所有中间的步骤，包括错误。
* 如果出现错误，将保持原始文件。
* 系统状态将尝试保持为干净尽可能。

但是，因此许多可能中间点故障，使用没有故障的可能性。 发生错误时它可能很难理解过程失败的位置。 以下各节存在一些你可能会遇到使用的**写入**方法时，并提供可能的解决方案的故障。

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>FileIO 和 PathIO 类写入方法的常见错误代码

此表列出了常见应用开发人员使用的**写入**方法时遇到的错误代码。 在表中的步骤对应于在上图中的步骤。

|  错误名称 （值）  |  步骤  |  原因  |  解决方案  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0X80070005)  |  5  |  原始文件可能会被标记为删除，可能来自前一操作。  |  重试该操作。</br>确保同步访问该文件。  |
|  ERROR_SHARING_VIOLATION (0X80070020)  |  5  |  原始文件打开的另一个独占写入。   |  重试该操作。</br>确保同步访问该文件。  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0X80070497)  |  19-20  |  不会替换的原始文件 (file.txt)，因为它是使用。 另一个进程或操作并访问该文件之前无法替换它。  |  重试该操作。</br>确保同步访问该文件。  |
|  ERROR_DISK_FULL (0X80070070)  |  7、 14、 16、 20  |  事务处理的模型创建额外的文件，而这会使用额外的存储。  |    |
|  ERROR_OUTOFMEMORY (0X8007000E)  |  14、 16  |  这可能由于多个未完成的 I/O 操作或大文件大小。  |  通过控制流的更细化地方法可能解决错误。  |
|  E_FAIL (0X80004005) |  Any  |  杂项  |  重试该操作。 如果再次失败，这可能是一个平台错误，应用应终止，因为它处于不一致的状态。 |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>可能会导致错误的文件状态的其他注意事项

除**写入**方法返回的错误，下面是应用可以期望的写入文件时的一些指南。

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>当且仅当操作完成数据写入到文件

你的应用不应在文件中进行任何假设数据，写入操作正在进行时。 尝试访问该文件在操作完成之前可能会导致不一致的数据。 你的应用应负责跟踪未完成 I/o。

### <a name="readers"></a>阅读器

如果该文件的写入也是正在由礼貌用语读取器 （即，使用[**FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode)打开，后续读取将失败并出错 ERROR_OPLOCK_HANDLE_CLOSED (0x80070323)。 有时应用重试再次打开文件的读**写**操作正在进行时。 这可能会导致在其最终**编写**失败尝试覆盖原始文件，因为它无法替换时的争用条件。

### <a name="files-from-knownfolders"></a>KnownFolders 中的文件

你的应用可能不会尝试访问任何[**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)驻留的文件的唯一应用。 就不能保证，如果操作成功，应用写入该文件的内容将保持不变下次它尝试将文件读取。 此外，共享或访问被拒绝变得更常见在此方案中的错误。

### <a name="conflicting-io"></a>冲突 I/O

如果我们的应用使用适用于在其本地数据中的文件的**写入**方法，但仍需要一些警告的可以降低并发错误的几率。 如果多个**写入**操作将同时发送到该文件，就有关哪些数据结束文件中不能保证。 若要缓解这种风险，我们建议你的应用将向文件**写入**操作序列化。

### <a name="tmp-files"></a>~ TMP 文件

有时，如果 （例如，如果应用已暂停或终止操作系统），则强制取消操作，不提交或事务相应地关闭。 这会使在使用的文件后 (。 ~ TMP) 扩展。 请考虑删除这些临时文件 （如果它们存在于应用的本地数据） 时处理应用激活。

## <a name="considerations-based-on-file-types"></a>根据文件类型的注意事项

一些错误可能会变得更流行具体取决于文件、 在其它们正在访问的频率和其文件大小的类型。 通常情况下，有三个类别的你的应用可以访问的文件：

* 文件中创建和编辑用户在应用的本地数据文件夹中。 这些是创建和编辑仅在使用你的应用，且仅在应用中存在。
* 应用元数据。 你的应用使用这些文件以跟踪其自己的状态。
* 你的应用已声明功能即可访问的位置的文件系统位置中的其他文件。 这些通常位于[**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)之一。

你的应用具有的文件前, 两个类别的完全控制，因为它们是你的应用包文件的一部分，并且以独占方式访问你的应用。 对于最后一个类别中的文件，必须知道，其他应用和操作系统服务可能要访问文件同时设置为你的应用。

具体取决于应用访问的文件可能频率：

* 非常低。 这些通常是后，当应用启动并保存该应用暂停时打开的文件。
* 低。 这些是用户 （如保存或加载） 对其专门执行操作的文件。
* 中等或高。 这些是应用必须不断更新数据 （例如，自动保存功能或跟踪的常量元数据） 的文件。

文件大小，请考虑下图中的**WriteBytesAsync**方法中的性能数据。 此图表将完成操作 vs 文件的大小，通过在受控环境中的文件大小每 10000 操作平均性能的时间进行比较。

![WriteBytesAsync 性能](images/writebytesasync-performance.png)

Y 轴上的时间值是有意从忽略此图表因为不同的硬件和配置将产生不同的绝对时间值。 但是，我们已完全一致地观察这些趋势，在我们的测试：

* 非常小的文件 (< = 1 MB): 是完全一致地快速完成的操作的时间。
* 对于较大的文件 (> 1 MB): 完成的操作的时间开始成倍增加。

## <a name="io-during-app-suspension"></a>在应用挂起期间的 I/O

你的应用必须设计为处理暂停，如果想要保持状态信息或元数据在更高版本的会话中使用。 有关应用挂起的背景信息，请参阅[应用生命周期](../launch-resume/app-lifecycle.md)和[这篇博客文章](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97)。

除非操作系统授予对你的应用，扩展的执行在应用暂停时它有 5 秒时间来释放所有资源并保存其数据。 为了获得最佳的可靠性和用户体验，始终假定您需要处理暂停任务的时间有限。 请记住以下指南在 5 秒时间段内处理暂停任务：

* 尝试使 I/O 保持在最小值，以避免出现争用情况由刷新和发布操作。
* 避免编写需要数百毫秒或更多以写入的文件。
* 如果你的应用使用的**写入**方法，请记住所有这些方法需要的中间步骤。

如果在挂起期间，少量的状态数据上运行应用，在大多数情况下你可以使用**写入**方法刷新数据。 但是，如果你的应用使用大量状态数据，请考虑使用流直接存储数据。 这可以帮助减少引入的**写入**方法事务性模型的延迟。 

有关示例，请参阅[BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension)示例。

## <a name="other-examples-and-resources"></a>其他示例和资源

下面是几个示例和其他资源的特定方案。

### <a name="code-example-for-retrying-file-io-example"></a>用于重试文件 I/O 示例的代码示例

以下是有关如何重试写入 (C#) 假设写入后用户选取要保存的文件的伪代码示例：

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

### <a name="synchronize-access-to-the-file"></a>同步访问文件

[使用.NET 博客的并行编程](https://blogs.msdn.microsoft.com/pfxteam/)是有关并行编程指南的绝佳资源。 特别是，[有关 AsyncReaderWriterLock 文章](https://blogs.msdn.microsoft.com/pfxteam/2012/02/12/building-async-coordination-primitives-part-7-asyncreaderwriterlock/)介绍了如何同时允许并发读取访问权限的写入维护独占访问的文件。 请记住，序列 I/O 将影响性能。

## <a name="see-also"></a>另请参阅

* [创建、写入和读取文件](quickstart-reading-and-writing-files.md)
